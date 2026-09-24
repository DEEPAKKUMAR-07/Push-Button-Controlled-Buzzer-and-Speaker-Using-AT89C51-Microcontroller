# Push-Button-Controlled-Buzzer-and-Speaker-Using-AT89C51-Microcontroller
Design and Implementation of a Push-Button-Controlled Buzzer and Speaker Using AT89C51 Microcontroller
# Design and Implementation of a Push-Button-Controlled Buzzer and Speaker Using AT89C51 Microcontroller

## Aim

To interface a push button, buzzer, and speaker with the AT89C51 microcontroller and activate the audible indicators when the button is pressed.

## Components Required

| S. No. | Component | Specification | Quantity |
|---:|---|---|---:|
| 1 | Microcontroller | AT89C51 | 1 |
| 2 | Push button | Normally open | 2 |
| 3 | Buzzer | 5 V | 1 |
| 4 | Speaker | Suitable low-power speaker | 1 |
| 5 | NPN transistor | BC547 | 1 |
| 6 | Base resistor | 330 Ω | 1 |
| 7 | Pull-down resistor | 10 kΩ | 1 |
| 8 | Reset resistor | 10 kΩ | 1 |
| 9 | Capacitor | 0.1 µF | 1 |
| 10 | Power supply | Regulated +5 V DC | 1 |
| 11 | Connecting wires | As required | — |

## Circuit Connections

| AT89C51/Device Pin | Connection |
|---|---|
| `P1.2` (pin 3) | Input push button |
| Input push-button terminal 1 | `+5 V` |
| Input push-button terminal 2 | `P1.2` |
| `R3` (10 kΩ) | Connected between `P1.2` and ground as a pull-down resistor |
| `P3.2/INT0` (pin 12) | Connected to the BC547 base through a 330 Ω resistor |
| BC547 emitter | Ground |
| BC547 collector | Negative terminals of the buzzer and speaker |
| Buzzer and speaker positive terminals | `+5 V` |
| `RST` (pin 9) | Reset circuit containing a push button, capacitor, and 10 kΩ resistor |
| `VCC` (pin 40) | `+5 V` |
| `GND` (pin 20) | Ground |

> **Note:** All devices must share a common ground. For a real inductive or magnetic buzzer, connect a flyback diode across it to protect the transistor. The AT89C51 also requires a suitable clock circuit connected to `XTAL1` and `XTAL2`.

## Working Principle

The push button connected to `P1.2` provides a digital input to the AT89C51 microcontroller. The 10 kΩ pull-down resistor keeps the input LOW when the button is released. When the button is pressed, it connects the input pin to `+5 V`, producing a HIGH logic level.

The microcontroller continuously monitors the state of `P1.2`. When it detects a HIGH input, it makes `P3.2` HIGH. The resulting base current switches ON the BC547 transistor, allowing current to flow through the buzzer and speaker. Both devices then produce sound.

When the push button is released, `P1.2` becomes LOW. The microcontroller makes `P3.2` LOW, switching OFF the transistor, buzzer, and speaker.

## Algorithm

1. Start the system.
2. Initialize the input and output pins.
3. Configure `P1.2` as the push-button input.
4. Configure `P3.2` as the buzzer-control output.
5. Read the state of the push button.
6. Check whether the button is pressed.
7. If the button is pressed, make `P3.2` HIGH.
8. Switch ON the transistor, buzzer, and speaker.
9. If the button is released, make `P3.2` LOW.
10. Switch OFF the transistor, buzzer, and speaker.
11. Repeat the process continuously.

## Embedded C Program

```c
#include <reg51.h>

sbit BUTTON = P1^2;
sbit BUZZER = P3^2;

void delay_ms(unsigned int ms)
{
    unsigned int i, j;

    for (i = 0; i < ms; i++)
    {
        for (j = 0; j < 112; j++)
        {
            /* Approximate delay */
        }
    }
}

void main(void)
{
    /* Configure the button pin as input */
    BUTTON = 1;

    /* Initially switch OFF the buzzer and speaker */
    BUZZER = 0;

    while (1)
    {
        /* Check whether the push button is pressed */
        if (BUTTON == 1)
        {
            /* Debouncing delay */
            delay_ms(20);

            if (BUTTON == 1)
            {
                /* Switch ON the buzzer and speaker */
                BUZZER = 1;
            }
        }
        else
        {
            /* Switch OFF the buzzer and speaker */
            BUZZER = 0;
        }
    }
}
```

## Program Explanation

- `BUTTON` is assigned to the input pin `P1.2`.
- `BUZZER` is assigned to the output pin `P3.2`.
- `BUTTON = 1` configures the quasi-bidirectional 8051 port pin for input operation.
- `BUZZER = 0` initially keeps the buzzer and speaker switched OFF.
- The microcontroller continuously monitors the button inside the `while` loop.
- A delay of approximately 20 milliseconds reduces false triggering caused by switch bouncing.
- When the button input is HIGH, `P3.2` becomes HIGH.
- The HIGH output drives the BC547 transistor and activates the buzzer and speaker.
- When the input becomes LOW, the transistor, buzzer, and speaker are switched OFF.

## Expected Output

| Push-button condition | `P1.2` input | `P3.2` output | Buzzer and speaker |
|---|---:|---:|---|
| Released | LOW | LOW | OFF |
| Pressed | HIGH | HIGH | ON |

## Output
<img width="1312" height="1199" alt="image" src="https://github.com/user-attachments/assets/89277448-3cdd-4afa-8211-e61953db64d3" />

## Applications

- Security alarm systems
- Emergency warning systems
- Doorbell circuits
- Industrial fault indicators
- Vehicle alert systems
- Patient assistance systems

## Result

The push button was successfully interfaced with the AT89C51 microcontroller. The buzzer and speaker were activated when the push button was pressed and switched OFF when the button was released.
