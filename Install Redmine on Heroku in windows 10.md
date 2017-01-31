# Install Ruby in windows 10
Download Ruby for windowns from <http://rubyinstaller.org/downloads>

We use Ruby 2.2.X (32bit) installers. These provide a stable language and a extensive list of packages (gems) that are compatible and updated.

Redmine don't support Jruby.

# Install DevKit
Download from <https://github.com/oneclick/rubyinstaller/wiki/Development-Kit>

Modify config.yml, add ruby's location, For example, "E:/ruby-i386-mingw32"

# Install bundler
    gem install bundler

# Install ImageMagick and rmagick
<https://www.imagemagick.org/script/download.php#windows>

Reference: [HowTo install rmagick gem on Windows](http://www.redmine.org/projects/redmine/wiki/HowTo_install_rmagick_gem_on_Windows)

The rmagick supports the last known version of Imagemagick is 6.9.1-3.

The version will change according to [Installing on Windows](https://github.com/rmagick/rmagick/wiki/Installing-on-Windows) from rmagick

Then run command:

    gem install rmagick -- '--with-opt-dir="C:\Program Files (x86)\ImageMagick-6.9.7-Q16"'


# Install Heroku CLI
<https://cli-assets.heroku.com/branches/stable/heroku-windows-amd64.tar.gz>

# Clone the newest Redmine for github
    $ git clone https://github.com/redmine/redmine.git -b 3.2-stable
    $ cd redmine
    $ git checkout -b master

# Edit .gitignore

~~~
/config/additional_environment.rb
- /config/configuration.yml
/config/database.yml
- /config/email.yml
/config/secrets.yml
- /config/initializers/session_store.rb
- /config/initializers/secret_token.rb
/coverage
/db/*.db
/db/*.sqlite3

/public/dispatch.*
- /public/plugin_assets/*
/public/themes/*

*.rbc

/.bundle
- /Gemfile.lock
- /Gemfile.local
~~~

# Edit Gemfile

~~~
-database_file = File.join(File.dirname(__FILE__), "config/database.yml")
-if File.exist?(database_file)
-  database_config = YAML::load(ERB.new(IO.read(database_file)).result)
-  adapters = database_config.values.map {|c| c['adapter']}.compact.uniq
-  if adapters.any?
-    adapters.each do |adapter|
-      case adapter
-      when 'mysql2'
-        gem "mysql2", "~> 0.3.11", :platforms => [:mri, :mingw, :x64_mingw]
-        gem "activerecord-jdbcmysql-adapter", :platforms => :jruby
-      when 'mysql'
-        gem "activerecord-jdbcmysql-adapter", :platforms => :jruby
-      when /postgresql/
-        gem "pg", "~> 0.18.1", :platforms => [:mri, :mingw, :x64_mingw]
-        gem "activerecord-jdbcpostgresql-adapter", :platforms => :jruby
-      when /sqlite3/
-        gem "sqlite3", :platforms => [:mri, :mingw, :x64_mingw]
-        gem "jdbc-sqlite3", ">= 3.8.10.1", :platforms => :jruby
-        gem "activerecord-jdbcsqlite3-adapter", :platforms => :jruby
-      when /sqlserver/
-        gem "tiny_tds", "~> 0.6.2", :platforms => [:mri, :mingw, :x64_mingw]
-        gem "activerecord-sqlserver-adapter", :platforms => [:mri, :mingw, :x64_mingw]
-      else
-        warn("Unknown database adapter `#{adapter}` found in config/database.yml, use Gemfile.local to load your own database gems")
-      end
-    end
-  else
-    warn("No adapter found in config/database.yml, please configure it first")
-  end
-else
-  warn("Please configure your config/database.yml first")
-end


+group :development, :test do
+  gem 'sqlite3'
+end

+group :production do
+  gem 'pg'
+  gem 'rails_12factor'
+  gem 'thin' # change this if you want to use other rack web server
+end
~~~

# Bundle it without development and test
    bundle install --without test development

# Edit config/application.rb

~~~
module RedmineApp
  class Application < Rails::Application
+   config.assets.initialize_on_precompile = false
    # Settings in config/environments/* take precedence over those specified here.
    # Application configuration should go into files in config/initializers
~~~

# Edit config/environment.rb

~~~
vendor_plugins_dir = File.join(Rails.root, "vendor", "plugins")
if Dir.glob(File.join(vendor_plugins_dir, "*")).any?
  $stderr.puts "Plugins in vendor/plugins (#{vendor_plugins_dir}) are no longer allowed. " +
    "Please, put your Redmine plugins in the `plugins` directory at the root of your " +
    "Redmine directory (#{File.join(Rails.root, "plugins")})"
- exit 1
+ # exit 1
end
~~~

# Generate secret token for redmine
    $ rake generate_secret_token


# Create heroku app and a empty database
    heroku create APP_NAME
    heroku addons:create heroku-postgresql

if you want to reset db, run the following command

    heroku pg:reset DB_NAME

# Deploy and Configure Redmine

~~~
$ git add -A
$ git commit -m "preparing for heroku" 
$ git push heroku master
~~~

~~~
heroku run rake db:migrate
heroku run rake redmine:load_default_data
~~~

# Run Redmine on Heroku
    herorku open

Create a user, then login with admin(admin), set the new created user to administration and active it, then change the password of admin user

# Reference
 - [Getting Started on Heroku with Ruby](https://devcenter.heroku.com/articles/getting-started-with-ruby#introduction)
 - [Installation Guide of Redmine](http://www.redmine.org/projects/redmine/wiki/RedmineInstall#Notes-on-Windows-installation)
 - [在 Heroku 上架 redmine , 並完成串 Slack + Mailgun + S3 的功能](http://sdlong.logdown.com/posts/711757-deploy-redmine-to-heroku)
 - [How to Install and Deploy Redmine 2.6.x on Heroku](http://andystu.github.io/blog/2015/02/16/how-to-install-and-deploy-redmine-on-heroku/)
