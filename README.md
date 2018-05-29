# Sharetribe Docker Image
Sharetribe Dockerfile to build docker images on docker hub

***Note:*** 
 - This image doesn't include any database so you will need to config an external database or install MySQL server yourself.
 - We are continuing to improve the image to make it easier to install, so please create issues/pull requests if you see anything can be improved
 - Feature versions for AWS EC2, Elastic Beanstalk

## Usage

**Step 1:** Create docker image from the preconfigured sharetribe docker image
```
docker pull lthh89vt/sharetribe
```

**Step 2:** Config database

```
# Copy database config file
cp config/database.example.yml config/database.yml 
# Update database server configuration in this file
vi config/database.yml
```

**Step 3:** Run sharetribe

```
source /etc/profile.d/rvm.sh

# Generate rake secret key
rake secret

# Update config file with the new key
vi config/config.yml
# Add the following
# production:
#  secret_key_base: # add here the generated key

# Prepare environment
RAILS_ENV=production bundle exec rake db:create RAILS_ENV=production bundle exec rake db:structure:load RAILS_ENV=production bundle exec rake ts:index RAILS_ENV=production bundle exec rake ts:start RAILS_ENV=production NODE_ENV=production bundle exec rake assets:precompile

# Invoke the delayed job worker
RAILS_ENV=production bundle exec rake jobs:work

# Run production
bundle exec rails server -e production
```

