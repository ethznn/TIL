# order to update table data

### 로컬에서

1. mysql database 에서 name_table 삭제

   ```
   dbeaver에서 해당 name_table 삭제
   ```

2. 모델삭제

   ```ruby
   model/name.rb
   class Name < ApplicationRecord
     내용 복사
   end

   db/migrate/datedata_name.rb
   class CteateName < ActiveRecord::Migration[5.1]
     def change
       create_table :name do |t|
   			내용 복사
       end
     end
   end

   rails d model name
   ```

3. 모델 생성(그대로)

   ```bash
   rails g model name

   복사한 내용 붙여넣기
   ```

4. 데이터 입히기

   ```ruby
   services/data_update_service.rb

   def create_data
     Rails.logger.level = 1 if Rails.env.development?
     사이 create_name 만 제외하고 나머지 주석
     Rails.logger.level = 0 if Rails.env.development?
   end

   def create_name
     items = []
     parcels_hash = parcel_ids
     columns = "column들 , 기준으로 나열".split(",").push("parcel_id")
     CSV.foreach("#{Rails.root}/tmp/data/seoul/seoul_building_owner.txt", encoding: "bom|utf-8", col_sep: ",", headers: true) do |row|
       parcel_id = parcels_hash[row[0]]
       items << row.fields.push(parcel_id)
     end

     Name.import(columns, items, validate: false)
   end

   완료 후 
   bash 창에서 rails data:create 하면 데이터 입혀짐.
     
   데이터 확인 후 
   git push 로 master branch 최신화
   ```

5. EC2로 데이터 올리기

   ```ruby
   config/deploy.rb 에서 코드 확인 후 맞는 명령어

   ex) 내 로컬 tmp/data/data.zip 파일을 EC2 같은 루트로 보낼 때,
   namespace :data do
     desc "Upload data file"
     task :upload do
       on roles(:app) do
         upload! "tmp/data/data.zip", "#{shared_path}/tmp/data/data.zip"
         within current_path do
           execute :unzip, "-o tmp/data/data.zip -d tmp/data"
         end
       end
     end
   end

   cap production deploy data upload
   ```

   ​

### 서버내에서 EC2

1. ssh 접속 후 RDS(mysql) 접속

   ```bash
   mysql -u root -p -h RDS_url
   password : 입력
   ```

2. Table 삭제

   ```sql
   USE RAILS_PROJECT_production;
   DROP table talbe_name;
   ```

3. (로컬에서 작업)서버 코드 최신화 및 deploy 

   ```bash
   cap production deploy 
   ```

4. 레일즈 디렉토리 이동 후 데이터 입히기

   ```ruby
   cd apps/RAILS_PROJECT/current
   vi app/services/data_update_service.rb 에서 로컬과 같이 똑같이 주석처리하고
   bin/rails data:create
   ```

   ​