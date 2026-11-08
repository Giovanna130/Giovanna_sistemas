# Sesion 6 — UART
**Goal:** _Implement serial communication between two computers using two Raspberry Pis, using UART to transmit and receive messages in both directions, understanding the configuration of the TX and RX pins, the transmission speed, and the processing of the received data._

**Prediction:** _It is expected that both computers will be able to send and receive messages correctly via the Raspberry Pi using UART communication._

### Set Up  
The two Raspberry Pi Pico 2W boards had to be connected via UART communication, crossing the transmit and receive pins: the GP0 (TX) pin of the first Raspberry Pi had to be connected to the GP1 (RX) pin of the second, and vice versa. In addition, both boards had to share a common ground (GND) connection.

### What I did
The team connected the two Raspberry Pis by crossing the TX and RX pins and joining their GND connections. Then, we programmed each board with the corresponding code to send and receive messages, configured the UART communication with the same transmission speed, and connected each Raspberry Pi to a computer via USB. Finally, the serial monitor confirmed that messages were being sent and received correctly in both directions.

### Evidence 


### Code


### What went wrong
During the lab, we had difficulty establishing communication between the two Raspberry Pis, as neither computer was able to send or receive messages correctly. We were unable to determine whether the problem stemmed from an incorrect connection of the TX and RX pins, the UART communication configuration, or an error in the code.

### Disclosure
During the exercise, difficulties arose establishing communication between the two Raspberry Pis, as it was not possible to send or receive messages from either computer. It was not possible to determine whether the cause was related to the physical connections or the implemented code, so it was necessary to verify both aspects to identify and correct the errors.
