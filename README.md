# Tartarus-test

1. Install latest version of Raspbian on Raspberry pi. The OS can be downloaded from the following [link](https://www.raspberrypi.org/downloads/raspbian/) .
2. Install swi-prolog on the system(as root user) using the command:<br>```% sudo apt-get install swi-prolog```
3. Download/Copy the WiringPi-Prolog tar file([provided here](https://drive.google.com/open?id=0B5fn-iVXebaTOFFPUUNtdUxTdXM)) onto the Raspberry pi.
4. Unzip the WiringPi-Prolog zip file using the command:<br>```$ unzip <file_name>```
5. Check the swi-prolog installation directory. Generally the location is ‘/usr/lib/swi-prolog’. To check otherwise use the following commands:
<br>```$ swipl```
<br>```?- file_search_path(swi, X).			//returns installation directory.```
6. Copy the wipi folder from the unzipped folder(step 4) to the folder returned in step 5.
7. Get out of superuser if still in it.
8. Go to your working directory(folder where you will be working with your project files).
9. Copy the ‘platform_pi.pl’([provided here](https://drive.google.com/open?id=0B5fn-iVXebaTR3hnYnpLc3RoRTQ)) file in this directory.
10. Now start prolog with sudo permission(very important):
<br>```$ sudo swipl```
11. Consult the `platform_pi.pl` using:
<br>```?- consult('platform_pi.pl').``` 
      <br>This will load all of the Tartarus predicates.
12. Execute the command:
<br>```?- start_peripherals.```
       <br>This will start the peripheral Interface. Make sure no errors occur during this step.
13. Now the peripheral interface is ready, and you can use WiringPi Prolog predicates to control the GPIO pins.
## GPIO pin mapping for WiringPi
![alt text](https://github.com/krishnarohila/Tartarus-test/blob/master/gpio_readall1.png)
## Heading
# General Purpose Input Output
1. Set the mode of a pin to input or output using the pinMode command:
      <br>Syntax: pinMode(pin,mode)
      * pin: GPIO pin number according to WiringPi.
      * mode: INPUT(0) or OUTPUT(1) mode.
2. Use digitalWrite command to write a value to the particular pin.
      <br>Syntax: digitalWrite(pin,value)
      * pin: GPIO pin number according to WiringPi.
      * value: set value HIGH(1) or LOW(0) for the pin.
3. Use digitalRead command to read the value that a particular pin is set to
      <br>Syntax: digitalRead(pin,X)
      * pin: GPIO pin number according to WiringPi.
      * X: variable in which the response of the command is stored.
<align = "left"><br><b>Example for LED:</b></align>
      1. Connect positive of led to GPIO pin 1(see WiringPi pin mapping) along with a resistance in series.
      2. Connect negative of led to GPIO pin 0(see WiringPi pin mapping).
      3. Write the following code to light up the LED.
            <br>```?- pinMode(0,0).```                --set pinmode of pin 0 to 0 for output.
            <br>```?- pinMode(1,0).```                --set pinmode of pin 1 to 0 for output.
            <br>```?- digitalWrite(0,0).```           --set value of pin 0 to 0 for LOW/ground.
            <br>```?- digitalWrite(1,1).```           --set value of pin 1 to 1 for HIGH/(3.3v).
      4. This will light up the LED.
      5. To turn it off use the digitalWrite command to write value 0 to pin 1
# I2C
1. Connect the I2C device to the Raspberry pi.
2. Check if the device is connected properly using the command:
<br>```$ sudo i2cdetect -y 1```
      <br> It should show something like:<br>
      ![alt text](https://github.com/krishnarohila/Tartarus-test/blob/master/i2cdetect1.png)
3. The address of the I2C device that we interact with is the one that appears on the table in the above image.
4. The commands that can be used to interact with the I2C device are:
      * <b>wiringPiI2CSetup(addr,Fd):</b> To setup a connection with the device.
            <br>- Addr: The address of the device obtained in step 2. Example: 0x68 (refer image in step 2).
            <br>- Fd: The value returned by the setup command for furthur referencing to the device.
      * <b>wiringPiI2CWriteReg8(Fd,reg,val):</b> To write a value in a particular register on the device.
            <br>- Fd: The value of Fd obtained from the setup command.
            <br>- reg: Address of the register to which the value is to be written.
            <br>- val: The value to be written to the particular register (reg).
            **Note: wiringPiI2CWriteReg16(Fd,reg,Val) is also available**
      * <b>wiringPiI2CReadReg8(Fd,reg,Val):</b> To read the output from a particular register on the device.
            <br>- Fd: The value of Fd obtained from the setup command.
            <br>- reg: Address of the register from which the value is to be read.
            <br>- Val: The value stored in the register returned by the command.
            **Note: wiringPiI2CReadReg16(Fd,reg,Val) is also available**
<br><b>Example for IMU:</b>
      1. Follow the first three steps of I2C to connect the IMU to Raspberry pi.
      2. Now once we get the address we type the following commands:
      <br>```?- wiringPiI2CSetup(0x68, Fd).```                  --returns the value of Fd(4 in this case) to be used in later commands.
      <br>```?- wiringPiI2CWriteReg8(4,0x6b,0x00).```           --sets a value to particular registers (refer to IMU datasheet) to awaken the device.
      <br>```?- wiringPiI2CWriteReg8(4,0x6c,0x00).```           --sets a value to particular registers to awaken the device.
      <br>```?- wiringPiI2CWriteReg8(4,0x74,0x00).```           --sets a value to particular registers to awaken the device.
      <br>```?- wiringPiI2CReadReg8(4,0x43,T).```               --returns the value of stored in 0x43 register.

# Time based functions
The commands are mentioned below:
1. <b>delay(t):</b> Used to apply a delay of t milliseconds.
      * t: Time in milliseconds.
2. <b>delayMicroseconds(t):</b> Used to apply a delay of t microseconds.
      * t: Time in microseconds.
3. <b>millis(T):</b> It returns a number(T) representing the number of milliseconds since the program called one of the wiringPiSetup functions. It returns an unsigned 32-bit number which wraps after 49 days.
      * T: Time in milliseconds returned by the function.
4. <b>micros(T):</b> It returns a number representing the number of microseconds since the program called one of the wiringPiSetup functions. It returns an unsigned 32-bit number which wraps after approximately 71 minutes.
      * T: Time in microseconds returned by the function.

<b>Example for LED blinking:</b>
* Commect a LED to Raspberry Pi at GPIO pins 1(HIGH) and 0(LOW).
* Write the following code in a prolog file(in the working directory).
<br>```loop(C):-
		(C<20 ->
				(0 is mod(C,2)->
				digitalWrite(1,0),
				delayMicroseconds(1000000),
				writeln('second'),
		millis(T),
		write(T),
				C1 is C+1,
				loop(C1)
				;
				digitalWrite(1,1),
				delayMicroseconds(1000000),
				writeln('first'),
				C1 is C+1,
				loop(C1)
				)).


start:-
		pinMode(1, 1),
		pinMode(0, 1),
		digitalWrite(0,0),
		digitalWrite(1,1),
		C is 1,
		loop(C),
		writeln('done').```
*  Now start prolog with sudo permission(very important):
<br>```$ sudo swipl```
* Consult the `platform_pi.pl` using:
<br>```?- consult('platform_pi.pl').``` 
      <br>This will load all of the Tartarus predicates.
* Execute the command:
<br>```?- start_peripherals.```
* Execute the following command to see the LED blink and the output of millis command on terminal.
<br>```?- start.```
