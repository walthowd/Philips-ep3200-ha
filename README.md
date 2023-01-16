# SmartPhilips3200 with Home Assistant

The Project can be used to control a Philips coffee machine via MQTT using an ESP8266 module.

Forked from:
https://github.com/chris7topher/SmartPhilips2200
https://github.com/micki88/Philips-ep3200-testing

A video for explanation can be found here:
https://youtu.be/jhzEMkL5Xek

Used these parts:
- [Molex 90325-0008 Connector](https://www.mouser.com/ProductDetail/Molex/90325-0008?qs=P41GyhEsKL7wtbj5ylImAA%3D%3D&countryCode=US&currencyCode=USD)
- [Molex 92315-0825 Cable](https://www.mouser.com/ProductDetail/Molex/92315-0825?qs=sfs0HZCnrVBO%252B%2Fha6s8VfA%3D%3D&countryCode=US&currencyCode=USD)
- [BC33725TFR NPN Transistor](https://www.mouser.com/ProductDetail/onsemi-Fairchild/BC33725TFR?qs=zGXwibyAaHYlHlvhRz3mQw%3D%3D&countryCode=US&currencyCode=USD)

Didn't need to convert voltage as the NodeMCU has an onboard voltage regulator. 



### Wiring Explaination

The wiring within the coffee machine is as shown in the picture:
![Wiring](https://github.com/walthowd/Philips-ep3200-ha/blob/main/images/wiring.png)

*Warning!*  You need a voltage regulator if your ESP8266 can't handle more then 3V. 

 ##### Molex Cable has black/red line on side for PIN1 (Shown going right to left above):

- PIN1 - 4-5V from Coffee Machine
    - Connect to ESP8266 and PIN1 on Molex 90325-0008 Connector that goes to display
- PIN2 - Ground 
    - Connect to Ground / GND on ESP8266 and connect to [collector leg of NPN transistor](https://www.mouser.com/datasheet/2/308/1/BC338_D-1802398.pdf)
- PIN3 - Not used
- PIN4 - Not used
- PIN5 - RX
    - This is actually TX from the coffee machine, but connects to RX pin on ESP8266 
        - This pin is `RXD0` / `GPIO3` on most boards
    - Also connect to PIN5 on Molex 90325-0008 Connector that goes to display
- PIN6 - TX
    - This is actually RX from coffee machine, but connects to TX pin on ESP8266
        - This pin is `TXD0` / `GPIO1` on most boards
        - This is the pin that we are stealing and routing through the ESP8266
- PIN7 - Not used
- PIN8 - Not used

##### From the ESP8266 connections not already referenced
- `D5` / `GPIO14` for TX that connects to PIN6 on Molex 90325-0008 Connector that goes to display
- `D7` / `GPIO13` for GND to turn off display
    - Connects to [middle base leg of NPN transistor](https://www.mouser.com/datasheet/2/308/1/BC338_D-1802398.pdf)

##### From NPN transistor, connections not alread referenced
- [Emitter leg of NPN transistor](https://www.mouser.com/datasheet/2/308/1/BC338_D-1802398.pdf) to PIN2 on Molex 90325-0008 Connector that goes to display

#### NodeMCU assembled with breadboard
![NodeMCU](https://github.com/walthowd/Philips-ep3200-ha/blob/main/images/nodemcu.jpg)