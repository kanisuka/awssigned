---
title: "썸네일 다운로드"
weight: 3
pre: "<b>1-3 </b>"
---

---

## CloudFront Thumbnail Download Signed URL 발급

기존 대부분의 방식들은 이미지 파일을 업로드할 때, 썸네일을 생성하도록 진행되었습니다. 그러나 이미지가 업로드할 때마다 썸네일을 강제로 생성하도록 하면, 생성된 썸네일 이미지가 사용자 (클라이언트)에 의해 사용되지 않을 경우에는 쓸데 없이 S3 스토리지 비용만 차지하는 경우가 발생될 수 있습니다.

업로드된 이미지에 대한 썸네일이 항상 사용되지 않고, 일부만 사용하는 경우에는 사용자 (클라이언트)로부터 썸네일 요청이 발생될 때만 실시간으로 생성해서 사용자에게 전달하도록 진행합니다. 더 나아가, 생성된 썸네일은 S3에 저장하지 않고, CloudFront에 일정 시간 동안 캐싱을 하도록 하여 S3 스토리지 비용을 더욱 줄이면서, 동시에 캐싱된 시간동안 썸네일 요청에 대해 빠르게 썸네일을 전달할 수 있습니다.

![ArchThumbnailSigned](/images/architecture/arch_thumb_signed.png)

위 과정은 아래와 같은 순서로 진행됩니다.
1. 클라이언트는 API Gateway에서 제공하는 REST API 호출
2. API Gateway는 해당 요청을 Thumbnail URL Gen Lambda Function으로 전달 및 CloudFront Signed URL 생성

이렇게 생성된 CloudFront Signed URL은 클라이언트에게 전달되고, 클라이언트는 아래 과정을 통해 썸네일 다운로드 작업을 수행합니다.

---

## CloudFront로부터 썸네일 다운로드 받기

앞서 발급받은 CloudFront Thumbnail Download Signed URL을 호출하면, 해당 요청을 받은 CloudFront는 S3로부터 원본 이미지 바이너리 파일을 받아와서 CloudFront Lambda@Edge Function을 통해서 원본 이미지 파일을 썸네일로 변환합니다. CloudFront는 이렇게 변환된 썸네일 파일을 캐싱 정책에 따라 캐싱을 하고, 클라이언트에게 생성된 썸네일 이미지 파일을 전달합니다.

한 가지 주의할 점은 썸네일을 요청하는 첫 번 째 호출에 대해서는 썸네일 생성 시간이 포함되기 때문에 느릴 수 있습니다. 그러나 두 번 째 호출부터는 생성된 썸네일 이미지 파일이 CloudFront에 캐싱되어 있기 때문에 빠르게 다운로드 받을 수 있습니다.

![ArchThumbnailCF](/images/architecture/arch_thumb_cf.png)
