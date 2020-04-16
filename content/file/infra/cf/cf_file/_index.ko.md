---
title: "파일 다운로드용 CloudFront 생성"
weight: 40
pre: "<b>4-1-2-1 </b>"

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
- Grant Read Permission on Bucket은 Yes를 선택합니다.
- Restric Viewer Access는 Yes를 선택합니다.
- 마지막으로 제일 밑에 존재하는 Create Distribution 버튼을 클릭합니다.

![cf_3](/images/cf/cf_3.png)

다음으로는 생성된 CloudFront에 비디오 관련 S3 버킷을 Origin으로 설정하는 작업을 진행하도록 합니다.
- 왼쪽 메뉴 목록 중, Distributions를 클릭합니다.
- 생성된 CloudFront의 ID를 클릭하여 설정 화면으로 진입합니다.
- Origins and origin Groups 메뉴를 클릭합니다.
- Create Origin 버튼을 클릭합니다.
- Origin Domain Name은 기존에 생성한 signed-video-[my_id] 버킷을 선택합니다. ([my_id]는 임의의 고유한 값을 입력합니다)
- Restric Viewer Access는 Yes를 선택합니다.
- Origin Access Identity는 Use an Existing Identity를 선택합니다.
- 마지막으로 제일 밑에 존재하는 Create 버튼을 클릭합니다.

![cf_4](/images/cf/cf_4.png)

업로드되는 동영상 파일로부터 HLS 스트리밍 포맷과 동영상 썸네일 파일이 추가로 생성됩니다. 이렇게 생성된 파일들에 접근하는 경로는 따로 구분해서 진행합니다.
- CloudFront의 Behaviors 탭을 선택합니다.
- Create Behavior 버튼을 클릭합니다.

![cf_5](/images/cf/cf_5.png)

아래와 같이 Path Pattern에 대한 설정 값들을 입력합니다.
- Path Pattern은 `input/*`을 입력합니다.
- Origin or Origin Group은 S3-signed-video-[my_id]를 선택합니다.
- Restric Viewer Access는 Yes를 선택합니다.
- 마지막으로 제일 밑에 존재하는 Create 버튼을 클릭합니다.

![cf_6](/images/cf/cf_6.png)
- CloudFront의 Behaviors 탭을 선택합니다.
- Create Behavior 버튼을 클릭합니다.
- Path Pattern은 `assets/*`을 입력합니다.
- Origin or Origin Group은 S3-signed-video-[my_id]를 선택합니다.
- Restric Viewer Access는 Yes를 선택합니다.
- 마지막으로 제일 밑에 존재하는 Create 버튼을 클릭합니다.

![cf_7](/images/cf/cf_7.png)