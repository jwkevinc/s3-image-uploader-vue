# Front-end (vue.js) s3 image uploader component

![Image description](https://www.imgur.com/N3twHAL.jpg)


A simple uploader with onSuccess event emitter on successsful image upload.
The component requires a working back-end that returns a signed s3 upload url.

Below are the bucket policy and cors configuration that I have used for testing purposes.
You should not use these settings for production. It is recommended that you use your domain for cors origin.

### Bucket Policy
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AddPerm",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::signed-url-serverless/*"
        }
    ]
}
```

### Cors Configuration
```
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>*</AllowedOrigin>
    <AllowedOrigin>http://localhost:8081</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <AllowedMethod>POST</AllowedMethod>
    <MaxAgeSeconds>3000</MaxAgeSeconds>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```


