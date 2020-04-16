---
title: "이미지 썸네일용 CloudFront 생성"
weight: 43
pre: "<b>4-1-3 </b>"

---

CloudFront 메뉴로 진입하여 Create Distribution 버튼을 클릭합니다.
![cf_1](/images/cf/cf_1.png)

Web 메뉴의 Get Started 버튼을 클릭합니다.
![cf_2](/images/cf/cf_2.png)

{{% notice info %}}
CloudFront의 Signed URL 기능을 사용하여 파일을 다운로드 받기 위해 아래와 같은 설정 작업을 진행합니다. CloudFront의 Signed URL 및 Signed Cookies 기능을 더 자세히 알고자 하기 위해서는 [AWS 공식 문서](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/PrivateContent.html)를 참조하세요.
{{% /notice %}}

아래와 같이 CloudFront 설정 작업을 진행합니다.
- Origin Domain Name은 기존에 생성한 signed-img-[my_id] 버킷을 선택합니다. ([my_id]는 임의의 고유한 값을 입력합니다)
- Restrict Bucket Access는 Yes를 선택합니다.
- Origin Access Identity는 Use an Existing Identity를 선택합니다.
- Your Identities는 기존에 존재하는 access-identity-signed-img-[my_id].s3.amazon.com을 선택합니다.
- Grant Read Permission on Bucket은 No를 선택합니다.
- Object Caching은 Customize를 선택합니다.
- minimum TTL은 86500을 입력합니다.
- Restric Viewer Access는 Yes를 선택합니다.
- 마지막으로 제일 밑에 존재하는 Create Distribution 버튼을 클릭합니다.

현재 생성한 CloudFront에 추후 생성할 실시간 이미지 썸네일 생성 Lambda@Edge Function을 아래와 같이 적용할 예정입니다. 아래 설정은 추후 다시 설명 드릴 예정입니다.

![cf_8](/images/cf/cf_8.png)