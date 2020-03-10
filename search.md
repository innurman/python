

          [ urls.py ]

          path('search', views.search, name='search'),

          [ search.html ]

          <form action="search">
              <input autocomplete="off" name="value" type="text" placeholder="검색어를 입력하세요.">
          </form>

          [ views.py ]
          from django.db.models import Q

          ...

          def search(request):
              # name=value인 값을 가져옴 값이 없으면 ''으로 대체함
              value = request.GET.get('value', '')

              # 템플릿에 랜더링 될 변수를 모아놓을 딕셔너리 자료형
              render_args = {
                  'value' : value,
              }

              # value의 값이 존재하는 경우
              if not value == '':
                  # 포스트를 최신순으로 가져옴
                  posts = Post.objects.order_by('created_date').reverse()
                  # 제목에 value가 포함되거나 작성자가 value인 포스트만 걸러냄
                  posts = posts.filter(Q(title__icontains=value) | Q(author__username=value))
                  render_args['posts'] = posts
              return render(request, 'search.html', render_args)
