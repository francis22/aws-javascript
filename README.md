# Deploy React to AWS

### Overview
The code in this repository is configured to deploy to AWS S3 using AWS CodePipeline. The coorelating server-side Node.js code may be foundt at https://github.com/aaronwht/aws-javascript-api.

### Running locally
Create an ```.env``` file in the root of your project as pictured below.
![.env file](https://www.aaronwht.com/images/s3-build/env-variables.png)

Create the variable ```REACT_APP_API``` and set it's value to ```http://localhost:8080/```. (include the suffix slash)

```npm install```

```npm start```

The ```buildspec.yml``` file (code below) runs on the AWS Build Server.
```
version: 0.1
phases:
    install:
        commands:
        - npm install
        - npm run build
        - aws s3 cp build s3://$S3_BUCKET --recursive --exclude 'index.html'
        - aws s3 cp build/index.html s3://$S3_BUCKET
```

```npm install``` installs dependencies on the AWS Build Server and ```npm run build``` creates a production build of your app.

The next line instruct the AWS CLI to recursively copy all files and folders in the ```build``` folder to your AWS Bucket, excluding your ```index.html``` file.

This ensures application dependencies are copied to the AWS S3 bucket before the ```index.html``` file is copied so your application isn't broken through the deployment process.

```aws s3 cp build/index.html s3://$S3_BUCKET```
Lastly, the ```index.html``` file us copied to the AWS S3 bucket to complete your deployment.


```npm install``` - Installs NPM packages on the AWS build server.
```npm run build``` - Creates a production version of the application on the AWS build server in the ```build``` folder.
```aws s3 cp build``` instructs the build server to copy the ```build``` folder to the AWS S3 bucket (environment variable).
```$S3_BUCKET``` is the environment variable used by the ```AWS Build Pipeline``` (displayed below)

![environment variable](https://www.aaronwht.com/images/s3-build/pipeline-envs.png)