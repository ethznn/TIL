# aws-sdk-ruby example

### Using aws-sdk in ruby

* #### github page : [aws-sdk-ruby](https://github.com/aws/aws-sdk-ruby)

* #### aws documents : [sdk-for-ruby/v3](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/index.html)



### Example) my case

* #### s3 file upload / read 

* #### sqs massege queue send / receive



1. Add gem file

   ```ruby
   # Gemfile
   ...
   gem 'aws-sdk', '~> 3'
   
   # or
   gem 'aws-sdk-s3', '~> 1'
   gem 'aws-sdk-ec2', '~> 1'
   ...
   
   # $ bundle install
   ```

2. Add aws access key

   ```bash
   $ EDITOR="vi" bin/rails credentials:edit 
   
   ...
   aws:
    access_key_id: AWS_ACCESS_KEY_ID
    secret_access_key: AWS_SECRET_ACCESS_KEY
   ...
   
   ESC :wq # Save in credentials.yml.enc3
   ```

3. Add configure file for aws-sdk

   ```ruby
   # config/initializers/aws-sdk.rb
   
   Aws.config.update(
     credentials: Aws::Credentials.new(Rails.application.credentials.aws[:access_key_id], Rails.application.credentials.aws[:secret_access_key]),
     region: 'region_id',
   )
   ```

4. Add codes in controller 

   ```ruby
   # app/controller/examples_controller.rb
   # For convenience, prefix( in S3 ) is called a folder_name.
   
   class ExamplesController < ApplicationController
   
     def show
       filename = created_file(parameters) # return file (is called filename)
       
       File.open("public/#{file_name}.json","w") do |file|
         file.write(JSON.pretty_generate(created_file))
       end 
   		
       aws_s3_resourse = Aws::S3::Resource.new
       bucket = aws_s3_resourse.bucket('bucket_name')
       new_object = bucket.object("#{folder_name}/#{file_name}.json")
       File.open("public/#{file_name}.json", 'rb') do |file|
         new_object.put(body: file, content_type: "application/json")
       end
   
       File.delete("public/#{file_name}.json") if File.exist?("public/#{file_name}.json")
   
       aws_sqs_client = Aws::SQS::Client.new
       aws_sqs_client.send_message_batch({ # Send message queue to request
         queue_url: "request_queue_url",
         entries: [
           {
             id: 'custom_id',
             message_body: "#{file_name}"
           }
         ]
       })
       
       begin
         Timeout.timeout(30) do
           loop do 
             receive_message_result = aws_sqs_client.receive_message({ # Read message queue in response
               queue_url: "reponse_queue_url", 
               message_attribute_names: ["All"], # Receive all custom attributes.
               max_number_of_messages: 1, # Receive at most one message.
               wait_time_seconds: 0 # Do not wait to check for the message.
             })
             @file_name = nil
             receive_message_result.messages.each do |message|
               if message[:body] == file_name
                 @file_name = file_name
   
                 aws_sqs_client.delete_message({ # Delete message queue in response
                   queue_url: "reponse_queue_url",
                   receipt_handle: message.receipt_handle    
                 })
   
                 @response = aws_s3_resourse.client.get_object(bucket:'bucket_name', key:"#{folder_name}/#{file_name}.json")
               end
             end
             break if @file_name
           end
         end
       rescue Timeout::Error
         return render json: { errors: 'Internal Server Error', status: 504, exception: "#<Timeout::Error: execution expired>" }
       end
       
       return render :json => @response.body.read
     end
     
   end
   ```