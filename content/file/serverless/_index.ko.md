---
title: "서버리스 어플리케이션 구축"
weight: 42
pre: "<b>4-2 </b>"

---

{{% notice info %}}
기본적으로 API Gateway, Lambda Function, Dynamo DB 등과 같은 자원은 SAM template을 통해서 자동으로 생성할 수 있습니다. SAM 프로젝트를 통해 기본적은 서버리스 환경을 구축합니다. 추가적으로 실시간 이미지 썸네일 생성을 위한 Lamdbda@Edge 함수와 동영상 스트리밍 파일 생성을 위한 Lambda 함수도 구현하여 AWS 환경에 배포합니다. 
{{% /notice %}}

- SAM 프로젝트 생성
- 실시간 이미지 썸네일 Lambda@Edge 함수 생성
- 동영상 스트리밍 파일 변환을 위한 Lambda 함수 생성

