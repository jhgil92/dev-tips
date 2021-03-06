# AWS의 APIGateway-Lambda를 통해 Slack으로 알림 보내기

## 이걸 왜?

- Server의 물리적 상황은 AWS에서 감지 가능하지만, Application 수준에서의 이상 상황은 AWS에서 감지 불가(최근 새로 나온 L7 로드 밸런서인 Application LoadBalancer로는 감지 가능)
- 따라서 Application 수준에서의 이상 상황을 감지하는 로직을 만들고, 그 로직에서 AWS의 APIGateway-Lambda를 통해 Slack으로 알림을 발송하는 기능은 의미가 있다.

## 큰 흐름

1. AWS에서 날라온 알림 메시지를 Slack이 받을 수 있도록 Slack에 Incoming WebHook을 만든다.
1. Application에서의 이상 상황 발생 시 요청을 보낼 대상인 API Gateway를 만든다.
1. Slack의 WebHook으로 메시지를 보내는 Lambda를 만든다.
1. API Gateway의 특정 endpoint에 앞에서 만든 Lambda를 연동한다.

## Slack WebHook 생성

[api.slack.com/incoming-webhooks](https://api.slack.com/incoming-webhooks) 에서 Slack에 메시지를 보낼 수 있는, Slack 입장에서는 메시지를 받을 수 있는 Incoming webhooks를 생성한다.

대략 아래와 같은 설정을 통해 쉽게 만들 수 있으며, 사실 결국 중요한 것은 WebHook URL 이다.

1. Slack Incoming WebHook 페이지 접속
    ![Imgur](http://i.imgur.com/XaDRyIz.png)   
1. Add Incoming WebHook
    ![Imgur](http://i.imgur.com/EvL2mMh.png)    
1. 중요한 건 나중에 AWS Lambda가 수신자 주소로 사용할 이 WebHook URL. 자동으로 생성된다.
    ![Imgur](http://i.imgur.com/mUWplBV.png)    
1. 기타 여러가지 설정 사항이 있다.
    ![Imgur](http://i.imgur.com/fWd9Oe6.png)    
1. Save settings를 눌러서 Incoming WebHook 생성(정확하게는 2번 단계에서 생성이 되고 3번 이하는 수정이다.)
    ![Imgur](http://i.imgur.com/fj9MHgQ.png)



## API Gateway 생성

### IAM Role 생성

먼저 API Gateway를 사용할 수 있는 IAM Role을 생성한다.

![Imgur](http://i.imgur.com/2VkBvNx.png)

![Imgur](http://i.imgur.com/yby2Peo.png)

![Imgur](http://i.imgur.com/GXhotsR.png)

![Imgur](http://i.imgur.com/UwCBvYf.png)

![Imgur](http://i.imgur.com/J91pbgf.png)

![Imgur](http://i.imgur.com/8BxNO30.png)

### API Gateway 생성


----
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="크리에이티브 커먼즈 라이선스" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a>

<a href='https://www.facebook.com/hanmomhanda' target='_blank'>HomoEfficio</a>가 작성한 이 저작물은

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">크리에이티브 커먼즈 저작자표시-비영리-동일조건변경허락 4.0 국제 라이선스</a>에 따라 이용할 수 있습니다.





## Lambda 생성




## API Gateway - Lambda 연동
