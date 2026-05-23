# STM32-FreeRTOS-Counting-Semaphore-Resource-Sharing
FreeRTOS-based STM32 project demonstrating counting semaphore synchronization, shared resource management, multitasking, and task interleaving analysis using SWV ITM trace debugging and CMSIS-RTOS V2.

---

## Overview

This project demonstrates how counting semaphores can be used in FreeRTOS to control access to a limited shared resource when multiple tasks execute simultaneously.

Three independent tasks:

- TaskA
- TaskB
- TaskC

attempt to access a shared SWV ITM trace output resource concurrently.

A counting semaphore is used to limit simultaneous access to the shared resource and observe task interleaving behavior.

The project uses:

- FreeRTOS Middleware
- CMSIS-RTOS V2 API
- SWV ITM Trace Debugging
- Counting Semaphores

---

## Features

- FreeRTOS multitasking
- Counting semaphore synchronization
- Shared resource access control
- SWV ITM trace debugging
- Task interleaving analysis
- CMSIS-RTOS V2 implementation
- Real-time task scheduling

---

## Hardware Requirements

- STM32 Nucleo-F446RE Development Board
- USB Type-A to Mini-B Cable

---

## Software Requirements

- STM32CubeIDE
- STM32CubeMX
- FreeRTOS Middleware

---

## RTOS Configuration

| Component | Configuration |
|------------|----------------|
| RTOS Interface | CMSIS_V2 |
| Tasks | 3 |
| Semaphore Type | Counting Semaphore |
| Initial Count | 2 |
| Maximum Count | 2 |
| Debug Mode | SWV ITM Trace |

---

## Tasks

| Task Name | Output Character |
|------------|-------------------|
| TaskA | A |
| TaskB | B |
| TaskC | C |

Each task continuously:

1. Acquires semaphore
2. Prints characters to SWV console
3. Releases semaphore
4. Delays briefly
5. Repeats execution

---

## Counting Semaphore Working

The counting semaphore allows a limited number of tasks to access the shared resource simultaneously.

### Semaphore Configuration

```c
osSemaphoreNew(2, 2, NULL);
```

Where:

- Maximum Count = 2
- Initial Count = 2

This means only two tasks can access the trace resource at the same time.

---

## Working Principle

### Resource Access Flow

1. Task requests semaphore
2. If semaphore count > 0:
   - Task acquires resource
   - Semaphore count decreases
3. If semaphore count = 0:
   - Task enters blocked state
4. After task completion:
   - Semaphore is released
   - Count increases again

---

## Source Code

### SWV ITM Print Function

```c
int _write(int file, char *ptr, int len)
{
    for (int i = 0; i < len; i++)
    {
        ITM_SendChar(*ptr++);
    }

    return len;
}
```

---

### TaskA Function

```c
void func_TaskA(void *argument)
{
    char ch = 'A';

    for(;;)
    {
        osSemaphoreAcquire(
            myCountingSem01Handle,
            osWaitForever);

        printf("1");

        for(int i = 0; i < 10; i++)
        {
            printf("%c", ch);

            HAL_Delay(50);
        }

        osSemaphoreRelease(
            myCountingSem01Handle);

        osDelay(5);
    }
}
```

---

## SWV ITM Trace Output

Example output:

```text
1AAAAAAAAAA
2BBBBBBBBBB
3CCCCCCCCCC
```

Due to multitasking and semaphore sharing, task outputs may interleave:

```text
ACACACAC
BABABABA
BCBCBCBC
```

This demonstrates concurrent access to a shared communication resource.

---

## Build and Run

1. Open project in STM32CubeIDE
2. Configure FreeRTOS middleware
3. Create three tasks
4. Add counting semaphore
5. Build the project
6. Connect STM32 board via USB
7. Start debugging mode
8. Enable SWV ITM Data Console
9. Start Trace and run the program

---

## Expected Output

- Multiple tasks execute concurrently
- Semaphore controls resource access
- SWV ITM console displays interleaved task outputs
- Only two tasks access resource simultaneously

---

## Learning Outcomes

- Understanding counting semaphores
- Shared resource management
- RTOS synchronization techniques
- Task scheduling and blocking
- SWV ITM debugging
- Task interleaving analysis
- CMSIS-RTOS V2 APIs

---

## Future Improvements

- Replace counting semaphore with mutex
- Add priority-based scheduling
- Implement queue communication
- Add UART-based logging
- Use dynamic task creation

---

## Author

**Ayush Jangra**  
ECE Student | Chitkara University

---

## License

This project is created for educational and academic purposes.
