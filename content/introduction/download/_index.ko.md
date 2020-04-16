---
title: "이미지 파일 다운로드"
weight: 2
pre: "<b>1-2 </b>"
---

---

## CloudFront Download Signed URL 발급

S3에 업로드한 파일을 다운받기 위해서는 다양한 방법들이 존재합니다. S3에서 제공하는 Pre-signed URL을 통해서도 다운로드를 받을 수 있지만, AWS에서 제공하는 CDN 서비스인 CloudFront를 통해 고속으로 파일을 다운로드 받는 방법에 대해서 알아봅니다. 

![ArchDownloadSignedURL](/images/architecture/arch_download_signed.png)

위 과정은 아래와 같은 순서로 진행됩니다.
1. 클라이언트는 API Gateway에서 제공하는 REST API 호출
2. API Gateway는 해당 요청을 Download URL Gen Lambda Function으로 전달 및 CloudFront Download Signed URL 생성

이렇게 생성된 Signed URL은 클라이언트에게 전달되고, 클라이언트는 아래 과정을 통해 다운로드 작업을 수행합니다.

---

## CloudFront로부터 이미지 파일을 다운로드받기

앞서 발급받은 CloudFront Download Signed URL을 호출하면, 해당 요청을 받은 CloudFront는 S3로부터 요청한 바이너리 파일을 받아와서 캐싱 정책에 따라 CloudFront에 캐싱을 한 뒤에 클라이언트에게 해당 이미지 파일을 전달합니다.

![ArchDownloadCF](/images/architecture/arch_download_cf.png)
