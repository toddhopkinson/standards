# Deployment

## Adopt

  - [CloudFormation](http://aws.amazon.com/cloudformation/)
    Useful for initial creation of the software stack and pushing occasional subsequent changes.
    A few examples of resources that can be created with CloudFormation: ELBs,
    ASGs, instance profiles, IAM profiles, S3 buckets, dynamo db tables.
    Think of it as creating a network structure into which your code can be deployed.


  - [CodeDeploy](http://aws.amazon.com/codedeploy/)
    Useful for deploying application code and any accompanying artifacts, agents, scripts, etc
    into an existing software stack, perhaps created with CloudFormation.
    This deals with concepts such as deployment groups, application versions, rollout strategies.


  - [Docker](https://www.docker.com/)

    Docker is largely in use as a replacement for RPMs, to wrap a program in some easily deployable format.
    We believe the ability to specify runtime requirements at development time, and not having to worry about the runtime capabilities of the server is extremely valuable.

  - [Docker Registry](https://docs.docker.com/registry/)

    The registry is the place where Docker images are uploaded at build time, and where they are downloaded from at deployment time. For open-source projects, we use [DockerHub](https://hub.docker.com). For private projects we use [ECR](https://aws.amazon.com/ecr/), it is available in each team's AWS account.

  - [Schema Evolution Manager](https://github.com/gilt/schema-evolution-manager)

    Used for deploying Postrgesql schema migrations. Solves the problem of coupling schema changes with
    code changes by completely separating the two deployments. By necessity, schema migrations must be
    backwards-compatible (because code deployment happens later), thereby minimizing the risk of schema
    deployments. Furthermore, it allows different code versions to run on different nodes during code
    rollout.

  - [Java SE Runtime Environment 8](http://openjdk.java.net/projects/jdk8u/)

    Java 8 is the latest stable release of the Java Platform.  All new deployments of JVM based services should run on JRE 8. Likewise, all JVM based source code (Scala, Java, etc) should be compiled using JDK 8. The only exception should be library code that is used by legacy services that are still on Java 7. Both for compilation and runtime, either OpenJDK or Oracle JDK can be used.

##Trial

  - [NOVA](https://github.com/gilt/nova)

    Nova has had increasing levels of adoption throughout the Gilt org and has proven to be a stable and reliable tool for deploying services into AWS.

## Assess

  - [CodePipeline ](http://aws.amazon.com/codepipeline/)
  - [EC2 Container Service](http://aws.amazon.com/ecs/)

  - [AWS Lambda](http://aws.amazon.com/lambda/)

    A nice alternative to deploying/managing service clusters.
    Consider using in places where functionality is very simple, especially if the traffic is low and it's hard to justify the cost of running EC2 nodes all the time.

## Hold
  - [Ionroller](https://github.com/gilt/ionroller/)

    Ionroller is an exploration into 'immutable deploys'. It trades off speed of initial deploy (which involves new machines/elbs/etc being created) with the ability to:
      - test independently in production
      - quickly roll over to a different stack, enabling [Blue Green Deploys](http://martinfowler.com/bliki/BlueGreenDeployment.html)

  - [sbt-codedeploy](https://github.com/gilt/sbt-codedeploy)

    Active development on sbt-code-deploy has come to a halt, and teams have begun to move their deployments over to Nova.

  - Ioncannon (Closed Source)

    The initial premise of Ioncannon, to deploy to a staging environment, run a bunch of functional tests, and promote to production automatically has had some difficulties.
    Particularly, maintaining said stage environment has proven challenging and not particularly efficient. Other methodologies are being explored in tooling above.

  - [Play Evolutions](https://www.playframework.com/documentation/2.5.x/Evolutions)

    Too tightly couples the deployment of schema with the deployment of code. This can be a problem with
    difficult rollbacks. It also obfuscates the necessity of avoiding breaking changes to the schema in
    order to avoid downtime when the code on a second node is incompatible with the new schema. Though this tool
    may be useful in some small microservices, it is likely that the service will outgrow it and eventually need
    Schema Evolution Manager; recommendation is to simply start with SEM from the beginning.

  - Java SE Runtime Environment 7

    Java 7 reached end-of-life in April 2015, and since then there have been no further public security/patch releases. No new codebases and deployments should use Java 7 or older version.
