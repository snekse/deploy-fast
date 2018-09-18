
# Deploy an app to heroku

Heroku is a full service cloud provider.  It has many services built in to to help
you get your POC or MVP off the ground faster.  It can get expensive once high volumes set in,
so it's worth while to consider a migration plan once your app gains traction.

# Prerequisites

- Heroku free account - https://signup.heroku.com/

# Pricing

https://www.heroku.com/pricing

# Pros

- Very quick to set up
- `git push` to deploy
- 2 dynos on free plan is enough for an app and a database
- lots of addons available such as redis, postgress, rabbitmq and many many more.

# Cons

- Can get expensive at scale, best to have an exit plan from the beginning

# How to


1) Install and update heroku cli

        brew install heroku
        heroku update

2) Login to Heruku CLI

        heroku login


3) Create a new app, make a note of the name.

        # create a new app
        heroku apps:create

        # it will be given a random name, make sure to note the name for later.
        # if you forget, you can list all apps like this
        heroku apps


4) install jhipster

        brew install jhipster

4) Create a spring boot project with jhipster

        mkdir -p ~/projects/heroku-jhipster-demo
        cd ~/projects/heroku-jhipster-demo

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

5) Test new app. make sure it runs

        cd ~/projects/heroku-jhipster-demo
        ./gradlew build
        ./gradlew bootrun

        # open the new app
        open 'http://localhost:8081/jhipster/'
        curl -v 'http://localhost:8081/jhipster/v2/api-docs' | jq

        # if everything looks good, stop the boot server with ctrl-C

6) Create a heroku Procfile

        # tell heroku how to start our app
        echo 'web: java -Dserver.port=$PORT $JAVA_OPTS -jar build/libs/*.war' > Procfile

6) add project to git and create a heroku remote 

        git init

        # get app name
        heroku apps

        # replace '<my-app-1234>' with your actual app name
        export HEROKU_APP_NAME='<my-app-1234>'

        # add a remote to your git repo
        heroku git:remote -a ${HEROKU_APP_NAME}

7) Commit and push repo to heroku

        git add .
        git commit -am "initial commit, jhipster + heroku demo"
        git push heroku master

8) Visit Heroku site

        # NOTE: you might have to wait a minute for spring boot to start up.

        heroku open

        # or

        open "https://${HEROKU_APP_NAME}.herokuapp.com/jhipster/"
        curl -v "https://${HEROKU_APP_NAME}.herokuapp.com/jhipster/v2/api-docs" | jq

9) View app logs

        heroku logs --tail

10) Heroku java docs

        open 'https://devcenter.heroku.com/articles/getting-started-with-java'

11) Clean up

        heroku apps:destroy ${HEROKU_APP_NAME}
