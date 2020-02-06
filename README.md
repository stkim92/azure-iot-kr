# WIZnet Azure IoT Documents and Samples
이 Repository는 IoT 장치를 Cat.M1, WiFi, Ethernet을 이용하여 Azure IoT Hub에 연결하고 Azure의 다른 서비스와 연동하기 위해 작성되었습니다.

## Folder Structure

### /docs/IoT_device/Connectivity
Cat.M1, WiFi, Ethernet을 이용한 Azure IoT Hub를 활용시 참고할 수 있는 문서가 위치합니다.
* LTE/Cat.M1 : Cat.M1을 이용하여 Azure IoT Hub에 연결시 참고할 수 있는 문서
* Wi-Fi : WizFi360을 이용하여 Azure IoT Hub에 연결시 참고할 수 있는 문서
* Wi-Fi/Gateway : WizFi630을 이용하여 Azure IoT Hub에 연결시 참고할 수 있는 문서
* Ethernet : WIZ-ASG-200을 이용하여 Azure IoT Hub에 연결시 참고할 수 있는 문서

### /docs/Azure_Cloud
IoT Hub를 이용한 Azure Cloud를 활용시 참고할 수 있는 문서가 위치합니다.


### /samples
Cat.M1, WiFi, Ethernet, Gateway를 이용하여 Azure IoT Hub에 연갈하는 예제 코드가 위치합니다. 
* LTE : MCU, 라즈베리파이, PC를 이용한 예제 코드
* Wi-Fi: WizFi360등 을 이용한 예제 코드
* Ethernet: WIZ-ASG-200을 이용한 예제 코드
* Gateway : WizFi630을 이용한 예제 코드



## Key features and Roadmap
:heavy_check_mark: 활용 가능  :heavy_minus_sign: 작성중  :heavy_multiplication_x: 지원 예정

## 사전준비
|         Doc        |                            Description                           |
|:------------------:|:------------------|
| :heavy_check_mark: | [Azure Portal을 사용하여 IoT Hub 만들기](https://docs.microsoft.com/ko-kr/azure/iot-hub/iot-hub-create-through-portal)



## Cat.M1
|         Doc        |                            Description                           |
|:------------------:|:------------------|
| :heavy_check_mark: | [ST MCU와 Cat.M1 모듈을 이용하여 Azure IoT Hub에 연결하기](https://github.com/Wiznet/azure-iot-kr/blob/master/docs/IoT_device/Connectivities/LTE/Cat.M1/nucleo_stm32l496_azure_st_sdk_bg96.md)
| :heavy_minus_sign: | [라즈베리파이와 Cat.M1 모듈을 이용하여 Azure IoT Hub에 연결하기](https://github.com/Wiznet/azure-iot-kr/blob/master/docs/IoT_device/Connectivities/LTE/Cat.M1/raspberrypi_azure_c_sdk.md)
| :heavy_multiplication_x: | PC와 Cat.M1모듈을 이용하여 Azure IoT Hub에 연결하기

## WiFi
|         Doc        |                            Description                           |
|:------------------:|:------------------|
| :heavy_minus_sign: | [WizFi630S를 이용하여 Azure IoT Hub에 연결하기](https://github.com/Wiznet/azure-iot-kr/blob/master/docs/IoT_device/Connectivities/Wi-Fi/Gateway/wizfi630s_azure_c_sdk.md)
| :heavy_check_mark: | [PC와 WizFi360(MQTT AT Command)을 이용하여 Azure IoT Hub에 연결하기](https://github.com/Wiznet/azure-iot-kr/blob/master/docs/IoT_device/Connectivities/Wi-Fi/standalone_mqtt_atcmd_wizfi360.md)
| :heavy_check_mark: | [PC와 WizFi360(Azure AT Command)을 이용하여 Azure IoT Hub에 연결하기](https://github.com/Wiznet/azure-iot-kr/blob/master/docs/IoT_device/Connectivities/Wi-Fi/standalone_azure_atcmd_wizfi360.md)

## Ethernet
|         Doc        |                            Description                           |
|:------------------:|:------------------|
| :heavy_minus_sign: | [WIZ-ASG-200를 이용하여 Azure IoT Hub에 연결하기](https://github.com/Wiznet/azure-iot-kr/blob/master/docs/IoT_device/Connectivities/Ethernet/wiz-asg-200_azure_sphere_sdk.md)


## Support

[![WIZnet Developer Forum][forum]](https://forum.wiznet.io/c/korean-forum/oshw/)

**[WIZnet Developer Forum](https://forum.wiznet.io/c/korean-forum/oshw/)** 에서 전세계의 WIZnet 기술 전문가들에게 질문하고 의견을 전달할 수 있습니다.<br>지금 방문하세요!

## License
**WIZnet IoT Shield for Arm Mbed** 저장소의 모든 문서와 예제는 [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0)으로 배포됩니다.



[forum]: https://github.com/Wiznet/wiznet-iot-shield-mbed-kr/blob/master/docs/imgs/forum.jpg

