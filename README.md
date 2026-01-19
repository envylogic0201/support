 # 2CH Frame Grabber Script User Manual
 **Supported Models:**
 - MQS2(R), MQE1, MQE2

```
© 2022 EnvyLogic Co.,Ltd. All rights reserved.
6F, 8, Heungan-daero 439beon-gil, Dongan-gu, Anyang-si, Gyeonggi-do, Republic of Korea
TEL: 82-31-421-6678
FAX: 82-31-421-6679
http://www.envylogic.com
Technical Support: support@envylogic.com
```

---

## Script Commands (Quick List)

## Contents

- **[1. Script Overview](#1-script-overview)**

- **[2. Script Command List](#2-script-command-list)**
  - [2.1 Opening Device](#21-opening-device)
    - [open](#open)
    - [open_wh](#open_wh)
  - [2.2 TEST PATTERN](#22-test-pattern)
    - [set_test_cam](#set_test_cam)
    - [set_mipi_test_cam](#set_mipi_test_cam)
  - [2.3 MIPI & Frame Setting](#23-mipi--frame-setting)
    - [set_mipi_default](#set_mipi_default)
    - [set_mipi_input](#set_mipi_input)
    - [set_mipi_info](#set_mipi_info)
    - [set_mipi_dsi](#set_mipi_dsi)
  - [2.4 ISM I/O Settings](#24-ism-io-settings)
    - [set_ism_default](#set_ism_default)
    - [set_ism_volt](#set_ism_volt)
    - [set_ism_freq](#set_ism_freq)
    - [set_ism_reset](#set_ism_reset)
    - [set_ism_io](#set_ism_io)
    - [get_ism_io](#get_ism_io)
  - [2.5 I2C](#25-i2c)
    - [set_i2c_freq](#set_i2c_freq)
    - [set_i2c](#set_i2c)
    - [get_i2c](#get_i2c)
    - [set_slv_addr](#set_slv_addr)
  - [2.6 SPI](#26-spi)
    - [set_spi_freq](#set_spi_freq)
    - [set_spi](#set_spi)
    - [set_spi_rvs](#set_spi_rvs)
    - [get_spi](#get_spi)
  - [2.7 GPIO](#27-gpio)
    - [set_gpio](#set_gpio)
    - [get_gpio](#get_gpio)
    - [set_gpio_16](#set_gpio_16)
    - [set_gpiooe_16](#set_gpiooe_16)
    - [get_gpio_16](#get_gpio_16)
  - [2.8 REGISTER](#28-register)
    - [set_register](#set_register)
    - [get_register](#get_register)
  - [2.9 PMU](#29-pmu)
    - [set_pmu_init](#set_pmu_init)
    - [set_pmu_end](#set_pmu_end)
    - [set_pmu_pwr](#set_pmu_pwr)
    - [set_pmu_sw](#set_pmu_sw)
    - [set_pmu_gnd](#set_pmu_gnd)
    - [set_pmu_mon](#set_pmu_mon)
    - [get_pmu_volt](#get_pmu_volt)
    - [get_pmu_cur](#get_pmu_cur)
    - [get_pmu_cut](#get_pmu_cut)
    - [set_pmu_reg](#set_pmu_reg)
    - [get_pmu_reg](#get_pmu_reg)
  - [2.10 ETC](#210-etc)
    - [delay](#delay)
    - [echo](#echo)
    - [run_script](#run_script)

---

### 1. Script Overview
We now support text file based scripts to enable easier usage of Frame Grabber API and avoid recompiling the Viewer when internal settings and sensor change.

This document guides you through the basic structure and usages of the scripts.

### Comments
Comments are non-executable portions of the texts in the script used to record information.
Any text that comes after ```//``` or ```#``` will be considered as comments.
Note: ```#``` should appear at the start of the line.

### Section
A section is an execution unit of the script
A section begins and ends with ```[``` and ```]``` respectively.
It is executed in cluster until next section is encountered.
Each section is assigned to a push-button in dgView program, and its name appears on the button.
Scripts are executed when their corresponding buttons are pushed.

### Separators
3 separators can be used to distinguish each command and parameter: SPACE, TAB, or ```,```. Any of the above 3 separators will make program to interpret two statements separately.

* Example
```python
open 3000, 4000, 12
```
or
```python
 open 3000 4000 12
```
Both of the above statements are equivalent to each other.

### Mapping
Each command in the script is matched to a single function.
For example, set_i2c is mapped to the DG_I2cWrite() function.

---

## 2. Script Command List

### 2.1 Opening Device
---
#### open
*```open [height] [widthbytes] [frame number]```*

 It opens channel used for video data transmission Also it allocates frame buffers and specifies

* **Example**
```python
open 3000, 8000, 12 // 4000x3000 16Bpp
```

* **Parameters**

|Type|Descriptions|
|---|---|
|*height*|number of lines|
|*widthbytes*|Size of a single line BYTES. Normally, it is:</br><span style="color:blue">*number of pixels * (Bits Per Pixel / 8)*</span>|
|*frame number*|number of Frame Buffer|

---

#### open_wh
*```open_wh [width] [height] [datatype] [fps]```*

It opens channel used for video data transmission with width & height.

* **Example**
```python
open_wh 4000, 3000, 0x1E, 24 // 4000x3000 YUV422 24 FPS
```

* **Parameters**

|Type|Descriptions|
|---|---|
|*width*|pixel width|
|*height*|number of lines|
|*datatype*|MIPI data type|
|*fps*|frame per second|

---

### 2.2 TEST PATTERN

#### set_test_cam

*```set_test_cam [enable]```*

Activates test pattern on the PCIe Board. Frames will be captured during the test Pattern is active.
Test pattern is sent out in YUV8 format, so the DataType of MIPI should be set to 0X1E.

* **Example**
```python
set_test_cam 1
```

* **Parameters**

|Type|Descriptions|
|---|---|
|*enable*|1 to activate,<br/>0 to deactivate|

---

#### set_mipi_test_cam

*```set_mipi_test_cam [enable]```*

Activates test pattern on the MIPI Board. Frames will be captured during the test pattern is active.
Test pattern is sent out in YUV8 format, so the DataType of MIPI should be set to 0X1E.

* **Example**
```python
set_mipi_test_cam 1
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*enable*|1 to activate,<br/>0 to deactivate|

---

### 2.3 MIPI & Frame Setting

#### set_mipi_default

*```sset_mipi_default [datatype]```*

Set MIPI parameters to default.

 Three functions
(set_mipi_input, set_mipi_info, set_mipi_dsi, set_mipi_tail_info) will be called with default parametrs.

* **Example**
```python
set_mipi_default 0x1e
```
* **Parameters**
*datatype*&emsp;&emsp;Datatype of MIPI Packet to be captured
<br/>
* **Supported data types**

| Code | Description |
| --- | --- |
| 0x1E | MIPI_CSI2_YUV422_8 |
| 0x24 | MIPI_CSI2_RGB888 |
| 0x2A | MIPI_CSI2_RAW8 |
| 0x2B | MIPI_CSI2_RAW10 |
| 0x2C | MIPI_CSI2_RAW12 |
| 0x2D | MIPI_CSI2_RAW14 |
| 0x2E | MIPI_CSI2_RAW16 |


---

#### set_mipi_input
*```set_mipi_input [lanes] [8b9b]```*

 MIPI Input settings, default is 4 lane.

* **Example**
```python
set_mipi_input 4, 0
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*lanes*|Number of MIPI Lanes|
|*8b9b*|8b9b decode enable|

---

#### set_mipi_info

*```set_mipi_info [vc] [dt1] [dt2] [dt3] [dt4]```*

 Sets virtual channel and datatype of the packets to be captured on MIPI input.

* **Example**
```python
set_mipi_info 0, 0x1e, 0, 0, 0
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*vc*|Virtual Channel of the packet(0~3)|
|*dt1*|Data Type1 of the packet|
|*dt2*|Virtual Channel2 & Data Type2 of the packet<br/>&ensp;- VC[7:6] : Virtual Channel 2bit<br/>&ensp;- DT[5:0] : Data Type 6bit|
|*dt3*|Virtual Channel3 & Data Type3 of the packet<br/>&ensp;- VC[7:6] : Virtual Channel 2bit<br/>&ensp;- DT[5:0] : Data Type 6bit|
|*dt4*|Virtual Channel4 & Data Type4 of the packet<br/>&ensp;- VC[7:6] : Virtual Channel 2bit<br/>&ensp;- DT[5:0] : Data Type 6bit|

<br/>

* **Supported data types**

| Code | Description |
| --- | --- |
| 0x1E | MIPI_CSI2_YUV422_8 |
| 0x24 | MIPI_CSI2_RGB888 |
| 0x2A | MIPI_CSI2_RAW8 |
| 0x2B | MIPI_CSI2_RAW10 |
| 0x2C | MIPI_CSI2_RAW12 |
| 0x2D | MIPI_CSI2_RAW14 |
| 0x2E | MIPI_CSI2_RAW16 |

---

 #### set_mipi_dsi

*```set_mipi_dsi [enable] [dsimode] [hsyncgen] [vsyncgen]```*

DSI MIPI related settings.

* **Example**
```python
set_mipi_dsi 0, 0, 0, 0
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*enable*|Enable MIPI DSI Capture|
|*dsimode*|DSI Mode. Video or Command.|
|*hsyncgen*|SI Hsync Gen|
|*vsyncgen*|DSI Vsync Gen|

---

### 2.4 ISM I/O Settings

#### set_ism_default

*```set_ism_default```*

Sets ISM I/O to its default state.
Executes two commands (set_ism_volt, set_ism_freq) with their default parameters.
Default voltage is 1.8V, and default frequency is 24MHz.

* **Example**
```python
set_ism_default
```
* **Parameters**
*None*

---

#### set_ism_volt
*```set_ism_volt [voltage]```*

Sets VCC I/O level of ISM in volts.

* **Example**
```python
set_ism_volt 1.8
```

* **Parameters**

|Type|Descriptions|
|---|---|
|*voltage*|Voltage level|

---

#### set_ism_freq
*```set_ism_freq [pin_mode] [freq]```*

Sets MCLK Clock Pin mode and frequency of ISM in MHz.

* **Example**
```python
 set_ism_freq 0, 24.0
```

* **Parameters**

|Type|Descriptions|
|---|---|
|*pin_mode*|The mclk pin mode<br/>&emsp;- 0 : Frequency Mode<br/>&emsp;- 1 : Set Pin status to float<br/>&emsp;- 2 : Set Pin status to HIGH<br/>&emsp;- 3 : Set Pin status to LOW|
|*freq*|ISM MCLK Clock frequency|

<!--|*freq*|![asdf](https://avatars.githubusercontent.com/u/255615560?v=4&size=64)ISM MCLK Clock frequency|-->

---

#### set_ism_reset

*```set_ism_reset [reset]```*

Changes the state of ISM's reset line.
`1` for High, `0` for Low.
To reset ISM, set the line to LOW and then to High.

* **Example**
```python
set_ism_reset 1
delay 10
set_ism_reset 0
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*reset*|Status of ISM Reset Pin. 1 for Low, 0 for High|

---

#### set_ism_io

*```set_ism_io [bit] [oe] [io]```*

ISM's 2BIT GPIO Output

* **Example**
```python
set_ism_io 0, 1, 1
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*bit*|2BIT GPIO level. (0 or 1)|
|*oe*|GPIO Direction. 1 for Outpu t, 0 for Input|
|*io*|1 for High, 0 for Low|

---

#### get_ism_io

*```get_ism_io [bit]```*

Reads ISM's bit GPIO and prints the result to log window.

* **Example**
```python
set_ism_io 0, 0, 0
get_ism_io 0
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*bit*|2BIT GPIO level. (0 or 1)|

---

### 2.5 I2C

#### set_i2c_freq

*```set_i2c_freq [freq]```*

Sets I2C Frequency in KHz.

* **Example**
```python
set_i2c_freq 200
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*freq*|I2C frequency in KHz.|

---

#### set_i2c

*```set_i2c [start] [stop] ...```*

Writes data to I2C.
Use `0x` prefix to send hexadecimal data.

* **Example**
```python
set_i2c 1, 1, 0x78, 0x22, 22, 10, 34
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*start*|set to 1 to send I2C START before data frame.|
|*stop*|set to 1 to send I2C STOP after data frame.|
|*...*|sequence of data|

---

#### get_i2c

*```get_i2c [stop] [count]```*

Reads data sequence from I2C and prints it out to log.

* **Example**
```python
get_i2c 1, 3
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*stop*|set to 1 to send I2C STOP after reading data.|
|*count*|number of byte data to read.|

---

#### set_slv_addr

*```set_slv_addr [addr]```*

Sets I2C slave address.
This address will be used when third-party scripts are executed.

* **Example**
```python
set_slv_addr 0x20
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*addr*|I2C Slave Address.|

---

### 2.6 SPI

#### set_spi_freq

*```set_spi_freq [freq]```*

Sets SPI frequency in MHz

* **Example**
```python
set_spi_freq 1 // 1Mhz
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*freq*|SPI frequency in MHz|

---

#### set_spi

*```set_spi [stop] ...```*

Sends a data sequence to SPI.
Use `0x` prefix to send hexadecimal data.
When the function is called, CS will be set to Low.
If [stop] is set to 1, CS will be set to High when data transmission is compete.

* **Example**
```python
set_spi 1, 0x22, 0x11, 0x33, 10, 34
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*stop*|Set to 1 to set CS to High after data transmission|
|*...*|Data Sequence|

---

#### set_spi_rvs

*```set_spi_rvs [stop] ...```*

Transmits each data in the sequence in bit-reversed format.
Some SONY sensors require this feature.
CS will be set to low when the function is called, and will be set to high after data transmission if [stop] is 1.

* **Example**
```python
set_spi_rvs 1, 0x22, 0x11, 0x33, 10
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*stop*|If this parameter is set to 1, CS will be set to High after data transmission|
|*...*|Data Sequence|

---

#### get_spi

*```get_spi [stop] [count]```*

Reads data sequence from SPI and prints it out to log.

* **Example**
```python
get_spi 1, 3
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*stop*|If this parameter is set to 1, CS will be set to High after|
|*count*|number of byte data to read|

---

### 2.7 GPIO

#### set_gpio

*```set_gpio [oe] [gpio]```*

Direction and output setting of 32bit GPIO

* **Example**
```python
set_gpio 0xFF 0x3F // 8 pins from LSB will be output pins, and seven pins // will be set to High
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*oe*|32 direction bits. 1 for output pins, 0 for input pins|
|*gpio*|pin output levels|

---

#### get_gpio

*```get_gpio```*

Reads 32bit GPIO levels and prints them out to log.

* **Example**
```python
get_gpio
```
* **Parameters**
*None*

---

#### set_gpio_16

*```set_gpio_16 [gpio]```*

Output level settings of a single channel (16 pins out of 32 pins).

* **Example**
```python
set_gpio_16 0x003F // Set lowest 7 pins to High
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*gpio*|GPIO pin levels (16 bit)|

---

#### set_gpiooe_16

*```set_gpiooe_16 [oe]```*

Direction settings of a single channel (16 pins out of 32 pins).

* **Example**
```python
set_gpiooe_16 0xFF3F // Pin 8 will be a pin input pin. Rest will be output pins
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*oe*|GPIO pin directions (16 bit)|

---

#### get_gpio_16

*```get_gpio_16```*

Reads a single 16bit GPIO channel and prints it out to log

* **Example**
```python
get_gpio_16
```
* **Parameters**
*None*

---

### 2.8 REGISTER

#### set_register

*```set_register [addr] [data]```*

Writes data directly to Frame Grabber I/O map.

* **Example**
```python
set_register 0x4000, 0x0001
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*addr*|Frame Grabber I/O map address|
|*data*|Data to write in I/O map|

---

#### get_register

*```get_register [addr]```*

Reads Frame Grabber I/O map and prints it out to log

* **Example**
```python
get_register 0x4000
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*addr*|Frame Grabber I/O map address|

---

### 2.9 PMU

#### set_pmu_init

*```set_pmu_init```*

Initializes and Activates PMU.
PMU-related commands can only be used after this command is called.
`set_pmu_end` should be called when PMU usage is over.

* **Example**
```python
set_pmu_init
[pmu commands ...]
set_pmu_end
```
* **Parameters**
*None*

---

#### set_pmu_end

*```set_pmu_end```*

Stops PMU and destroys corresponding objects

* **Example**
```python
set_pmu_init
[pmu commands ...]
set_pmu_end
```
* **Parameters**
*None*

---

#### set_pmu_pwr

*```set_pmu_pwr [ch] [volt] [cuta]```*

Set PMU channel voltages in volts CUT threshold current.

* **Example**
```python
set_pmu_pwr 0, 1.8, 0.5
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*ch*|PMU channel|
|*volt*|Voltage in volts|
|*cuta*|CUT threshold current in ampere.<br>When current flow exceeds this threshold, the circuitry will break.|

---

#### set_pmu_sw

*```set_pmu_sw [ch] [switch]```*

PMU channel switch select command.

* **Example**
```python
set_pmu_sw 0, 2
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*ch*|PMU channel|
|*switch*|0 for off, 1 for low current switch, 2 for high current switch.|

---

#### set_pmu_gnd

*```set_pmu_gnd [onoff]```*

GND setting command for PMU.

* **Example**
```python
set_pmu_gnd 1
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*onoff*|1 to connect GND, 0 to open|

---

#### set_pmu_mon

*```set_pmu_mon [onoff] [srcload]```*

 Turns PMU voltage & current monitor on/off.

* **Example**
```python
set_pmu_mon 1, 0xfff
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*onoff*|1 to turn monitor on, 0 to turn it off|
|*srcload*|Source Load Flag|

---

#### get_pmu_volt

*```get_pmu_volt [ch]```*

Reads PMU voltage on specified channel and prints it out to log.

* **Example**
```python
get_pmu_volt 0
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*ch*|PMU channel|

---

#### get_pmu_cur

*```get_pmu_cur [ch]```*

Reads PMU current on specified channel and prints it out to log.

* **Example**
```python
get_pmu_cur 0
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*ch*|PMU channel|

---

#### get_pmu_cut

*```get_pmu_cut```*

Prints out CUT status of all channels to log in hexadecimal.
For each bit, 1 means circuitry is CUT, and 0 means normal.

* **Example**
```python
get_pmu_cut
```
* **Parameters**
*None*

---

#### set_pmu_reg

*```set_pmu_reg [addr] [data]```*

Writes data to PMU's FPGA I/O map.

* **Example**
```python
set_pmu_reg 0x100, 0x20
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*addr*|PMU FPGA I/O Map address|
|*data*|Data to write|

---

#### get_pmu_reg

*```get_pmu_reg [addr]```*

Reads PMU's FPGA I/O map and prints it out to log.

* **Example**
```python
get_pmu_reg 0x100
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*addr*|PMU FPGA I/O Map address|

---

### 2.10 ETC

#### delay

*```delay [delay]```*

Waits for specified time

* **Example**
```python
delay 10
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*delay*|delay in ms|

---

#### echo

*```echo [enable]```*

Decide whether to output the log message.

* **Example**
```python
echo 1
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*enable*|1 : Enable output the log message.<br>2 : Disable output the log message.|

---

#### run_script

*```run_script [filename]```*

Executed a scriptfile in same directory

* **Example**
```python
run_script "IMX350_MHT-Y602A_SonyIMX350_D1_30fps.dat"
```
* **Parameters**

|Type|Descriptions|
|---|---|
|*filename*|script file name|

