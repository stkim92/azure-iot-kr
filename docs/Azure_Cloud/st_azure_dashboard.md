# WIZnet Azure IoT Documents and Samples

## 목차

-   [시작하기 전에](#Prerequisites)
-   [소개](#Introduction)
-   [ST AZURE Dashboard 기능 설명](#FunctionExamplanation)

<a name="Prerequisites"></a>
## 시작하기 전에

> * 하드웨어 설정과 개발환경 구축은 **[MBED 기반으로 Cat.M1 디바이스 개발 시작하기][mbed-getting-started]** 문서에 상세히 설명되어 있습니다.
>
> * Cat.M1과 같은 Cellular IoT 디바이스는 통신 서비스 사업자의 운영 기준 및 규정에 따라 모듈 펌웨어 및 동작 방식에 차이가 있을 수 있습니다. 본 문서는 한국 **[SK Telecom Cat.M1 서비스][skt-iot-portal]** 를 기준으로 작성되었습니다.
>
> * 본 예제는 아래 링크의 예제를 참고하여 제작하였으며, HW 구성의 경우 외장형 모뎀을 모듈로 변경하였고 SW의 경우 버튼 카운트에 온도를 센싱하여 주기적으로 Pelion IoT Platform으로 전송하도록 수정하였습니다.


<a name="Introduction"></a>
## 소개

**문서의 주요 내용**

이 문서는 [Keil Compiler](https://www.keil.com/)를 기반으로 WIZnet IoT Shield(WIoT-QC01)와 STM32L496 Nucleo-144를 이용하여 X-CUBE-AZURE SDK 개발 환경을 구축하는 방법에 대하여 설명합니다.

각 과정에는 다음 내용들이 포함되어 있습니다:
- WIZnet IoT Shield(WIoT-QC01)와 STM32L496 Nucleo-144 하드웨어 설정
- STM32L496 Nucleo-144와 앰투앰넷 Cat.M1 모듈을 이용하여 X-CUBE-AZURE SDK를 사용하기 위한 수정 사항
- 디바이스와 Azure Cloud 상에서 정상 통신 확인 과정

<a name="FunctionExamplanation"></a>
## ST AZURE Dashboard 기능 설명

#### 1) ST AZURE Dashboard 접속
해당 [링크](https://stm32ode-v2.azurewebsites.net)를 접속하면 다음과 같이 ST AZURE Dashboard 홈페이지에 접속할 수 있습니다.

![][1]

#### 2) 회원가입 및 로그인
좌측 메뉴의 `Sign-Up`을 클릭하여 회원가입을 한 후, 로그인을 합니다.

![][2]

#### 3) 디바이스 등록
디바이스를 등록하기 위해 메뉴의 `Devices`를 클릭하고 `+`버튼을 클릭합니다.
![][3]

`DeviceID`란에 **[WIZnet IoT Shield(WIoT-QC01)와 STM32L496 Nucleo-144를 이용한 X-CUBE-AZURE SDK 개발 환경 구축][nucleo_stm32l496_azure_st_sdk_]**~에서 확인한 12자리 문자로 구성된 값을 입력합니다. Device Type은 `Auto-provisioned`로 선택하여 `SUBMIT`버튼을 클릭합니다.
![][4]
![][5]
#### 4) 데이터 확인
데이터를 확인 하기 위해 메뉴의 `Telemetry`을 클릭하고, 데이터를 조회할 디바이스, 데이터, 조회 기간을 선택하신 후 오른쪽 화실표를 클릭하시면, 하단에 그래프가 출력되는 것을 확인 할 수 있습니다.

본 예제는 온도데이터를 실시간으로 선택한 화면 입니다.
![][6]

[1]: ../../images/st-azure-dashboard-1.png
[2]: ../../images/st-azure-dashboard-2.png
[3]: ../../images/st-azure-dashboard-3.png
[4]: ../../images/st-azure-dashboard-4.png
[5]: ../../images/st-azure-dashboard-5.png
[6]: ../../images/st-azure-dashboard-6.png