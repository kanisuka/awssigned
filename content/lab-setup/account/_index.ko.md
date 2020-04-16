---
title: "계정 생성"
weight: 31
pre: "<b>3-1. </b>"
---

## AWS 계정
{{% notice warning %}}
이미 AWS 계정을 가지고 있다면 즉시 이 실습의 가이드를 따라 진행할 수 있으나, **계정이 없다면** 먼저 AWS 계정을 만들어야 합니다.
{{% /notice %}}
AWS 계정 생성 및 활성화 가이드는 다음 [링크](https://aws.amazon.com/ko/premiumsupport/knowledge-center/create-and-activate-aws-account/)를 참조하시기 바랍니다.  

{{% notice info %}}
실습은 **ap-northeast-2 (SEOUL) 리전**을 선택합니다. 
{{% /notice %}}

## IAM 사용자
AWS 계정을 생성했지만 직접 IAM 사용자를 생성하지 않은 경우, IAM 콘솔을 사용하여 IAM 사용자를 생성 할 수 있습니다. 다음 스텝에 따라 Administrator (관리자) 사용자를 생성합니다. 이미 관리자 사용자가 있다면, 다음 IAM 사용자 생성 작업을 건너 뜁니다. 

1.	AWS 계정 이메일 주소와 비밀번호를 사용하여 AWS 계정의 **Root** 사용자로 [IAM 콘솔](https://console.aws.amazon.com/iam/) 에 로그인 합니다.
1.	IAM 콘솔 왼쪽 메뉴 패널에서 **Users** (사용자)를 선택한 다음 **Add user** (사용자 추가)를 클릭합니다.
1.	**User name** (사용자 이름)은 `Administrator`로 입력합니다.
1.	**Programmatic access**와 **AWS Management Console access** 체크박스를 선택하고, **Custom password**를 선택한 다음 비빌번호를 입력합니다. 
1.	**Next: Permissions** (다음: 권한)을 클릭합니다.
![IAMPermission](/images/iam_user_01.png)
1.	**Attach existing policies directly** (기존 정책 직접 연결)를 선택하고 **AdministratorAccess** 정책에 체크박스를 선택하고 **Next: Tags** (다음: 태그)를 클릭합니다.
![IAMPolicy](/images/iam_user_02.png)
1.	**Next: Review** (다음: 검토)를 클릭합니다.
1.	Administrator 사용자에 AdministratorAccess 관리형 정책이 추가 된 것을 확인하고 **Create user** (사용자 만들기)를 클릭합니다.
1.	이제 **Root** 사용자를 **로그아웃**하고 새로 생성한 **Administrator** 사용자로 **로그인**을 합니다. 다음 URL을 사용하여 로그인 할 수 있습니다.
> `https://<your_aws_account_id>.signin.aws.amazon.com/console/`  
{{% notice warning %}}
<your_aws_account_id>는 본인 AWS 계정의 고유 ID를 입력합니다. Root 사용자로는 해당 실습을 진행할 때 에러가 발생할 수 있습니다. 반드시 admin 유저 계정으로 로그인하여 진행하세요.
{{% /notice %}}
