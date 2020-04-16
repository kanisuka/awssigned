---
title: "동영상 파일 스트리밍"
weight: 5 
pre: "<b>1-5 </b>"
---

---

## CloudFront Signed Cookies 발급

앞에서 동영상 파일을 S3에 업로드하면서 트리거링된 Media Convert Lambda Function을 통해 생성된 스트리밍 파일들을, 인증된 호출을 통해 다운받아서 스트리밍 서비스를 제공하기 위해서 CloudFront에서 제공하는 Signed Cookies를 사용합니다. 

Streaming URL Gen Lambda Function은 스트리밍 파일을 다운로드 받을 수 있는 m3u8 파일 주소와 해당 파일 및 일련의 호출되는 파일들을 접근하여 다운로드 받을 수 있도록 Signed Cookies 값을 제공합니다.

![ArchStreamingSignedCookies](/images/architecture/arch_streaming_signed.png)

위 과정은 아래와 같은 순서로 진행됩니다.
1. 클라이언트는 API Gateway에서 제공하는 REST API 호출
2. API Gateway는 해당 요청을 Streaming URL Gen Lambda Function으로 전달 및 CloudFront Signed Cookies 값 및 Streaming 호출에 대한 Endpoint 파일 주소 생성

이렇게 생성된 Signed Cookies 값 및 Streaming 호출에 대한 Endpoint 파일 주소는 클라이언트에게 전달되고, 클라이언트는 아래 과정을 통해 스트리밍 파일을 다운로드 받을 수 있습니다.

---

## CloudFront로부터 스트리밍 파일을 다운로드받기

앞서 발급받은 Streaming Endpoint 주소를 호출할 때, 위 과정을 통해 발급 받은 CloudFront Signed Cookies 값을 HTTP 요청에 함께 설정해서 CloudFront로 전달합니다. CloudFront는 전달 받은 Signed Cookies 값이 Valid한 지 여부를 체크하고, 유효한 요청이면 Origin으로 설정되어 있는 S3로부터 다운로드 받아서 클라이언트에게 전달합니다.

클라이언트는 일반적으로 HLS 스트리밍을 제공해줄 수 있는 스트리밍 플레이어이고, 해당 스트리밍 플레이어는 첫 번 째 요청 이후로 일련된 HTTP 스트리밍 호출을 진행합니다. 이 때도 위 과정을 통해 발급은 Signed Cookies 값을 함께 전달 해주어야 합니다.

![ArchDownloadCF](/images/architecture/arch_streaming_cf.png)
