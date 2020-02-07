# Raspberry Pi를 이용한 AZURE C SDK 개발 환경 구축

## 목차

-   [소개](#Introduction)
-   [Step 1: 필수 구성 요소](#Step-1-Prerequisites)
-   [Step 2: 디바이스 준비](#Step-2-PrepareDevice)
-   [Step 3: 에이엠 텔레콤 Cat.M1 외장형 모뎀의 RNDIS 모드 설정](#Step-3-SetRNDIS)
-   [Step 4: 예제 코드 빌드](#Step-4-Build)
-   [Step 5: 예제 실행 결과](#Step-5-Execute)
-   [더 보기](#ReadMore)


<a name="Introduction"></a>
## 소개

이 문서는 에이엠 텔레콤의 Cat.M1 외장형 모뎀과 Raspberry pi를 이용하여 AZURE C SDK 개발 환경을 구축하는 방법에 대하여 설명합니다.

Raspberry Pi를 이용하면 Cat.M1 모듈의 RNDIS(Remote Network Driver Interface Specification) 기능을 사용할 수 있습니다. RNDIS는 주로 USB 인터페이스를 통해 사용할 수 있으며, 대부분의 Window, Linux, FreeBSD 등 운영체제에 대한 가상 이더넷 링크를 제공합니다.

데스크 탑 PC에서 무선 인터넷을 사용하기 위해 무선 랜카드를 PC USB 포트에 연결하는 것을 예로 들 수 있습니다.이와 같이 Cat.M1 모듈 및 외장형 모뎀을 RNDIS 형태로 사용하면 연결된 기기에서 Cat.M1 망을이용할 수 있습니다.

각 과정에는 다음 내용들이 포함되어 있습니다.
- 에이엠 텔레콤의 Cat.M1 외장형 모뎀과 Raspberry pi 연결
- 에이엠 텔레콤 Cat.M1 외장형 모뎀의 RNDIS 모드 설정
- AZURE C SDK 기반으로 Cat.M1을 통해 AZURE IoT Hub로 온도 데이터 전송


<a name="Step-1-Prerequisites"></a>
## Step 1: 필수 구성 요소
이 문서를 따라하기 전에 다음과 같은 것들이 준비되어야 합니다.

### Hardware
  - Raspberry Pi Model B
  - AMT5920SK(에이엠 텔레콤 Cat.M1 외장형 모뎀, [Openhouse **이용신청**](https://www.sktiot.com/iot/support/openhouse/reservation/openhouseMain) 시, 요청 사항에 AMT5920SK를 구매 요청 부탁드립니다.)
  - SD Card(16G)

### Software
  - [Raspbian](https://www.raspberrypi.org/downloads/raspbian/)
  - [AZURE C SDK](https://docs.microsoft.com/ko-kr/azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started)


### Cat.M1 외장형 모뎀 상용개통 
  - Cat.M1 외장형 모뎀으로 통신 기능을 구현하려면 **망 개통 과정**이 선행되어야 합니다.
    - 한국의 경우, 국내 Cat.M1 서비스 사업자인 SK Telecom의 망 개통 과정이 필요합니다.

> 모듈은 개발 단계에 따라 시험망 개통 - 상용망 개통 단계를 거쳐야 하며 외장형 모뎀은 즉시 상용망 개통이 가능합니다.
> - 상용망 개통의 경우 USIM 구매 대리점이나 디바이스 구매처 또는 오픈 하우스에 개통 문의

<a name="Step-2-PrepareDevice"></a>
## Step 2: 디바이스 준비

### 1) 하드웨어 준비

WIZnet IoT Shield를 STM32L496 Nucleo-144 보드와 결합합니다.
- 두 장치 모두 Arduino UNO Rev3 호환 핀 커넥터를 지원하므로 손쉽게 결합(Stacking) 할 수 있습니다.

![][1] 

WIZnet IoT Shield는 다양한 밴더의 Cat.M1 모듈을 활용 할 수 있도록 하드웨어 설정을 제공합니다. 따라서 선택한 Cat.M1 인터페이스 보드를 확인하여 장치 설정이 필요합니다.


<a name="#Step-3-SetRNDIS"></a>
## Step 3:에이엠 텔레콤 Cat.M1 외장형 모뎀의 RNDIS 모드 설정

Cat.M1 모듈을 RNDIS 모드로 사용하기 위해서는 다음과 같은 AT 명령어를 통해 설정이 필요하며, 정상적으로 변경이 되면 모듈은 자동으로 재부팅을 수행합니다.

### 3.1 RNDIS 모드 설정

**AT Command:** AT@USBCHG=RNDIS

| Type | Syntax | Response | Example
|:--------|:--------|:--------|:--------|
| Write  | AT@USBCHG=(Mode) | @USBCHG:(Mode)<br><br>OK | AT@USBCHG=RNDIS<br>@USBCHG:RNDIS<br><br>OK<br> |

**Defined values:**

| Parameter | Type | Description |
|:--------|:--------|:--------|
| (Mode) | String | ACM : USB 인터페이스를 ACM 모드로 설정<br>RNDIS : USB 인터페이스를 RNDIS 모드로 설정<br>RMNET : USB 인터페이스를 RMNET 모드로 설정 |

### 3.2 WIoT-AM01 RNDIS 동작 구조 예제

```cpp
/* AMM592SK RNDIS 모드 설정 */

// RNDIS 모드 설정
AT@USBCHG=RNDIS
@USBCHG:RNDIS

OK

// 모듈은 자동으로 재부팅
```

### 3.2 WIoT-AM01 RNDIS 동작 확인
위와 같이 하드웨어 연결이 정상적으로 되었고 Cat.M1 모듈 또한 RNDIS 모드를 이용하기 위한 설정이 정상적으로 되었다면 다음과 같이 Raspberry Pi에서 Cat.M1 네트워크가 연결된 것을 확인 할 수 있습니다.
![][2]


<a name="Step-4-Build"></a>
## Step 4: 예제 코드 빌드[To do]

### 1) 프로그램 빌드

라즈비안 Console에서 다음과 같은 명령을 통해 Microsoft에서 제공하는 샘플 애플리케이션을 받고, 설치 스크립트를 실행 합니다.

```cpp
sudo apt-get install git-core
git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-client-app.git

cd ./iot-hub-c-raspberrypi-client-app
sudo chmod u+x setup.sh
sudo ./setup.sh --simulated-data

```

설치한 샘플 애플리케이션을 빌드하기 위해 다음 명령을 수행합니다.
```cpp
cmake . && make
```

> 자세한 내용은 Microsoft [`C를 사용하는 Raspberry Pi`](https://docs.microsoft.com/ko-kr/azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started) 부분을 참고하여 진행
> **[To do]**

<a name="Step-5-Execute"></a>
## Step 5: 예제 실행 결과[To do]
정상적으로 빌드가 되었다면, 다음과 같이 실행할 수 있습니다.

```cpp
sudo ./app '<DEVICE CONNECTION STRING>'
```
> 자세한 내용은 Microsoft [`C를 사용하는 Raspberry Pi`](https://docs.microsoft.com/ko-kr/azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started) 부분을 참고하여 진행
> **[To do]**

이와 같이 AZURE C SDK 기반으로 Raspberry pi와 에이엠 텔레콤 Cat.M1 외장형 모뎀을 이용하여 Azure IoT hub 에 데이터 보내는 것을 확인 해보았습니다.


<a name="ReadMore"></a>
## 더 보기
* [WIZnet IoT Shield(WIoT-QC01)와 STM32L496 Nucleo-144를 이용한 X-CUBE-AZURE SDK 개발 환경 구축][nucleo-stm32l496_azure-st-sdk]
* [Azure Portal을 사용하여 IoT Hub 만들기](https://docs.microsoft.com/ko-kr/azure/iot-hub/iot-hub-create-through-portal)
* [Azure IoT Explorer 사용하기](https://github.com/Azure/azure-iot-explorer#getting-azure-iot-explorer)



[nucleo-stm32l496_azure-st-sdk]: ./nucleo_stm32l496_azure_st_sdk_bg96.md
[st-azure-dashboard]: ../../../../Azure_Cloud/st_azure_dashboard.md

[1]: ../../../../../images/raspberrypi-azure-c-sdk-1.png "장치 관리자"
[2]: ../../../../../images/raspberrypi-azure-c-sdk-2.png "Keil 프로젝트 화면"



