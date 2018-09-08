set-cookie in rails controller

### Generating cookie that using login with token 

#### And Readings cookie in request



1. 토큰을 사용하는 로그인에 이용되는 쿠키 생성

```ruby
# 쿠키 생성해서 response 에 저장

def show
  ...
  response.set_cookie( 
    '#cookie_name', { 
      value: token,
      expires: 24.hours.from_now,
      path: '/',
      secure: true, #HTTPS 프로토콜 상에서 암호화된(encrypted) 요청
      httponly: true, # Cross-site 스크립팅 (XSS) 공격을 방지하기 위해, HttpOnly쿠키는 JavaScript의 Document.cookie API에 접근할 수 없습니다
      domain: set_domain # cookie 가 저장될 도메인 주소
    }
  )
end

private

def set_domain
  case Rails.env
  when "Production"
    ".production 서버 도메인"
  when "staging"
    ".staging 서버 도메인"
  else
    "localhost"
  end
end
```



---



2. 요청에 있는 쿠키 읽기 

```ruby
# devise 사용시에 when using devise gem
# 먼저 cookie 를 읽고 없을 시에는 request.headers['Authorization'] 이용해서 로그인

def authenticate_user
  if request.cookies["cookie_name"].present?
    token = request.cookies["cookie_name"]
    begin
      jwt_payload = JWT.decode(JSON.parse(token)["jwt"], Rails.application.credentials.secret_key_base).first
      @current_user_id = jwt_payload['id']
      @current_user = User.find(@current_user_id)
    rescue JWT::ExpiredSignature, JWT::VerificationError, JWT::DecodeError
      head :unauthorized
    end
  elsif request.headers['Authorization'].present?
    authenticate_or_request_with_http_token do |token|
      begin
        jwt_payload = JWT.decode(token, Rails.application.credentials.secret_key_base).first
        @current_user_id = jwt_payload['id']
        @current_user = User.find(@current_user_id)
      rescue JWT::ExpiredSignature, JWT::VerificationError, JWT::DecodeError
        head :unauthorized
      end
    end
  end
end
```

