# Udagram Infrastructure

Udagram is deployed using AWS ( Amazon Web Services ) Cloud. The main 3 services used are:
 * AWS Relational Database Service (RDS): for creating a postgres database that will contain all the data related to users.
 * AWS ELastic Beanstalk: for hosting the api and is connected to the RDS database.
 * AWS S3: for hosting the frontend that fetches the API.



## RDS Database

* Created a postgres database with the name of postgres. It's publiclly accessible and the endpoint is "database-1.ccwhzrd9933t.us-east-1.rds.amazonaws.com". I edited inbound rules of VPC security group and added a role source of 0.0.0.0/0.
* Created .env file in the udagram-api folder that contains all the database information so that Sequelize is able to connect and create new tables.

## ELastic Beanstalk

* Created a new application through EB CLI with the command eb init. The name is the default name recommended by the CLI which is udagram-api. Then created a new environmrnt with the name of udagram-api-env using eb create.
* Used the command eb setenv to give EB all the environment variables I put in .env before.
* Used eb deploy to deploy the zipped build folder after building the api using npm run build.
* In EB config file, The path of the zipped folder is specified using:
   ```
   deploy:
    artifact: ./www/Archive.zip
   ```
* The app is deployed, The health is ok, and environment variables are set.


# S3 Bucket

* Created a new S3 bucket, made it publiclly accessible, ACL (access control list)is enabled, edited bucket policy to make it public as in the classroom, and enabled static web hosting.
* Provided the EB environment link to the frontend in src/environments/environment.ts:
   ```apiHost: 'http://udagram-api-env.eba-mhw3sv9j.us-east-1.elasticbeanstalk.com/api/v0'```

* Used deploy.sh file in bin folder which was given with the starter files to upload the frontend to my S3 bucket.. The frontend is deployed and connected to Elastic Beanstalk. By going to the link provided under static web hosting - Bucket website endpoint: http://udagram-project-bucket12345.s3-website-us-east-1.amazonaws.com/ , You can find that the app is fully deployed and the user is able to register and login.


