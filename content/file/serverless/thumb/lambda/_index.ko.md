---
title: "람다 생성"
weight: 42
pre: "<b>4-2-2-2 </b>"

---

{{% notice info %}}
본 실습은 Virginia (us-east-1) 리전에서 Cloud9 IDE 툴을 사용하여 진행합니다.
{{% /notice %}}

화면에 보이는 Create Lambda Function... 링크를 클릭합니다.
![c9_5](/images/tool/c9_5.png)

Function Name 및 Application Name 모두 `ImageResize`로 입력합니다.
![c9_6](/images/tool/c9_6.png)

Node.js를 선택하고, empty-nodejs를 선택합니다. 그리고 Next 버튼을 클릭합니다.
![c9_7](/images/tool/c9_7.png)

Function trigger는 none으로 설정하고, Next 버튼을 클릭합니다.
![c9_8](/images/tool/c9_8.png)

Next 버튼을 클릭합니다.
![c9_9](/images/tool/c9_9.png)

Finish 버튼을 클릭합니다.
![c9_10](/images/tool/c9_10.png)


아래 코드를 복사해서 index.js 파일 내용과 교체해줍니다.
```
'use strict';

const querystring = require('querystring'); // Don't install.
const AWS = require('aws-sdk'); // Don't install.

// http://sharp.pixelplumbing.com/en/stable/api-resize/
const Sharp = require('sharp');

const S3 = new AWS.S3({
  signatureVersion: 'v4',
  region: 'ap-northeast-2'  // 버킷을 생성한 리전 입력
});

const BUCKET = 'signed-img-[my_id]' // Input your bucket

// Image types that can be handled by Sharp
const supportImageTypes = ['jpg', 'jpeg', 'png', 'gif', 'webp', 'svg', 'tiff'];

exports.handler = async(event, context, callback) => {
  const { request, response } = event.Records[0].cf;
  
  const { uri } = request;
  const ObjectKey = decodeURIComponent(uri).substring(1);
  const params = querystring.parse(request.querystring);

  /**
   * ex) https://dilgv5hokpawv.cloudfront.net/dev/myimage.jpg
   * - ObjectKey: 'dev/myimage.jpg'
   */
  
  // 크기 조절이 없는 경우 원본 반환.
  // indicate width, height, format and quality.
  const w = '100'
  const h = '100'
  const q = '50'
  const f = 'jpg'
  
  if (!(w || h)) {
    return callback(null, response);
  }

  const extension = 'jpg'
  const width = parseInt(w, 10) || null;
  const height = parseInt(h, 10) || null;
  const quality = parseInt(q, 10) || 100; // Sharp는 이미지 포맷에 따라서 품질(quality)의 기본값이 다릅니다.
  let format = (f || extension).toLowerCase();
  let s3Object;
  let resizedImage;

  // 포맷 변환이 없는 GIF 포맷 요청은 원본 반환.
  if (extension === 'gif' && !f) {
    return callback(null, response);
  }

  // Init format.
  format = format === 'jpg' ? 'jpeg' : format;

  if (!supportImageTypes.some(type => type === extension )) {
    responseHandler(
      403,
      'Forbidden',
      'Unsupported image type', [{
        key: 'Content-Type',
        value: 'text/plain'
      }],
    );

    return callback(null, response);
  }


  // Verify For AWS CloudWatch.
  console.log(`parmas: ${JSON.stringify(params)}`); // Cannot convert object to primitive value.\
  console.log('S3 Object key:', ObjectKey)

  try {
    s3Object = await S3.getObject({
      Bucket: BUCKET,
      Key: ObjectKey
    }).promise();

    console.log('S3 Object:', s3Object);
  }
  catch (error) {
    responseHandler(
      404,
      'Not Found',
      'The image does not exist.', [{ key: 'Content-Type', value: 'text/plain' }],
    );
    return callback(null, response);
  }

  try {
    resizedImage = await Sharp(s3Object.Body)
      .resize(width, height)
      .toFormat(format, {
        quality
      })
      .toBuffer();
  }
  catch (error) {
    responseHandler(
      500,
      'Internal Server Error',
      'Fail to resize image.', [{
        key: 'Content-Type',
        value: 'text/plain'
      }],
    );
    return callback(null, response);
  }
  
  // 응답 이미지 용량이 1MB 이상일 경우 원본 반환.
  if (Buffer.byteLength(resizedImage, 'base64') >= 1048576) {
    return callback(null, response);
  }

  responseHandler(
    200,
    'OK',
    resizedImage.toString('base64'), [{
      key: 'Content-Type',
      value: `image/${format}`
    }],
    'base64'
  );

  /**
   * @summary response 객체 수정을 위한 wrapping 함수
   */
  function responseHandler(status, statusDescription, body, contentHeader, bodyEncoding) {
    response.status = status;
    response.statusDescription = statusDescription;
    response.body = body;
    response.headers['content-type'] = contentHeader;
    if (bodyEncoding) {
      response.bodyEncoding = bodyEncoding;
    }
  }
  
  console.log('Success resizing image');

  return callback(null, response);
};
```
### sharp 패키지 설치
cloud9 콘솔에서 아래와 같이 명령어를 입력합니다.
- cd ImageResize
- npm init -y
- npm i sharp
![c9_11](/images/tool/c9_11.png)

### Lambda@Edge Role 변경
- Lambda 함수에서 Permissions 선택
- Execution Role에서 Role Name 리소스 클릭
- Trust relationships에서 Edit trust relationship 선택
- 아래 권한으로 교체
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
        "Effect": "Allow",
        "Principal": {
            "Service": [
                "lambda.amazonaws.com",
                "edgelambda.amazonaws.com"
            ]
        },
        "Action": "sts:AssumeRole"
        }
    ]
}
```
- Update trust policy 클릭
- Permmisions Tab으로 이동
- Attach Policies -> Create Policy -> JSON 선택
- 아래 Policy Copy & Paste
- Review Policy 클릭
- Name에는 `LambdaExecutionImageResizeRole` 입력
- Create policy 버튼 클릭
- Lambda 실행할 Role로 돌아와서, 위 Policy 연결

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "iam:CreateServiceLinkedRole",
                "lambda:GetFunction",
                "lambda:EnableReplication",
                "cloudfront:UpdateDistribution",
                "s3:GetObject",
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:DescribeLogStreams"
            ],
            "Resource": "*"
        }
    ]
}
```

### Lambda@Edge 함수 배포 (to CloudFront)
- AWS Console에서 Lambda -> Application -> cloud9-ImageResize 선택
- Resource에서 ImageResize 선택
- Actions 버튼을 클릭하고 Deploy to Lambda@Edge 선택
- Distribution은 image thumbnail용 CloudFront ARN 입력
- CloudFront Event는 Origin Response를 선택
- Deploy 버튼 클릭
