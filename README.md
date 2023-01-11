# 物聯網與大數據 期末報告
Internet of Things and Big Data Analysis

---
# Getting Started

學生一開始發想希望能運用 raspberry pi4 在物聯網上，並結合上 Home Assistant 這個 open source framework，收集各種資料送上雲端，並能使用上 rasberry pi4 的諸多電子套件，譬如：Intel NCS2 神經棒，在領論與實作過程中有甘苦，當然也就有很多收穫及挫折。依序將諸多嘗試過程及處理方案說明如下：

# Components
## Hardward
```
Raspberry Pi 4 *1
Jumper wires
breadboard
LCD Display *1
DHT22 *1
```
## Software
```
Python 3.8
OpenVino
Open-model-Zoo
PySocks-1.7.1 
azure-iot-device-2.12.0 
deprecation-2.1.0 
janus-1.0.0 
paho-mqtt-1.6.1
teams_logger
board
adafruit_dht
subprocess
adafruit_ssd1306
PIL
azure-iot-device
```
## Try 1： [Raspberry Pi4 安裝pyspark 心得](https://github.com/RalphLiang/ncu-imgt-iot-hw2023/blob/master/spark_install.md)

## Try 2： 安裝Intel NCS2 神經棒 心得
## [32位元 Raspbian 安裝說明](https://github.com/RalphLiang/ncu-imgt-iot-hw2023/blob/master/intel_NCS2_install.md) / [64位元 Ubuntu20 安裝說明](https://github.com/RalphLiang/ncu-imgt-iot-hw2023/blob/master/install_NCS2_detail.md) 

## Try 3： [安裝 Home Assistant 心得](https://github.com/RalphLiang/ncu-imgt-iot-hw2023/blob/master/Install_Home_Assistant.md)

## Try 4: [Azure IoT Hub]
## [參考文件：](https://gloveboxes.github.io/Raspberry-Pi-Python-Environment-Monitor-with-the-Pimoroni-Enviro-Air-Quality-PMS5003-Sensor/zdocs/module_1_create_iot_hub/)

Step1: [開通 Azure for Students](https://azure.microsoft.com/en-us/free/students/)，此部分是免費的 

Step2: 開通 ResourceGroup、Azure IoT Hub

    login cloud shell
![image](https://user-images.githubusercontent.com/47648250/211613950-a5db8dce-0b9f-4cc1-ab37-dc30edcb6ecb.png)    
```
    PS /home/ycliang> az extension add --upgrade --name azure-iot
    PS /home/ycliang> az group create --name ncu-iot-rg --location eastus
    PS /home/ycliang> az iot hub create --resource-group ncu-iot-rg --name ncu-imgt-rpi4
    PS /home/ycliang> az iot hub connection-string show --hub-name ncu-imgt-rpi4
    PS /home/ycliang> az iot hub device-identity create --device-id ncu-rpi4 --hub-name ncu-imgt-rpi4
```

Step3: 安裝 Raspberry pi 4 上的 python 套件
```
pi@rpi64-ssd:~$ python3 --version
Python 3.8.10

PySocks-1.7.1 
azure-iot-device-2.12.0 
deprecation-2.1.0 
janus-1.0.0 
paho-mqtt-1.6.1
```
![image](https://user-images.githubusercontent.com/47648250/211614468-fcc70f21-d2f5-4581-9b47-003e553b7065.png)
![image](https://user-images.githubusercontent.com/47648250/211614897-a7a555eb-8e98-4b13-beee-9670f6213560.png)

Step4: 設定環境變數
![image](https://user-images.githubusercontent.com/47648250/211616518-519f0f60-388c-451d-8849-79f72c0d8b58.png)

Step5: 推送 monitor event 到 Azure
![image](https://user-images.githubusercontent.com/47648250/211617378-4490322b-2b06-4ab9-92eb-3112897bf199.png)
![image](https://user-images.githubusercontent.com/47648250/211617691-c5e22cfd-a7e5-4946-96d0-7933832d5fcd.png)

Step6: Azure IoT Hub 開始遙測收集
![image](https://user-images.githubusercontent.com/47648250/211617221-545a3ab7-3983-45cd-bf09-a8dcdaa78d51.png)

Step7: 通知使用者

![image](https://user-images.githubusercontent.com/47648250/211783613-cb10ed38-7bd1-4dca-8bdd-04646478fe8d.png)

---

# 程式碼部分
## [結合溫溼度](https://github.com/RalphLiang/Adafruit_Python_SSD1306/blob/master/examples/stats2.py)
## [LCD顯示](https://github.com/RalphLiang/Adafruit_Python_SSD1306/blob/master/examples/dht22.py) 
## [MSTeams通知](https://github.com/RalphLiang/Adafruit_Python_SSD1306/blob/master/examples/Team-Notification-IoT.py)
## [整合Azure IoT Hub](https://github.com/RalphLiang/azure-iot-sdk-python/blob/master/samples/pnp/ncu_controller_with_thermostats.py)

# 設備接線
![image](https://user-images.githubusercontent.com/47648250/211786362-8c61aa5d-2893-4257-b0d5-786a948a8ede.png)
![image](https://user-images.githubusercontent.com/47648250/211786231-08cfbea8-0371-4248-9d2b-4426ff31110b.png)

---

# [操作影片](https://drive.google.com/file/d/1cZ8wN2L1ZRrBtWlk0n_6CHMhKaV-o6YX/view?usp=sharing)

