

# TODO clean up and re-test this whole thing 



# Deploy an app to aws elastic beanstalk

AWS Elastic Beanstalk offers a full service approach to application deployment.
It is similar to the Heroku model.  There is a pricing premium vs managing your own
EC2 servers and Load balancer, but the time savings is almost always worth it for
small teams in the POC or MVP stage.

# Prerequisites

- aws account - https://signup.heroku.com/
- python 2.7
- pip
- docker


# Pricing

https://aws.amazon.com/elasticbeanstalk/pricing/


# Pros

- free tier is available
- Good option for projects or teams that don't have a devops or CI/CD team.
- Once you are in AWS, you can take advantage of hundreds of other services.

# Cons

- Pricing is complex because EB uses many AWS services that have their own pricing model
- setup is "easy" by AWS standards, but still much more complicated than Hekoru, Netlify, or surge.sh
- EB uses many AWS services such as S3, EC2, and ElasticLoad Balancers.  If you plan to go to prod with EB,
        it would be best to understand these services or have access to support people who can help.


# How to


1) Install dependencies

        # install the elastic beanstalk cli
        # windows
        pip install awsebcli --upgrade --user

        # Macos
        brew install awsebcli

        # verify the install worked
        eb --version

2) Create AWS Security credentials

        a) Log in to AWS console
        b) navigate to `Services` -> `IAM` -> `Users` -> `<your user name>` -> `Security Credentials` tab
        c) Ensure you have an access key (and you know the secret key for it)
           otherwise
           Click the `Create Access Key` button to generate an access key, save this in a safe place.


3a) hello world example


        mkdir -p ~/projects/aws-eb-hello-world
        cd ~/projects/aws-eb-hello-world
        eb init -p PHP
        echo "Hello World" > index.html
        eb create dev-env
        eb open
        eb terminate
        rm -rf ~/projects/aws-eb-hello-world
        # manually clean up any eb S3 artifacts

This is the simplest possible example, feel free to continue for a more real world example.

3b) Real world example


4) install jhipster

        brew install jhipster

5) Create a spring boot project with jhipster

        mkdir -p ~/projects/aws-eb-jhipster
        cd ~/projects/aws-eb-jhipster

        # start jhipster
        jhipster

        # choose mostly default options:
        # ? Which *type* of application would you like to create? Microservice application
        # ? What is the base name of your application? jhipster
        # ? As you are running in a microservice architecture, on which port would like your server to run? It should be unique to avoid port # conflicts. 8081
        # ? What is your default Java package name? com.mycompany.myapp
        # ? Which service discovery server do you want to use? No service discovery
        # ? Which *type* of authentication would you like to use? JWT authentication (stateless, with a token)
        # ? Which *type* of database would you like to use? SQL (H2, MySQL, MariaDB, PostgreSQL, Oracle, MSSQL)
        # ? Which *production* database would you like to use? PostgreSQL
        # ? Which *development* database would you like to use? H2 with in-memory persistence
        # ? Do you want to use the Spring cache abstraction? No - Warning, when using an SQL database, this will disable the Hibernate 2nd level # cache!
        # ? Would you like to use Maven or Gradle for building the backend? Gradle
        # ? Which other technologies would you like to use?
        # ? Would you like to enable internationalization support? No
        # ? Besides JUnit and Jest, which testing frameworks would you like to use?
        # ? Would you like to install other generators from the JHipster Marketplace? No

        # verify jhipster created the project
        ls

6) Test new app. make sure it runs

        cd ~/projects/aws-eb-jhipster
        ./gradlew build
        ./gradlew bootrun

        # open the new app
        open 'http://localhost:8081/jhipster/'
        curl -v 'http://localhost:8081/jhipster/v2/api-docs' | jq

        # if everything looks good, stop the boot server with ctrl-C

7) Create an EB Procfile and Buildfile

        # https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-se-procfile.html
        # https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-se-buildfile.html

        echo "web: java -Xms256m -jar build/libs/jhipster-0.0.1-SNAPSHOT.war" > Procfile
        echo "build: ./gradlew build -x test" > Buildfile

        # we run on port 8081 for simplicity, see these docs about using nginx as a TLS proxy
        # https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-se-nginx.html

8) Create Loadbalancer configs

        cd ~/projects/aws-eb-jhipster
        mkdir -p .ebextensions
        touch .ebextensions/alb-default-process.config

        # add these line to the .ebextensions/alb-default-process.config file
        # NOTE: it must be valid yaml, watch indenting when copy/pasting

        option_settings:
                aws:elasticbeanstalk:environment:
                        LoadBalancerType: application
                aws:elasticbeanstalk:environment:process:default:
                        DeregistrationDelay: '20'
                        HealthCheckInterval: '15'
                        HealthCheckPath: /jhipster/management/health
                        HealthCheckTimeout: '5'
                        HealthyThresholdCount: '3'
                        UnhealthyThresholdCount: '5'
                        Port: '8081'
                        Protocol: HTTP
                        StickinessEnabled: 'true'
                        StickinessLBCookieDuration: '43200'


8) Commit all code to git

        git add -A
        git commit -m "initial commit"

2) Configure the eb client

        # in a terminal, run this command
        # NOTE: this does not use the typical credentials found in ~/.aws/credentials
        cd ~/projects/aws-eb-jhipster
        eb init

        # region: us-east-1, 
        # application to use: Create new Application
        # application name: aws-eb-jhipster
        # It appears you are using Node.js. Is this correct? : no
        # Select a platform: Java
        # Select a platform version: Java8
        # Do you wish to continue with CodeCommit?: No
        # Do you want to set up SSH for your instances? Yes
        # Select a keypair: "Create new Keypair"
        # Type a keypair name: aws-eb


3) Create a new EB environment

        cd ~/projects/aws-eb-jhipster
        eb create

        # Enter Environment Name: aws-eb-jhipster-dev
        # Enter DNS CNAME prefix: aws-eb-jhipster-dev
        # Select a load balancer type: application

3) Deploy an app

        cd ~/projects/aws-eb-jhipster
        eb deploy


4) install jhipster

        brew install jhipster

5) Create a spring boot project with jhipster

        mkdir -p ~/projects/heroku-jhipster-jhipster
        cd ~/projects/heroku-jhipster-jhipster

        # start jhipster
        jhipster

        # choose mostly default options:
        # ? Which *type* of application would you like to create? Microservice application
        # ? What is the base name of your application? jhipster
        # ? As you are running in a microservice architecture, on which port would like your server to run? It should be unique to avoid port # conflicts. 8081
        # ? What is your default Java package name? com.mycompany.myapp
        # ? Which service discovery server do you want to use? No service discovery
        # ? Which *type* of authentication would you like to use? JWT authentication (stateless, with a token)
        # ? Which *type* of database would you like to use? SQL (H2, MySQL, MariaDB, PostgreSQL, Oracle, MSSQL)
        # ? Which *production* database would you like to use? PostgreSQL
        # ? Which *development* database would you like to use? H2 with in-memory persistence
        # ? Do you want to use the Spring cache abstraction? No - Warning, when using an SQL database, this will disable the Hibernate 2nd level # cache!
        # ? Would you like to use Maven or Gradle for building the backend? Gradle
        # ? Which other technologies would you like to use?
        # ? Would you like to enable internationalization support? No
        # ? Besides JUnit and Jest, which testing frameworks would you like to use?
        # ? Would you like to install other generators from the JHipster Marketplace? No

        # verify jhipster created the project
        ls

6) Test new app. make sure it runs

        cd ~/projects/heroku-jhipster-jhipster
        ./gradlew build
        ./gradlew bootrun

        # open the new app
        open 'http://localhost:8081/jhipster/'
        curl -v 'http://localhost:8081/jhipster/v2/api-docs' | jq

        # if everything looks good, stop the boot server with ctrl-C

7) Create a heroku Procfile

        # tell heroku how to start our app
        echo 'web: java -Dserver.port=$PORT $JAVA_OPTS -jar build/libs/*.war' > Procfile

8) add project to git and create a heroku remote 

        git init

        # get app name
        heroku apps

        # replace '<my-app-1234>' with your actual app name
        export HEROKU_APP_NAME='<my-app-1234>'

        # add a remote to your git repo
        heroku git:remote -a ${HEROKU_APP_NAME}

9) Commit and push repo to heroku

        git add .
        git commit -am "initial commit, jhipster + heroku demo"
        git push heroku master

10) Visit Heroku site

        # NOTE: you might have to wait a minute for spring boot to start up.

        heroku open

        # or

        open "https://${HEROKU_APP_NAME}.herokuapp.com/jhipster/"
        curl -v "https://${HEROKU_APP_NAME}.herokuapp.com/jhipster/v2/api-docs" | jq

11) View app logs

        heroku logs --tail

12) Heroku java docs

        open 'https://devcenter.heroku.com/articles/getting-started-with-java'

13) Clean up

        a) Login to AWS console, navigate to your user in IAM, delete your Access Key
        b) delete any elasticbeanstalk s3 buckets


        eb terminate

