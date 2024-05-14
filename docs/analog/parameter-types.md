# Parameter types

Command body structures for [SetParameter](./commands/SetParameter.md).

* [Reporting data interval](#reporting-data-interval)
* [Day checkout hour](#day-checkout-hour)
* [Reporting data type](#reporting-data-type)
* [Priority data delivery type](#priority-data-delivery-type)
* [Activation method](#activation-method)
* [Battery depassivation info](#battery-depassivation-info)
* [Battery minimal load time](#battery-minimal-load-time)
* [Channels config](#channels-config)
* [RX2 config](#rx2-config)
* [Absolute data](#absolute-data)
* [Enable absolute data](#enable-absolute-data)
* [Serial number](#serial-number)
* [Geolocation](#geolocation)
* [Extra frame interval](#extra-frame-interval)
* [Absolute data multi channel](#absolute-data-multi-channel)
* [Enable absolute data multi channel](#enable-absolute-data-multi-channel)
* [Pulse channels scan configuration](#pulse-channels-scan-configuration)
* [Pulse channels set config](#pulse-channels-set-config)
* [Battery depassivation config](#battery-depassivation-config)
* [MQTT session config](#mqtt-session-config)
* [MQTT broker address](#mqtt-broker-address)
* [MQTT ssl enable](#mqtt-ssl-enable)
* [MQTT topic prefix](#mqtt-topic-prefix)
* [MQTT data receive config](#mqtt-data-receive-config)
* [MQTT data send config](#mqtt-data-send-config)
* [NBIOT ssl config](#nbiot-ssl-config)
* [NBIOT ssl cacert write](#nbiot-ssl-cacert-write)
* [NBIOT ssl cacert set](#nbiot-ssl-cacert-set)
* [NBIOT ssl client cert write](#nbiot-ssl-client-cert-write)
* [NBIOT ssl client cert set](#nbiot-ssl-client-cert-set)
* [NBIOT ssl client key write](#nbiot-ssl-client-key-write)
* [NBIOT ssl client key set](#nbiot-ssl-client-key-set)
* [NBIOT device software update](#nbiot-device-software-update)
* [NBIOT module firmware update](#nbiot-module-firmware-update)
* [Reporting data config](#reporting-data-config)
* [Events config](#events-config)
* [NBIOT module info](#NBIOT-module-info)
* [NBIOT bands](#NBIOT-bands)

## Reporting data interval

Setting time parameters of operation. Applicable to all types of modules.

### Format

| Size | Type    | Field                              |
| ---- | ------- | ---------------------------------- |
| `1`  | `uint8` | parameter type = `1`               |
| `3`  | `uint8` | [reserved](#reserved) = `0x000000` |
| `1`  | `uint8` | [period](#period)                  |

#### **reserved**

Obsolete parameter. This field is no longer used.

#### **period**

This parameter defines the periodicity of sending data from the sensor.
<br>
It is a `1`-byte value with a resolution of `600` seconds plus a pseudo-random value of up to `511` seconds.
If the parameter is not set, the data transmission period will be `13320` seconds plus a pseudo-random value of up to `2551` seconds.

### Examples

#### set `reporting data interval` to `600` seconds:

| Field          | Value | Hex        |
| -------------- | ----- | ---------- |
| command id     | `3`   | `0x03`     |
| command size   | `5`   | `0x05`     |
| parameter type | `1`   | `0x01`     |
| reserved       |       | `0x000000` |
| period         | `1`   | `0x01`     |

Message hex dump with LRC: `03 05 01 00 00 00 01 53`


## Day checkout hour

The parameter defines the hour of the day by which the daily consumption is calculated.

### Format

| Size | Type    | Field                |
| ---- | ------- | -------------------- |
| `1`  | `uint8` | parameter type = `4` |
| `1`  | `uint8` | [hour](#hour)        |

#### **hour**

The value from `0` to `23`. The default calculation hour is `0`.

### Examples

#### set `day checkout hour` to `12:00`:

| Field          | Value | Hex    |
| -------------- | ----- | ------ |
| command id     | `3`   | `0x03` |
| command size   | `2`   | `0x02` |
| parameter type | `4`   | `0x04` |
| hour           | `12`  | `0x0c` |

Message hex dump with LRC: `03 02 04 0c 5c`


## Reporting data type

Parameter is used to setup type of values received from device.

### Format

| Size | Type    | Field                   |
| ---- | ------- | ----------------------- |
| `1`  | `uint8` | parameter type = `5`    |
| `1`  | `uint8` | [data type](#data-type) |

#### **data type**

| Value | Description                                |
| ----- | ------------------------------------------ |
| `0`   | hour                                       |
| `1`   | day                                        |
| `2`   | current                                    |
| `3`   | hour and day (since software version `98`) |

### Examples

#### set `reporting data type` to `current`:

| Field          | Value | Hex    |
| -------------- | ----- | ------ |
| command id     | `3`   | `0x03` |
| command size   | `2`   | `0x02` |
| parameter type | `5`   | `0x05` |
| data type      | `2`   | `0x02` |

Message hex dump with LRC: `03 02 05 02 53`


## Priority data delivery type

Data delivery method for priority data.
Can be with acknowledgement or not.
This parameter applies to all types of modules.

### Format

| Size | Type    | Field                           |
| ---- | ------- | ------------------------------- |
| `1`  | `uint8` | parameter type = `8`            |
| `1`  | `uint8` | [delivery type](#delivery-type) |

#### **delivery type**

| Value | Description                                    |
| ----- | ---------------------------------------------- |
| `0`   | data with delivery confirmation                |
| `1`   | data transmitted without delivery confirmation |

### Examples

#### set `priority data delivery type` to `data with delivery confirmation`:

| Field          | Value | Hex    |
| -------------- | ----- | ------ |
| command id     | `3`   | `0x03` |
| command size   | `2`   | `0x02` |
| parameter type | `8`   | `0x08` |
| delivery type  | `0`   | `0x00` |

Message hex dump with LRC: `03 02 08 00 5c`


## Activation method

Parameter is used to setup activation method in LoRaWAN network.
[Comparison and description of methods](https://www.thethingsindustries.com/docs/devices/abp-vs-otaa/).

### Format

| Size | Type    | Field                                             |
| ---- | ------- | ------------------------------------------------- |
| `1`  | `uint8` | parameter type = `9`                              |
| `1`  | `uint8` | [activation method type](#activation-method-type) |

#### **activation method type**

| Value | Description       |
| ----- | ----------------- |
| `0`   | OTAA (by default) |
| `1`   | ABP               |

### Examples

#### set `activation method` to `ABP`:

| Field                  | Value | Hex    |
| ---------------------- | ----- | ------ |
| command id             | `3`   | `0x03` |
| command size           | `2`   | `0x02` |
| parameter type         | `9`   | `0x09` |
| activation method type | `1`   | `0x01` |

Message hex dump with LRC: `03 02 09 01 5c`


## Battery depassivation info

Defines battery depassivation parameters for all module types except `MTXLora`.

### Format

| Size | Type    | Field                                       |
| ---- | ------- | ------------------------------------------- |
| `1`  | `uint8` | parameter type = `10`                       |
| `2`  | `uint8` | [load time](#load-time)                     |
| `2`  | `uint8` | [internal resistance](#internal-resistance) |
| `2`  | `uint8` | [low voltage](#low-voltage)                 |

#### **load time**

Represents the battery load time in milliseconds.
The first byte is the most significant byte of the number.
The default value is `2` seconds.

#### **internal resistance**

Represents the internal resistance threshold of the battery.
When this threshold is exceeded, depassivation is enabled.
The value is in `mOhms` and the first byte is the most significant byte of the number.
The default value is `18432` `mOhms`.

#### **low voltage**

Represents the battery voltage without load.
If the voltage drops below this threshold, depassivation is not enabled.
The value is in `mV` and the first byte is the most significant byte of the number.
The default value is `3200` `mV`.

### Examples

#### set `battery depassivation info`:

| Field               | Value  | Hex      |
| ------------------- | ------ | -------- |
| command id          | `3`    | `0x03`   |
| command size        | `7`    | `0x07`   |
| parameter type      | `10`   | `0x0a`   |
| load time           | `100`  | `0x0064` |
| internal resistance | `3222` | `0x0c96` |
| low voltage         | `233`  | `0x00e9` |

Message hex dump with LRC: `03 07 0a 00 64 0c 96 00 e9 4c`


## Battery minimal load time

This parameter is related to the minimum necessary active time of the battery per day to prevent battery deactivation.
It applies to all module types except `MTXLora`.

### Format

| Size | Type    | Field                   |
| ---- | ------- | ----------------------- |
| `1`  | `uint8` | parameter type = `11`   |
| `4`  | `uint8` | [load time](#load-time) |

#### **load time**

It is a `4`-byte value in `32768` `Hz` intervals.
The most significant byte comes first.
By default, the value is `327680` or `0x50000` (`10` seconds).

### Examples

#### set `battery minimal load time` to `100` seconds:

| Field          | Value                     | Hex          |
| -------------- | ------------------------- | ------------ |
| command id     | `3`                       | `0x03`       |
| command size   | `5`                       | `0x05`       |
| parameter type | `11`                      | `0x0b`       |
| load time      | `100` seconds = `3276800` | `0x00320000` |

Message hex dump with LRC: `03 05 0b 00 32 00 00 6a`


## Channels config

This parameter set configuration of the module channels, only for universal 4-channels devices

### Format

| Size | Type    | Field                                                         |
| ---- | ------- | ------------------------------------------------------------- |
| `1`  | `uint8` | parameter type = `13`                                         |
| `1`  | `uint8` | [channels configuration value](#channels-configuration-value) |

#### **channels configuration value**

Description: `+` enable channel, `-` disable channel

| configuration value | channel 0 | channel 1 | channel 2 | channel 3 | serial |
| ------------------- | --------- | --------- | --------- | --------- | ------ |
| `0`                 | `+`       | `+`       | `+`       | `+`       | `-`    |
| `1`                 | `+`       | `+`       | `+`       | `-`       | `-`    |
| `2`                 | `+`       | `+`       | `-`       | `+`       | `-`    |
| `3`                 | `+`       | `+`       | `-`       | `-`       | `-`    |
| `4`                 | `+`       | `-`       | `+`       | `+`       | `-`    |
| `5`                 | `+`       | `-`       | `+`       | `-`       | `-`    |
| `6`                 | `+`       | `-`       | `-`       | `+`       | `-`    |
| `7`                 | `+`       | `-`       | `-`       | `-`       | `-`    |
| `8`                 | `-`       | `+`       | `+`       | `+`       | `-`    |
| `9`                 | `-`       | `+`       | `+`       | `-`       | `-`    |
| `10`                | `-`       | `+`       | `-`       | `+`       | `-`    |
| `11`                | `-`       | `+`       | `-`       | `-`       | `-`    |
| `12`                | `-`       | `-`       | `+`       | `+`       | `-`    |
| `13`                | `-`       | `-`       | `+`       | `-`       | `-`    |
| `14`                | `-`       | `-`       | `-`       | `+`       | `-`    |
| `15`                | `-`       | `-`       | `-`       | `-`       | `+`    |
| `16`                | `+`       | `+`       | `-`       | `-`       | `+`    |
| `17`                | `+`       | `-`       | `-`       | `-`       | `+`    |
| `18`                | `-`       | `+`       | `-`       | `-`       | `+`    |

### Examples

#### enable 1-4 channels, and disable serial channel for device:

| Field                        | Value | Hex    |
| ---------------------------- | ----- | ------ |
| command id                   | `3`   | `0x03` |
| command size                 | `2`   | `0x02` |
| parameter type               | `13`  | `0x0d` |
| channels configuration value | `0`   | `0x00` |

Message hex dump with LRC: `03 02 0d 00 59`


## RX2 config

Parameter is used to setup `RX2` window configuration.

### Format

| Size | Type    | Field                           |
| ---- | ------- | ------------------------------- |
| `1`  | `uint8` | parameter type = `18`           |
| `1`  | `uint8` | [spread factor](#spread-factor) |
| `3`  | `uint8` | [frequency](#frequency)         |

#### **spread factor**

The transmission speed or `Data Rate` of a LoRaWAN message, ranging from `SF7` (highest Data Rate) to `SF12` (lowest Data Rate).
Making the spreading factor `1` step lower (from `SF10` to `SF9`) allows you to roughly send the same amount of data use half the time on air.
Lowering the spreading factor makes it more difficult for the gateway to receive a transmission, as it will be more sensitive to noise.
[More info.](https://www.thethingsnetwork.org/docs/lorawan/spreading-factors/)

| Value | Description |
| ----- | ----------- |
| `0`   | `SF12B125`  |
| `1`   | `SF11B125`  |
| `2`   | `SF10B125`  |
| `3`   | `SF9B125`   |
| `4`   | `SF8B125`   |
| `5`   | `SF7B125`   |
| `6`   | `SF7B250`   |

#### **frequency**

`RX2` data rate frequency.
The second receive window (RX2) uses a fixed frequency and data rate.
This value configures the frequency to use in `RX2`.
Changing the desired value makes the Network Server transmit the RXParamSetupReq MAC command.
<br>
It is a `3`-byte unsigned int BE, real frequency value divided by `100`.

### Examples

#### set spread factor to `SF7B125` and frequency `20000` for RX2 window:

| Field          | Value | Hex        |
| -------------- | ----- | ---------- |
| command id     | `3`   | `0x03`     |
| command size   | `5`   | `0x05`     |
| parameter type | `18`  | `0x12`     |
| spread factor  | `5`   | `0x01`     |
| frequency      | `200` | `0x0000c8` |

Message hex dump with LRC: `03 05 12 05 00 00 c8 8c`


## Absolute data

Parameter is used to setup absolute data for device.

### Format

| Size | Type    | Field                                          |
| ---- | ------- | ---------------------------------------------- |
| `1`  | `uint8` | parameter type = `23`                          |
| `1`  | `uint8` | [meter value](#meter-value)                    |
| `3`  | `uint8` | [pulse coefficient](#pulse-coefficient)        |
| `3`  | `uint8` | [pulse counter's value](#pulse-counters-value) |

#### **meter value**

Base absolute meter value.
<br>
It is a `4`-byte unsigned int BE.

#### **pulse coefficient**

[See details](./types.md#pulse-coefficient)

#### **pulse counter's value**

Base pulse counter's value.
<br>
It is a `4`-byte unsigned int BE.

### Examples

#### set absolute data:

| Field                 | Value  | Hex          |
| --------------------- | ------ | ------------ |
| command id            | `3`    | `0x03`       |
| command size          | `10`   | `0x0a`       |
| parameter type        | `23`   | `0x17`       |
| meter value           | `204`  | `0x000000cc` |
| pulse coefficient     | `100`  | `0x83`       |
| pulse counter's value | `2023` | `0x000007e7` |

Message hex dump with LRC: `03 0a 17 00 00 00 cc 83 00 00 07 e7 e4`


## Enable absolute data

Parameter is used to enable absolute data for device.

### Format

| Size | Type    | Field                                       |
| ---- | ------- | ------------------------------------------- |
| `1`  | `uint8` | parameter type = `24`                       |
| `1`  | `uint8` | [absolute data state](#absolute-data-state) |

#### **absolute data state**

| Value | Description                       |
| ----- | --------------------------------- |
| `0`   | device send pulse counter's value |
| `1`   | device send absolute value        |

### Examples

#### enable absolute data:

| Field               | Value | Hex    |
| ------------------- | ----- | ------ |
| command id          | `3`   | `0x03` |
| command size        | `2`   | `0x02` |
| parameter type      | `24`  | `0x18` |
| absolute data state | `1`   | `0x01` |

Message hex dump with LRC: `03 02 18 01 4d`


## Serial number

Parameter is used to set device serial number.

### Format

| Size | Type    | Field                           |
| ---- | ------- | ------------------------------- |
| `1`  | `uint8` | parameter type = `25`           |
| `6`  | `uint8` | [serial number](#serial-number) |

#### **serial number**

Device serial number.
<br>
It is a `6`-byte hex value.

### Examples

#### set serial number to `1b 0a 3e dc 3e 22`:

| Field          | Value               | Hex              |
| -------------- | ------------------- | ---------------- |
| command id     | `3`                 | `0x03`           |
| command size   | `2`                 | `0x07`           |
| parameter type | `25`                | `0x19`           |
| serial number  | `1b 0a 3e dc 3e 22` | `0x1b0a3edc3e22` |

Message hex dump with LRC: `03 07 19 1b 0a 3e dc 3e 22 a7`


## Geolocation

Parameter is used to set device geolocation.

### Format

| Size | Type    | Field                   |
| ---- | ------- | ----------------------- |
| `1`  | `uint8` | parameter type = `26`   |
| `4`  | `uint8` | [latitude](#latitude)   |
| `4`  | `uint8` | [longitude](#longitude) |
| `2`  | `uint8` | [altitude](#altitude)   |

#### **latitude**

It is a `4`-byte float.

#### **longitude**

It is a `4`-byte float.

#### **altitude**

It is a `2`-byte int.

### Examples

#### set latitude: `34.43`, longitude: `43.43`, altitude: `23`:

| Field          | Value   | Hex          |
| -------------- | ------- | ------------ |
| command id     | `3`     | `0x03`       |
| command size   | `11`    | `0x0b`       |
| parameter type | `26`    | `0x1a`       |
| latitude       | `34.43` | `0x4209b852` |
| longitude      | `43.43` | `0x422db852` |
| altitude       | `23`    | `0x1700`     |

Message hex dump with LRC: `03 0b 1a 52 b8 09 42 52 b8 2d 42 17 00 74`


## Extra frame interval

`ExtraFrames` handling parameter.

### Format

| Size | Type    | Field                 |
| ---- | ------- | --------------------- |
| `1`  | `uint8` | parameter type = `28` |
| `2`  | `uint8` | [interval](#interval) |

#### **interval**

The parameter is `2` bytes long and measured in seconds.
Its value must not be less than `90` seconds.
If the parameter is set to `0`, it means that the issuance of `ExtraFrames` will be prohibited.
GazMoldova modules provide for issuing `ExtraFrame` `UPLINK` frames in case of loss of communication with `NS`.
This will happen after transmitting `16` standard `UPLINK` frames without receiving a confirmation from `NS`.
In `OTAA` connection mode, after more than `5` unsuccessful attempts to connect via `JOINREQ`, the `ExtraFrame` `UPLINK` frame transmission mode will be initiated.

### Examples

#### set `extra frame interval` to `3600` seconds:

| Field          | Value  | Hex      |
| -------------- | ------ | -------- |
| command id     | `3`    | `0x03`   |
| command size   | `3`    | `0x03`   |
| parameter type | `28`   | `0x1c`   |
| interval       | `3600` | `0x100e` |

Message hex dump with LRC: `03 03 1c 10 0e 57`


## Absolute data multi channel

Parameter is used to setup absolute data for device.

### Format

| Size | Type    | Field                                          |
| ---- | ------- | ---------------------------------------------- |
| `1`  | `uint8` | parameter type = `29`                          |
| `1`  | `uint8` | [channel index](#channel-index)                |
| `1`  | `uint8` | [meter value](#meter-value)                    |
| `3`  | `uint8` | [pulse coefficient](#pulse-coefficient)        |
| `3`  | `uint8` | [pulse counter's value](#pulse-counters-value) |

#### **channel index**

Channel index number.
<br>
It is a `1`-byte unsigned int.

#### **meter value**

Base absolute meter value.
<br>
It is a `4`-byte unsigned int BE.

#### **pulse coefficient**

[See details](./types.md#pulse-coefficient).

#### **pulse counter's value**

Base pulse counter's value.
<br>
It is a `4`-byte unsigned int BE.

### Examples

#### set absolute data:

| Field                 | Value  | Hex          |
| --------------------- | ------ | ------------ |
| command id            | `3`    | `0x03`       |
| command size          | `11`   | `0x0b`       |
| parameter type        | `29`   | `0x1d`       |
| channel index number  | `1`    | `0x00`       |
| meter value           | `402`  | `0x00000192` |
| pulse coefficient     | `1000` | `0x84`       |
| pulse counter's value | `2032` | `0x000007f0` |

Message hex dump with LRC: `03 0b 1d 00 00 00 01 92 84 00 00 07 f0 a0`


## Enable absolute data multi channel

Parameter is used to enable absolute data for device.

### Format

| Size | Type    | Field                                       |
| ---- | ------- | ------------------------------------------- |
| `1`  | `uint8` | parameter type = `30`                       |
| `1`  | `uint8` | [channel index](#channel-index)             |
| `1`  | `uint8` | [absolute data state](#absolute-data-state) |

#### **channel index**

Channel index number.
<br>
It is a `1`-byte unsigned int.

#### **absolute data state**

| Value | Description                                    |
| ----- | ---------------------------------------------- |
| `0`   | device send pulse counter's value from channel |
| `1`   | device send absolute value from channel        |

### Examples

#### enable absolute data for 2 channel:

| Field               | Value | Hex    |
| ------------------- | ----- | ------ |
| command id          | `3`   | `0x03` |
| command size        | `3`   | `0x03` |
| parameter type      | `30`  | `0x1e` |
| channel index       | `1`   | `0x01` |
| absolute data state | `1`   | `0x01` |

Message hex dump with LRC: `03 03 1e 01 01 4b`


## Pulse channels scan configuration

Parameter is used to set scan configuration for pulse devices.
Available from software version = `0x6C` for:<br>
hardware type - `0x0f` hardware version - `0x02`<br>
hardware type - `0x06` hardware version - `0x0a`

[Hardware types](./basics.md#hardware-types)

### Format

| Size | Type    | Field                                 |
| ---- | ------- | ------------------------------------- |
| `1`  | `uint8` | parameter type = `31`                 |
| `1`  | `uint8` | [channels bit set](#channels-bit-set) |
| `1`  | `uint8` | [pull up time](#pull-up-time)         |
| `1`  | `uint8` | [scan time](#scan-time)               |

#### **channels bit set**

[Channels to set configuration](./types.md#channels-bit-set).

#### **pull up time**

Channel pull up time in microseconds.
<br>
Minimal value - `17` `μs`, maximum - `255` `μs`, `18` `μs` by default.
<br>
It is a `1`-byte unsigned int.

#### **scan time**

Channel scan time in microseconds `μs`.
<br>
Minimal value - `15` `μs`, maximum - `255` `μs`, `50` `μs` by default.
<br>
It is a `1`-byte unsigned int.

### Examples

#### set pulse channels configuration:

| Field          | Value    | Hex    |
| -------------- | -------- | ------ |
| command id     | `3`      | `0x03` |
| command size   | `4`      | `0x04` |
| parameter type | `31`     | `0x1f` |
| channels       | `1`, `4` | `0x09` |
| pull up time   | `18`     | `0x12` |
| scan time      | `23`     | `0x17` |

Message hex dump with LRC: `03 04 1f 09 12 17 41`


## Pulse channels set config

Parameter is used to set channels for pulse devices.
Available from software version = `0x6C` for:<br>
hardware type - `0x0f` hardware version - `0x02`<br>
hardware type - `0x06` hardware version - `0x0a`

[Hardware types](./basics.md#hardware-types)

### Format

| Size | Type    | Field                                   |
| ---- | ------- | --------------------------------------- |
| `1`  | `uint8` | parameter type = `32`                   |
| `1`  | `uint8` | [channels bit set](#channels-bit-set-1) |

#### **channels bit set**

[Channels to set](./types.md#channels-bit-set).

### Examples

#### enable channels: 1, 2, disable channels: 3, 4:

| Field          | Value    | Hex    |
| -------------- | -------- | ------ |
| command id     | `3`      | `0x03` |
| command size   | `2`      | `0x02` |
| parameter type | `32`     | `0x20` |
| channels       | `1`, `2` | `0x03` |

Message hex dump with LRC: `03 02 20 03 77`


## Battery depassivation config

Parameter is used to set thresholds for start/stop device battery depassivation.
Applies to all types of modules with battery power.
With the introduction of this parameter, the following listed parameters cease to function.

* [Battery depassivation info](#battery-depassivation-info)
* [Battery minimal load time](#battery-minimal-load-time)

The table displays the version from which this change took effect.

| Device    | Transceiver | HARD_TYPE | HARD_VERSION           | SOFT_VERSION |
| --------- | ----------- | --------- | ---------------------- | ------------ |
| `NOVATOR` | `SX1276`    | `4`       | `3`                    | `0x58`       |
| `NOVATOR` | `WLE`       | `4`       | `4`                    | `0x70`       |
| `IMP4EU`  | `WLE`       | `6`       | `16`, `17`, `18`, `19` | `0x74`       |
| `GAZZWLE` | `WLE`       | `12`      | `1`, `5`               | `0x70`       |
| `WATER`   | `WLE`       | `13`      | `2`                    | `0x09`       |

[Hardware types](./basics.md#hardware-types)

### Format

| Size | Type    | Field                                                     |
| ---- | ------- | --------------------------------------------------------- |
| `1`  | `uint8` | parameter type = `33`                                     |
| `2`  | `uint8` | [resistance start threshold](#resistance-start-threshold) |
| `2`  | `uint8` | [resistance stop threshold](#resistance-stop-threshold)   |

#### **resistance start threshold**

Represents the value of the internal resistance of the battery (in `mΩ`) upon exceeding which the depassivation process will be initiated. For WLE modules, this value is set by default to `35000` `mΩ`, and for modules using the `SX1276` transceiver, this value is set to `16000` `mΩ`.

#### **resistance stop threshold**

Represents the value of the internal resistance of the battery (in `mΩ`). If the internal resistance of the battery falls below this threshold, the depassivation process will stop. For WLE modules, this value is set by default to `25000` `mΩ`, and for modules using the `SX1276` transceiver, this value is set to `10350` `mΩ`.

### Examples

#### set depassivation start threshold to 36000 mΩ and stop threshold to 26000 mΩ:

| Field           | Value   | Hex      |
| --------------- | ------- | -------- |
| command id      | `3`     | `0x03`   |
| command size    | `5`     | `0x05`   |
| parameter type  | `33`    | `0x21`   |
| start threshold | `36000` | `0x8ca0` |
| stop threshold  | `26000` | `0x6590` |

Message hex dump with LRC: `03 05 21 8c a0 65 90`


## MQTT session config

Parameter is used to set MQTT session.
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size   | Type     | Field                                   |
| -----  | ------   | --------------------------------------- |
| `1`    | `uint8`  | parameter type = `34`                   |
| `1-64` | `string` | [client id](#client-id)                 |
| `1-64` | `string` | [username](#username)                   |
| `1-64` | `string` | [password](#password)                   |
| `1`    | `uint8`  | [clean session](#clean-session)         |

#### **client id**
unique identifier that distinguishes each MQTT client connecting to a broker and enables the broker to keep track of the client’s current state.
allows for an empty client_id. However, this connection must have the clean session flag set to true, or the broker will reject the connection.
Up to 64 byte. No default value

#### **username**
MQTT username for client authentication and authorization. Up to 64 byte. No default value

#### **password**
MQTT password for client authentication and authorization. Up to 64 byte. No default value

#### **clean session**
If the client has requested a clean session, the sessionPresent flag will always be false, indicating there is no previous session.
No default value

### Examples

#### set clear session

| Field          | Value      | Hex                    |
| -------------- | --------   | ------                 |
| command id     | `3`        | `0x03`                 |
| command size   | `20`       | `0x14`                 |
| parameter type | `34`       | `0x22`                 |
| client_id      | `0`        | `0x00`                 |
| username       | `login`    | `0x056c6f67696e`       |
| password       | `password` | `0x0870617373776f7264` |
| clean_session  | `0`        | `0x00`                 |

Message hex dump LRC: `03 14 22 05 6c 6f 67 69 6e 08 70 61 73 73 77 6f 72 64 00 11`


## MQTT broker address

Parameter is used to set MQTT broker address.
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size   | Type     | Field                                   |
| ------ | ------   | --------------------------------------- |
| `1`    | `uint8`  | parameter type = `35`                   |
| `1-64` | `string` | [host name](#host-name)                 |
| `2`    | `uint8`  | [port](#port)                           |

#### **host name**
The address of the server. It can be an IP address or a domain name. No default value
Maximum length: 64 bytes.

#### **port**
Integer type. The port of the server. Range: 1–65535.

### Examples

#### Connect to hivemq broker

| Field          | Value                   | Hex                                        |
| -------------- | --------                | ------                                     |
| command id     | `3`                     | `0x03`                                     |
| command size   | `23`                    | `0x17`                                     |
| parameter type | `35`                    | `0x23`                                     |
| host_name      | `s2.eu.hivemq.cloud`    | `0x1273322e65752e686976656d712e636c6f7564` |
| port           | `8883`                  | `0x22B3`                                   |

Message hex dump LRC: `03 17 23 12 73 32 2e 65 75 2e 68 69 76 65 6d 71 2e 63 6c 6f 75 64 70`


## MQTT ssl enable

Parameter is used to enable ssl
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size | Type         | Field                                   |
| ---- | ------       | --------------------------------------- |
| `1`  | `uint8`      | parameter type = `36`                   |
| `1`  | `enable`     | [enable](#ssl-enable)                   |

#### **ssl enable**
Indicates whether to use SSL/TLS secure connection for MQTT. Accepted immediately for next connection without device reset. Default value 1

### Examples

#### Use SSL/TLS TCP secure connection

| Field           | Value                   | Hex    |
| --------------  | --------                | ------ |
| command id      | `3`                     | `0x03` |
| command size    | `4`                     | `0x04` |
| parameter type  | `36`                    | `0x24` |
| ssl_enable      | `1`                     | `0x01` |

Message hex dump LRC: `03 04 24 01 77`


## MQTT topic prefix

Parameter is used to set topic prefix
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size   | Type            | Field                                   |
| ----   | ------          | --------------------------------------- |
| `1`    | `uint8`         | parameter type = `37`                   |
| `1-64` | `string`        | [topic prefix](#topic-prefix)           |

#### **topic prefix**
topic prefix that will be used to make topic to subscribe topic_prefix/elster-<short_mac>/down and publish topic_prefix/elster-<short_mac>/up,
where <short_mac> is lora devaddr. Accepted immediately for next pub/sub without device reset. No Default value. topics will be /elster-<short_mac>/up - /elster-<short_mac>/down.

### Examples

#### set topic prefix "/root/mac"

| Field           | Value                   | Hex                      |
| --------------  | --------                | ------                   |
| command id      | `3`                     | `0x03`                   |
| command size    | `12`                    | `0x0c`                   |
| parameter type  | `37`                    | `0x25`                   |
| topic_prefix    | `/root/mac`             | `0x0a2f726f6f742f6d6163` |

Message hex dump LRC: `03 0c 25 0a 2f 72 6f 6f 74 2f 6d 61 63 1c`


## MQTT data receive config

Parameter is used to set MQTT data receive config
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size | Type    | Field                                   |
| ---- | ------  | --------------------------------------- |
| `1`  | `uint8` | parameter type = `38`                   |
| `1`  | `uint8` | [qos](#qos)                             |

#### **QoS**
QoS option for subscribing. Default value QOS=1.

### Examples

#### Set receive config to subscribe with QOS=1  

| Field          | Value    | Hex    |
| -------------- | -------- | ------ |
| command id     | `3`      | `0x03` |
| command size   | `2`      | `0x02` |
| parameter type | `38`     | `0x26` |
| qos            | `1`      | `0x01` |

Message hex dump LRC: `03 02 26 01 73`


## MQTT data send config

Parameter is used to set MQTT send config.
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size | Type    | Field                                                           |
| ---- | ------- | --------------------------------------------------------------- |
| `1`  | `uint8` | parameter type = `39`                                           |
| `1`  | `uint8` | [qos](#qos)                                                     |
| `1`  | `uint8` | [retain](#retain)                                               |
| `1`  | `uint8` | [newest send first](#newest-send-first)                         |
| `1`  | `uint8` | [send count attempts](#send-count-attempts)                     |
| `1`  | `uint8` | [send timeout between attempts](#send-timeout-between-attempts) |

#### **qos**
QOS is used to publish. In the case of QoS 0. Data delivered in case if broker connection is established.
in case qos1/2 Data delivered in case if message published to broker successful
The default value is QOS1. Default value QOS=1

#### **retain**
use the retain flag when publishing. Default value retain=0

#### **newest send first**
if we have undelivered data first data will be sent from the newest to the oldest. Default value newest_send_first=1

#### **send count attempts**
count to try to resend if failure was. Default value send_count_attempts=1

#### **send timeout between attempts**
timeout minutes between read attempts. Default value send_timeout_between_attempts=5

### Examples

#### Set QoS to 1 and send undelivered data from old to new

| Field                         | Value    | Hex    |
| ----------------------------- | -------- | ------ |
| command id                    | `3`      | `0x03` |
| command size                  | `2`      | `0x02` |
| parameter type                | `39`     | `0x27` |
| qos                           | `1`      | `0x01` |
| retain                        | `0`      | `0x00` |
| newest_send_first             | `0`      | `0x00` |
| send_count_attempts           | `3`      | `0x03` |
| send_timeout_between_attempts | `30`     | `0x1e` |

Message hex dump LRC: `03 06 27 01 00 00 03 1e 6b`


## NBIOT SSL config

Parameter is used to config NBIOT SSL.
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size | Type    | Field                             |
| ---- | ------  | --------------------------------- |
| `1`  | `uint8` | parameter type = `40`             |
| `1`  | `uint8` | [security level](#security-level) |
| `1`  | `uint8` | [version](#version)               |

#### **security level**
The authentication mode.

| Value | Description                                                                |
| ----- | -------------------------------------------------------------------------- |
| `0`   | No authentication                                                          |
| `1`   | Perform server authentication                                              |
| `2`   | Perform server and client authentication if requested by the remote server |

#### **version**
SSL version.

| Value | Description                                                                                             |
| ----- | ------------------------------------------------------------------------------------------------------- |
| `1`   | TLS 1.0                                                                                                 |
| `2`   | TLS 1.1                                                                                                 |
| `3`   | TLS 1.2                                                                                                 |
| `4`   | All protocols are supported, the specific protocol version used needs to be negotiated with the server  |

### Examples

#### set SSL config to perform server conversation and all protocol

| Field          | Value    | Hex    |
| ---------------| -------- | ------ |
| command id     | `3`      | `0x03` |
| command size   | `3`      | `0x02` |
| parameter type | `40`     | `0x28` |
| security_level | `1`      | `0x01` |
| version        | `4`      | `0x04` |

Message hex dump LRC: `03 03 28 01 04 78`


## NBIOT ssl cacert write

Parameter is used to store chunk of SSL certificate on device
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size   | Type    | Field                       |
| ----   | ------  | --------------------------- |
|  1     | `uint8` | parameter type = `41`       |
|  2     | `uint8` | [size](#size)               |
|  2     | `uint8` | [position](#position)       |
|  0...n | `uint8` | [chunk](#chunk)             |

#### **size**
chunk size

#### **position**
chink position

#### **chunk**
chunk of bytes

### Examples

#### Write a chunk of ssl certificate

| Field          | Value    | Hex        |
| ---------------| -------- | ------     |
| command id     | `3`      | `0x03`     |
| command size   | `200`    | `0xc8`     |
| parameter type | `41`     | `0x29`     |
| size           | `196`    | `0xc4`     |
| position       | `500`    | `0x01F4`   |
| chunk          | `0...65` | `0x0...5f` |

Message hex dump no LRC: `03 c8 29 c4 01 F4 00...5f`


## NBIOT ssl cacert set

Parameter is used to set stored SSL certificate into nbiot
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size | Type    | Field                       |
| ---- | ------  | --------------------------- |
| `1`  | `uint8` | parameter type = `42`       |
| `4`  | `uint8` | [crc32](#crc32)             |

#### **crc32**
crc32 of stored cacert to verify and if crc valid write to nbiot

### Examples

#### Write SSL certificate to nbiot

| Field          | Value        | Hex          |
| ---------------| --------     | ------       |
| command id     | `3`          | `0x03`       |
| command size   | `5`          | `0x05`       |
| parameter type | `42`         | `0x2a`       |
| crc32          | `3171672888` | `0xBD0BE338` |

Message hex dump LRC: `03 05 2a bd 0b e3 38 14`


## NBIOT ssl client cert write

Parameter is used to store chunk of SSL certificate
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size    | Type    | Field                       |
| ----    | ------  | --------------------------- |
| `1`     | `uint8` | parameter type = `43`       |
| `2`     | `uint8` | [size](#size)               |
| `2`     | `uint8` | [position](#position)       |
| `0...n` | `uint8` | [chunk](#chunk)             |

#### **size**
chunk size

#### **position**
chink position

#### **chunk**
chunk of bytes

### Examples

#### Write a chunk of ssl certificate

| Field          | Value    | Hex        |
| ---------------| -------- | ------     |
| command id     | `3`      | `0x03`     |
| command size   | `200`    | `0xc8`     |
| parameter type | `43`     | `0x2b`     |
| size           | `196`    | `0xc4`     |
| position       | `500`    | `0x01F4`   |
| chunk          | `0...65` | `0x0...5f` |

Message hex dump no LRC: `03 c8 2b c4 01 F4 00...5f`


## NBIOT ssl client cert set

Parameter is used to set stored SSL certificate into nbiot
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size | Type   | Field                       |
| ---- | ------ | --------------------------- |
| `1`  | `uint8` | parameter type = `44`       |
| `4`  | `uint8` | [crc32](#crc32)             |

#### **crc32**
crc32 to check if client cert write correct and set it in nbiot module

### Examples

#### Write SSL certificate to nbiot

| Field          | Value        | Hex          |
| ---------------| --------     | ------       |
| command id     | `3`          | `0x03`       |
| command size   | `5`          | `0x05`       |
| parameter type | `44`         | `0x2c`       |
| crc32          | `3171672888` | `0xBD0BE338` |

Message hex dump LRC: `03 05 2c bd 0b e3 38 12`


## NBIOT ssl client key write

Parameter is used to set mqtt broker address.
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size   | Type    | Field                       |
| ----   | ------  | --------------------------- |
| `1`    | `uint8` | parameter type = `45`       |
| `2`    | `uint8` | [size](#size)               |
| `2`    | `uint8` | [position](#position)       |
| `0..n` | `uint8` | [chunk](#chunk)             |

#### **size**
chunk size

#### **position**
chink position

#### **chunk**
chunk of bytes

### Examples

#### Write a chunk of ssl certificate

| Field          | Value    | Hex        |
| ---------------| -------- | ------     |
| command id     | `3`      | `0x03`     |
| command size   | `200`    | `0xc8`     |
| parameter type | `45`     | `0x2d`     |
| size           | `196`    | `0xc4`     |
| position       | `500`    | `0x01F4`   |
| chunk          | `0...65` | `0x0...5f` |

Message hex dump no LRC: `03 c8 2d c4 01 F4 00...5f`


## NBIOT ssl client key set

Parameter is used to set mqtt broker address.
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size | Type    | Field                       |
| ---- | ------  | --------------------------- |
| `1`  | `uint8` | parameter type = `46`       |
| `4`  | `uint8` | [crc32](#crc32)             |

#### **crc32**
crc32 to check if client key write correct and set it in nbiot module

### Examples

#### Write SSL certificate to nbiot

| Field          | Value        | Hex          |
| ---------------| --------     | ------       |
| command id     | `3`          | `0x03`       |
| command size   | `5`          | `0x05`       |
| parameter type | `46`         | `0x2e`       |
| crc32          | `3171672888` | `0xBD0BE338` |

Message hex dump no LRC: `03 05 2e bd 0b e3 38 10`


## NBIOT device software update

Parameter is used to start updating from URL
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size   | Type            | Field                                     |
| ----   | ------          | ----------------------------------------- |
| `1`    | `uint8`         | parameter type = `47`                     |
| `1-64` | `string`        | [software image url](#software-image-url) |

#### **software image url**
software image URL where the image is stored. Will use nbiot to download the image and start the update.
After the update is finished successfully device will reboot and start updating in bootloader. The device will restart and send Activate.
If the update unsuccessful no indication will be.

### Examples

#### update device on imagefile url

| Field           | Value                   | Hex                                |
| --------------  | --------                | --------------------------------   |
| command id      | `3`                     | `0x03`                             |
| command size    | `16`                    | `0x10`                             |
| parameter type  | `47`                    | `0x2f`                             |
| topic_prefix    | `test/image.bin`        | `0x0e746573742f696d6167652e62696e` |

Message hex dump LRC: `03 10 2f 0e 74 65 73 74 2f 69 6d 61 67 65 2e 62 69 6e 72`


## NBIOT module firmware update

Parameter is used to set firmware image file update URL
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size   | Type            | Field                                                   |
| ----   | ------          | ---------------------------------------                 |
| `1`    | `uint8`         | parameter type = `48`                                   |
| `1-64` | `string`        | [module firmware image url](#module-firmware-image-url) |

#### **module firmware image url**
module firmware image URL where the image is stored(special DFOTA image). Will use nbiot DFOTA over HTTP/HTTPS to download image using and start update module

### Examples

#### set nbiot module firmware image url

| Field           | Value                   | Hex                                |
| --------------  | --------                | --------------------------------   |
| command id      | `3`                     | `0x03`                             |
| command size    | `16`                    | `0x10`                             |
| parameter type  | `48`                    | `0x30`                             |
| topic_prefix    | `test/image.bin`        | `0x0e746573742f696d6167652e62696e` |

Message hex dump no LRC: `03 10 30 0e 74 65 73 74 2f 69 6d 61 67 65 2e 62 69 6e 6d`


## Reporting data config

Parameter is used to set reporting data config
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size | Type    | Field                                   |
| ---- | ------- | --------------------------------------- |
| `1`  | `uint8` | parameter type = `49`                   |
| `1`  | `uint8` | [data type](#data-type)                 |
| `1`  | `uint8` | [hour](#minutes-hour)                   |
| `1`  | `uint8` | [minutes](#minutes-hour)                |
| `1`  | `uint8` | [count to send](#count-to-send)         |

#### **minutes** **hour**
start time from sending reporting data 

#### **count to send**
how many data samples to send

### Examples

#### set hour data receive config to start sending data from 03:00 with 4 records

| Field          | Value    | Hex    |
| -------------- | -------- | ------ |
| command id     | `3`      | `0x03` |
| command size   | `6`      | `0x07` |
| parameter type | `49`     | `0x31` |
| data_type      | `0`      | `0x00` |
| hour           | `3`      | `0x03` |
| minutes        | `0`      | `0x00` |
| count_to_send  | `4`      | `0x04` |

Message hex dump no LRC: `03 07 31 00 03 00 04`


## Events config

Parameter is used to set event config
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size | Type    | Field                                   |
| ---- | ------- | --------------------------------------- |
| `1`  | `uint8` | parameter type = `50`                   |
| `1`  | `uint8` | [event id](#event-id)                   |
| `1`  | `uint8` | [enable event](#enable-event)           |
| `1`  | `uint8` | [send event](#send-event)               |
| `1`  | `uint8` | [save event](#save-event)               |

#### **event id**

One of the [event types](./basics.md#device-events).

#### **enable event**
if need to check on the event

#### **send event**
is needed to send the event in a flash

#### **save event**
is needed to store events in flash

### Examples

#### config event=0 to enable it and send but not to send

| Field          | Value    | Hex    |
| -------------- | -------- | ------ |
| command id     | `3`      | `0x03` |
| command size   | `5`      | `0x05` |
| parameter type | `50`     | `0x32` |
| event_id       | `0`      | `0x00` |
| enable_event   | `1`      | `0x01` |
| send_event     | `1`      | `0x01` |
| save_event     | `0`      | `0x01` |

Message hex dump no LRC: `03 05 32 01 01 00`


## NBIOT module info

The parameter is used to get nbiot module info
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size | Type    | Field                                   |
| ---- | ------- | --------------------------------------- |
| `1`  | `uint8` | parameter type = `51`                   |

### Examples

#### get module info

| Field          | Value    | Hex    |
| -------------- | -------- | ------ |
| command id     | `3`      | `0x03` |
| command size   | `1`      | `0x01` |
| parameter type | `51`     | `0x33` |

Message hex dump no LRC: `03 05 33`

### Response:

#### Format

| Size   | Type     | Field                                   |
| ----   | ------   | --------------------------------------- |
| `1`    | `uint8`  | parameter type = `51`                   |
| `1-64` | `string` | [module info](#module-info)             |

##### **module info**

Product identification information including the identifier of the device type 
and the revision of software NBIOT module.


## NBIOT bands

Parameter to set preferred NB-IoT bands to be searched for.
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size   | Type    | Field                                   |
| ----   | ------- | --------------------------------------- |
| `1`    | `uint8` | parameter type = `52`                   |
| `1`    | `uint8` | [count](#count)                         |
| `0-17` | `uint8` | [bands](#bands)                         |

#### **count**
band count to set

#### **bands**
Currently, preferred NB-IoT bands to be searched for. if set to 0 all bands that support the module will be searched (default)

### Examples

#### set band to 20 

| Field          | Value    | Hex    |
| -------------- | -------- | ------ |
| command id     | `3`      | `0x03` |
| command size   | `5`      | `0x05` |
| parameter type | `52`     | `0x34` |
| count          | `1`      | `0x01` |
| bands          | `20`     | `0x14` |

Message hex dump no LRC: `03 05 34 01 14`


## NBIOT APN

Parameter to get/set APN
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size   | Type     | Field                                   |
| ----   | -------- | --------------------------------------- |
| `1`    | `uint8`  | parameter type = `52`                   |
| `1-64` | `string` | [apn](#apn)                             |

#### **apn**
Access Point Name - A logical name that is used to select the GGSN or the external packet data
network. If the value is null or omitted, then the subscription value will be requested. By default null.

### Examples

#### config vodafone sim apn for NB-IoT - "nbiot" 

| Field          | Value    | Hex            |
| -------------- | -------- | -------------- |
| command id     | `3`      | `0x03`         |
| command size   | `5`      | `0x05`         |
| parameter type | `53`     | `0x35`         |
| apn            | `nbiot`  | `0x6e62696f74` |

Message hex dump: `03 07 35 06 6e 62 69 6f 74 1c`


## NBIOT LED Indication

Parameter to set/get enable option for LED indication
Available from software version = `1` for:<br>
hardware type - `24`

[Hardware types](./basics.md#hardware-types)

### Format

| Size   | Type    | Field                                   |
| ----   | ------- | --------------------------------------- |
| `1`    | `uint8` | parameter type = `52`                   |
| `1`    | `uint8` | [enable](#enable)                         |

#### **enable**
option to enable or disable LED indication. Led indication significantly raises consumption. Use only for debugging purposes.
By default - false.

### Examples

#### enable led indication

| Field          | Value    | Hex    |
| -------------- | -------- | ------ |
| command id     | `3`      | `0x03` |
| command size   | `5`      | `0x05` |
| parameter type | `54`     | `0x36` |
| enable         | `1`      | `0x01` |

Message hex dump no LRC: `03 02 34 01 61`

