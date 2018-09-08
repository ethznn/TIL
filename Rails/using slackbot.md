# Using slackbot in Rails

레일즈에서 slackbot 사용하기

1. Gemfile 에 [slack-ruby-client](https://github.com/slack-ruby/slack-ruby-client) 추가
   Add [slack-ruby-client](https://github.com/slack-ruby/slack-ruby-client) to gemfile

   ```ruby
   # Gemfile
   ...
   # for Slackbot
   gem 'slack-ruby-client'
   ...
   ```

2. [Slack API Site](https://api.slack.com/)  에서 Bot 생성
   Creating bot in [Slack API Site](https://api.slack.com/) 

   ```bash
   Create APP 후 # setting config slack server in here
   left banner Features의 Bot User click 
   left banner Features의 OAuth & Permissions click 
   Copy Bot User OAuth Access Token and save in codes
   # 내 경우 rails credentials.yml.enc 에 저장
   EDITOR="vi" bin/rails credentials:edit
   slack_api_token = ************************ # after recording
   ESC :wq # save in credentials.yml.enc
   
   # You must invite slackbot created to Slack_channel_name
   ```

3. 사용할 레일즈 파일에 코드 추가
   Add codes to rails file in using

   ```ruby
   #for slackbot ex) 문의하기 / feedback
   Slack.configure do |config|
     config.token = Rails.application.credentials.slack_api_token
   end
   client = Slack::Web::Client.new
   client.chat_postMessage(
     channel: '#Slack_channel_name', 
     text: "• post_name: #{@post.id} \n• writer: #{@post.user_email.present? ? @post.user_email : 'non_email'}님 \n\n• post_content: \n  #{@post.contents} \n\n• date: #{@post.created_at.strftime("%F %T")}", 
     as_user: true 
   )
   ```