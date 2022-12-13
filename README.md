
![cover](https://user-images.githubusercontent.com/87741718/207465402-837aba90-a046-41e4-96e2-2781a0f30781.jpg)

# 1. Introduction
This tutorial explains how to use kaluma basics and how to connect to WiFi using WizFi360 and send AT commands.

## 1.1. What is Kaluma?
Kaluma is a tiny and efficient JavaScript runtime for RP2040 (Raspberry Pi Pico). Please refer the below link for more its details.
[https://kalumajs.org/](https://kalumajs.org/)
![image](https://user-images.githubusercontent.com/87741718/207465548-4a764ede-b296-4ea3-ac18-752b6717f890.png)

# 2. Getting Started (Turn on the built-in LED)

## 2.1. Upload the firmware
Download the .uf2 firmware file from the link below and upload it to the WizFi360-EVB-Pico.

[https://kalumajs.org/download/](https://kalumajs.org/download/)

## 2.2. Install Kaluma CLI
In order to program Pico with Kaluma runtime, [Kaluma CLI](https://github.com/kaluma-project/kaluma-cli) must first be installed. Node.js must be pre-installed.
```bash
# install kaluma
npm install -g @kaluma/cli
```

After the installation, run the blow commands to check the help and the name of the connected serial port.

```bash
# Check the CLI help
kaluma help

# Check the serial port
kaluma ports 
```

## 2.3. Connect with Terminal
Connect to Pico and send commands through a serial terminal program such as `Putty` or `TeraTerm`. <br />
Sending the `.hi` command prints kaluma's welcome message.
```bash
.hi
```
![image](https://user-images.githubusercontent.com/87741718/207466278-cb069683-6328-4396-8d46-3491a4292373.png)

## 2.4. Turn on the built-in LED
Enter the below commands sequentially in the terminal to turn on the built-in LED.
Sending `digitalWrite(25, HIGH)` command turns the light on, and sending the `digitalWrite(25, LOW)` command turns it off.
```bash
> pinMode(25, OUTPUT);
> digitalWrite(25, HIGH);
```
![image](https://user-images.githubusercontent.com/87741718/207466385-49653a38-5b9d-4464-acaf-ac6271de88ac.png)
![image](https://user-images.githubusercontent.com/87741718/207466397-41700e9d-d399-40b7-b263-364c97124f85.png)

# 3. Send a AT Command to WizFi360
In Kaluma's basic example, WiFi communication is implemented by connecting an ESP8266 to a basic Pico board.
However, in this tutorial, WizFi360 built into the board is used to implement Wi-Fi communication.

## 3.1. UART
Enter the following commands in the terminal. Set the port and other options for UART communication. 
If you enter the last command, you can check the set port and other option information.

```bash
> const {UART} = require('uart');
> let uart1 = new UART(1, {baudrate: 115200, tx: 4, rx: 5, bufferSize: 1024});
> uart1.on('data', d=>{print(String.fromCharCode.apply(null, d))});
```
Items in `_native` can be set as options when creating a UART object.
![image](https://user-images.githubusercontent.com/87741718/207466497-06e9eb74-5ba7-4587-8342-7695935a103d.png)

## 3.2. Send a AT Command

### 3.2.1. Test AT Startup
Check if WizFi360 is properly connected and responds to AT commands normally.
If you send the string `AT\r\n` to the Pico via UART1, you can receive an echo of the sent command followed by a result `OK`.
```bash
> uart1.write('AT\r\n');
```
![image](https://user-images.githubusercontent.com/87741718/207466559-56e10ab3-4ab7-49f3-b574-458820e2e142.png)

### 3.2.2. Reset WizFi360
if you see the ready, initialization is successful.

```bash
> uart1.write('AT+RST\r\n');
```
![image](https://user-images.githubusercontent.com/87741718/207466606-a6392c6c-d272-4cc4-8d08-7b2e217975ba.png)

### 3.2.3. Check Mac address
```bash
> uart1.write('AT+CIPSTAMAC_CUR?\r\n');
```
![image](https://user-images.githubusercontent.com/87741718/207466660-e34dcf3d-e3e3-4d11-a121-6c753f0693e1.png)

### 3.2.4. Set station mode
Set WizFi360 to Station mode.
```bash
> uart1.write('AT+CWMODE_CUR=1\r\n');
```
![image](https://user-images.githubusercontent.com/87741718/207466716-c7f4a63e-9ef8-4646-ad54-4e9e329eb218.png)

### 3.2.5. Enable DHCP
```bash
> uart1.write('AT+CWDHCP_CUR=1,1\r\n');
```
![image](https://user-images.githubusercontent.com/87741718/207466763-2de00362-7b9e-4c8f-938a-780e23b08c84.png)

### 3.2.6. Connect to the AP
Connect to an available AP. Enter the SSID and PASSWORD of the WiFi router to use and send the command.
```bash
> uart1.write('AT+CWJAP_CUR="WIFI_SSID","WIFI_PASSWORD"\r\n');
```
If the connection is successful, the output is as shown below.
![image](https://user-images.githubusercontent.com/87741718/207466840-a14ae123-3e58-43d4-bb4a-5a881ebfbe02.png)

So we learned how to send AT commands to WizFi360-EVB-Pico and connect to WiFi using Kaluma.
More various AT commands of WizFi360 can be found the [link](https://docs.wiznet.io/img/products/wizfi360/wizfi360ds/wizfi360atcomex_v103e.pdf).

---

Reference <br />
[https://kalumajs.org/](https://kalumajs.org/) <br />
[https://github.com/kaluma-project/kaluma](https://github.com/kaluma-project/kaluma) <br />
[https://javascript.plainenglish.io/physical-computing-with-javascript-1-8-lets-get-started-642a9954adb2](https://javascript.plainenglish.io/physical-computing-with-javascript-1-8-lets-get-started-642a9954adb2)<br />
[https://javascript.plainenglish.io/physical-computing-with-javascript-8-8-connecting-to-internet-151ba3dfce59](https://javascript.plainenglish.io/physical-computing-with-javascript-8-8-connecting-to-internet-151ba3dfce59) <br />
