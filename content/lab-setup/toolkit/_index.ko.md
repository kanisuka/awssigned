---
title: "개발 환경 설정"
weight: 32
pre: "<b>3-2. </b>"
---

## AWS Toolkit 설치
SAM을 이용하여 작업을 진행하기 위해서는 자신이 원하는 IDE Tool에 AWS Toolkit을 설치해서 코드 개발 후에 바로 배포할 수 있도록 진행합니다.

워크샵은 기본적으로 Intellij IDE를 사용해서 작업을 진행합니다. 설치 순서는 아래를 참조하세요.

1. 먼저, [AWS SAM CLI](https://aws.amazon.com/ko/blogs/korea/aws-toolkit-for-intellij-now-generally-available/)를 설치합니다.
2. 다음으로, [JetBrains Plugin Repository](https://plugins.jetbrains.com/plugin/11349-aws-toolkit)를 통해 [AWS Toolkit for Intellij를 설치](https://aws.amazon.com/ko/serverless/sam/)합니다. [Settings/Preferences] 대화상자에서 [Plugins]를 클릭하고 [Marketplace]를 선택한 다음 “AWS Toolkit”을 검색하고 [Install] 버튼을 클릭합니다. 그리고 변경 내용이 적용되도록 IDE를 다시 시작합니다.

![IntellijPref](/images/tool/intellij_1.png)
![IntellijAWS](/images/tool/intellij_2.png)

## Credential 정보 설정
SAM을 통해 배포 작업을 진행하기 위해서는 AWS 자격 증명(AWS Credential)정보가 로컬 AWS 구성 파일에 설정되어 있어야 합니다. 좀 더 자세한 설정 방법은 [AWS 개발을 위한 AWS 자격 증명 및 리전 설정 문서](https://docs.aws.amazon.com/ko_kr/sdk-for-java/v1/developer-guide/setup-credentials.html)에서 확인 가능합니다.

* 로컬 시스템의 다음 위치에 있는 AWS 자격 증명 프로필 파일에서 자격 증명을 설정합니다.
  * Linux, macOS, or Unix의 ~/.aws/credentials
  * Windows의 경우 C:\Users\USERNAME\.aws\credentials

이 파일에는 다음 형식의 행이 포함되어야 합니다.
```
[default]
aws_access_key_id = your_access_key_id
aws_secret_access_key = your_secret_access_key
```
your_access_key_id 및 your_secret_access_key 값을 자신의 고유 AWS 자격 증명 값으로 대체합니다. 본 실습에서는 이전에 생성한 Administrator IAM 사용자의 고유 AWS 자격 증명 값을 사용합니다.

Intellij에서는 이전에 설치한 AWS Toolkit에서 제공하는 메뉴를 통해서 손쉽게 구성이 가능합니다.

1. 왼쪽 메뉴 하단에 존재하는 AWS Explorer를 선택합니다.
2. AWS Explorer 창에서 Configure AWS Connection을 클릭합니다.
3. Seoul Region을 선택하고, Edit AWS Credential 메뉴를 선택하여, AWS Credential 정보 (Access Key, Secret Access Key)를 파일에 입력하고 저장합니다.

![IntellijConfig](/images/tool/intellij_config_1.png)
![IntellijRegion](/images/tool/intellij_config_2.png)
