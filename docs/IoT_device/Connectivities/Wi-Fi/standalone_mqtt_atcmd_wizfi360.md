# WizFi360 MQTT AT Command를 이용하여 Auzre IoT Hub에 연동



## 목차
- [개요](#Overview)

- [학습 내용](#Learning_Content)

- [필요한 항목](#Required_Item)

- [동작 예제](#Example)

	1. [IoT Hub 생성](#Step-1-Create_IoT_Hub)

	2. [IoT Device 생성](#Step-2-Create_IoT_Device)

	3. [Blob Storage 생성](#Step-3-Create_Blob_Storage)

	4. [Stream Analytics 생성](#Step-4-Create_Stream_Analytics)

	5. [작업 입력 구성](#Step-5-Configure_Job_Input)

	6. [작업 출력 구성](#Step-6-Configure_Job_Output)

	7. [변환 Query 정의](#Step-7-Define_The_Transformation_Query)

	8. [WizFi360 설정 및 실행](#Step-8-Read_Data_From_IoT_Hub)

	9. [Stream Analytics 작업 시작 및 출력 확인](#Step-9-Start_The_Stream_Analytics_Job_And_Check_The_Output)

- [다음 단계](#Next)



<a name="Overview"></a>
## 개요
Microsoft Azure는 Microsoft의 Cloud Computing Service입니다.

Microsoft Azure Service에 WizFi360을 연동하여, Data를 Cloud로 전송하고, Monitoring을 할 수 있습니다.

Data 통신은 다음과 같은 구조로 이루어집니다.

![][Link-Data_Communication_Structure]

WizFi360 MQTT AT Command를 이용하여, IoT Hub Service 연결 및 Data 송신을 합니다.

IoT Hub로 송신이 된 Data는 Stream Analytics를 통하여 Data 저장소 Blob Storage로 저장이 됩니다.



<a name="Learning_Content"></a>
## 학습 내용
- IoT Hub 생성 방법 및 IoT Device 생성 방법

- Blob Storage 생성 방법

- Stream Analytics 생성 방법

- WizFi360 MQTT AT Command를 이용하여 설정 방법 및 IoT Hub로 Data 송신하는 방법

- IoT Hub에서 수신한 Data 읽는 방법



<a name="Required_Item"></a>
## 필요한 항목
![][Link-Required_Item]

- [WizFi360-EVB-Shield][Link-WizFi360-EVB-Shield]

- Micro 5 pin USB Cable



<a name="Example"></a>
## 동작 예제



<a name="Step-1-Create_IoT_Hub"></a>
### 1. IoT Hub 생성



<a name="Step-2-Create_IoT_Device"></a>
### 2. IoT Device 생성



<a name="Step-3-Create_Blob_Storage"></a>
### 3. Blob Storage 생성



<a name="Step-4-Create_Stream_Analytics"></a>
### 4. Stream Analytics 생성



<a name="Step-5-Configure_Job_Input"></a>
### 5. 작업 입력 구성



<a name="Step-6-Configure_Job_Output"></a>
### 6. 작업 출력 구성



<a name="Step-7-Define_The_Transformation_Query"></a>
### 7. 변환 Query 정의



<a name="Step-8-Read_Data_From_IoT_Hub"></a>
### 8. WizFi360 설정 및 실행



<a name="Step-9-Start_The_Stream_Analytics_Job_And_Check_The_Output"></a>
### 9. Stream Analytics 작업 시작 및 출력 확인



<a name="Next"></a>
## 다음 단계



[Link-WizFi360-EVB-Shield]: https://wizwiki.net/wiki/doku.php/products:wizfi360:start

[Link-Data_Communication_Structure]: ./images/standalone_mqtt_atcmd_wizfi360_data_communication_structure.png
[Link-Required_Item]: ./images/standalone_mqtt_atcmd_wizfi360_required_item.png
