基于 ESP32 的多传感器环境监测与物联网系统本项目是一个集成了环境温湿度监测、姿态感知、本地双屏显示以及 MQTT 云端上报功能的嵌入式物联网系统。系统基于 ESP32 开发，并针对 Wokwi 仿真平台进行了完整适配。
1. 系统核心功能多维度感知：通过 DHT22 采集环境温湿度，通过 MPU6050 获取三轴加速度、角速度及片上温度。 
   本地自动化控制：系统内置逻辑，当环境湿度低于 80.0% 时自动开启水泵（继电器），高于该阈值则关闭。
   双屏本地交互：OLED (SSD1306)：实时显示当前温湿度及水泵的工作状态。  LCD 2004：详细展示 MPU6050 的姿态数据（加速度、旋转角速度）及传感器温度。
   云端可视化：通过 MQTT 协议将所有传感器数据以 JSON 格式发布至 ThingsBoard 物联网平台。
2. 硬件连接配置系统主要硬件组件及引脚连接如下（基于 diagram.json）：
   硬件模块引脚 / 协议连接至 ESP32DHT22SDAGPIO 15  MPU6050I2C (SCL/SDA)GPIO 22 / GPIO 21  SSD1306 OLEDI2C (SCL/SDA)GPIO 22 / GPIO 21  LCD 2004I2C (SCL/SDA)GPIO 22 / GPIO 21  Relay (Pump)INGPIO 25
3. 软件架构说明工程采用模块化设计，主要文件结构如下：
   main.ino：程序入口，负责系统初始化、WiFi/MQTT 连接及主逻辑循环。
   Motor.cpp/h：水泵联动逻辑实现，定义了湿度阈值 HUM_LOW = 80.0。
   MQTT.cpp/h：处理 WiFi 连接及 ThingsBoard MQTT 通信，数据封装为 JSON 格式上报。  OLED.cpp/h：驱动 OLED 与 LCD 2004 进行分工显示。
   mpu6050.cpp/h：姿态传感器驱动及数据计算处理。  4. 联网与云平台配置WiFi 设置：系统默认连接至 Wokwi 模拟 WiFi Wokwi-GUEST。  MQTT 平台：使用 ThingsBoard 云服务器 mqtt.thingsboard.cloud。  设备令牌 (Access Token)：默认使用 LbUewRvumZifp7fr8BlV 进行身份验证。
   上报数据示例：
   JSON{
  "ax": 0.00, "ay": 0.00, "az": 9.81, 
  "gx": 0.00, "gy": 0.00, "gz": 0.00,
  "tempmpu": 25.0, "tempDHT": 35.6, 
  "hum": 87.5, "pump": 0
      }
  7. 仿真运行指南在 Wokwi 平台 中导入 diagram.json 与代码文件。
  8. 确保 libraries.txt 中已包含 DHT sensor library for ESPx, PubSubClient, Adafruit MPU6050 等必要库。
  9.  点击 Start Simulation，系统将自动连接 WiFi 并开始本地控制与远程数据同步。  
