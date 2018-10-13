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

### Global configuration
```ruby
RailsAdmin.config do |config|

  ### Popular gems integration

  ## == Cancan ==
  # config.authorize_with :cancan

  ## == Pundit ==
  # config.authorize_with :pundit

  ## == PaperTrail ==
  # config.audit_with :paper_trail, 'User', 'PaperTrail::Version' # PaperTrail >= 3.0.0

  ### More at https://github.com/sferik/rails_admin/wiki/Base-configuration

  ## == Gravatar integration ==
  ## To disable Gravatar integration in Navigation Bar set to false
  # config.show_gravatar = true
  
  # 리스트에 필요한 모델만 들고 오기
  config.included_models = ["model1", "model2"]

  # admin 페이지를 들어갈 떄 필요한 인증
  config.authorize_with do
    redirect_to "/" unless current_user.policies.map{|index| index.name}.include? "admin"
  end

  # config.model UserLog do
  #   list do
  #     field :name
  #     field :created_at
  #   end
  # end

  #config.current_user_method(&:current_user)

  # 필요한 액션만 사용할 수 있음
  config.actions do
    dashboard                     # mandatory
    index                         # mandatory
    new
    export
    bulk_delete
    show
    edit
    delete
    show_in_app

    ## With an audit adapter, you can add:
    history_index
    history_show
  end
end
```

### model config
```ruby
def example_function
  logs.where(name: "small_housing").count
end

rails_admin do
  # 리스트에 들어가는 부분 config
  configure :example_function do
    visible false # so it's not on new/edit 
  end

  # 모델 클릭시 보여줄 모델의 column
  list do
    field :id
    field :email
    field :organization
    field :example_function
  end

  show do
    field :id
    field :email
    field :organization
    field :example_function
  end
end
```