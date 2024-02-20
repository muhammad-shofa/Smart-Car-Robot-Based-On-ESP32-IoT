# Smart-Car-Robot-Based-On-ESP32-IoT
This project was started on January 22, 2024 by me, this is an IoT ESP32-based remote car project with an L298N motor driver that can be controlled via a website.

So on this Smart Car Robot there is an ESP32 microcontroller that has a web server and can be connected to Wifi, this web server is what will be used to control the movement of the Smartcar, this IoT Smart Car Robot has 4 DC motors to drive its wheels and can move Forward, Backward, Turn Right and Turn Left.

**Forward**: all 4 wheels forward
**Backward**: all 4 wheels backward
**Right**: 2 wheels on the right will go backwards and 2 wheels on the left will go forward.
**Left**: 2 wheels on the right will move forward and 2 wheels on the left will move backward.

---

## How to operate it?
1. Connect the power supply to the ESP32 *(here I combine the power supply for the ESP32 and the motor driver)*
2. Make sure the lights on the ESP32 and Motor driver are on, this indicates the power supply is successfully connected
3. Make sure the Smart Car Robot is connected to wifi
4. Find the IP address of the Smart Car Robot website, you can find it in the serial monitor when you connect your ESP32 microcontroller with the arduino IDE *(make sure you are at the correct boud rate when checking)*
5. after finding the IP address of the Smart Car Robot website, enter it in the browser and enter the IP address in the search engine, *(make sure your device must also be connected to the internet / wifi, not necessarily the same wifi)*
6. If the website is found then try to press the forward, backward, right and left buttons
7. if the Smart Car Robot moves according to the button you press, congratulations you can operate this Smart Car Robot

---

## Want to build your own Smart Car Robot?
If you want to make your own Smart Car Robot, make sure to prepare all the materials written below.

### Material List
- ESP32
- Motor Driver L298N
- Breadboard Circuit
- Data Cable *(to input the program from the IDE to the ESP32)*
- Jumper Cables male to famale
- Jumper Cable
- 4WD Smart Car Robot Chassis Kit
- Power Supply *(I recommend using a power bank with an output of 5-12V.)*

