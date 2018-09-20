
# Deploy an app to surge.md 


# Prerequisites

- `brew install npm`

# Pricing

https://surge.sh/pricing


# Pros

- Extremely easy to deploy code, maybe 30 seconds total.
- great for SPA's or jekyll type sites


# Cons

- no https for the free account
- static content only
- no databases


# How to

## install surge cli
        
        npm install --global surge


## Create an account and deploy code

        cd <deploy-fast>/surge.sh
        surge


## List deployed projects

        # this will show the current domains which will be something 
        # like `unnatural-pencil.surge.sh`
        # save the domain for later
        surge list
        export DOMAIN='unnatural-pencil.surge.sh'


## View the site

        open "http://${DOMAIN}"


## Update the site and redeploy

        # update the site
        echo "Update: `date`. <br/>" >> index.html

        # deploy the changes
        surge . ${DOMAIN}

        # view the results
        open "http://${DOMAIN}"


## Tear down

        surge teardown ${DOMAIN}
        git checkout -- index.html
        surge logout
