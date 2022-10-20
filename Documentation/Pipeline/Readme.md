# Pipeline Process

A pipeline is used to automate our deployment process so that every time we make some code changes and push the code to Github, The pipeline service is trigered and is starting to make the rest for us: installs dependencies, builds, tests, and deploy both the backend and frontend.

I used CircleCI for this process, connected CircleCI account to my Github account and followed the udagram project repository.

A config.yml file is required for CircleCI. It's present in the root of project in .circleci folder


## CircleCI usage and configuration

In the config file, We specify how CircleCI should manage orbs, jobs, and workflows.

### Orbs

Orbs are a set of instructions created by CircleCi that allow us to configure the pipeline on which we will run our actions. These instructions will instruct the server to setup specific software on the server executing our pipeline. In our case the software we needed CircleCi to setup is:
 ```
 orbs:
   node: circleci/node@5.0.2
   eb: circleci/aws-elastic-beanstalk@2.0.1
   aws-cli: circleci/aws-cli@3.1.1
 ```

### Jobs

Jobs are groups of commands that we want to run. From the name, We can expect this the place CircleCI does the main job. It excutes all the commands we've given in its own CLI so that we can install dependencies, lint, build, test, and then deploy our app.


For installing dependencies, linting, building and testing, We used a "build" job.
And for deploying both the backend and frontend, We used a job called "deploy"

Here are our jobs:
```
jobs:
  build:
    docker:
      # the base image can run most needed actions with orbs
      - image: "cimg/node:14.15"
    steps:
      # install node and checkout code
      - node/install:
          node-version: '14.15'         
      - checkout
      # Use root level package.json to install dependencies in the frontend app
      - run:
          name: Install Front-End Dependencies
          command: |
            echo "NODE --version" 
            echo $(node --version)
            echo "NPM --version" 
            echo $(npm --version)
            npm run frontend:install
      # TODO: Install dependencies in the the backend API          
      - run:
          name: Install API Dependencies
          command: |
           echo "TODO: Install dependencies in the the backend API"
           npm run api:install
      # TODO: Lint the frontend
      - run:
          name: Front-End Lint
          command: |
            echo "TODO: Lint the frontend"
            npm run frontend:lint
      # TODO: Build the frontend app
      - run:
          name: Front-End Build
          command: |
            echo "TODO: Build the frontend app"
            npm run frontend:build
      # TODO: Build the backend API      
      - run:
          name: API Build
          command: |
            echo "TODO: Build the backend API"
            npm run api:build
      # TODO: Test the frontend app     
      - run:
          name: Front-End Test          
          command: |
            echo "# TODO: Test the frontend app"
            npm run frontend:test 
      # TODO: Test the backend API      
      - run:
          name: API Test          
          command: |
            echo "# TODO: Test the backend app"
            npm run api:test
      # TODO: End to end test for the frontend app      
      - run:
          name: Front-End e2e Test          
          command: |
            echo "# TODO: Test the frontend app"
            npm run frontend:e2e
  # deploy step will run only after manual approval
  deploy:
    docker:
      - image: "cimg/base:stable"
      # more setup needed for aws, node, elastic beanstalk
    steps:
      - node/install:
          node-version: '14.15' 
      - eb/setup
      - aws-cli/setup
      - checkout
      - run:
          name: Deploy App
          # TODO: Install, build, deploy in both apps
          command: |
            echo "# TODO: Install, build, deploy in both apps"
            npm run deploy
```
CircleCI commands run from the root folder of the project; That's why we use scripts from our project-level package.json. These scripts are pointing to other scripts in the package.json of each folder of the api and frontend. I created a deploy.sh file for the backend as well where I put the commands I used in EB CLI so that CircleCI can run them. The file is run with a new script "ebdeploy" and I left the original deploy script that came with starter files as it is.

With our build and deploy jobs, We've achieved our goal of continous integration and continous deployment.


### Workflows

Workflows are instructions about the order of the jobs. They allow to create complex flows and specify manual approvals.
When we want a job to run before another job, We make the second requires the first which means the first is required for the second to run. We created a hold that requires the build job to run first. And the deploy job requires that hold. The of hold is: approval which means we should manually approve so that the hold is done and deploy starts.

```
workflows:
  udagram:
    jobs:
      - build
      - hold:
          filters:
            branches:
              only:
                - master
          type: approval
          requires:
            - build
      - deploy:
          requires:
            - hold
```

> - Just like we did using the .env file and the command line. CircleCI needs the environment variables of the database and the credentials of AWS account that has deployed the app. I gave CircleCI these environment variables in CircleCI project settings.
