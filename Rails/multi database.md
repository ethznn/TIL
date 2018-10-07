# multiple database usage

config/initializers/db_service.rb

```ruby
DB_SERVICE = YAML::load(ERB.new(File.read(Rails.root.join("config","database_service.yml"))).result)[Rails.env]
```

config/database_service.yml

```yaml
default: &default
  adapter: postgis
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  port: 5432
  password:

development:
  <<: *default
  database: service_development

test:
  <<: *default
  database: service_test

staging:
  <<: *default
  database: service_staging
  pool: 25
  host: <%= Rails.application.credentials.staging_service_database_host %>
  password: <%= Rails.application.credentials.staging_database_password %>

# TODO: Create service database of production
# production:
#   <<: *default
#   database: service_production

```

app/models/service_record.rb

```ruby
class ServiceRecord < ActiveRecord::Base
  self.abstract_class = true
  establish_connection DB_SERVICE
end
```

app/models/users.rb

```ruby
class User < ServiceRecord
end
```

lib/tasks/db_service.rake

```ruby
task spec: ["service:db:test:prepare"]

namespace :service do
  namespace :db do |ns|
    task :drop do
      Rake::Task["db:drop"].invoke
    end

    task :create do
      Rake::Task["db:create"].invoke
    end

    task :setup do
      Rake::Task["db:setup"].invoke
    end

    task :migrate do
      Rake::Task["db:migrate"].invoke
    end

    task :rollback do
      Rake::Task["db:rollback"].invoke
    end

    task :seed do
      Rake::Task["db:seed"].invoke
    end

    task :version do
      Rake::Task["db:version"].invoke
    end

    namespace :schema do
      task :load do
        Rake::Task["db:schema:load"].invoke
      end

      task :dump do
        Rake::Task["db:schema:dump"].invoke
      end
    end

    namespace :test do
      task :prepare do
        Rake::Task["db:test:prepare"].invoke
      end
    end

    # append and prepend proper tasks to all the tasks defined here above
    ns.tasks.each do |task|
      task.enhance ["service:set_custom_config"] do
        Rake::Task["service:revert_to_original_config"].invoke
      end
    end
  end

  task :set_custom_config do
    # save current vars
    @original_config = {
      env_schema: "db/schema.rb",
      config: Rails.application.config.dup
    }

    # set config variables for custom database
    ENV['SCHEMA'] = "db_service/schema.rb"
    Rails.application.config.paths['db'] = ["db_service"]
    Rails.application.config.paths['db/migrate'] = ["db_service/migrate"]
    Rails.application.config.paths['db/seeds.rb'] = ["db_service/seeds.rb"]
    Rails.application.config.paths['config/database'] = ["config/database_service.yml"]
  end

  task :revert_to_original_config do
    # reset config variables to original values
    ENV['SCHEMA'] = @original_config[:env_schema]
    Rails.application.config = @original_config[:config]
  end
end
```



실행 execute

```bash
# ex) rails db:migrate 
rails service:db:migrate
```

