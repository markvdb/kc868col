# KC868-COL automation controller communication protocol

v1. 1

[1. Network communication physical layer](#1-network-communication-physical-layer)

[2. Wireless communication protocol](#)

  [2.1. Data packet structure](#)

  [2.2. Function code](#)

   [2.2.1 relay board passthrough command 0x C0 01](#)

   [2.2.2 custom content passthrough command 0x C0 0 2](#)

   [2.2.3 the RS485 passthrough command 0x C0 0 . 3](#)

   [2.2.11 ifttt functional configuration commands 0xC011](#)

   [2.2.12 PC download custom content command 0x C012 Midnight](#)

   [2.2.13 Delete ifttt parameters command 0xC013](#)

   [2.2.14 to delete the contents of the PC download custom command 0x C014](#)

   [2.2.21 is read ifttt Function Configuration Commands 0xC021](#)

   [2.2.22 is reading the contents of the PC download custom command 0x C022](#)

   [2.2.23 is the read command switch 0x C023](#)

   [2.2.24 reads the analog command 0x C024](#)

   [2.2.25 reads the temperature value of the command 0x C025](#)

   [2.2.26 ifttt running status report command 0xC026](#)

   [2.2.31 is the main control board setting command parameter 0x C031](#)

   [2.2.32 time synchronization command 0x C032](#)

   [2.2.33 is turned on / off automatic reporting command 0x C033](#)

   [2.2.34 is read parameter command MPU 0x C034](#)

   [2.2.41 is the automatic reporting command switch 0x C041](#)

   [2.2.42 is automatically reported to the analog command 0x number C042](#)

   [2.2.43 is automatically reported temperature value command 0x C043](#)

[3. Communication time limit](#)

[Revision history](#revision-history)

## 1. Network communication physical layer

The physical layer of the protocol is wired network communication. The KC868-COL series device acts as a server.
 
## 2. Wireless communication protocol


### 1. Data packet structure

The transmission data adopts network byte order. The basic data format is defined as follows:

![](kc868col_hexprotocol_bestanden/image_0.html)

 The data header part is defined as follows:

![](kc868col_hexprotocol_bestanden/image_1.html)

 Field description:

 Mark bit: 0x5E 

 Version number: 0x01  

 Message type: used to distinguish data or instruction type (see message type definition) 

 Data length: the length of the data content excluding the header part 

 Device ID: Serial number of the device ( string length does not exceed 28 bytes ) 

 Extended data: varies according to the message type 

The communication protocol adopts the little endian mode . When the data is larger than 1 byte, the high bit of the data is placed in the high address;

The data uses hexadecimal.

### 2. Function code

The function code is 1 byte and is used to select a command (read, write, response, etc.).

Function code (host request)

Function code (slave response)

meaning

0x C0 01

0x C0 01

Relay board transparent transmission command

0x C0 02

0x C0 02

RS232 transparent transmission command

0x C003

0x C003

RS485 transparent transmission command

0x C011

0x C011

Ifttt function configuration command

0x C012

0x C012

PC download custom content command

0x C021

0x C021

Read Ifttt function configuration command

0x C022

0x C022

Read command to download custom content from upper computer

0x C023

0x C023

Read switch command

0x C024

0x C024

Read analog command

0x C025

0x C025

Read temperature value command

0x C031

0x C031

Main control board parameter setting command

0x C032

0x C032

Time synchronization command

0x C033

0x C033

Turn on / off automatic reporting commands

The function code is explained in detail as follows.

#### 2 .1 relay board passthrough command 0x C0 01

##### 1 ) Description

This command is a transparent transmission command, and the slave computer forwards the command of the upper computer control relay board;

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x01 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

N

valid data

Control relay commands

0x00

##### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x01 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

N

valid data

Control relay response

0x00

#### 2 . 2 custom content passthrough command 0x C0 0 2

##### 1 ) Description

This command is a transparent transmission command, and the slave machine forwards the customized content of the host computer to RS232 or RS485 ;

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x0 2 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Output direction

1/0 ( 232/485 )

N-1

Custom content

0x00

##### 4 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x0 2 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

N

valid data

answer

0x00

#### 2 . . 3 the RS485 passthrough command 0x C0 0 . 3

##### 1 ) Description

This command is a transparent transmission command, and the slave computer forwards the host computer RS485 command;

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x0 3 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

N

valid data

Control RS485 commands

0x00

##### 5 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x0 3 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

N

valid data

Control RS485 response

0x00

#### 2 . 11 ifttt functional configuration commands 0xC011

##### 1 ) Description

1\. Each condition parameter is 6 bytes, each output action is 3 bytes, each function has up to 10 conditions, and up to 50 action outputs. Therefore, each can occupy a space of 210Byte ;

2\. The microcontroller RAM is 48K , 50 th ifttt function, 50 \* 210 = 10K , therefore, it supports up to 50 th ifttt function;

3\. Download one at a time;

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 11 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Ifttt function configuration number

0x00 ( 1~50 )

1

Number of conditions

M ( 1~10 )

1

Then number

K ( 1~50 )

1

I fttt runs

0 x 00

6

Condition 1 parameters

6

Condition 2 parameters

( M-2 ) \*6

...

Remaining condition type parameters

3

Then1 parameters

3

Then2 parameters

( K-2 ) \*3

...

The remaining Then parameters

##### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 1 1 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

Operation result

1/0

\=1 , success

\=0 , failed, data is full

The condition type is resolved as follows:

There are 4 types of conditions, the type codes are ( 0x01 , 0x02 , 0x03 , 0x04 ), each type is 6 bytes;

Timing work type

byte

meaning

Instance

Parsing

1

Type code

0x01

1

Week cycle

0x7F

Each represents one day

\=7F , means Monday to Sunday

Bit0 represents Monday

1

24 o'clock

0x07

1

minute

0x0a

Time 7:10

2

Reserved

Switch input type

byte

meaning

Instance

Parsing

1

Type code

0x02

1

Switch number

0x01 (The first switch value)

Number range 1~16

1

State hold time

0x02

Keep 200ms , maximum 25 seconds

1

Trigger action

0x0/0x01

\=0 , means rising edge

\=1 , means falling edge

2

Reserved

Analog input type

byte

meaning

Instance

Parsing

1

Type code

0x03

1

Analog number

0x01 (The first analog quantity)

Number range 1~16

1

State hold time

0x02

Keep 200ms , maximum 25 seconds

1

Triggering conditions

0x01/0x02

\=1 , means the analog quantity is greater than or equal to the threshold

\=2 , indicating that the analog quantity is less than the threshold

2

Trigger threshold

0x64 (low byte)

100=1.00V , reserve 2 valid digits

0x00 (high byte)

Temperature sensor type

byte

meaning

Instance

Parsing

1

Type code

0x04

1

Temperature sensor number

0x01 (the first sensor quantity)

Number range 1~5

1

State hold time

0x02

Keep 200ms , maximum 25 seconds

1

Triggering conditions

0x01/0x02

\=1 , means the analog quantity is greater than or equal to the threshold

\=2 , indicating that the analog quantity is less than the threshold

2

Trigger threshold

0x64 (low byte)

100=1.00V , reserve 2 valid digits

0x00 (high byte)

The output function type is resolved as follows:

There are 3 types of Then output, the type codes are ( 0x11 , 0x12 , 0x13 ), each type has 3 bytes;

Time delay type

byte

meaning

Instance

Parsing

1

Type code

0x11

2

time

0x05 (low byte)

Delay 5 seconds, maximum 65535 seconds

0x00 (high byte)

Relay output type

byte

meaning

Instance

Parsing

1

Type code

0x12

1

Relay number

0x01

The first relay

1

action

2/1/0

\=2 , means flip

\=1 , means open

\=0 , means closed

Custom content output type

byte

meaning

Instance

Parsing

1

Type code

0x13

1

Custom content number

0x01

Custom content of number 1

Range ( 1~32 )

1

Output direction

0x00

\=0 , RS232

\=1 , RS485

#### 2 . 12 PC download custom content command 0x C012 Midnight

##### 1 ) Description

1\. The host computer downloads the customized content to the main control board as an action task, waiting for the condition to trigger the output;

2\. The maximum length of each custom content is 120 bytes, the minimum is 1 byte, and the maximum is 32 ;

3\. Download one at a time;

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 12 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Content number

0x00

1

Content length

0x00

N-2

Custom content

0x00

##### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 12 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

Operation result

1/0

\=1 , success

\=0 , failed, data is full

#### 2 . 13 Delete ifttt parameters command 0xC013

##### 1 ) Description

1\. Delete ifttt parameter, you can delete one or all;

2\. The number range is 1~50 , and the response fails when it exceeds the range;

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 13 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Ifttt function number

0x0 1 (delete one of 1~50 )

0xFF (delete all)

##### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 13 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

Operation result

1/0

\=1 , success

\=0 , failed, the number is out of range

#### 2 . 14 to delete the contents of the PC download custom command 0x C014

##### 1 ) Description

1\. Delete the custom content data downloaded by the upper computer, you can delete one or all;

2\. The number range is 1~32 , and the response fails if it exceeds the range;

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 14 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Content number

0x0 1 (delete one of 1~32 )

0xFF (delete all)

#### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 14 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

Operation result

1/0

\=1 , success

\=0 , failed, the number is out of range

#### 2 . 21 is read ifttt Function Configuration Commands 0xC021

##### 1 ) Description

1\. The upper computer reads the ifttt function configuration parameters, and reads one function parameter at a time, starting from the number 1\. If there is no corresponding ifttt function configuration, the response operation result is 0 ;

2\. Ifttt function configuration is up to 50 ;

###### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 21 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

ifttt function configuration number

0x01 (The first number)

##### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 21 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Operation result

0x01/0x00 (success / failure)

1

Total number of Ifttt functions

0x00

1

Ifttt function configuration number

0x00 ( 1~50 )

1

Number of conditions

M ( 1~10 )

1

Then number

K ( 1~50 )

1

Then times

6

Condition 1 parameters

6

Condition 2 parameters

( M-2 ) \*6

...

Remaining condition type parameters

3

Then1 parameters

3

Then2 parameters

( K-2 ) \*3

...

The remaining Then parameters

#### 2 . 22 is reading the contents of the PC download custom command 0x C022

##### 1 ) Description

1\. The upper computer reads the upper computer to download the custom content parameters, and reads one parameter at a time, starting with number 1\. If there is no corresponding number, the response operation result is 0 ;

2\. The maximum length of each custom content is 120 bytes, the minimum is 1 byte, and the maximum is 32 ;

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 22 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

ifttt function configuration number

0x01 (The first number)

##### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 22 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Operation result

0x01/0x00 (success / failure)

1

Total content

0x00

1

Content number

0x00

1

Content length

0x00

N-4

Custom content

0x00

#### 2 . 23 is the read command switch 0x C023

##### 1 ) Description

1\. The host computer reads the switch value, and it can read 1 or 16 channels at a time;

2\. Number 1 , which means to read the switch value of channel 1 , and the slave returns 1 group of data ( 1 byte);

3\. No. 255 , which means to read all channel switch values, and the slave returns 16 groups of data ( 16 bytes);

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 23 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

Analog number

0x01 = (first 1 channels)

\=0x10 (the 16th channel)

\=0xFF (all 16 channels)

#### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 23 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Analog number

0x01 = (first 1 channels)

\=0x10 (the 16th channel)

\=0xFF (all 16 channels)

1

Switch

0x01/0x00 (on / off)

#### 2 . 24 reads the analog command 0x C024

##### 1 ) Description

1\. The upper computer reads the analog value, and it can read 1 or 16 channels at a time;

2\. No. 1 , means to read the analog quantity of channel 1 , and the slave returns 1 group of data ( 2 bytes);

3\. No. 255 , which means to read all channel analogs, and the slave returns 16 groups of data ( 32 bytes);

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 24 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

Analog number

0x01 = (first 1 channels)

\=0x10 (the 16th channel)

\=0xFF (all 16 channels)

#### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 24 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Analog number

0x01 = (first 1 channels)

\=0x10 (the 16th channel)

\=0xFF (all 16 channels)

2

Value

0x00 (low byte)

0x00 (high byte)

#### 2 . 25 reads the temperature value of the command 0x C025

##### 1 ) Description

1\. The upper computer reads the value of the temperature sensor, and it can read 1 or 5 channels at a time;

2\. Number 1 , which means to read the temperature value of channel 1 , and the slave returns 1 group of data ( 2 bytes);

3\. The number 255 means that the temperature values ​​of all channels are read, and the slave returns 5 sets of data ( 10 bytes);

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 25 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

Analog number

0x01 = (first 1 channels)

\=0x04 (the 4th channel)

\=0xFF (all 5 channels)

##### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 25 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Analog number

0x01 = (first 1 channels)

\=0x04 (the 4th channel)

\=0xFF (all 5 channels)

2

Value

0x00 (low byte)

0x00 (high byte)

#### 2.26 ifttt running status report command 0xC026

##### 1 ) Description

1\. Get ifttt T operating state, a bar ifttt time function satisfies all the conditions, state 1 , i.e. in the running Then , the state is 1 ; otherwise, the state is 0 ;

2 . Ifttt operating state changes, equipment actively reports;

3 . Each represents a Ifttt , ie . 7 bytes . 5 0 th bit representatives . 5 0 th Ifttt ;

##### 2 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 2 6 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

i fttt status

0 x 00 (No. 1~ 8 ifttt )

0 x 01 represents the number 1ifttt is in Then state

1

0 x 00 (No. 9 ~ 16 ifttt )

1

0 x 00 (No. 17 ~ 24 ifttt )

1

0 x 00 (No. 25 ~ 32 ifttt )

1

0 x 00 (No. 33 ~ 40 ifttt )

1

0 x 00 (No. 41 ~ 48 ifttt )

1

0 x 00 (No. 49 ~ 50 ifttt )

High 6bit is invalid

#### 2 . 31 is the main control board setting command parameter 0x C031

##### 1 ) Description

1\. Actively report the temperature when the temperature change exceeds the threshold;

2\. If the analog quantity change exceeds the threshold, it will actively report the analog quantity;

3\. The time unit is second, the maximum is 65535 seconds;

4.  The temperature change threshold is 5000=50.00 degrees Celsius, and the analog value change threshold is 200=2.50V ;

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 31 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

2

valid data

Temperature change threshold

0x00 (low byte)

0x00 (high byte)

2

Analog quantity change threshold

0x00 (low byte)

0x00 (high byte)

2

Time reporting

0x00 (low byte)

0x00 (high byte)

2

Reserved

0x00

##### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 31 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

#### 2 . 32 time synchronization command 0x C032

##### 1 ) Description

The upper computer is connected to the main control board for time synchronization;

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 32 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

year

0x00 ( 2 digits after the year )

1

month

0x00

1

day

0x00

1

Time

0x00

1

Minute

0x00

1

second

0x00

##### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 32 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

#### 2 . 33 is turned on / off automatic reporting command 0x C033

##### 1 ) Description

Whether the upper computer setting is automatically reported;

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 33 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Features

0x01/0x00

\=1 , turn on reporting

\=0 , close reporting

##### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 33 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

#### 2 . 34 is read parameter command MPU 0x C034

##### 1 ) Description

The upper computer reads the temperature change threshold of the control board, the analog change threshold, and reports the switch time and clock regularly;

Clock format 0x19 0x03 0x11 0x05 0x04 0x03 =2019/03/11 5:04:03

##### 2 ) Host request

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 34 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

##### 3 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 34 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

2

valid data

Temperature change threshold

0x00 (low byte)

0x00 (high byte)

2

Analog quantity change threshold

0x00 (low byte)

0x00 (high byte)

2

Time reporting

0x00 (low byte)

0x00 (high byte)

6

Date \+ time

0x00

1

Actively report the flag

0x01/0x00 ( open / close )

3

Version number V1.0.2

0x02

0x00

0x01

#### 2 . 41 is the automatic reporting command switch 0x C041

##### 1 ) Description

1\. The switch reports regularly, and the 16 channels report together;

##### 2 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 41 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Analog number

0xFF (all 16 channels)

16

Switch

0x01/0x00 (on / off)

#### 2 . 42 is automatically reported to the analog command 0x number C042

##### 1 ) Description

1\. When the analog quantity change exceeds the threshold, 16 channels will be reported together;

##### 2 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 42 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Analog number

0xFF (all 16 channels)

2

Value

0x00 (low byte)

0x00 (high byte)

#### 2 . 43 is automatically reported temperature value command 0x C043

##### 1 ) Description

1\. When the analog quantity change exceeds the threshold, 5 channels are reported together;

##### 2 ) Slave response

byte

meaning

Example

1

Mark bit

0x 5E

1

version number

0x01

2

Message type

0x 43 (low byte)

0xC0 (high byte)

2

Message length

N (low byte)

N>>8 (high byte)

38

spare

0x00

1

valid data

Analog number

0xFF (all 5 channels)

2

Value

0x00 (low byte)

0x00 (high byte)

## 4. Communication time limit
----------------------------

When performing certain operations, the target device needs to perform certain operations to complete, which will take more time. Under normal circumstances, it is required to respond within 1s .

## Revision history

Data

Version

Description

Author

20200308

1 .0

Created document.

KinCony IOT

amend record

1.  0x C034 command, the reply data adds 1 byte (automatically reported flag) \+ software version number;
2.  0x C0 01 command, reply to increase the reply content of the relay;
3.  0x C0 0 2 command, response to increase valid data;
4.  0x C0 0 3 command, add RS485 reply content in response;
5.  0xC021 command, add a data bit;
6.  0xC011 command and 0xC012 command response add a data bit;

![Google Translate](kc868col_hexprotocol_bestanden/translate_24dp.png)

![](kc868col_hexprotocol_bestanden/iws_n.png)![](kc868col_hexprotocol_bestanden/iws_n.png)![](kc868col_hexprotocol_bestanden/iws_w.png)![](kc868col_hexprotocol_bestanden/iws_e.png)![](kc868col_hexprotocol_bestanden/iws_s.png)![](kc868col_hexprotocol_bestanden/iws_s.png)![](kc868col_hexprotocol_bestanden/iws_c.png)

![](kc868col_hexprotocol_bestanden/iw_n.png)![](kc868col_hexprotocol_bestanden/iw_n.png)![](kc868col_hexprotocol_bestanden/iw_w.png)![](kc868col_hexprotocol_bestanden/iw_e.png)![](kc868col_hexprotocol_bestanden/iw_s0.png)![](kc868col_hexprotocol_bestanden/iw_s0.png)![](kc868col_hexprotocol_bestanden/iw_c.png)
