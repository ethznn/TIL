# about linux ubuntu

### package update

```bash
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt-get update
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev nodejs yarn

// 추가 설명
패키지 인덱스 인덱스 정보를 업데이트: apt-get은 인덱스를 가지고 있는데
이 인덱스는 /etc/apt/sources.list에 있습니다. 이곳에 저장된 저장소에서 사용할 패키지의 정보를 얻습니다. 
sudo apt-get update

설치된 패키지 업그래이드: 설치되어 있는 패키지를 모두 새버전으로 업그래이드 합니다.
sudo apt-get upgrade
```

### install ruby 2.5.0

```bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

rbenv install 2.5.0
rbenv global 2.5.0
ruby -v

gem install bundler

rbenv rehash
```

### git config

```bash
ssh-keygen -t rsa -b 4096

sudo cat /.ssh/id_rsa.pub
```

