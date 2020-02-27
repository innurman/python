# python



Part I. 파이썬 3.7 설치 (ubuntu 18.04 LTS)

    $ sudo apt update
    $ sudo apt install software-properties-common
    $ sudo add-apt-repository ppa:deadsnakes/ppa
    $ sudo apt update
    $ sudo apt install python3.7
    $ python3.7 --version

    $ nano ./.bash_aliases 
    alias python=python3.7
    alias pip=pip3

    $ source ./.bash_aliases

    $ sudo apt install python3-pip
    $ sudo apt install python3.7-venv

    $ mkdir workspace/
    $ cd workspace/
    $ mkdir django
    $ cd django
    $ python -m venv myvenv
    $ source myvenv/bin/activate
    $ pip --version
     pip 10.0.1 from /hom
    $ pip install django
    $ deactivate


Part II. 장고 웹서버 실행

    $ cd workspace/django
    $ source myvenv/bin/activate
    $ django-admin startproject start
    $ cd start
    $ python manage.py runserver 0.0.0.0:8000

Part III. 장고 웹서버 첫 프로젝트 생성

    $ python manage.py startapp hello
    $ nano hello/models.py
    
    from django.db import models
    # Create your models here.
    class Candidate(models.Model):
    	id = models.AutoField(primary_key=True)
    	title = models.CharField(max_length=20)
    	introduction = models.TextField()
    	created_at = models.DateTimeField(auto_now_add=True)

    	def __str__(self):
        	return self.title
        
    $ nano hello/admin.py    
    from django.contrib import admin
    from .models import Candidate #앞에서 만든 Post모델을 가져온다.
    
    # Register your models here. 
    admin.site.register(Candidate) #만든 모델을 관리자 페이지에서 볼려면!   
    
    $ nano hello/views.py
    
    from .models import Candidate #models에 정의된 Candidate를 불러온다
    
    def notice(request):
    	if (DEBUG): print("Method: ", request.method)
	candidates = Candidate.objects.all()[0:3]
	context = {'candidates':candidates}
    
	if request.method == "POST":
		if (DEBUG): print("userId: ", request.POST["email"])	
		return redirect('main')
	return render(request, 'notice.html', context)
    
    $ nano templates/notice.html
    {% for candidate in candidates %} 
    <div class="card">
        <div class="card-header" id="headingOne">
            <h5 class="mb-0">
            <button class="btn btn-link" type="button" data-toggle="collapse"
            data-target="#ab{{candidate.id}}" aria-expanded="false"
            aria-controls="collapseOne">
            {{candidate.title}} / {{candidate.created_at}}
            </button>
            </h5>
        </div>
        <div id='ab{{candidate.id}}' class="collapse" aria-labelledby="headingOne"
           data-parent="#accordion">
              <div class="card-body">
                  {{candidate.introduction}}
              </div>
         </div>
     </div>
     {% endfor %}
    
    $ python manage.py createsuperuser
    
    db 테이블 생성 후 최종 디비에 반영하기    
    $ python manage.py makemigrations
    $ python manage.py migrate

    http://sotolab.com:8000/admin/

Part IV. 배포

    I. package를 통한 nginx 설치
    package를 이용하여 nginx를 설치한다.

    $ sudo apt update
    $ sudo apt install nginx
    $ nginx -v
    nginx version: nginx/1.4.6 (Ubuntu)

    $ sudo service nginx start

    $ sudo service nginx status
     * nginx is running


    http://localhost/
    Welcome to nginx!
    If you see this page, the nginx web server is successfully

    II. Nginx Setting
    Django를 이용해서 서버에 요청을 보낼 수 있도록 Nginx 설정 파일을 생성한다.

    /etc/nginx/sites-available/sotolab

    server {
        listen 80;
        server_name sotolab;

        location / {
            proxy_pass http://localhost:8000;
        }
    }
    이 설정은 로컬 포트 8000으로 들어오는 모든 요청을 Django로 보내서 응답하도록 한다


    $ sudo ln -s /etc/nginx/sites-available/sotolab /etc/nginx/sites-enabled/sotolab
    $ ls -l /etc/nginx/sites-enabled


    $ sudo rm /etc/nginx/sites-enabled/default


    $ sudo service nginx reload

    $ cd workspace/django
    $ source myvenv/bin/activate
    $ cd start_django
    $ python manage.py runserver


    
    
    


