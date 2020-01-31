# WizFi360 Azure AT Command를 이용하여 Azure IoT Hub에 연결



## 목차
- [시작하기 전에](#What_To_Do)

- [소개](#Learning_Content)

- [디바이스 준비](#Device_Prep)

- [AT 명령어](#Required_Item)

- [동작 예제](#Example)

	1. [IoT Hub 생성](#Step-1-Create_IoT_Hub)

	2. [IoT Hub에 Device 등록](#Step-2-Register_To_IoT_Hub)
	
	3. [Device Explorer 설정](#Step-3-Setting_DeviceExplorer)
	
	4. [WizFi360 설정](#Step-4-Setting_WizFi360)

	5. [IoT Hub에서 수신한 Data 읽기](#Step-5-Read_Data_From_IoT_Hub)

- [다음 단계](#Next)



<a name="What_To_Do"></a>
## 시작하기 전에

##### Hardware Requirement
-   Desktop or laptop computer
-   MicroUSB 케이블
-   WizFi360-EVB-Shield

##### Software Requirement

- 	MS Azure Account (Azure 구독이 아직 없는 경우 체험 무료[계정]을 만듭니다.)
-   Preferred Serial Terminal (TeraTerm, YAT, etc.)
-   [Device Explorer]

<a name="Learning_Content"></a>
## 소개
Microsoft Azure 는 Microsoft 의 클라우드 컴퓨팅 서비스입니다.
Microsoft Azure 의 서비스에 [WizFi360] 을 연동하여 데이터를 클라우드로 전송하고, 모니터링 할 수 있습니다.

본 문서에서는 WizFi360 이용하여 MS Azure Services에 연결 방법에 대한 가이드를 제공합니다.
이 프로세스는 다음 단계로 구성됩니다.
- Azure IoT Hub 준비
- IoT 디바이스 등록
- Azure IoT와 연결 및 데이터 통신

WiFi모듈 테스트를 위해 [WizFi360-EVB-Shield] Evaluation 보드를 사용되었습니다.

![](/images/standalone_mqtt_atcmd_wizfi360_required_item.png)

<a name="Device_Prep"></a>
## 디바이스 준비

####하드웨어 설정

본 문서에서 WizFi360-EVB-Shield가 standalone mode에서 사용됩니다. 따라서 UART를 위해 MicroUSB를 사용할겁니다.
MicroUSB 사용하는경우 SW1을 ON 시키고 MicroUSB 연결해야됩니다.
![](/images/WizFi360EVB_SW1.JPG)

####디바이스 연결
하드웨어 설정 후 MicroUSB 이용하여 PC와 연결합니다. PC운영체제에서 보드와 연결된 COM 포트를 확인할 수 있습니다.
> 윈도우 운영체제의 경우, 장치 관리자(Device Manager)에서 COM 포트를 확인할 수 있습니다.
![](/images/DeviceManager.JPG)

> 장치 관리자에서 COM 포트를 확인할 수 없는 경우, 다음 링크에서 드라이버를 다운로드하여 설치하시기 바랍니다.
  - [Silicon Labs CP210x USB to UART Driver]


<a name="Required_Item"></a>
## AT 명령어


#### 1. Set current WiFi mode (not saved in flash)
**AT Command:** AT+CWMODE_CUR
Syntax:

| Type | Command | Response |
|:--------|:--------|:--------|
| Query | AT+CWMODE_CUR? | +CWMODE:&lt;mode&gt; <br> OK |
| Set| AT+CWMODE_CUR=&lt;mode&gt; | OK |

Defined values:

| Mode | Value |
|:--------|:--------|
| 1 | Station mode|
| 2 | SoftAP mode (factory default) |
| 3 | Station+SoftAP mode|

#### 2. Enable DHCP
**AT Command:** AT+CWDHCP_CUR
Syntax:

| Type | Command | Response |
|:--------|:--------|:--------|
| Query | AT+CWDHCP_CUR? | +CWDHCP_CUR:&lt;para&gt; <br> OK |
| Set | AT+CWMODE_CUR=&lt;para&gt;,&lt;en&gt; | OK |

Defined values:

| Parameter | Value |
|:--------|:--------|
| 0 |  SoftAP DHCP 와 Station DHCP 를 disable 한다.|
| 1 | SoftAP DHCP 는 enable 하고 Station DHCP 는 disable 한다. |
| 2 | 2: SoftAP DHCP 는 disable 하고 Station DHCP 는 enable 한다. |
| 3 | SoftAP DHCP 와 Station DHCP 를 enable 한다. (factory default)|

#### 3. List available APs
**AT Command:** AT+CWLAP
Syntax:

| Type | Command | Response |
|:--------|:--------|:--------|
| Query | AT+CWLAP | +CWLAP:([&lt;ecn&gt;,&lt;ssid&gt;,&lt;rssi&gt;,&lt;mac&gt;,&lt;channel&gt;,&lt;wps&gt;]) |


Defined values:

| Parameter | Value |
|:--------|:--------|
| &lt;ecn&gt;| 0: Open <br> 1: WEP <br> 2: WPA_PSK<br>3: WPA2_PSK<br>4:WPA_WPA2_PSK |
| &lt;ssid&gt; | string parameter. AP의 ssid |
| &lt;rssi&gt; | signal strength |
| &lt;mac&gt; |  string parameter. AP의 mac|
| &lt;wps&gt; | 0: WPS는 disable된다 <br> 1: WPS는 enable된다 |

#### 4. Connect to AP
**AT Command:** AT+CWJAP_CUR
Syntax:

| Type | Command | Response |
|:--------|:--------|:--------|
| Set | AT+CWJAP_CUR=&lt;ssid&gt;,&lt;pwd&gt;,[&lt;bssid&gt;] | +CWJAP_CUR:&lt;ssid&gt;,&lt;bssid&gt;,&lt;channel&gt;,&lt;rssi&gt; <br> OK|

Defined values:

| Parameter | Value |
|:--------|:--------|
| &lt;ssid&gt; | string parameter. Target AP의 ssid. MAX: 32 bytes |
| &lt;pwd&gt; | string parameter. Target AP의 password. MAX: 64-byte ASCII |
| &lt;bssid&gt; | string parameter, target AP 의 MAC address, 같은 SSID 를 가진 여러 개의 AP 들이 있을 때 사용된다. |

#### 5. Azure IoT Hub configuration set

**AT Command:** AT+AZSET

Syntax:

| Type | Command | Response |
|:--------|:--------|:--------|
| Set | AT+AZSET=&lt;iothub_name&gt;,&lt;device_id&gt;,&lt;device_key&gt; | OK |

Defined values:

| Parameter | Value |
|:--------|:--------|
| &lt;hub ID&gt; | string parameter. IoT Hub의 ID |
| &lt;device ID&gt; | string parameter. IoT Device의 ID |
| &lt;key&gt; | string parameter, IoT Device의 Key |

#### 6. Set MQTT Topic

**AT Command:** AT+MQTTTOPIC

Syntax:

| Type | Command | Response |
|:--------|:--------|:--------|
| Set | AT+MQTTTOPIC=&lt;publish topic&gt;,&lt;subscribe topic&gt;,&lt;subscribe topic2&gt;,&lt;subscribe topic3&gt; | OK |

Defined values:

| Parameter | Value |
|:--------|:--------|
| &lt;publish topic&gt; | string parameter, WizFi360 이 publish 하는 topic |
| &lt;subscribe topic&gt; |  string parameter, WizFi360 이 subscribe 하는 topic|
| &lt;subscribe topic2&gt; | string parameter, WizFi360 이 subscribe 하는 topic |
| &lt;subscribe topic3&gt; | string parameter, WizFi360 이 subscribe 하는 topic |

> Note:
- 이 command 는 broker 에 연결하기전에 설정되어야 합니다.
- &lt;subscribe topic2&gt; 와 &lt;subscribe topic3&gt;는 Firmware v1.0.5.0 이후 version 부터 사용가능 합니다.

#### 7. Connect to Azure

**AT Command:** AT+AZCON

Syntax:

| Type | Command | Response |
|:--------|:--------|:--------|
| Set | AT+AZCON | CONNECT <br> OK |
> Note:
• 이 command 를 전송하기전에 AT+AZSET command 와 AT+MQTTTOPIC command 를 설정합니다.
• Connect 이후 AT+MQTTPUB command 를 통해 Azure Sever 에 데이터를 전송합니다.
• 자세한 내용은 https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-mqtt-support 를 참조하세요.

#### 8. Publish a message

**AT Command:** AT+MQTTPUB

Syntax:

| Type | Command | Response |
|:--------|:--------|:--------|
| Set | AT+MQTTPUB=&lt;message&gt;| OK |

> Note:
• 이 command 는 MQTT 가 연결되어 있을 때 사용됩니다.
• Publish 한 data 의 topic 은 AT+MQTTTOPIC command 에 의해 결정되며, 사용자는 broker 에 연결하기전에 topic 을 설정합니다.

<a name="Example"></a>
## 동작 예제



<a name="Step-1-Create_IoT_Hub"></a>
### 1. IoT Hub 생성

1. [Azure Portal]에 로그인합니다.
2. **리소스 만들기 > 사물 인터넷 > IoT Hub**를 선택합니다.
![](/images/create_hub.jpg)
3. IoT Hub 창에서 다음 정보를 입력합니다.
4. 완료되면 다음: 크기 및 규모 설정을 선택합니다.
![](/images/iot-hub-size-and-scale.png)
5. 가격 책정 및 규모 계층을 선택합니다. 이 빠른 시작의 경우 구독에서 아직 사용할 수 있다면 F1 - 무료 계층을 선택합니다.
6. 검토 + 만들기를 선택합니다. IoT Hub 정보를 검토하고 만들기를 클릭합니다. IoT Hub를 만드는 데 몇 분 정도 걸릴 수 있습니다. 알림 창에서 진행 상황을 모니터링할 수 있습니다.

<a name="Step-2-Register_To_IoT_Hub"></a>
### 2. IoT Hub에 Device 등록

1. IoT Hub 탐색 메뉴의 IoT 디바이스 아래에서 추가를 클릭합니다. 디바이스 ID를 추가하고 저장을 클릭합니다.
![](/images/add-device-iot-hub.png)
2. 디바이스가 만들어진 후에 IoT 디바이스 목록에서 디바이스를 엽니다.

<a name="Step-3-Setting_DeviceExplorer"></a>
### 3. Device Explorer 설정

1. IoT Hub > Shared access policies > iothubowner 클릭하여 connection string primary key 복사합니다.
![](/images/hubownerstring.jpg)
2. Device Explorer 다운로드 후 실행합니다.
3. Configuration 탭에서 iothubowner connection string 붙여넣습니다.
4. Update 버튼을 클릭합니다.

![](/images/rpi2_deviceexplorer01.png)

<a name="Step-4-Setting_WizFi360"></a>
### 4. WizFi360 설정

#### 시리얼 터미널 연결 및 실행

시리얼 터미널 프로그램을 실행하여 디바이스 연결 단계에서 확인한 보드의 COM 포트와 Baudrate 115200을 선택하여 시리얼 포트를 연결합니다.
> 디버그 메시지 출력용 시리얼 포트 설정 정보: 115200-8-N-1, None.



##### 1.  Set Wifi station mode

| Command | Response |
|:--------|:--------|
| AT+CWMODE_CUR=1 // station mode | OK |

##### 2.  Set DHCP Enable

| Command | Response |
|:--------|:--------|
| AT+CWDHCP_CUR=1,1 // DHCP enable on Station mode | OK |

##### 3.  Get possible Wi-Fi AP List for connection

| Command | Response |
|:--------|:--------|
| AT+CWLAP  | +CWLAP : (3,"ssid",-57,"mac address",1,1) // encryption method, ssid, rssi, mac address, channel, wps |

##### 4.  Connect to Wi-Fi AP

| Command | Response |
|:--------|:--------|
| AT+CWJAP_CUR="ssid","password"  | WIFI CONNECTED <br> WIFI GOT IP |

##### 5.  Query WizFi360 device' IP address

| Command | Response |
|:--------|:--------|
| AT+CIPSTA_CUR?  | +CIPSTA_CUR:ip:”192.168.10.13” <br> +CIPSTA_CUR:gateway:”192.168.10.1” <br> +CIPSTA_CUR:network:”255.255.255.0” |

#### 다음 AT 명령을 실행하여 Azure 서비스에 연결합니다.

##### 1.  Set Azure connection


| Command | Response |
|:--------|:--------|
| AT+AZSET="iothub_name","device_id","device_key" | OK |

##### 2. Set MQTT Topic

| Command | Response |
|:--------|:--------|
| AT+MQTTTOPIC="/devices/{device_id}/messages/events/","/devices/{device_id}/messages/devicebound/#"<br><br>Example<br> AT+MQTTTOPIC="/devices/testDevice/messages/events/","/devices/testDevice/messages/devicebound/#"| OK |

> Note: 
> MQTT Topic follows the rules defined in Azure IoT Hub. 
> Refer to reference: [Communicate with your IoT hub using the MQTT protocol: Using the MQTT protocol directly (as a device)]

##### 3. Connect to Azure

| Command | Response |
|:--------|:--------|
| AT+AZCON | OK |

##### 4. Publish data

| Command | Response |
|:--------|:--------|
| AT+MQTTPUB="{"deviceId":"WizFi360","temperature":28.16,"humidity":46.04}" | OK |

![](/images/Token2Shell.jpg)

<a name="Step-5-Read_Data_From_IoT_Hub"></a>
### 5. Device Explorer에서 수신한 Data 읽기

1. Device Explorer 에서 Monitor 버튼을 클릭합니다.
2. 터미널에서 MQTTPUB command으로 수신한 데이터를 확인 할 수 있습니다.

![](/images/DeviceExplorerscreenshot.PNG)
<a name="Next"></a>
## 다음 단계




[계정]: https://azure.microsoft.com/ko-kr/free/
[Azure Portal]: https://portal.azure.com/
[WizFi360]: https://wizwiki.net/wiki/doku.php/products:wizfi360:start
[WizFi360-EVB-Shield]: https://wizwiki.net/wiki/doku.php/products:wizfi360:board:wizfi360-evb:start
[Device Explorer]: https://catalog.azureiotsolutions.com/docs?title=Azure/azure-iot-device-ecosystem/manage_iot_hub
[Silicon Labs CP210x USB to UART Driver]: https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers
