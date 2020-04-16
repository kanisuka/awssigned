---
title: "시나리오 소개"
weight: 10
pre: "<b>1. </b>"
tag:
  - Lambda
  - SignedUrl
---

---
본 실습은 S3 및 CloudFront Signed URL을 이용하여 S3에 파일 업로드 및 다운로드를 수행하도록 서비스를 구성하는 것이 목적입니다. 또한, 해당 작업들을 API Gateway, Lambda, DynamoDB 등과 같이 서버리스 아키텍처를 통해 구성하고, 개발 환경은 SAM (Serverless Architecture Model)을 IDE (e.g., Intellij, VS Code, Cloud 9 등)에 연동시켜서 작업을 진행합니다.
- [S3 Pre-signed URL을 통한 이미지 업로드 구현](upload/)
- [CloudFront Signed URL을 통한 다운로드 구현](download/)
- [CloudFront Lambda@Edge를 이용하여 실시간 썸네일 다운로드 구현](thumbnail/)
- [S3 Pre-signed URL을 통한 동영상 업로드 구현](streaming_upload/)
- [CloudFront Signed Cookies를 통한 HLS 스트리밍 구현](streaming_download/)
