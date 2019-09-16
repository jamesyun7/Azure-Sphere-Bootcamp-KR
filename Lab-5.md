# Lab-5: Over-the-Air로 어플리케이션 배포하기

- [Home Page](README.md)로 돌아가기

## 목적

- Azure Sphere 에 Wi-Fi 네트워크 설정하는 것을 익힐 수 있습니다.
- Azure Sphere 유틸리티를 통해 어플리케이션을 Over-The-Air로 배포하는 방법을 배울 수 있습니다.
- Over-The-Air 배포를 이해하는데 필요한 모든 컨셉을 이해할 수 있습니다.
  
## 단계

1. 아래의 절차대로 Wi-Fi credential 이 설정되었는지 확인하고 Azure Sphere 를 AP에 연결합니다.

- Wi-Fi SSID 와 패스워드를 설정하고 Azure Sphere 디바이스를 Azure Sphere 보안 서비스에 연결하도록 합니다.   
   `azsphere device wifi add --ssid <yourSSID> --key <yourNetworkKey>`
   
   > - 보안설정이 없는 Wi-Fi 네트워크 연결에서는 --key 플래그를 생략합니다.
   > - 만약 SSID 나 패스워드에 스페이스가 있는 경우는 " "처리 합니다. e.g. --ssid "My iPhone"

-  아래의 커맨드로 AP에 연결되었는지 Wi-Fi 상태를 확인할 수 있습니다.
   
   `azsphere device wifi show-status`

    ![](images/show-wifi-status.png)
    
2. *product SKU* 를 추가하고 고유의 이름을 할당합니다. GUID 는 이후에 사용되니 따로 적어둡니다. Description 파라메터는 옵션이므로 필수는 아닙니다.

   `azsphere sku create --name <sku-name> --description <optional-desc>`
   
4. *device group* 을 추가하고 고유의 이름을 할당합니다. GUID 는 이후에 사용되니 따로 적어둡니다. 이 디바이스 그룹에는 Azure Sphere 보안 서비스 의 모든 업데이트를 허용하는 기본 정책이 포함됩니다. (Azure Sphere OS와 어플리케이션 둘 다 해당)

    `azsphere device-group create --name <device-group-name>`

5. 디바이스에 SKU를 붙이고, 디바이스를 디바이스 그룹에 할당합니다. 동시에 아래 커맨드는 기존 어플리케이션을 삭제하고 디버그 모드를 해제합니다. 이제 디바이스는 오직 Azure Sphere Security Service(이하 AS3)에서 production-signed 이미지만 허용합니다.

    `azsphere device prep-field --skuid <productsku-GUID> --devicegroupid <devgroup-GUID>`

    이 시점에서 기존에 로드된 LED blink 동작이 없어진 것을 볼 수 있습니다.

6. `azsphere device show-ota-config` 커맨드를 통해 AS3 의 디바이스 관련한 정보들을 얻을 수 있습니다. 나중을 위해 *chip SKU* 를 따로 기록해둡니다.

   ![](images/chip-sku.png)

7. 어플리케이션 빌드가 성공적으로 끝나면, Visual Studio는 어플리케이션에 메타데이터를 포함하여 *.imagepackage*파일로 패키징합니다. 아래 커맨드는 이것을 AS3에 업로드하고 내 특정 어플리케이션을 나타내는 *component* 를 자동으로 생성합니다.
   
   `azsphere component image add --autocreatecomponent --filepath <file_path>`

   - `<file_path>`파라메터는 " " 기호 안에 imagepackage 파일의 절대경로로 입력 되어야 합니다.  
        
        ![](images/path.png)

   - Component ID, Name 은 *app_manifest.json* 파일에서 정의됩니다. imagepackage 에 이 메타데이터가 포함됩니다.
        
        ![](images/component-id.png)

   image ID 는 다음 단계에 사용되니 따로 기록해둡니다.
   이 ID 는 내 어플리케이션의 현재 버젼을 상징하며 빌드할 때마다 변경됩니다.

    ![](images/image-id.png)

8. *image set*을 추가하고 AS3 에 업로드되는 이미지를 연결합니다. image-GUID 는 이전 단계에서 기록한 것을 입력합니다.
image set 이름은 현재 tenant 안에서 고유한 것을 사용합니다.

    `azsphere image-set create --name <imageset-name> --imageid <image-GUID>`

    > 아래와 같이 현재 시간과 날짜를 image-set 이름 뒤에 붙여서 사용해면 실습 시 구분에 도움이 됩니다.    
    ImageSet-GPIOHighLevelApp-2019.08.27.12.00

    다음 단게를 위해 image-set GUID를 따로 기록해둡니다.
![](images/image-set.png)

9. 아래 커맨드로 어플리케이션 *feed* 를 생성합니다. 이는 이전 단계에서 다루었던 *SKU* set (chip SKU + product SKU) 와 *component* 를 포함합니다.

    `azsphere feed create --name <feed-name> --componentid <component-id> --chipSkuid <chipsku-GUID> --productskuid <productsku-GUID> --dependentfeedid 3369f0e1-dedf-49ec-a602-2aa98669fd61`

    *--dependentfeedid* 파라메터는 Azure Sphere OS feed 의 ID 를 입력합니다. *(3369f0e1-dedf-49ec-a602-2aa98669fd61)*  모든 어플리케이션 feed 는 Azure Sphere OS feed에 의존성을 가져야 합니다.

    다음단계를 위해 *feed GUID* 를 따로 기록해둡니다.

    ![](images/feed-id.png)

10. 아래 커맨드로 *feed*를 *device group*에 추가합니다.
    
    `azsphere device-group feed add --devicegroupid <devgroup-GUID> --feedid <feed-GUID>`

11. *image set* 을 *feed* 에 추가하여 OTA 배포를 활성화 합니다.

    `azsphere feed image-set add --feedid <feed-GUID> --imagesetid <imageset-GUID>`

12. 보드를 리셋하고 몇 분 기다리면, OTA 배포가 완료 후 LED1 이 한 번 깜박이는 것을 볼 수 있습니다.

13. 한 번 *feed* 를 활성화하면, 그 뒤 새로운 image 를 배포하는 건 꽤 간단해집니다. 내 어플리케이션을 약간 수정해서 빌드한 다음 아래 커맨드를 실행해보고 보드에서 어떻게 되는지 지켜봅니다.

    `azsphere component publish --feedid <feed-GUID> --imagepath <file-path>`

## 더 보기
- [Azure Sphere OS networking requirements](https://docs.microsoft.com/en-us/azure-sphere/network/ports-protocols-domains)
- [Deloyment basics](https://docs.microsoft.com/en-us/azure-sphere/deployment/deployment-concepts)
- [Link the device to a feed](https://docs.microsoft.com/en-us/azure-sphere/deployment/link-to-feed)
- [Set up a device group for OS evaluation](https://docs.microsoft.com/en-us/azure-sphere/deployment/set-up-evaluation-device-group)