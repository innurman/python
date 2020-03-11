
1 [Django] User 모델 email을 기본으로 하기& 썸네일 추가하기

https://milooy.wordpress.com/2016/02/18/extend-django-user-model/

2  파이썬 프로젝트 블로그 웹

https://github.com/nachwon/Djangogirls_assignment

3 장고 배포시 고려해야 될 사항 정리

https://dailyheumsi.tistory.com/21 [하나씩 점을 찍어 나가며]

1. django 의 runserver는 배포용으로 사용될 수 없다.
django girls 로 django를 처음 입문했었다. 빠르게 블로그를 만들 수 있었기에 django는 그만큼 강력했고, 초반에 전반적인 파일구조들을 익히느라 조금 헷갈리고 복잡하긴 하지만, 그래도 한번 익혀놓으면 확장시키기에 매우 좋은 프레임워크였다. node.js 의 경우 mysql 연결을 따로 설정하여 query문을 작성해야 했기에, sql문도 잘 다뤄야 했는데, django는 객체와 db가 연동되어있고 다루기도 쉬워서, query문을 따로 신경쓰지 않아도 됐었다.

하지만, 슬프게도 이는 로컬에서 개발할 때 까지만 편한 것이었다. 위의 모든 장점을 가진다 치더라도, 배포시에는 서버를 실행시키는manage.py runserver 를 사용할 수 없다. 정확히 말해, 사용할 수는 있으나, 누구도 그렇게 사용하지않고, 심지어 공식 django 홈페이지에서 이를 권장하고 있지 않다. 그럼 어떻게 서버를 실행시켜야하는걸까? gunicorn 이나 uwsgi 를 이용해서 django 서버를 실행시켜야 한다. 그런데 이는 또 django 와는 다른 영역이다. 사용하고 익히는 것 쯤이야 금방할 수 있겠지만, 제대로 알려고하면 또 뭐가 많다... 배포라는 것이 django만으로는 안된다는 것이다.

 

2. Web server와 Web Application Server의 차이를 안다.
위의 내용의 연장선으로, 우리는 django만으로 안된다는 것을 알았다. 그리고 gunicorn 이나 uwsgi 를 이용해서 서버를 실행시켜야 하는 것을 알았다. 그런데 이게 끝이 아니다. 클라이언트로부터 들어온 http request를 앞단에서 '적절히' 처리해주고 이를 django 서버로 보내주는 것이 필요하다. 왜 이렇게 하는지는 아래에서 다시 적겠다.

django server 와 같이, 프로그램 혹은 앱 단위의 로직을 처리하는 서버를 WAS(Web application server) 라고 하고, 앞단에 http request 를 처리하는 서버를 Web server 라고 한다. 내가 사용한 Web server는 nginx 다.

위에서 '적절히' 처리해주어야 한다고 했는데, 그렇다면 '적절히'는 무엇일까. http 연결과 관련된 기본적인 것들은 어느정도 설정이 되어있지만, 나의 '의도' 가 들어가는 '적절함' 이 반영되려면 내가 무언가를 수정하거나 덧붙여야한다. 즉, 나는 Web server로 들어오는 어떤 요청들은 나의 django 서버로 보내고 싶은데, 이를 위해서 nginx 라는 Web server를 일부 수정해야 한다는 것이다.

Web server 설정까지 와버렸다. nginx 에 대해서도 또 알아야 한다.

WAS와 WEB SERVER 차이 : http://jeong-pro.tistory.com/84

 
3. django의 collectstatic 에 대해서 이해한다.
다시 django로 돌아간다. 공식 django 홈페이지에, 배포시에는 manage.py collectstatic 을 써서 이미지나 css 등을 따로 처리하라고 나와있다. 이게 뭐하는 짓이고, 안해도 그냥 잘 돌아가는데 왜 해야만 하는건지 몰랐었다.

사실 해야만 하는 것은 아니다. 안해도 잘 된다. 하지만 '성능' 이라는 이유로 이를 하는 것을 권장하고 있다. 사실상 다 한다는 말이다.

manage.py collectstatic 을 해주면, django 내에 있는 모든 정적인 파일을 한 디렉토리에 이쁘게 모아준다. 이를 왜 하느냐? 이 정적인 파일들에 대한 요청이 들어올 때, django (WAS)가 이를 처리하는 것이 아니라, nginx (Web server) 가 이를 처리한다. 이렇게 하려고 collectstatic을 하는 것이다.

그럼 이런 짓을 왜하는가? django 가 이를 처리하면, 요청이 올 때마다 매번 django 디렉토리 내에서 해당 파일을 찾아야하고, 이에 대한 시간이 들어간다. 즉 django 서버에 부담이 된다는 것이다. 여러 요청이 동시에 많이 들어오면, 서버처리가 느려질 것이다. 그런데, 정적인 파일들의 위치는 항상 같아서, 이를 매번 검색하는 것은 매우 비효율적이다. 그래서 이를 위해 미리 정적인 파일들만 따로 모아두는 폴더를 만들고, 이를 django가 아닌 nginx 에서 처리하게 하는 것이다. nginx 에서 처리해야 하니, nginx 설정파일을 원하는 대로 수정해야 한다.

이러한 설정들을 위해 django settings.py 내 STATIC_URL , STATIC_ROOT, STATICFILES_DIR 에 대해서 제대로 이해하고 있어야 한다.

DJANGO 정적 파일 기능 이해하기 : https://blog.hannal.com/2015/04/start_with_django_webframework_06/

 
4. 배포 시, 설정 파일에 비밀 값들은 따로 분리해야 한다.
보통은 대게 git과 github를 개발하며 버전관리용도로 사용할 것이다. 특히 로컬에서 작업하다가 배포 서버로 올릴 때, 로컬에서 github repio에 업로드하고, 배포 서버에서 git clone 을 받아와서 작업할 것인데, 이때 로컬에서 github에 아무 생각없이 그대로 올리면 문제가 생긴다. 바로, 오픈되어서는 안되는 값들이 그대로 github에 노출되는 경우다. django의 경우 settings.py에 있는 SECRET_KEY 가 그렇다.

이러한 값들은 따로 다른 파일을 만들어 그 안에 저장하고, 해당 파일을 .gitignore로 추가해서 git 에 추가가 안되게 해야한다. 한편, 파일을 분리했으니, settings.py 에서 해당 파일을 import 해서 변수형태로 읽어와야 한다.

나의 경우, secret_config.py 에 SECRET_KEY를 따로 저장하였고, settings.py 에서 import secret_config 를 통해서 읽어왔다. 그러면 settings.py 는 다음과 같이 수정된다.

    from . import secret_config
    ...
    SECRET_KEY = secret_config.SECRET_KEY
    ...
 
5. 배포하려면 생각보다 해야할게 많다.
말 그대로다. 로컬에서 작업하는 것과는 다르게, 생각보다 해야할게 많다. 순차적으로 생각해보면,

로컬에서 django 웹사이트를 완성하고 서버에 업로드 한다.

aws ec2의 경우, SSH 연결, 보안그룹, elastic IP 설정을 해준다.
gunicorn 혹은 uwsgi 를 이용하여 배포 서버에서의 django 실행 환경을 만든다.

manage.py runserver 가 아니라 guicorn이나 uwsgi 를 이용하여 django 서버를 실행시켜야 한다.

80 port 로 요청이 들어와도 (일반적인 브라우저 http 요청) 8000번에서 실행되고 있는 django 서버로 요청을 라우팅 하기위해 nginx 설정파일을 수정해야 한다.

정적인 파일들에 대한 처리를 해준다.

manage.py collectstatic 으로 django 프로젝트 내 정적파일을 한군데 모아준다.

nginx 설정파일을 수정하여, 정적파일 요청 시, 정적파일이 모인 곳으로 라우트 해준다.

도메인 연결 작업을 하여 IP가 아닌 도메인으로 접속할 수 있게 한다.

whois 와 같은 도메인 업체에서 원하는 도메인을 구매한다.

aws ec2의 경우 route 53 을 이용하여, 내 서버와 도메인을 연결한다.

한번 이렇게 고생해봤으니, 다음에는 좀 더 수월하게 할거란 생각이 든다...

그 외에도 알게 된 점을 간략히 쓴다면,

써드파티 플러그인 사용법

summer note (Text editor)

el_pagination (포스팅된 글 페이지 네비게이션)

미디어 파일 추가, 삭제 깔끔하게 하는 법.

(일반적으로 사용하는 추가, 삭제 시 DB에는 삭제가 되지만, 서버 내 미디어 디렉토리에는 파일이 남아있다. 따라서 깔끔하게 다 지우려면, 파일 처리 작업도 해줘야함.)

django로 배포할 때, 참고하면 좋은 글, 시리즈 : https://nachwon.github.io/django-deploy-1-aws/

