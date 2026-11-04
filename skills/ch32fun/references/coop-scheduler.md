# Cooperative Scheduler Pattern (SysTick PeriodicJob)

A lightweight C++ cooperative periodic task scheduler designed specifically for bare-metal `ch32fun` firmware on WCH RISC-V microcontrollers (e.g. CH32V003 with only 2KB SRAM and 16KB Flash), eliminating the need for an RTOS or heap allocations (`malloc`/`free`).

---

## When to Use

- **Multiple periodic tasks**: When an MCU firmware needs to run multiple independent recurring routines (e.g. status LED heartbeat, sensor sampling, debouncing inputs, communication timeouts).
- **Resource-constrained WCH targets**: On devices like CH32V003 where FreeRTOS / RT-Thread is too heavy for the 2KB RAM budget.
- **Avoiding `Delay_ms()` blocking**: When `Delay_ms()` in the main loop would freeze other activities, responsiveness, or serial communication.
- **Strict bare-metal / no-heap**: When deterministic, zero-allocation memory usage is required.

### When NOT to Use
- **Hard real-time / pre-emptive tasks**: Cooperative tasks must yield control quickly. If a task requires sub-millisecond guaranteed latency regardless of what else is running, use hardware timer interrupts (`TIM1`/`TIM2`) or external interrupts (`EXTI`).
- **Long-blocking operations**: If any task calls blocking delay loops or waits indefinitely on polling loops, it will starve all other cooperative jobs.

---

## Architecture & Code

### 1. Header: `periodic_job.h`

```cpp
#pragma once

#include <stdint.h>

class PeriodicJob
{
public:
    explicit PeriodicJob(void (*cb)());

    // Call inside while(1) with the desired recurrence interval in milliseconds
    void runEvery(uint32_t period);

    static void init();
    static uint32_t millis();

    // Used by the SysTick ISR in PeriodicJob.cpp
    static volatile uint32_t millisCounter;

private:
    uint32_t lastRun;
    void (*callback)();
};
```

---

### 2. Implementation: `periodic_job.cpp`

```cpp
#include "periodic_job.h"
#include "ch32fun.h"

volatile uint32_t PeriodicJob::millisCounter = 0;

PeriodicJob::PeriodicJob(void (*cb)())
    : lastRun(0),
      callback(cb)
{
}

uint32_t PeriodicJob::millis()
{
    return millisCounter;
}

void PeriodicJob::runEvery(uint32_t period)
{
    uint32_t now = millis();

    // Safe subtraction avoids 32-bit unsigned rollover overflow issues
    if ((uint32_t)(now - lastRun) >= period)
    {
        lastRun = now;
        callback();
    }
}

void PeriodicJob::init()
{
    millisCounter = 0;

    SysTick->CTLR = 0;
    SysTick->CNT  = 0;
    SysTick->CMP  = DELAY_MS_TIME - 1;

    // CRITICAL HARDWARE NOTE:
    // Do NOT enable SYSTICK_CTLR_STCLK.
    // In ch32fun, DELAY_MS_TIME is calculated assuming HCLK/8.
    SysTick->CTLR =
        SYSTICK_CTLR_STE |
        SYSTICK_CTLR_STIE;

    NVIC_EnableIRQ(SysTick_IRQn);
}

// Ensure proper QingKe RISC-V interrupt linkage and attribute
extern "C"
void SysTick_Handler(void) __attribute__((interrupt));

extern "C"
void SysTick_Handler(void)
{
    SysTick->SR = 0;
    SysTick->CMP += DELAY_MS_TIME;

    PeriodicJob::millisCounter++;
}
```

---

## Concrete Usage Examples

### Example 1: Multi-Rate Task Scheduling (Debounce + Sensor + Heartbeat)

```cpp
#include "ch32fun.h"
#include "periodic_job.h"

int main()
{
    SystemInit();

    funGpioInitAll();
    funPinMode(PC1, GPIO_Speed_10MHz | GPIO_CNF_OUT_PP); // Status LED
    funPinMode(PC2, GPIO_Speed_10MHz | GPIO_CNF_OUT_PP); // Output pin

    PeriodicJob::init();

    // Fast task: 100ms button poll / debounce
    PeriodicJob pollInputs([]() {
        // Read input pin and update button state
    });

    // Medium task: 500ms sensor read / compute
    PeriodicJob sampleSensors([]() {
        // Read ADC or I2C sensor
    });

    // Slow task: 1000ms LED heartbeat blink
    PeriodicJob heartbeat([]() {
        static bool ledState = false;
        ledState = !ledState;
        funDigitalWrite(PC1, ledState ? FUN_HIGH : FUN_LOW);
    });

    // Non-blocking cooperative main event loop
    while (1)
    {
        pollInputs.runEvery(100);
        sampleSensors.runEvery(500);
        heartbeat.runEvery(1000);
    }
}
```

### Example 2: Dynamic Interval / One-Shot Trigger Pattern

Because `runEvery()` evaluates against `lastRun`, you can dynamically change intervals:

```cpp
uint32_t currentInterval = 100;

PeriodicJob adaptiveBlink([]() {
    static bool state = false;
    state = !state;
    funDigitalWrite(PC1, state ? FUN_HIGH : FUN_LOW);
});

while (1)
{
    // Speed up blink if error condition detected
    uint32_t period = (systemError) ? 100 : 1000;
    adaptiveBlink.runEvery(period);
}
```

---

## Integration in `ch32fun` Projects

### Compilation with `Makefile`
When adding C++ files to a `ch32fun` project:
1. Ensure your Makefile invokes `$(CXX)` for `.cpp` files with flags `-fno-rtti -fno-exceptions` to preserve minimal flash footprint.
2. Link `periodic_job.o` alongside your application source and `ch32fun.o`.
3. `SysTick_Handler` in `periodic_job.cpp` overrides the default weak handler in `ch32fun.c`.

---

## Critical Pitfalls & Rules

1. **Clock Divider (`DELAY_MS_TIME`)**:
   - In `ch32fun`, `DELAY_MS_TIME` equals `FUNCONF_SYSTEM_CORE_CLOCK / 8000`. Setting `SYSTICK_CTLR_STCLK` makes the timer tick at `HCLK` (8x faster), making 1ms intervals fire every 125 microseconds. Keep `STCLK` disabled.
2. **Never Call `Delay_ms()` in Callbacks**:
   - The scheduler is cooperative. A `Delay_ms(500)` inside a callback blocks the main thread, causing all other periodic jobs to miss their deadlines.
3. **Rollover Safe Math**:
   - Always check elapsed time via subtraction: `(uint32_t)(now - lastRun) >= period`. Never do `now >= lastRun + period`, which breaks when 32-bit `millisCounter` rolls over after ~49.7 days.
4. **Static Allocation Only**:
   - Instantiate `PeriodicJob` objects statically or in `main()` before the infinite loop. Never use dynamic allocation (`new` / `malloc`).
