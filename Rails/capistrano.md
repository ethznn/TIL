# capistrano usage

4/1 update

capistrano 관련 gem 설치

```ruby
# Gemfile
gem "capistrano-rbenv"
gem "capistrano-rbenv-install"
gem "capistrano-bundler"
gem "capistrano-rails"
gem "capistrano3-puma"
gem "capistrano-safe-deploy-to"

후에 
bundle install
```

```bash
$ bundle install

$ bundle exec cap install

modify Capfile
modify config/deploy/production.rb
modify config/deploy.rb

$ bundle exec cap production deploy
```

