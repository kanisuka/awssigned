---
title: "이미지 파일 업로드"
weight: 1
pre: "<b>1-1 </b>"
---

---

## S3 Upload Pre-signed URL 발급

S3에 직접 업로드하기 위한 S3 Upload Pre-signed URL을 발급합니다. 해당 과정은 API Gateway와 Lambda Function을 통해서 REST API 형태로 발급 요청하고, 요청을 받은 Upload URL Generator Lambda Function이 S3 SDK를 통해서 S3 Upload용 Pre-signed URL을 생성합니다.

![ArchUploadPresignedURL](/images/architecture/arch_upload_signed.png)

위 과정은 아래와 같은 순서로 진행됩니다.
1. 클라이언트는 API Gateway에서 제공하는 REST API 호출
2. API Gateway는 해당 요청을 Upload URL Gen Lambda Function으로 전달 및 S3 Upload Pre-signed URL 생성
3. Pre-signed URL 생성 후, Dynamo DB에 업로드할 파일에 대한 메타데이터 저장 및 S3 Pre-signed URL을 클라이언트에게 전달

이렇게 생성된 S3 Pre-signed URL은 클라이언트에게 전달되고, 클라이언트는 아래 과정을 통해 업로드 작업을 수행합니다.

---

## S3에 이미지 파일을 업로드하기

앞서 발급받은 S3 Upload Pre-signed URL을 호출하면서, 이미지 파일을 HTTP Request Body에 포함시켜 업로드 작업을 수행합니다.

![ArchUpload](/images/architecture/arch_upload_s3.png)




