마크다운 작성법 참조: https://gist.github.com/ihoneymon/652be052a0727ad59601

# fabsite 만들기
빠르게 원하는 웹사이트 만들기

## 목차
1. 구름 IDE 환경설정
2. 
3. 





## 구름IDE 환경설정
1. 파이썬 스택으로 Ubuntu 18.04 LTS 환경의 컨테이너이며, 그 안의 가상환경을 만들어 실행한다.
2. source myvenv/bin/activate 가상환경안의 패키지들이 간소화된다.
3. pip install django 장고와 관련된 패키지 설치한다.
4. django-admin startproject config . 장고는 프로젝트 단위로 어플리케이션을 만든다.
    그 안의 앱들을 만들어 기능을 추가해 나간다. 현 위치에 설정파일을 생성하여 첫 프로젝트를 만든다.
5. python mangage.py migrate 데이터베이스에 내용을 추가한다.


