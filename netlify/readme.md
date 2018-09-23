
# Deploy an app to Netlify

Quick start showing how to deploy a sample site to netlify.


# Prerequisites

- github account is needed for this demo.

# Pricing

- https://www.netlify.com/pricing/#features

# Pros

- supports Node, Ruby, php, and Python (no java or go)
- supports Github, Gitlab, and Bitbucket.
- support for deploying functions (like AWS Lambdas)
- support for forms like forms.io
- support for single sign on via `Identity` product.
- Custom domains available
- HTTPS available.
- once set up, `git push` to deploy.

# Cons

- must grant access Netlify to your github, gitlab, or bitbucket account
- no java or go support
- builds and deploys are VERY slow
- UI is a bit clunky, hard to find build logs


# How to

1) Create a free netlify account, no CC needed.

        https://app.netlify.com/signup


2) Grant netlify read access to github account

        This might be optional, but I could not find a way around it.


3) Choose a site template

        Note the various templates here: https://templates.netlify.com/

        We are going to use the "Hugo starter blog theme - Kali" template.

        Click the link to go to the github page:
        https://github.com/netlify-templates/kaldi-hugo-cms-template

        You MUST fork the repo to your own github account.

4) Clone the forked repo project

        mkdir ~/projects
        cd ~/projects
        
        # make sure to use your own github account here.
        git clone git@github.com:<replace_me>/kaldi-hugo-cms-template.git
        
        cd kaldi-hugo-cms-template

        npm install
        npm start

        # test new site
        open 'http://localhost:3000'
        open 'http://localhost:3001'

5) Note the netlify.toml

        This is an optional file that alows mor control of the build and 
        deploy process.  
        
        The docs are here: https://www.netlify.com/docs/netlify-toml-reference/ 


6) Add a new site to netlify

        a) login to `https://app.netlify.com/`
        b) Click the `New site from Git` button
        c) Click the `Github` building
        d) Choose the `<some-user-name>/kaldi-hugo-cms-template` project.
        e) branch: `master`
            build command: `npm run build`
            publish directory: `dist`
        f) Click the `Deploy Site` button.

7) Wait 30 seconds for deploy to finish

8) Visit site

        The DNS name will be auto generated, it will look something like this:

        https://stupefied-dijkstra-fb9e58.netlify.com

9) Make a change to the site

        edit line 5 of ./site/layouts/index.html

10) Commit and push changes

        git add -A
        git commit -m "content updates"
        git push origin master


11) Wait 30 seconds for deploy to finish

12) Visit site

        The DNS name will be auto generated, it will look something like this:

        https://stupefied-dijkstra-fb9e58.netlify.com

13) Clean up

        Navitage to Netlify console: https://app.netlify.com/

        Select your app from the list (i.e. stupefied-dijkstra-fb9e58)

        Click `Settings` in the top menu bar.

        Click `Danger Zone` in the left menu bar

        Click `Delete this Site` button at bottom of page.

