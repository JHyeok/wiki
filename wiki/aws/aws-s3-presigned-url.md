# AWS S3 Presigned URL (미리 서명된 URL)

서버에서 AWS SDK를 사용해서 미리 서명된 URL을 만들 수 있다.
이 URL을 아는 모든 사용자는 URL을 이용하여 Amazon S3에 객체를 업로드 할 수 있다.

#### createPresignedPost와 getSignedUrl의 차이

SSECustomerKey, ACL, Expires, ContentLength 또는 Tagging과 같은 특정 매개변수는 요청을 보낼 때 헤더로 제공되어야 한다.
미리 서명된 URL을 사용하여 브라우저에서 업로드하고 이러한 필드를 사용해야 하는 경우 `createPresignedPost()`를 사용하라고 한다.

[Module @aws-sdk/s3-presigned-post](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/modules/_aws_sdk_s3_presigned_post.html#import) 를 참고해서 URL을 만드는 것은 어렵지 않다.

`createPresignedPost()`를 사용하면 아래처럼 응답이 온다.

```json
"url": "https://s3.ap-northeast-2.amazonaws.com/testkjh",
"fields": {
    "acl": "public-read",
    "bucket": "....",
    "X-Amz-Algorithm": "AWS4-HMAC-SHA256",
    "X-Amz-Credential": "....",
    "X-Amz-Date": "20220830T014030Z",
    "key": "...",
    "Policy": "eyJleHBpcm...",
    "X-Amz-Signature": "...."
}
```

위의 구조에 맞게 내려온다. POST 요청에 필요한 URL과 필드들을 내려준다.

URL을 만들고 해당 URL로 POST 요청을 보내서 직접 이미지를 업로드 테스트를 하면서 자잘한 오류를 만났고 그 해결법을 정리한다.

오류와 해결법

#### Unsupported Authorization Type

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Error>
    <Code>InvalidArgument</Code>
    <Message>Unsupported Authorization Type</Message>
    <ArgumentName>Authorization</ArgumentName>
    <ArgumentValue>Bearer L5JDxAgNg</ArgumentValue>
    <RequestId>TNP39RHY7RC1AFQT</RequestId>
    <HostId>CmwuiGUYxuKWtLxmZlksrpPiyJ+syU54C8r6lZlROTaTtQRBsB/RRH42f33ZiBrakzhn//6d3I4=</HostId>
</Error>
```

기존에 서버에 요청하던 인증 토큰을 사용해서 문제였다. 포스트맨에서는 No Auth를 해서 인증 토큰을 사용하지 않고 보내니 해당 오류를 해결할 수 있다.

#### Bucket POST must contain a field named 'key'

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Error>
    <Code>InvalidArgument</Code>
    <Message>Bucket POST must contain a field named 'key'.  If it is specified, please check the order of the fields.</Message>
    <ArgumentName>key</ArgumentName>
    <ArgumentValue></ArgumentValue>
    <RequestId>RWX4YP02RZ4CB82S</RequestId>
    <HostId>3WRiUv1qQkGOoTZNk8tUMMih+4wu08pDN5hilpB+j1hwk1Jk8MjUXhKX3uGM1F0SdIWuwQ+/D4M=</HostId>
</Error>
```

FormData에 `file` Key를 끝에 위치하게한다. 예를 들어 Presigned URL에 있는 필드들을 FormData에 넣고 그 다음에 위치하게 하면 된다.

[예시 코드](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/modules/_aws_sdk_s3_presigned_post.html#post-file-using-formdata-in-nodejs)에서도 가장 마지막에 file을 append한다.

```js
const { createReadStream } = require("fs");
const FormData = require("form-data");

const form = new FormData();
Object.entries(fields).forEach(([field, value]) => {
  form.append(field, value);
});
form.append("file", createReadStream("path/to/a/file"));
form.submit(url, (err, res) => {
  //handle the response
});
```

---
#### 참고

https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getSignedUrl-property

https://stackoverflow.com/questions/31366986/uploading-to-amazon-s3-via-ng-file-upload-error-400

https://stackoverflow.com/questions/15234496/upload-directly-to-amazon-s3-using-ajax-returning-error-bucket-post-must-contai

