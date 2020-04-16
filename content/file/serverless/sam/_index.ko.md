---
title: "SAM 프로젝트 생성"
weight: 40
pre: "<b>4-2-1 </b>"

---

{{% notice info %}}
기본적으로 API Gateway, Lambda Function, Dynamo DB 등과 같은 자원은 SAM template을 통해서 자동으로 생성할 수 있습니다. 본 실습은 기 구현되어 있는 소스를 GIT으로부터 받아서 진행합니다.
{{% /notice %}}

Intellij IDE 툴을 통해서, 아래 git 주소에 있는 소스를 다운로드 받습니다.
```
[git url]
```

다운로드 받은 SAM 프로젝트에서 S3Manager 파일을 선택합니다. 그리고, 아래 부분을 생성된 S3 버킷 및 CloudFront 도메인 값을 설정합니다.

{{% notice warning %}}
KEY_PAIR_ID와 PRIVATE_KEY는 root 계정으로 들어가서 생성합니다. 생성 방법은 [AWS 공식 문서](https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/private-content-trusted-signers.html#private-content-creating-cloudfront-key-pairs)를 참조하세요.
{{% /notice %}}

![setting_1](/images/tool/setting_1.png)


Intellij IDE에서 template.yaml 파일을 선택하고, 마우스 오른쪽 클릭을 해서 Deploy Serverless Application을 선택합니다. 선택하면 아래와 같은 화면이 나타납니다.
- Create Stack에 `signed-template`을 넣어줍니다.
- S3 Bucket에서 Create 버튼을 클릭하여 `aws-sam-signed-[특정 ID 값]`으로 S3 Bucket을 생성합니다.
- 하단에 Deploy 버튼을 클릭하여 실행합니다.
![sam_execution_1](/images/tool/sam_execute_1.png)

Deploy 버튼을 클릭하면 아래와 같이 빌드 과정을 거치게 됩니다.
![sam_execute_2](/images/tool/sam_execute_2.png)

빌드 과정이 완료되면 AWS CloudFormation을 사용하여 AWS 서버리스 환경을 자동으로 구축하게 됩니다.
![sam_execute_3](/images/tool/sam_execute_3.png)

CloudFormation이 정상적으로 동작 완료되면, AWS 서버리스 환경 구축이 완성되어 있을 것을 보실 수 있습니다.

