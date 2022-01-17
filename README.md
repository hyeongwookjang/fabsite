마크다운 작성법 참조: https://gist.github.com/ihoneymon/652be052a0727ad59601
# fabsite 만들기
빠르게 원하는 웹사이트 만들기

## 전체 리스트
1. 구름 IDE 환경설정
2. 백엔드
3. 프론트엔드 
4. 데이터
5. 기타


아니 이건 리드미가 아닌데?

## 구름IDE 환경설정
1. 파이썬 스택으로 Ubuntu 18.04 LTS 환경의 컨테이너이며, 그 안의 가상환경을 만들어 실행한다.
2. source myvenv/bin/activate 가상환경안의 패키지들이 간소화된다.
3. pip install django 장고와 관련된 패키지 설치한다.
4. django-admin startproject config . 장고는 프로젝트 단위로 어플리케이션을 만든다.
    그 안의 앱들을 만들어 기능을 추가해 나간다. 현 위치에 설정파일을 생성하여 첫 프로젝트를 만든다.
5. python mangage.py migrate 데이터베이스에 내용을 추가한다.

## 백엔드
### 백엔드 리스트
1. accounts
2. static
3. post
4.
5. 

### accounts
---------- 1 base --------------------
python manage.py startapp accounts
어카운츠 앱 생성

config/settings.py에서 IISTALLED_APPS =[] 에 'django.contrib.sites'와 
'accounts' 를 추가
    TEMPLATES의 'DIRS': []안에 'os.path.join(BASE_DIR, 'config', 'templates')'
    입력하면 새로 루뜨를 잡아준다.
    마지막에 SITE_ID = 1 코드를 추가한다.
    
config폴더안에 templates 폴더를 만들어준다.

python manage.py runserver 0:80
하면 migrate하라는 창이뜬다

python manage.py migrate
python manage.py runserver 0:80
실행된다.
프로젝트의 url로 들어가면 서버가 열리는데 주소 끝에 /admin 을 붙이면 로그인창이 생성된다.

config/urls.py 파일을 보면 패턴에 경로를 보면 '/admin'이 있다.
/accounts의 경로를 추가해주고 include기능도 추가한다.

accounts/urls.py 를 하나 만들어주고 아래 #코드#를 기입한다.
#
from django.urls import path

app_name = 'accounts'

urlpatterns = [
    path('signup'/, signup, name='signup'),
    path('login/', login_check, name='login),
]
#

작성하고 함수형 뷰를 만들어야한다.
같은 폴더 views.py에 위치하고 있다.
이 파일에 아래 #코드#를 기입한다
#
def signup(request):
    return render(request, 'accounts/signup.html')


def login_check(request):
    return render(request, 'accounts/login.html')
#

accounts/urls.py에
from .views import * 를 넣어
views파일 urls파일에 불러오게 한다.

뼈대가 되는 HTML파일을 작성해보자
config/templates 안에 layout.html 을 만든다.

html:5 + 탭 하면 자동완성 -> 기본설정>모드>emmet 으로 설정해두었다.

파일을
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    {% block head %}
    {% endblock %}
    
    
    
    
    
</head>
<body>
    {% block contents %}
    {% endblock %}




    {% block js %}
    {% endblock %}


    
</body>
</html>
로 수정한다.

accounts에 templates 폴더를 만들고 그안에 accounts/layout.html 을 만든다
layout.html에 '{% extends 'layout.html' %}'를 기입한다.
account/templates에 login.html과 signup.html을 추가한다.
아래와 같이 #코드#를 기입한다.
#
{% extends 'layout.html' %}

{% block head %}
{% endblck %}

{% block head %}
login # 로그인아니면 signup
{% endblck %}

{% block js %}
{% endblcok %}

#

앱을 새로생성하고 url을 작성했기 떄문에 404가 나오는 것이다.


---------- 1 base --------------------

$데이터 베이스를 설계해보자$

pip install django-allauth
인증관련 패키지 설치
pip install django-imagekit
pip install pillow
이미지 업로드 관련 패키지 설치

python manage.py runserver 0:80
서버를 실행해두고 오류가 나면 어떤 내용인지 확인하자.

facebookclone/accounts/templates/models.py #아래코드#를 기입한다.
#
from django.conf import settings
from imagekit.models import ProcessedImageField
from imagekit.porcessors import ResizeToFill
import re


def user_path(instance, filename):
    from random import choice
    import string
    arr = [choice(string.ascii_letters) for _ in range(8)]
    pid = ''.join(arr)
    extension = filename.split('.')[-1]
    return 'accounts/{}/{}.{}'.format(instance.user.username, pid, extension)


class Profile(models.Model):
    user = models.OneToOneField(settings.AUTH_USER_MODEL, 
                                on_delete=models.CASCADE)
    nickname = models.CharField('별명', max_length=30, unique=True)
    picture = ProcessedImageField(upload_to=user_path,
                                    processors=[ResizeToFill(150,150)],
                                    format='JPEG'
                                    options={'quality': 99},
                                    blank=True,
                       )
    
    about = models.CharField(max_length=300, blank=True)
    
    GENDER_C = (
        ('선택사항', '선택안함'),
        ('여성', '여성')
        ('남성', '남성'),
    )
    
    gender = models.CharField('성별(선택사항)',
                                max_length=10,
                                choices = gender_C,
                                default='N')
    #
    
python manage.py makemigrations
마이그레이션 공간을 만든다
python manage.py migrate
옮긴다
