# fargate-nginx-php-fpm-for-laravel


## Features

・Laravel on AWS ECS Fargate

## ECS Task Containers

- Nginx
- PHP-FPM
- Fluentbit
- amazon/aws-xray-daemon
- aws-appmesh-envoy

## CodePipeline configuration example

1. CodeCommit
2. CodeBuild(using ./buildspec_build.yml)
3. Manual Apply
4. CodeBuild(using ./buildspec_migrate.yml)
5. CodeDeploy(Deploy ECS)

## Usage

- Build and deploy with Codebuild using CodePipeline
- Please use CodeBuild to build with CodePipeline. Then you need to set the following environment variables.
- Docker HubのtokenをParameter store

### Require CodeBuild ENVIRONMENT
key:  
ENVIRONMENT

value:  
enum:[develop|staging|production]

### Require Setting ./buildspec_build.yml

- Set it directly to the variable in the config file
- Set in Parameter Store in the configuration file

### Require Setting ./buildspec_migrate.yml

- Set in Parameter Store in the configuration file