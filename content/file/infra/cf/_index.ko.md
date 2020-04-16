---
title: "CloudFront 생성"
weight: 42
pre: "<b>4-1-2 </b>"

---

{{% notice info %}}
아래와 같이 총 2개의 CloudFront를 생성합니다.
{{% /notice %}}

- [파일 다운로드 용 CloudFront 생성](cf_file/)
- [이미지 썸네일 다운로드 용 CloudFront 생성](cf_thumb/)

## Origin access identity 생성
2개의 CloudFront를 생성하기 전, 공통적으로 적용되는 Origin access identity를 먼저 생성합니다.
- CloudFront에서 왼쪽 메뉴에 존재하는 Origin access identity 메뉴를 선택합니다.
- Create Origin Access identity 버튼을 클릭합니다.
- 팝업창에 Comment로 `access-identity-signed`를 입력하고, Create 버튼을 클릭합니다.

![cf_oai](/images/cf/cf_oai.png)
