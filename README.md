# ECE198_Final_Demo

Needs Assessment

### Client / Customer Definition
The client and customer for our project are mainly for 2B and 2G, which represent both companies and the government. Our project is a perfect model for education. A good example could be a tutorial product in universities, where students can build an initial sense of the STM32. Another initial client could be a company that needs training equipment for their new employees or intern students.

### Competitive Landscape
1. **Cheap Product with High Reliability**: Our project is cheap relative to two points: cheap human power and cheap equipment, including LED light and circuits.
2. **Simple Components**: Easy for students or new employees to understand and try out. There are also many tutorials online that help with learning STM32.
3. **Strong Scalability**: Users can easily build upon our project to improve their skills. By using online educational videos, students can enhance their abilities.
4. **Visible Light Application**: Easier for adjusting and debugging.

## Requirement Specification

### Functional Requirements
- Implement the system’s brightness control, configure PWM on the STM32F401RE to adjust LED brightness in 25% increments from 0% to 100%, verifying each step using an oscilloscope or duty-cycle multimeter.
- Set up a button-triggered interrupt or polling method to toggle the LED state within 0.5 seconds of a press, confirmed by stopwatch.
- For consistent LED states across two STM32F401REs, establish I2C or UART communication with a maximum 10 ms delay, verified using an oscilloscope.
- To enable smooth dimming, program STM32 timers to incrementally adjust PWM from 0% to 100% over 5 seconds, then back down, ensuring timing accuracy with a stopwatch and observing gradual brightness transitions.

### Technical Requirements
- The system must operate within a 3.0V to 3.6V range, consume no more than 30W of power, and store less than 500mJ of energy at any time.
- Incorporate a temperature sensor that triggers an alert if temperatures exceed 60°C to ensure component and user safety.

## Analysis

### Design

#### System Architecture
The system comprises two STM32 microcontrollers (STM32F401RE) connected via UART (Universal Asynchronous Receiver-Transmitter). We call them Microcontroller A and Microcontroller B. Microcontroller A is responsible for initiating the signal, while Microcontroller B drives an LED and reacts to the signal from Microcontroller A. Microcontroller B also changes the color of the LED and uses a PWM signal to control its brightness.

#### Communication Protocol
UART was chosen for its simplicity and reliability in establishing serial communication between the microcontrollers. The parameters for the UART are:
- Baud rate: 9600
- Data bits: 8
- No parity check
- 1 stop bit

This setup ensures efficient and error-free transmission.

#### Hardware Description
- **Microcontroller A**: STM32F401RE, one push button connected to the GPIO pin (any point that can output 0V or 3.3V). A 10kΩ resistor may be used (optional).
- **Microcontroller B**: STM32F401RE, one multi-color LED connected to a GPIO pin (any pin that can output 0V or 3.3V).
- **Connection**: A three-wire setup (TX, RX, GND) connects the UART ports of both microcontrollers.

#### Software Overview
The firmware for both microcontrollers is developed using STM32CubeIDE:
- Microcontroller A’s firmware detects a button press and sends a byte (0x01) via UART when the button is pressed.
- Microcontroller B’s firmware listens for incoming UART data and toggles the connected LED upon receiving the byte. It also changes the LED color and uses PWM to adjust brightness.

#### Operational Workflow
Upon pressing the button on Microcontroller A:
- The GPIO pin reads the input state.
- If a high state is detected (button pressed), the signal (0x01) is sent to Microcontroller B.
- Microcontroller B receives the signal and toggles the LED from off to on or on to off, depending on its previous state.

When the button on Microcontroller B is pressed:
- The color of the multi-color LED changes between three colors.

#### Addressing Design Requirements
- **Reactivity**: The LED responds in real-time to the button press.
- **Low Power Consumption**: Both microcontrollers and the communication protocol are selected for low power usage.
- **Cost-Effectiveness**: STM32 microcontrollers provide a cost-effective solution for businesses, schools, or training centers.

#### Consideration of Alternatives
Alternatives such as using wireless communication (e.g., Bluetooth or Wi-Fi) were considered. However, UART was selected for its simplicity and lower cost. The power consumption of UART is also significantly lower compared to Bluetooth and Wi-Fi.

### Picture Description
This is the minimum requirement for our project:
- From left to right, the first board is labeled Microcontroller A, followed by Microcontroller B and the breadboard with a LED.

### Scientific or Mathematical Principles

#### Ohm’s Law & Pulse Width Modulation (PWM)
- **Understanding the Principle**: Ohm's Law states that the current through a conductor between two points is directly proportional to the voltage across the two points and inversely proportional to the resistance between them. This is mathematically expressed as I = V/R.
  
  PWM is a technique for controlling analog circuits with digital outputs. By varying the proportion of time a signal is high (duty cycle), PWM alters the average voltage of the signal.
  
- **Application in Design**:
  - **Ohm’s Law**: Used to calculate the resistor value to ensure the LED operates within safe conditions.
  - **PWM**: Controls the brightness of the LED by changing the frequency of PWM signals sent to the LED through a GPIO pin. The change in voltage affects current, which in turn affects brightness.

#### Color Mixing
- **Understanding the Principle**: The primary colors of light are red, green, and blue. Mixing these colors in varying intensities can create any color visible to the human eye. When combined in equal measure, they produce white light.

- **Application in Design**: The multi-color LED requires control over the percentage of red, green, and blue light. Each color is represented by an 8-bit binary value, such as (255, 0, 0) for red and (255, 255, 255) for white.

#### Digital Signal Transmission and Communication
- **Understanding the Principle**: Digital signal transmission involves sending information as discrete signals over a communication channel. It includes methods like UART, SPI, and I2C.
  
- **Application in Design**: UART is used for communication between microcontrollers. The baud rate is set to 9600, with 8 data bits, no parity check, and one stop bit to minimize data transmission errors.

## Cost

### Manufacturing and Implementation Costs

| Item                        | Quantity | Unit Cost | Manufacturer       | Vendor Location  |
| --------------------------- | -------- | --------- | ------------------ | ---------------- |
| STM32F401RE Microcontroller | 2        | $46.99    | STMicroelectronics | Waterloo, Canada |
| Multi-color LED             | 1        | $14.14    | -                  | Amazon Canada    |
| Push Button                 | 1        | $15.99    | -                  | Amazon Canada    |
| Resistor (10kΩ)             | 1        | $9.23     | E-Projects         | Amazon Canada    |
| Wires                       | 3        | $9.99     | EDGELEC            | Amazon Canada    |
| Breadboard                  | 1        | $14.99    | ELEGOO             | Amazon Canada    |

### Setup

1. Connect the push button to the GPIO pin of Microcontroller A.
2. Connect the multi-color LED to the GPIO pin of Microcontroller B.
3. Connect both microcontrollers and the breadboard with wires via UART ports.
4. Place the resistor behind the LED light bulb.

**Usage**:
- When the user presses the button on Microcontroller A, a signal is sent to Microcontroller B, toggling the LED on/off.
- When the user presses the button on Microcontroller B, the color of the multi-color LED changes between three colors.

## Risks

### Energy Analysis
According to STM32F401RE specifications, energy consumption is below the maximum required (under 30W and under 500mJ). The system's total energy consumption will not exceed these limits.

### Risk Analysis

#### Negative Consequences on Safety or Environment from Intended Use:
- **Overheating**: Prolonged operation can cause overheating, potentially damaging components or causing minor burns if touched.
- **Power Consumption**: Continuous use may result in unnecessary power consumption, leading to slight environmental impact.

#### Negative Consequences from Incorrect Use:
- **Overvoltage Connection**: Connecting a higher voltage supply can damage the microcontroller and LED, potentially causing electrical fires.
- **Improper Handling**: ESD (electrostatic discharge) may damage components, rendering the system inoperable.

#### Negative Consequences from Misuse:
- **Environmental Hazard**: Exposure to moisture may cause short-circuiting, increasing the risk of electrical sparks or fire.
- **Inappropriate Application**: Using the system near flammable materials could increase the risk of overheating or fire.

#### Potential Malfunctions:
- **Component Failure**: A component failure could cause the system to stop functioning.
- **Microcontroller Glitch**: A software error could lead to malfunctioning LEDs.
- **Power Supply Fluctuations**: Variations in the power supply could