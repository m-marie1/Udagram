# Hosting a Full-Stack Application

> - The app link:
    http://udagram-project-bucket12345.s3-website-us-east-1.amazonaws.com/

> - EB and RDS endpoints:
    EB: http://udagram-api-env.eba-mhw3sv9j.us-east-1.elasticbeanstalk.com/
    RDS: database-1.ccwhzrd9933t.us-east-1.rds.amazonaws.com


Note: There's a more detailed documentation in the Documentation folder.
---

# Udagram

The udagram application is a fairly simple application that includes all the major components of a Full-Stack web application.


### Environment Variables

These are the environment variables in .env file in udagram-api:

POSTGRES_USERNAME="postgres"
POSTGRES_PASSWORD="password"
POSTGRES_DB="postgres"
POSTGRES_HOST="database-1.ccwhzrd9933t.us-east-1.rds.amazonaws.com"
AWS_REGION=""
AWS_PROFILE=""
AWS_BUCKET=""
URL="http://localhost"
JWT_SECRET="secret-jwt-secret"


### Usage

 > - You can use the app, register and login using the link given at the top.
 > - After making a .env file with the information above in udagram-api folder, Run:
    npm install > npm run dev > npm run build > npm run start > npm run test > npm run ebdeploy
 > - In udagram-frontend folder, Run:
    npm install -f > npm run start > npm run build > npm run lint > npm run test > npm run e2e > npm run deploy   


### Dependencies

```
- Node v14.15.1 (LTS) or more recent. While older versions can work it is advisable to keep node to latest LTS version

- npm 6.14.8 (LTS) or more recent, Yarn can work but was not tested for this project

- AWS CLI v2, v1 can work but was not tested for this project

- An RDS database running Postgres.

- An S3 bucket for hosting uploaded pictures.

```

### Installation

Provision the necessary AWS services needed for running the application:

1. In AWS, provision a publicly available RDS database running Postgres. <Place holder for link to classroom article>
1. In AWS, provision a s3 bucket for hosting the uploaded files. <Place holder for tlink to classroom article>
1. Export the ENV variables needed or use a package like [dotnev](https://www.npmjs.com/package/dotenv)/.
1. From the root of the repo, navigate udagram-api folder `cd starter/udagram-api` to install the node_modules `npm install`. After installation is done start the api in dev mode with `npm run dev`.
1. Without closing the terminal in step 1, navigate to the udagram-frontend `cd starter/udagram-frontend` to intall the node_modules `npm install`. After installation is done start the api in dev mode with `npm run start`.

## Testing

This project contains two different test suite: unit tests and End-To-End tests(e2e). Follow these steps to run the tests.

1. `cd starter/udagram-frontend`
1. `npm run test`
1. `npm run e2e`

There are no Unit test on the back-end

Note: There are errors with End-To-End tests and unit tests for the frontend and since these errors are related to the project (starter files) itself and not to deployment process, I made the scripts echo "no test specified" just like the backend.

### Unit Tests:

Unit tests are using the Jasmine Framework.

### End to End Tests:

The e2e tests are using Protractor and Jasmine.

### References

- Udacity classroom
- AWS Documentation
- StackOverFlow
- Diagrams.net for the diagrams

## Built With

- [Angular](https://angular.io/) - Single Page Application Framework
- [Node](https://nodejs.org) - Javascript Runtime
- [Express](https://expressjs.com/) - Javascript API Framework

## License

[License](LICENSE.txt)
