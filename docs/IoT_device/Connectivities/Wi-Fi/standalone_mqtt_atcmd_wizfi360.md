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

- [WizFi360 EVB Shield][Link-WizFi360-EVB-Shield]

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

 ![][Link-Create_IoT_Hub_1]

 ![][Link-Create_IoT_Hub_2]

2. **기본 사항**을 입력합니다. **리소스 그룹**을 **새로 만들기**를 선택하여 생성합니다.

 ![][Link-Create_IoT_Hub_5]

3. **크키 및 배율**을 설정합니다. **가격 및 크기 계층**을 **F1:무료 계층**으로 선택합니다.

 ![][Link-Create_IoT_Hub_6]

4. **검토 + 만들기**를 선택합니다.

 ![][Link-Create_IoT_Hub_7]

5. 모든 항목을 입력 및 설정을 하였다면, **만들기**를 눌러 IoT Hub를 생성합니다. IoT Hub를 생성 시, 몇 분 정도의 시간이 소요가 될 수 있습니다.

 ![][Link-Create_IoT_Hub_8]



<a name="Step-2-Create_IoT_Device"></a>
### 3. IoT Device 생성

1. IoT Hub에서 **탐색기** > **IoT 디바이스** > **새로 만들기**를 선택합니다.

 ![][Link-Create_IoT_Device_2]

2. **디바이스 ID**를 입력하고 **저장**을 하여 IoT Device를 생성합니다.

 ![][Link-Create_IoT_Device_4]

3. IoT Device 생성 후, 목록에서 Device를 선택하여 엽니다.

 ![][Link-Create_IoT_Device_5]

 ![][Link-Create_IoT_Device_6]

4. **기본 연결 문자열**을 복사하여 메모장과 같은 별도의 파일에 **저장**을 합니다.

 ![][Link-Create_IoT_Device_7]

 ![][Link-Create_IoT_Device_8]



<a name="Step-3-Create_Blob_Storage"></a>
### 4. Blob Storage 생성

1. Azure Portal에서 **리소스 만들기** > **저장소** > **Storage 계정**을 선택합니다.

 ![][Link-Create_Blob_Storage_1]

 ![][Link-Create_Blob_Storage_2]

2. **기본 사항**을 입력 및 설정합니다. 자신이 만든 **IoT Hub와 동일한 리소스 그룹 및 위치**를 선택합니다.

 ![][Link-Create_Blob_Storage_4]

3. **검토 + 만들기**를 선택합니다.

 ![][Link-Create_Blob_Storage_5]

4. 모든 항목을 입력 및 설정을 하였다면, **만들기**를 눌러 Blob Storage를 생성합니다.

 ![][Link-Create_Blob_Storage_6]

5. Blob Storage 생성 후, **개요** > **컨테이너** > **컨테이너**를 선택합니다.

 ![][Link-Create_Blob_Storage_10]

 ![][Link-Create_Blob_Storage_11]

6. Container의 **이름**을 입력 후, **확인**을 눌러 Container를 생성합니다.

 ![][Link-Create_Blob_Storage_12]



<a name="Step-4-Create_Stream_Analytics"></a>
### 5. Stream Analytics 생성

1. Azure Portal에서 **리소스 만들기** > **분석** > **Stream Analytics job**을 선택합니다.

 ![][Link-Create_Stream_Analytics_1]

 ![][Link-Create_Stream_Analytics_3]

2. **작업 이름**을 입력하고 나머지 항목을 다음 그림과 같이 입력 및 설정합니다.

 ![][Link-Create_Stream_Analytics_5]

3. **만들기**를 눌러 생성합니다.

 ![][Link-Create_Stream_Analytics_6]



<a name="Step-5-Configure_Job_Input"></a>
### 6. 작업 입력 구성

1. Stream Analytics에서 작업 **작업 토폴로지** > **입력** > **스트림 입력 추가**를 선택합니다.

 ![][Link-Configure_Job_Input_2]

2. **IoT Hub**를 선택합니다.

 ![][Link-Configure_Job_Input_3]

3. **입력 별칭**을 입력하고 나머지 항목을 다음 그림과 같이 입력 및 설정합니다.

 ![][Link-Configure_Job_Input_5]

4. **저장**을 눌러 작업 입력을 구성합니다.

 ![][Link-Configure_Job_Input_6]



<a name="Step-6-Configure_Job_Output"></a>
### 7. 작업 출력 구성

1. Stream Analytics에서 작업 **작업 토폴로지** > **출력** > **추가**를 선택합니다.

 ![][Link-Configure_Job_Output_2]

2. **Blob Storage/Data Lake Storage Gen2**를 선택합니다.

 ![][Link-Configure_Job_Output_3]

3. **출력 별칭**을 입력하고 나머지 항목을 다음 그림과 같이 입력 및 설정합니다.

 ![][Link-Configure_Job_Output_5]

4. **저장**을 눌러 작업 출력을 구성합니다.

 ![][Link-Configure_Job_Output_6]



<a name="Step-7-Define_The_Transformation_Query"></a>
### 8. 변환 Query 정의

1. Stream Analytics에서 작업 **작업 토폴로지** > **쿼리**를 선택합니다.

 ![][Link-Define_The_Transformation_Query_2]

2. **Query**를 다음 그림과 같이 **수정 및 작성**합니다.

 ![][Link-Define_The_Transformation_Query_3]

3. **쿼리 저장**을 눌러 저장합니다.

 ![][Link-Define_The_Transformation_Query_4]



<a name="Step-8-Set_Up_And_Run_WizFi360"></a>
### 9. WizFi360 설정 및 실행

WizFi360의 조금 더 상세한 **사용 방법**은 [**Quick Start Guide**][Link-WizFi360-Quick_Start_Guide], **AT Command**는 [**AT Instruction Set**][Link-WizFi360-AT_Instruction_Set]을 참고 바랍니다.

1. Mode 설정

 |  Command | Response |
 |:--------|:--------|
 | AT+CWMODE_CUR=1 | OK |

2. DHCP 설정

 | Command | Response |
 |:--------|:--------|
 | AT+CWDHCP_CUR=1,1 | OK |

3. AP 접속

 | Command | Response |
 |:--------|:--------|
 | AT+CWJAP_CUR="**ssid**","**password**" <br><br> Example : <br> AT+CWJAP_CUR="**wiznet**","**0123456789**" | WIFI CONNECTED <br> WIFI GOT IP <br><br> OK |

4. MQTT 연결 설정

 | Command | Response |
 |:--------|:--------|
 | AT+MQTTSET="**iot_hub_host_name**/**device_id**/?api-version=2018-06-30","**sas_token**","**device_id**",60<br><br> Example : <br> AT+MQTTSET="**MyWizFi360IoTHub.azure-devices.net**/**MyWizFi360IoTDevice**/?api-version=2018-06-30","**SharedAccessSignature sr=MyWizFi360IoTHub.azure-devices.net%2Fdevices%2FMyWizFi360IoTDevice&sig=t3R9nDS7ezMGBdb%2FNd5ktb3xQx5jx4NC02n325vRA6c%3D&se=1611895717**","**MyWizFi360IoTDevice**",60 | OK |

 ※ **SAS Token 생성**은 아래를 참고 바랍니다.

 ① IoT Hub에서 **설정** > **공유 액세스 정책**을 선택합니다.

 ![][Link-Create_SAS_Token_1]

 ② **iothubowner**를 선택합니다.

 ![][Link-Create_SAS_Token_2]

 ③ 공유 엑세스 키에서 **연결 문자열 - 기본 키**를 복사합니다.

 ![][Link-Create_SAS_Token_3]

 ④ [**Device Explorer**][Link-Device_Explorer]를 실행합니다.

 ⑤ **Configure** > **Connection Information** > **IoT Hub Connection String**에 복사한 **연결 문자열 - 기본 키**를 입력하고 Update를 눌러 정보를 갱신합니다.

 ![][Link-Create_SAS_Token_4]

 ![][Link-Create_SAS_Token_5]

 ⑥ **Management** > **SAS Token**을 눌러 엽니다.

 ![][Link-Create_SAS_Token_6]

 ⑦ **TTL(Days)**에 값을 입력한 후, **Generate**를 눌러 생성합니다.

 ![][Link-Create_SAS_Token_7]

5. Topic 설정

 | Command | Response |
 |:--------|:--------|
 | AT+MQTTTOPIC="devices/**device_id**/messages/events/",devices/**device_id**/messages/devicebound/# <br><br> Example : <br> AT+MQTTTOPIC="devices/**MyWizFi360IoTDevice**/messages/events/","devices/**MyWizFi360IoTDevice**/messages/devicebound/#" | OK |

6. Broker 연결

 | Command | Response |
 |:--------|:--------|
 | AT+MQTTCON=1,"**iot_hub_host_name**",8883 <br><br> Example : <br> AT+MQTTCON=1,"**MyWizFi360IoTHub.azure-devices.net**",8883 | CONNECT <br><br> OK |

7. Publish Message

 ※ Data를 Publish 할 때, [Stream Analytics 실행](#Run_Stream_Analytics) 중이어야 Blob Storage로 Data가 전달됩니다.

 | Command | Response |
 |:--------|:--------|
 | AT+MQTTPUB="publish_data" <br><br> Example : <br> AT+MQTTPUB="{"deviceId":"MyWizFi360IoTDevice","temperature":21.97,"humidity":43.58}"| OK |

 ```
 Note :
 Publish Data는 어떤 형태라도 가능하지만, Azure Guide의 Stream Analytics에서 작업 입력 구성 설정 중,
 Event Serialization 형식이 기본 JSON 형태이므로 맞춰주어야 합니다.
 ```

 ![][Link-Standalone_Mqtt_Atcmd_Wizfi360_Set_Up_And_Run_Wizfi360_1]



<a name="Step-9-Start_The_Stream_Analytics_Job_And_Check_The_Output"></a>
### 10. Stream Analytics 작업 시작 및 출력 확인

<a name="Run_Stream_Analytics"></a>
1. Stream Analytics에서 **개요** > **시작** > **현재** > **시작**을 선택합니다.

 ![][Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_1]

 ![][Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_2]

 ![][Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_3]

2. Blob Storage에서 **게요** > **컨테이너**를 선택합니다.

 ![][Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_4]

3. 목록에서 Container를 선택하여 엽니다.

 ![][Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_5]

4. 목록에서 Blob을 선택하여 엽니다.

 ![][Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_6]

5. **편집**을 눌러 WizFi360으로부터 수신한 Data를 확인합니다.

 ![][Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_7]

 ![][Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_8]



<a name="Next"></a>
## 다음 단계
준비 중 . . .



[Link-Data_Communication_Structure]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_data_communication_structure.png
[Link-Required_Item]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_required_item.png
[Link-WizFi360-EVB-Shield]: https://wizwiki.net/wiki/doku.php/products:wizfi360:start
[Link-Azure-Portal]: https://portal.azure.com/
[Link-Azure-Account-Free]: https://azure.microsoft.com/ko-kr/free/
[Link-Create_IoT_Hub_1]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_iot_hub_1.png
[Link-Create_IoT_Hub_2]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_iot_hub_2.png
[Link-Create_IoT_Hub_5]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_iot_hub_5.png
[Link-Create_IoT_Hub_6]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_iot_hub_6.png
[Link-Create_IoT_Hub_7]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_iot_hub_7.png
[Link-Create_IoT_Hub_8]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_iot_hub_8.png
[Link-Create_IoT_Device_2]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_iot_device_2.png
[Link-Create_IoT_Device_4]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_iot_device_4.png
[Link-Create_IoT_Device_5]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_iot_device_5.png
[Link-Create_IoT_Device_6]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_iot_device_6.png
[Link-Create_IoT_Device_7]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_iot_device_7.png
[Link-Create_IoT_Device_8]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_iot_device_8.png
[Link-Create_Blob_Storage_1]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_blob_storage_1.png
[Link-Create_Blob_Storage_2]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_blob_storage_2.png
[Link-Create_Blob_Storage_4]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_blob_storage_4.png
[Link-Create_Blob_Storage_5]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_blob_storage_5.png
[Link-Create_Blob_Storage_6]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_blob_storage_6.png
[Link-Create_Blob_Storage_10]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_blob_storage_10.png
[Link-Create_Blob_Storage_11]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_blob_storage_11.png
[Link-Create_Blob_Storage_12]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_blob_storage_12.png
[Link-Create_Stream_Analytics_1]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_stream_analytics_1.png
[Link-Create_Stream_Analytics_3]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_stream_analytics_3.png
[Link-Create_Stream_Analytics_5]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_stream_analytics_5.png
[Link-Create_Stream_Analytics_6]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_stream_analytics_6.png
[Link-Configure_Job_Input_2]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_configure_job_input_2.png
[Link-Configure_Job_Input_3]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_configure_job_input_3.png
[Link-Configure_Job_Input_5]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_configure_job_input_5.png
[Link-Configure_Job_Input_6]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_configure_job_input_6.png
[Link-Configure_Job_Output_2]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_configure_job_output_2.png
[Link-Configure_Job_Output_3]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_configure_job_output_3.png
[Link-Configure_Job_Output_5]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_configure_job_output_5.png
[Link-Configure_Job_Output_6]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_configure_job_output_6.png
[Link-Define_The_Transformation_Query_2]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_define_the_transformation_query_2.png
[Link-Define_The_Transformation_Query_3]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_define_the_transformation_query_3.png
[Link-Define_The_Transformation_Query_4]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_define_the_transformation_query_4.png
[Link-WizFi360-Quick_Start_Guide]: http://wizwiki.net/wiki/lib/exe/fetch.php/products:wizfi360:wizfi360ds:wizfi360qs_v113k.pdf
[Link-WizFi360-AT_Instruction_Set]: http://wizwiki.net/wiki/lib/exe/fetch.php/products:wizfi360:wizfi360ds:wizfi360_atset_v106k.pdf
[Link-Create_SAS_Token_1]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_sas_token_1.png
[Link-Create_SAS_Token_2]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_sas_token_2.png
[Link-Create_SAS_Token_3]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_sas_token_3.png
[Link-Device_Explorer]:https://github.com/Wiznet/azure-iot-kr/blob/master/tools/Device%20Explorer/SetupDeviceExplorer.msi
[Link-Create_SAS_Token_4]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_sas_token_4.png
[Link-Create_SAS_Token_5]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_sas_token_5.png
[Link-Create_SAS_Token_6]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_sas_token_6.png
[Link-Create_SAS_Token_7]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_create_sas_token_7.png
[Link-Standalone_Mqtt_Atcmd_Wizfi360_Set_Up_And_Run_Wizfi360_1]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_set_up_and_run_wizfi360_1.png
[Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_1]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_start_the_stream_analytics_job_and_check_the_output_1.png
[Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_2]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_start_the_stream_analytics_job_and_check_the_output_2.png
[Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_3]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_start_the_stream_analytics_job_and_check_the_output_3.png
[Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_4]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_start_the_stream_analytics_job_and_check_the_output_4.png
[Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_5]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_start_the_stream_analytics_job_and_check_the_output_5.png
[Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_6]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_start_the_stream_analytics_job_and_check_the_output_6.png
[Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_7]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_start_the_stream_analytics_job_and_check_the_output_7.png
[Link-Start_The_Stream_Analytics_Job_And_Check_The_Output_8]: https://github.com/Wiznet/azure-iot-kr/blob/master/images/standalone_mqtt_atcmd_wizfi360_start_the_stream_analytics_job_and_check_the_output_8.png