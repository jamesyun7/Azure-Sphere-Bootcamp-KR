# Azure-Sphere-Bootcamp 기술 실습 자료

이 Repo는 Azure Sphere Bootcamp 실습을 위해 작성되었습니다. 모든 실습을 따르고 마치는데 필요한 관련 정보를 제공합니다. 

# 사전준비 내용

## [필수 조건 :](https://docs.microsoft.com/ko-kr/azure-sphere/install/install)

- Windows 10 1주년 업데이트 이상을 실행하는 PC
- Visual Studio 2019 Enterprise, Professional, Community 버전 16.04 이상 또는 - Visual Studio 2017 버전 15.9 이상
- Azure Sphere Development Kit

시작하기 전에 [Install Azure Sphere](https://docs.microsoft.com/ko-kr/azure-sphere/install/overview) 링크의 안내에 따라 필요한 과정들을 완료합니다. 

## Quick check list:
- FTDI 드라이버가 설치되고, 장치관리자에 3개의 COM 포트가 인식되었는지 확인
- 최신 버젼의 [Visual Studio](https://www.visualstudio.com/) 와 [Azure Sphere SDK](https://aka.ms/AzureSphereHardware) 설치
- Azure 계정 등록 후 무료 구독 및 pay-as-you-go 구독 추가
- Azure Sphere CLI 의 `azsphere login` 를 이용해 로그인
- 기존 tenant 가 없는 경우 `azsphere tenant create -n <tenant name>` 를 이용해 Azure Sphere tenant 생성
- `azsphere tenant select -i <tenant id>` 로 Azure Sphere tenant 선택
- (New device ONLY) Device is claimed to user's tenant by `azsphere device claim`
- (새보드만 해당) `azsphere device claim` 을 통해 사용자의 tenant 에 디바이스를 클레임
- Device is recovered by `azsphere device recover` command to have a knowning good Azure Sphere OS.
- `azsphere device recover` 로 최적의 Azure Sphere OS 버젼으로 디바이스 복구
- [Git](https://git-scm.com/download/win) 설치 및 PATH 추가

# 실습 
- [Lab-2 IoT Hub Connection / Uplink Scenario](Lab-2.md)
- [Lab-3 IoT Central Connection](Lab-3.md)
- [Lab-4 Visualize real world data on Azure IoT Central](Lab-4.md)
- [Lab-5 Application Over-the-Air deployment](Lab-5.md)
