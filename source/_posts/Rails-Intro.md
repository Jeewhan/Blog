---
layout: post
title: "레일즈 기초"
categories: Rails
tags: Rails
date: 2018-05-08
---

## Framework?

일반적인 의미에서 프레임워크란? 문제를 일반화하고 해결하기 위한 틀

프로그래밍에서의 프레임워크란, 설계와 재사용이 가능한 클래스를 제공

라이브러리와의 차이는 **제어역전** (Inversion of Control)

라이브러리는 사용자 코드에서 호출되어지나 프레임워크는 자신의 라이프 사이클을 직접 관리하며 그 흐름 속에 개발자가 원하는 부분에 사용자 코드를 추가하여 기능을 구현하는 형태

프레임워크의 장점은 생산성, 유지보수성, 일정한 퀄리티 보장

---

## MVC

![img](http://adrianmejia.com/images/rails_arch.png)

![img](https://cdn-images-1.medium.com/max/629/1*lFMcocBQ4zF-Q-_SvM8c7Q.jpeg)

Controller : 요청 데이터 처리, 비즈니스 로직 호출, 출력으로 전달

View : 최종적인 출력 생성

Model ( 순수한 데이터 원형 ) : 비즈니스 로직, 데이터 조작



Controller의 책임

1. Model 생성
2. Model의 옵져버
   - Model과 강하게 바인딩되지 않기 위해서 옵저버 패턴 차용
   - Model의 데이터를 가져와서 View를 만드는 것이 목적이므로
3. View 생성
4. View에게 모델을 전달
5. View의 액션을 처리



View의 책임

1. 모델에 기반하여 렌더링
2. 입력의 처리를 Controller에게 위임



MVC, MVP, MVVM 의 차이점은 MV의 관계

MVC에서는 View가 Model을 알고 있는 것

Controller는 연결에 대한 책임을 지며, 그림을 그리는 것은 전적으로 View의 책임



View에서 Model을 직접적으로 수신하지 않는 이유

- View는 전체 공정 중의 일부만 그리게 되어있으므로 Controller가 데이터에 대한 후처리, 컨트롤러의 추가 작업 등을 거친 뒤에 모델을 넘겨준다



MVC에서의 View는 Model과 Controller를 모두 다 알고 있음

- Model은 렌더링할 데이터
- Controller는 이벤트의 처리를 위임할 대상



Controller는 App(또는 Router)에 의해 만들어진다

Model을 상황에 따라서 공급해줄 필요가 있다면 Service도 등장



위와 같은 형태를 제왕적 Controller 모델이라고 부른다



문제점은 Controller에게 책임이 너무 많기 때문에 잘 만들기가 대단히 어렵고 그에 따라 문제들이 발생한다는 것이고, 규모가 커질수록 Controller의 책임을 나누기 위해 많은 추상층을 만들어내게 되고 점점 더 복잡해진다

---

## Rails의 교리, [The Rails Doctrine](https://rubyonrails.org/doctrine/)

1. [Optimize for programmer happiness](https://rubyonrails.org/doctrine/#optimize-for-programmer-happiness)
2. [Convention over Configuration](https://rubyonrails.org/doctrine/#convention-over-configuration)
3. [The menu is omakase](https://rubyonrails.org/doctrine/#omakase)
4. [No one paradigm](https://rubyonrails.org/doctrine/#no-one-paradigm)
5. [Exalt beautiful code](https://rubyonrails.org/doctrine/#beautiful-code)
6. [Provide sharp knives](https://rubyonrails.org/doctrine/#provide-sharp-knives)
7. [Value integrated systems](https://rubyonrails.org/doctrine/#integrated-systems)
8. [Progress over stability](https://rubyonrails.org/doctrine/#progress-over-stability)
9. [Push up a big tent](https://rubyonrails.org/doctrine/#big-tent)



[Optimize for programmer happiness](https://rubyonrails.org/doctrine/#optimize-for-programmer-happiness)

- 루비( `“Ruby is designed to make programmers happy.”` )가 그러하듯, Rails도 프로그래머의 행복을 추구한다는 의미인 듯

[Convention over Configuration](https://rubyonrails.org/doctrine/#convention-over-configuration)

- Rails 하면 떠올리는 가장 대표적인 원칙 두 가지 중 하나인 듯 하다 ( 나머지 하나는 `Don't Repeat Yourself`  )  이 프로덕트에서 변별적 차이가 존재하는 부분을 제외한, 전형적인 부분들은 특정한 컨벤션을 사용하는 것을 통해 생산성을 증대하는 것을 목표로 한다

[The menu is omakase](https://rubyonrails.org/doctrine/#omakase)

- 프레임워크를 선택하는 이유일 것, 어떤 일을 할 때 무슨 도구가 좋을지 선별할 수 있는 눈이 아직 없다면 프레임워크에서 제공하는 잘 정제된 도구를 활용하여 생산성을 낼 수 있다

[No one paradigm](https://rubyonrails.org/doctrine/#no-one-paradigm)

- 특정 패러다임에 대한 순수주의를 고집하기 보다는 적절한 조합을 통한 생산성을 추구하는 듯하다

[Exalt beautiful code](https://rubyonrails.org/doctrine/#beautiful-code)

- Ruby의 특성과 CoC 등을 활용한 짧으면서도 많은 기능을 해내는 코드를 추구하는 듯하다
- 인상적인 예시가 나오는데, `if people.include? person` 과 `if person.in? people` 가 있을 때, 초점(Subject)이 무엇이냐에 따라서 어느 코드가 좋은지가 다르다

[Provide sharp knives](https://rubyonrails.org/doctrine/#provide-sharp-knives)

- 안정성을 추구하기 위해 가능성을 닫아놓기 보다는 보다 많은 자유도를 제공하고자 하는 듯하다 ( 흔히 말하는 `Opinionated` 와는 상반된 입장 )

[Value integrated systems](https://rubyonrails.org/doctrine/#integrated-systems)

- MSA를 추구하는 요즘 트렌드와는 상반된 철학이지만, `"What’s worse is when systems are prematurely disintegrated and broken into services or, even worse, microservices."` 이것이 더 나쁘다고 판단하고, Monolithic 의 장점을 보다 높게 본, 그리고 그것이 가능하도록 Rails에서 지원하겠다는 의미로 읽힌다

[Progress over stability](https://rubyonrails.org/doctrine/#progress-over-stability)

- 사실 이제는 유행을 선도하는 주체라고 보기 어렵지만, 그럼에도 끊임없는 진보를 추구한다는 이 철학이 아직까지 Rails가 살아있도록 한 것 같다

[Push up a big tent](https://rubyonrails.org/doctrine/#big-tent)

- 초점은 Value integrated systems 등에 맞춰져 있을지라도 API와 같은 다양한 니즈에 대응하려는 노력도 병행하고 있는 듯 하다

---

## Rails의 구성 요소, [Ruby on Rails Architectural Design](http://adrianmejia.com/blog/2011/08/11/ruby-on-rails-architectural-design/)

![img](http://adrianmejia.com/images/dynamic_view.png)

![img](http://adrianmejia.com/images/ror_static_view.png)

- Action Pack : MVC의 VC
  - Action Dispatcher : 웹 브라우저 요청의 라우팅, 요청을 파싱하고 쿠키 핸들링, 세션, 요청 메소드 등 HTTP에 대한 처리를 수행
  - Action Controller : 모델과 뷰를 제어하기 위한 요청 처리, 상태 관리, 응답 생성 등을 담당, 또한 사용자 세션, 애플리케이션 흐름, 캐싱 기능, 헬퍼 모듈, 사전 필터, during, post processing hooks를 관리
  - Action View : Controller에 의해 호출되어 요청된 형식에 해당하는 응답을 생성, 마스터 레이아웃, 템플릭 룩업, 헬퍼 등 제공, 템플릿 스키마는 세 종류가 있는데 rhtml은 erb를 HTML View로 생성, rxml은 XML 문서 구성, rjs는 ajax 기능을 위해 Ruby 코드 내에서 동적인 자바스크립트 코드를 만들도록 함
- Active Model : Action Pack과 Active Record 모듈 간 인터페이스, 이름 규칙, 유효성 검사 등 모델과 관련된 규약을 정의
- Active Record
  - 아키텍처 패턴으로서, Rails에서는 Class로 ORM을 제공, Instance는 각각의 row를 표현, Class 네이밍, 주키와 외래키 설정 등은 Rails의 관례에 따른다, 비즈니스 로직에 포함되는 Model Class를 생성하고, 테이블과 매핑시키고, getter, setter과 life cycle에 따른 Callback를 제공한다
- Active Resource : Model Class와 RESTful 웹 서비스를 매핑할 수 있도록 해준다
- Active Support : Ruby 표준 라이브러리의 확장 ( 국제화, 타임존, 테스팅 등 )
- Action Mailer : Action Controller를 래핑해서 템플릿을 이용한 email 메시지를 만드는 방법 제공
- Railties : Rails의 코어로서 위 모듈들이 잘 조합될 수 있게 하며 코드 생성기를 제공


---

## Rails 프로젝트 파악 방법

- db/schema.rb 를 보면서 데이터베이스 구조 파악
- config/routes.rb를 보면서 요청 경로와 Controller 연결고리 파악
- Gemfile에서 사용하고 있는 라이브러리 파악

---

## VC

Controller : Action의 집합 (Controller Class Method)

routes에서 어느 주소이냐에 따라 어떤 Controller의 어느 Method에게 처리를 위임할지 정하게 됨

action에서 데이터를 처리하고 동일한 이름에 해당하는 View ( `action.html.erb` ) 에서 결과를 렌더링

## routes

```ruby
Rails.application.routes.draw do
  root 'home#index' # get + '/'이면 home Controller의 index action이 처리
  
  get '/attack', to: 'home#attack' # get 'home/attack' 이라고 적으면 home/attack 로 접속하면 home Controller의 attack action이 처리
  get '/defense', to: 'home#defense'
end
```



form tag에서 submit을 받으면 input tag의 이름에 따라 각각 params hash에 담겨 action에게 넘어간다

```ruby
def attack
  @from = params[:userA]
  @to = params[:userB]
end
```



## CRUD

기능 구현 순서 : Controller -> action -> routes -> action.html.erb

`rails generate controller home index new create`

home Controller ( + index, new, create action )

```ruby
# new.html.erb
<form action='/home/create' method='post'>
  <%= hidden_field_tag :authenticity_token, form_authenticity_token %>
  제목 : <input type='text' name='post_title'><br />
  내용 : <textarea name='post_content'></textarea><br />
  <input type='submit' value='제출'>
</form>
```

### Model

`rails g model post title:string content:text`

`db/migrate/20180411092430_create_posts.rb` 과 `models/post.rb`를 생성

rails db:migrate : migration file을 DB에 반영

rails db:drop : 테이블 삭제

```ruby
def create
  @post = Post.new
  @post.title = params[:post_title]
  @post.content = params[:post_content]
  @post.save
  
  redirect_to 'home/index'
end

def index
  @posts = Post.all
end

# index.html.erb
<% @posts.each do |x| %>
  제목 : <%= x.title %><br />
  내용 : <%= x.content %><br />
  ---------------------<br />
<% end %>

# destroy.html.erb
<% @posts.each do |x| %>
  제목 : <%= x.title %><br />
  내용 : <%= x.content %><br />
  <a href='/home/destroy/<%=x.id%>'>삭제</a><br />
  ---------------------<br />
  
# routes.rb
get `home/destroy/:post_id' => 'home#destroy'

def destroy
  @post = Post.find(params[:post_id])
  @post.destroy

  redirect_to 'home/index'
end
```

edit == 덮어쓰기

새 글을 쓰는 것이 빈 테이블을 만들고 내용을 채우는 것이라면, 수정은 채워진 테이블의 내용을 바꾸는 것

수정 페이지 == 글 쓰기 양식
- edit.html.erb == new.html.erb
- 단, 이전 내용 필요

id는 두 번 필요
- 수정하는 양식에 이전 내용을 불러올 때
- 이전 글을 수정한 뒤 새로운 내용으로 업데이트 할 때

```ruby
# index.html.erb, edit action으로 id 넘기기
<% @posts.each do |x| %>
  제목 : <%= x.title %><br />
  내용 : <%= x.content %><br />
  <a href='/home/destroy/<%=x.id%>'>삭제</a><br />
  <a href='/home/edit/<%=x.id%>'>수정</a><br />
  ---------------------<br />
  
# routes.rb
get `home/edit/:post_id' => 'home#edit'

def edit
  @post = Post.find(params[:post_id])
end

<form action='/home/update/<%=@post.id%>' method='post'>
  <%= hidden_field_tag :authenticity_token, form_authenticity_token %>
  제목 : <input type='text' name='post_title' value='%<=@post.title%>'><br />
  내용 : <textarea name='post_content'><%=@post.content%></textarea><br />
  <input type='submit' value='제출'>
</form>

post 'home/update/:post_id' => 'home#update'

def update
  post = Post.find(params[:post_id])
  post.title = params[:post_title]
  post.content = params[:post_content]
  post.save
  
  redirect_to '/home/index'
end
```

---

## View Helper

`<%= link_to '텍스트', URL %>`

`<a href='/posts/destroy/<%=post.id%>'>삭제</a>`

`<%= link_to '삭제', "/posts/destroy/#{post.id}" %>`

<%= 안에 <%= 가 또 들어갈 수는 없으므로 #{} 를 통해 manipulation

```ruby
# routes.rb
get '/posts/destroy/:post_id' => 'posts#destroy', as: 'post_destroy'
# ( => 대신에 `, to: `도 사용 가능)

<%= link_to '삭제', post_destroy_path post_id: p.id %>
# post_destroy_path 에게 넘겨줄 값이 하나 뿐이라면 아래와 같이 작성 가능
<%= link_to '삭제', post_destroy_path p.id %>

# a태그를 감싸고 싶을 경우, 위 아래 두 코드는 동일
<a href='/posts/destroy/<%=post.id%>`>
  <button>삭제</button>
</a>

<%= link_to post_destroy_path post.id do %>
  <button>삭제</button>
<% end %>
```