## Presigned URL이란?

- 임사 권한으로써 말 그대로 **s3 접근을 위한 임시 URL**을 의미한다.
- 객체 소유자는 필요에 따라 자신의 보안 자격 증명을 사용하여 일정 기간 동안 객체 접근을 허가하는 presigned url을 만들어 다른 사용자와 객체를 공유할 수 있다.
- 일정 시간이 지나 만료되면 public 접근이 불가능하다.
- 파일에 접근 가능한 임시 URL을 생성하고, 지정한 사람만 공유 가능하다.
- URL의 만료 시간을 설정할 수 있으며, 파일을 받거나 업로드도 가능하다.

## flask boto3을 이용한 presigned url 발급

```jsx
get_url = s3_bucket.s3.generate_presigned_url('get_object', Params = { 'Bucket': bucket_name, 'Key': f'result/{image_name}.{image_type}' }, ExpiresIn = expire_time)
```

## 참고 자료

- [https://inpa.tistory.com/entry/AWS-📚-S3-Pre-signed-URL-공유하기](https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-S3-Pre-signed-URL-%EA%B3%B5%EC%9C%A0%ED%95%98%EA%B8%B0)
- https://gksdudrb922.tistory.com/223
- https://python.plainenglish.io/upload-files-to-aws-s3-using-pre-signed-urls-in-python-d3c2fcab1b41
- [https://docs.oofbird.me/python/boto3/s3/presigned-urls.html#파일업로드를-위한-사전인증-url-생성하기](https://docs.oofbird.me/python/boto3/s3/presigned-urls.html#%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%A5%E1%86%B8%E1%84%85%E1%85%A9%E1%84%83%E1%85%B3%E1%84%85%E1%85%B3%E1%86%AF-%E1%84%8B%E1%85%B1%E1%84%92%E1%85%A1%E1%86%AB-%E1%84%89%E1%85%A1%E1%84%8C%E1%85%A5%E1%86%AB%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%8C%E1%85%B3%E1%86%BC-url-%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5)
- https://medium.com/@serhattsnmz/securing-file-upload-download-with-using-aws-s3-bucket-presigned-urls-and-python-flask-a5c372436f6