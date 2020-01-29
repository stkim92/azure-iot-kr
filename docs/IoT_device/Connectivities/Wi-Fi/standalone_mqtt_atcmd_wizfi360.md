# WizFi360 MQTT AT Command를 이용하여 Auzre IoT Hub에 연동



## 목차
- [개요](#Overview)

- [학습 내용](#Learning_Content)

- [필요한 항목](#Required_Item)

- [동작 예제](#Example)

	1. [시작하기 전에 . . .](#Before_Start)

	2. [IoT Hub 생성](#Step-1-Create_IoT_Hub)

	3. [IoT Device 생성](#Step-2-Create_IoT_Device)

	4. [Blob Storage 생성](#Step-3-Create_Blob_Storage)

	5. [Stream Analytics 생성](#Step-4-Create_Stream_Analytics)

	6. [작업 입력 구성](#Step-5-Configure_Job_Input)

	7. [작업 출력 구성](#Step-6-Configure_Job_Output)

	8. [변환 Query 정의](#Step-7-Define_The_Transformation_Query)

	9. [WizFi360 설정 및 실행](#Step-8-Read_Data_From_IoT_Hub)

	10. [Stream Analytics 작업 시작 및 출력 확인](#Step-9-Start_The_Stream_Analytics_Job_And_Check_The_Output)

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



<a name="Before_Start"></a>
### 1. 시작하기 전에 . . .

[**Azure Portal**][Link-Azure-Portal]에 Login을 합니다.

계정이 없는 경우, 계정 생성 후에 Login을 진행합니다.

※ 본 문서에는 [**체험 계정**][Link-Azure-Account-Free]으로 진행합니다.



<a name="Step-1-Create_IoT_Hub"></a>
### 2. IoT Hub 생성

1. Azure Portal에서 **리소스 만들기** > **사물 인터넷** > **IoT Hub**를 선택합니다.

2. **기본 사항**을 입력합니다. **리소스 그룹**을 **새로 만들기**를 선택하여 생성합니다.

3. **크키 및 배율**을 설정합니다. **가격 및 크기 계층**을 **F1:무료 계층**으로 선택합니다.

4. **검토 + 만들기**를 선택합니다.

5. 모든 항목을 입력 및 설정을 하였다면, **만들기**를 눌러 IoT Hub를 생성합니다. IoT Hub를 생성 시, 몇 분정도의 시간이 소요가 될 수 있습니다.



<a name="Step-2-Create_IoT_Device"></a>
### 3. IoT Device 생성

1. IoT Hub에서 **탐색기** > **IoT 디바이스** > **새로 만들기**를 선택합니다.

2. **디바이스 ID**를 입력하고 **저장**을 하여 생성합니다.

3. IoT Device 생성 후, 목록에서 **Device를 선택하여 엽니다.**

4. **기본 연결 문자열**을 복사하여 메모장과 같은 별도의 파일에 **저장**을 합니다.



<a name="Step-3-Create_Blob_Storage"></a>
### 4. Blob Storage 생성

1. Azure Portal에서 **리소스 만들기** > **저장소** > **Storage 계정**을 선택합니다.

2. **기본 사항**을 입력 및 설정합니다. 자신이 만든 **IoT Hub와 동일한 리소스 그룹 및 위치**를 선택합니다.

3. **검토 + 만들기**를 선택합니다.

4. Blob Storage 생성 후, **개요** > **컨테이너** > **+ 컨테이너**를 선택합니다.

5. Container의 **이름**을 입력 후, 확인을 눌러 **Container를 생성**합니다.



<a name="Step-4-Create_Stream_Analytics"></a>
### 5. Stream Analytics 생성

1. Azure Portal에서 **리소스 만들기** > **분석** > **Stream Analytics job**을 선택합니다.

2. **작업 이름**을 입력하고 나머지 항목을 다음 그림과 같이 입력 및 설정합니다.

3. **만들기**를 하여 생성합니다.



<a name="Step-5-Configure_Job_Input"></a>
### 6. 작업 입력 구성

1. Stream Analytics에서 작업 **작업 토폴로지** > **입력** > **+ 스트림 입력 추가**를 선택합니다.

2. **IoT Hub**를 선택합니다.

3. **입력 별칭**을 입력하고 나머지 항목을 다음 그림과 같이 입력 및 설정합니다.

4. **저장**을 눌러 작업 입력을 구성합니다.



<a name="Step-6-Configure_Job_Output"></a>
### 7. 작업 출력 구성

1. Stream Analytics에서 작업 **작업 토폴로지** > **출력** > **+ 추가**를 선택합니다.

2. **Blob Storage/Data Lake Storage Gen2**를 선택합니다.

3. **출력 별칭**을 입력하고 나머지 항목을 다음 그림과 같이 입력 및 설정합니다.

4. **저장**을 눌러 작업 출력을 구성합니다.



<a name="Step-7-Define_The_Transformation_Query"></a>
### 8. 변환 Query 정의

1. Stream Analytics에서 작업 **작업 토폴로지** > **쿼리**를 선택합니다.

2. **Query**를 다음 그림과 같이 **수정 및 작성**합니다.

3. **쿼리 저장**을 눌러 저장합니다.



<a name="Step-8-Read_Data_From_IoT_Hub"></a>
### 9. WizFi360 설정 및 실행



<a name="Step-9-Start_The_Stream_Analytics_Job_And_Check_The_Output"></a>
### 10. Stream Analytics 작업 시작 및 출력 확인



<a name="Next"></a>
## 다음 단계



[Link-Data_Communication_Structure]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_data_communication_structure.png
[Link-Required_Item]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_required_item.png
[Link-WizFi360-EVB-Shield]: https://wizwiki.net/wiki/doku.php/products:wizfi360:start
[Link-Azure-Portal]: https://portal.azure.com/
[Link-Azure-Account-Free]: https://azure.microsoft.com/ko-kr/free/