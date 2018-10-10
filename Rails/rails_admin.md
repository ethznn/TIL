### rails admin

[rails_admin](https://github.com/sferik/rails_admin) 잼이 엄청 유용하다.

```ruby
# Gemfile

gem 'rails_admin', '~> 1.3' 
```

```bash
$ bundle install
$ rails g rails_admin:install
```

그리고 default route /admin 으로 가면 프로젝트의 모든 데이터들을 관리할 수 있는 페이지로 접속이 가능하다.

데이터가 엄청 많은 서비스라면 페이지가 로딩되이 엄청 오래걸린다.

접속 인증이나 모델 리스트 뷰 configuration이 가능하지만 자유도가 높지는 않음.