
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


3) Create working dir


        mkdir -p ~/projects/aws-eb-hello-world
        cd ~/projects/aws-eb-hello-world


4) Initialize a new elastic beanstalk PHP project

        eb init -p PHP

5) Add some content


        echo "Hello World" > index.html
        echo "<?php phpinfo(); ?>" > info.php

6) Create dev environment in the existing project

        eb create dev-env

7) View the new site

        eb open

8) Clean up

        # destroy the project (ec2 and load balancer)
        eb terminate

        # optionally delete the project dir
        rm -rf ~/projects/aws-eb-hello-world

        # login to aws console
        # manually clean up any eb S3 artifacts

