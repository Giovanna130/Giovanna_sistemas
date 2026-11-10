# Sesion 7 — I2C (Inter-Integrated Circuit) 
**Goal:** _Understand the operation of the I²C communication protocol, identifying the role of the SDA and SCL signals, the use of addresses to recognize connected devices, and the process of transmitting and receiving data between a microcontroller and its peripherals._

**Prediction:** _It is expected that, by correctly configuring the I²C protocol and running the codes in Wokwi, it will be possible to simulate the sending and reading of data between the RaspberryPico 2 Advanced and the connected devices, understanding how the device addresses and the SDA and SCL signals are used to establish communication._

### Set Up 
During this exercise, I performed a simulation in Wokwi using the Raspberry Pi pico 2 Advanced, the DS1307 RTC module, and the MPU6050 sensor. I configured the I²C connections using the SDA and SCL pins and ran the code to send and receive data.

### What I did
During this exercise, I worked on simulating a circuit in Wokwi using a Raspberry Pi Pico 2 Advanced, a DS1307 RTC module, and an MPU6050 sensor. Then, I ran the write and read codes to test the I²C communication, configuring the SDA and SCL pins and observing the messages and data obtained during execution.

### Evidence 
<iframe width="560" height="315" src="https://www.youtube.com/embed/tCy0nkmPuvY?si=mTV5HTFQeCjPz1t7" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### What went wrong
* Initially, the connections weren't configured correctly, so we had to review and correct them, as the RTC wasn't displaying the correct time and the MPU6050 wasn't properly reflecting the changes we were making to the data. Therefore, we had to review both the connections and the code configuration until we identified the errors.

## Disclosure
During the Wokwi lab, difficulties arose with the connections and code configuration, as the RTC was not displaying the correct time and the MPU6050 was not reflecting the expected changes. This experience allowed me to recognize the importance of verifying connections, checking device addresses, and ensuring that I²C communication is configured correctly. As areas for improvement, I believe it is important to identify errors, interpret the data obtained, and better understand the operation of the read and write functions, so I can troubleshoot problems more independently in future labs.