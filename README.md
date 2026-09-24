# EXTERNAL INTERRUPT AND TIMER INTERRUPT USING ARDUINO UNO

## EXP 3: EXTERNAL INTERRUPT AND TIMER INTERRUPT USING ARDUINO UNO

### Aim
To implement External Interrupt and Timer Interrupt using an Arduino UNO and observe interrupt-driven execution.

# Hardware / Software Tools Required

- Arduino UNO Board
- USB Cable
- PC/Laptop with Arduino IDE Installed
- Breadboard
- Push Button
- LED
- 220 Ω Resistor
- 10 kΩ Resistor (Pull-down, optional if not using INPUT_PULLUP)
- Jumper Wires

# Circuit Diagram
<img width="1076" height="593" alt="image" src="https://github.com/user-attachments/assets/627b718e-2384-42d8-aade-b83e65d81e02" />

# Procedure

## Step 1: Assemble the Circuit

1. Place the Arduino UNO, breadboard, push button, LED, and resistor on the workbench.
2. Connect the Arduino UNO to the computer using a USB cable.

## Step 2: Connect the External Interrupt Circuit

1. Connect the LED anode to Digital Pin 13 through a 220 Ω resistor.
2. Connect the LED cathode to GND.
3. Connect one terminal of the push button to Digital Pin 2 (INT0).
4. Connect the other terminal of the push button to GND.
5. Configure Pin 2 as INPUT_PULLUP in the program.

## Step 3: Configure the Timer Interrupt

1. Use Timer1 to generate a periodic interrupt.
2. Configure the timer in the Arduino program.
3. Define an Interrupt Service Routine (ISR) for Timer1.

## Step 4: Open the Arduino IDE

1. Open Arduino IDE.
2. Select **Tools → Board → Arduino UNO**.
3. Select the correct COM Port.

## Step 5: Write and Upload the Program

1. Write the program for external and timer interrupts.
2. Verify the program.
3. Upload it to the Arduino UNO.

## Step 6: Execute the Program

1. Press the push button and observe the external interrupt response.
2. Observe the LED blinking periodically due to the timer interrupt.
3. Open the Serial Monitor to observe interrupt messages (if included).

## Step 7: Verify the Output

1. Confirm that pressing the push button immediately triggers the external interrupt.
2. Confirm that the timer interrupt executes periodically without polling.
3. Record the observations.

# Program
```
volatile bool externalFlag = false;
volatile bool timerFlag = false;

void setup()
{
  pinMode(13, OUTPUT);
  pinMode(12, OUTPUT);
  pinMode(2, INPUT_PULLUP);

  // External interrupt on INT0 (D2)
  attachInterrupt(digitalPinToInterrupt(2), externalISR, FALLING);

  // Timer1 setup
  noInterrupts();

  TCCR1A = 0;
  TCCR1B = 0;

  // CTC mode
  TCCR1B |= (1 << WGM12);

  // Prescaler = 1024
  TCCR1B |= (1 << CS12) | (1 << CS10);

  // 16 MHz / 1024 = 15625 counts/sec
  // 15625 counts = 1 second
  OCR1A = 15624;

  // Enable Timer1 Compare Match A interrupt
  TIMSK1 |= (1 << OCIE1A);

  interrupts();
}

void loop()
{
  if (externalFlag)
  {
    externalFlag = false;

    // External interrupt action
    digitalWrite(13, !digitalRead(13));
  }

  if (timerFlag)
  {
    timerFlag = false;

    // Timer interrupt action
    digitalWrite(12, !digitalRead(12));
  }
}

// External Interrupt Service Routine
void externalISR()
{
  externalFlag = true;
}

// Timer1 Compare Match Interrupt Service Routine
ISR(TIMER1_COMPA_vect)
{
  timerFlag = true;
}
```
# Observation

<img width="1173" height="1600" alt="WhatsApp Image 2026-09-24 at 8 18 54 AM" src="https://github.com/user-attachments/assets/4e3da0fd-adb4-403a-87c3-59c867769bb3" />

# Result

The External Interrupt and Timer Interrupt were successfully implemented using the Arduino UNO. The external interrupt responded immediately to the push button event, while the timer interrupt executed periodically, demonstrating efficient interrupt-driven programming without continuous polling.
