# Azure-Sphere-Bootcamp 기술 실습 자료

이 Repo는 Azure Sphere Bootcamp 실습을 위해 작성되었습니다. 모든 실습을 따르고 마치는데 필요한 관련 정보를 제공합니다. 

# 사전준비 내용

## [필수 조건 :](https://docs.microsoft.com/ko-kr/azure-sphere/install/install)

- Windows 10 1주년 업데이트 이상을 실행하는 PC
- Visual Studio 2019 Enterprise, Professional, Community 버전 16.04 이상
- Azure Sphere Development Kit

시작하기 전에 [Install Azure Sphere](https://docs.microsoft.com/ko-kr/azure-sphere/install/overview) 링크의 안내에 따라 필요한 과정들을 완료합니다. 

## Quick check list:
- FTDI 드라이버가 설치되고, 장치관리자에 3개의 COM 포트가 인식되었는지 확인
- 최신 버젼의 [Visual Studio](https://www.visualstudio.com/) 와 [Visual Studio용 Azure Sphere SDK](https://docs.microsoft.com/ko-kr/azure-sphere/install/install-sdk) 설치
    > **중요!** 본 실습에서는 **Visual Studio** 용 SDK를 설치합니다. 
    Windows 용 SDK를 먼저 설치하면 Visual Studio 용이 설치되지 않습니다.
- Azure 계정 등록 후 무료 구독 및 pay-as-you-go 구독 추가
- Azure Sphere CLI 의 `azsphere login` 를 이용해 로그인 혹은 `azsphere login --newuser <MS account>` 로 계정 추가
- 기존 tenant 가 없는 경우 `azsphere tenant create -n <tenant name>` 를 이용해 Azure Sphere tenant 생성
- `azsphere tenant select -i <tenant id>` 로 Azure Sphere tenant 선택
- (새보드만 해당) `azsphere device claim` 을 통해 사용자의 tenant 에 디바이스를 클레임
    > Device Claim 은 되돌릴 수 없으므로 계정과 Tenant 선택을 신중히 합니다.
- `azsphere device recover` 로 최신 Azure Sphere OS 버젼으로 디바이스 복구
- [Git](https://git-scm.com/download/win) 설치 및 PATH 추가


# 하드웨어
실습은 MT3620_RDB (Reference Development Board) 혹은 AVNET_MT3620_SK (Starter Kit)보드를 사용할 수 있습니다.

## MT3620_RDB (Reference Development Board)
자세한 정보는 [azure-sphere-hardware-designs](https://github.com/Azure/azure-sphere-hardware-designs) 에서 확인 가능합니다.

![](images/RDB.png)

## AVNET_MT3620_SK (Starter Kit)
자세한 정보는 [링크](https://www.element14.com/community/community/designcenter/azure-sphere-starter-kits)에서 확인 가능합니다.

![](images/AzureSphereKit_front.png)


# 실습
- [Lab-1 : LED Blink](Lab-1.md) 
- [Lab-2 : Application Over-The-Air deployment ](Lab-2.md)
- [Lab-3 : IoT Central Connection](Lab-3.md)
- [Lab-4 : Visualize real world data on Azure IoT Central - MT3620 RDB](Lab-4.md)
- [Lab-4 : Visualize real world data on Azure IoT Central - Avnet Starter Kit](Lab-4-1.md)
- [Option Lab : IoT Hub Connection / Uplink / Downlink](Lab-5.md)


# Sample Code Disclaimer
Sample code – No Warranties THE SAMPLE CODE SOFTWARE IS PROVIDED "AS IS" AND WITHOUT WARRANTY. TO THE MAXIMUM EXTENT PERMITTED BY LAW, MICROSOFT DISCLAIMS ANY AND ALL OTHER WARRANTIES, WHETHER EXPRESS OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, ANY IMPLIED WARRANTIES OF MERCHANTABILITY, NON-INFRINGEMENT, OR FITNESS FOR A PARTICULAR PURPOSE, WHETHER ARISING BY A COURSE OF DEALING, USAGE OR TRADE PRACTICE OR COURSE OF PERFORMANCE. In no event shall Microsoft, its licensors, the authors or copyright holders be liable for any claim, damages or other liability, whether in an action of contract, tort or otherwise, arising from, out of or in connection with the software or the use thereof.

This code may contain errors and/or may not operate correctly. Microsoft undertakes no duty to correct any errors or update the software. Your use of this code is optional and subject to any license provided therewith or referenced therein, if any. Microsoft does not provide you with any license or other rights to any Microsoft product or service through the code provided to you.