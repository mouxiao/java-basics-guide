## 问题1
调用授权地址 http://127.0.0.1:8088/oauth/authorize?client_id=client&response_type=code&redirect_uri=http://www.baidu.com 返回错误信息

OAuth Error
error="invalid_grant", error_description="Invalid redirect: http://www.baidu.com does not match one of the registered values: [https://www.baidu.com/]"

解决方法:
申请授权地址中 redirect_uri=http://www.baidu.com,必须要和代码里面设置的一致(redirectUris("https://www.baidu.com/");)

## 问题2

OAuth Error
error="invalid_request", error_description="At least one redirect_uri must be registered with the client."

解决方法:
申请的回调地址必须要在服务端设置