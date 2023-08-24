# esp01-esphome-ld2410

本项目是一个使用ld2410雷达和esp01制作的人体存在传感器并接入ESPHOME 的简易说明，并提供ESPHOME8以上版本配置文件。之前还需要搞驱动，现在esphome直接支持了方便了很多。

## 硬件

1. HLK-LD2410B 淘宝采购约22元，带B的是蓝牙版本，最好是买蓝牙的方便调试。

2. ESP-01/ESP-01S 我这里使用的是01 最后买01S 省着加电阻了，我主要是有存货，价格几乎一样，淘宝采购约5元

3. AMS1117-3.3V 降压模块，淘宝采购约0.8元

4. 10K 电阻两只，买01S可以不需要

5. CH340 usb 转 TTL/或esp01专用的下载器（贵，省事） 主要是用来给esp首次输入esphome用的，之后基本上就在线升级。

## 接线图

专业的图咱也不会画，凑合看吧。

![Alt text](images/%E6%8E%A5%E7%BA%BF%E5%9B%BE.jpg)

## 组装硬件

我找了个万能版，把这些都焊在了一起，然后飞线。

![Alt text](images/Snipaste_2023-04-04_23-49-27.jpg)

![Alt text](images/Snipaste_2023-04-04_23-49-39.jpg)

成品，拿了一个棉签盒切了切，粘了粘，丑是丑点，又不是不能用。

![Alt text](images/Snipaste_2023-04-04_23-49-50.jpg)

## 刷入固件

首次刷入，需要使用CH340接入电脑，并连接上esp01。电脑装好CH340驱动

打开以下网址 https://web.esphome.io/?dashboard_wizard 选择相对应的端口输入即可

（这里写的草率了，偷懒~）

ESPHome 2023.8.0 - 16th August 2023LD2410
The LD2410 component has had a massive upgrade thanks to @regevbr! It now supports settings most if not all configuration parameters via switches / numbers and selects and exposes more data via various sensors. This includes breaking changes that mean the existing gate configuration options have been moved to the number platform.
代码网址：https://esphome.io/components/sensor/ld2410.html?highlight=ld2410
该传感器允许您使用LD2410传感器执行不同的测量。ld2410
## 配置文件ESPHOME 8以上版本

```yaml

esphome:
  name: esphome-web-210be0
  friendly_name: 人体传感器

esp8266:
  board: esp01_1m

# Enable logging
logger:

# Enable Home Assistant API
api:
  encryption:
    key: ""

ota:


wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Esphome-Web-210Be0"
    password: "AOrdGd1o2cSQ"

captive_portal:

# 上面的代码都是基础代码，注意修改，wifi ssid 和 password


uart:
  tx_pin: GPIO1   #使用D1 mini RX
  rx_pin: GPIO3   #使用D1 mini TX
  baud_rate: 256000
  parity: NONE
  stop_bits: 1
# Example configuration entry
ld2410:
number:
  - platform: ld2410
    timeout:
      name: timeout
    light_threshold:
      name: light threshold
    max_move_distance_gate:
      name: max move distance gate
    max_still_distance_gate:
      name: max still distance gate
    g0:
      move_threshold:
        name: g0 move threshold
      still_threshold:
        name: g0 still threshold
    g1:
      move_threshold:
        name: g1 move threshold
      still_threshold:
        name: g1 still threshold
    g2:
      move_threshold:
        name: g2 move threshold
      still_threshold:
        name: g2 still threshold
    g3:
      move_threshold:
        name: g3 move threshold
      still_threshold:
        name: g3 still threshold
    g4:
      move_threshold:
        name: g4 move threshold
      still_threshold:
        name: g4 still threshold
    g5:
      move_threshold:
        name: g5 move threshold
      still_threshold:
        name: g5 still threshold
    g6:
      move_threshold:
        name: g6 move threshold
      still_threshold:
        name: g6 still threshold
    g7:
      move_threshold:
        name: g7 move threshold
      still_threshold:
        name: g7 still threshold
    g8:
      move_threshold:
        name: g8 move threshold
      still_threshold:
        name: g8 still threshold


sensor:
  - platform: ld2410
    light:
      name: light
    moving_distance:
      name : Moving Distance
    still_distance:
      name: Still Distance
    moving_energy:
      name: Move Energy
    still_energy:
      name: Still Energy
    detection_distance:
      name: Detection Distance
    g0:
      move_energy:
        name: g0 move energy
      still_energy:
        name: g0 still energy
    g1:
      move_energy:
        name: g1 move energy
      still_energy:
        name: g1 still energy
    g2:
      move_energy:
        name: g2 move energy
      still_energy:
        name: g2 still energy
    g3:
      move_energy:
        name: g3 move energy
      still_energy:
        name: g3 still energy
    g4:
      move_energy:
        name: g4 move energy
      still_energy:
        name: g4 still energy
    g5:
      move_energy:
        name: g5 move energy
      still_energy:
        name: g5 still energy
    g6:
      move_energy:
        name: g6 move energy
      still_energy:
        name: g6 still energy
    g7:
      move_energy:
        name: g7 move energy
      still_energy:
        name: g7 still energy
    g8:
      move_energy:
        name: g8 move energy
      still_energy:
        name: g8 still energy

  - platform: dht
    pin: GPIO2   #使用D1 mini D4针脚
    temperature:
      name: "Living Room Temperature"
    humidity:
      name: "Living Room Humidity"
    update_interval: 60s

binary_sensor:
  - platform: ld2410
    has_target:
      name: Presence
    has_moving_target:
      name: Moving Target
    has_still_target:
      name: Still Target
    out_pin_presence_status:
      name: out pin presence status

## 效果预览

![Alt text](images/Snipaste_2023-04-04_23-45-44.jpg)

需要做自动化主要使用 `has_target` 即可。

