---
title: "이미지 API 테스트"
weight: 40
pre: "<b>4-3-1. </b>"

---

{{% notice info %}}
API 테스트는 [POSTMAN](https://www.postman.com/downloads/) 툴을 이용하여 진행합니다.
{{% /notice %}}

아래와 같이 포스트맨을 통해 File 업로드 Pre-Signed URL을 발급받을 수 있는 REST API를 호출합니다. 해당 REST API를 호출하면, 파일을 업로드할 수 있는 Pre-Signed URL이 HTTP 응답으로 전달됩니다.

## S3 Upload Pre-Signed URL 발급
API Gateway EndPoint는 SAM Template에 의해 생성된 API Gateway Endpoint를 의미합니다. 예를 들어, ***jsatf3u5x5.execute-api.ap-northeast-2.amazonaws.com/Prod*** 와 같은 값이 될 수 있습니다.

```
Method : GET
URL : https://[API Gateway EndPoint]/nfile/object?uid=uid001&sid=sid001&name=myimage
```
![postman_upload_1](/images/postman/pm_img_upload_1.png)

## 바이너리 파일 업로드
앞 서 발급받은 S3 Upload Pre-Signed URL(body 안에 url 값)을 복사해서 아래와 같이 주소창에 붙여 넣습니다. 그리고 Body 탭을 클릭한 후, binary 버튼을 클릭하여 업로드할 이미지 파일을 하나 첨부합니다. 

```
Method : PUT
URL : https://signed-img-myid.s3.ap-northeast-2.amazonaws.com/sid001/uid001/f24ed80c-c163-4e1e-8965-d88928375236?X-Amz-Security-Token=...
```
![postman_upload_2](/images/postman/pm_img_upload_2.png)

## 만료된 URL을 통한 이미지 파일 업로드 
발급 받은 S3 Upload Pre-Signed URL을 람다 함수에서 설정한 만료 시간 이후로 업로드 시도할 경우, 아래와 같이 Access Denied (Request has expired) 에러가 응답으로 내려오게 됩니다.
![postman_upload_fail](/images/postman/pm_img_upload_fail.png)

## 이미지 파일 다운로드
S3 Upload Pre-Signed URL을 발급받기 위해 호출한 API의 응답으로부터 objectid 값(e.g., f24ed80c-c163-4e1e-8965-d88928375216)을 가져와서 아래와 같이 설정합니다.
```
Method : GET
URL : https://[API Gateway EndPoint]/nfile/object/[objectid]?uid=uid001&sid=sid001
```
![postman_download](/images/postman/pm_img_download.png)

## 이미지 썸네일 다운로드
![postman_thumnail](/images/postman/pm_img_thumb.png)