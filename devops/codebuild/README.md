Codebuild:
- fully managed build service
- Compiles code
- runs unit tests
- produces artifacts which are ready to deploy
- eliminates the need to provision/manage/scale your own build servers
- Provides pre-packaged build environments
- Allows you to build your own customized build environment
- Scales automatically to meet business requirements

Comes with pre-configured build environments "AKA images" which can be used to run builds. You can use a custom docker image as the build environment if necessary.

Does it support Windows????
Does it support passwords?

You give build projects service roles...
You set build timeouts
You set build queue timeouts
You can install certificates (Only from S3 though)
Decide a VPC where to do the build
Assign memory and CPU (Like lambda, they are linked ) (Starts at 3GB up to 145GB)
You can pass in env variables when configuring the initial build plan AND you can override these variables when running individual jobs
You can add file system mounts too
Artifacts come from builds (Files basically) and they can be outputted to an S3 bucket. You can set the path to drop the file, you can keep versions and you can ZIP the artifact files too.  
You can stream all build steps to CloudWatch and S3

Builds can be triggered to execute on a H/D/W schedule or on a custom CRON.
When builds run you can see their progress in the console and look at the "PHASE DETAILS" page too to see how long each step takes.

You can use a buildspec.yaml file to define ALL the build steps needed to run the job.
- OR
You can insert build commands into the console when you create the code build job



```BUILDSPEC.YAML``` - Exists at the root folder level, it contains:

phases:
  install:
    run-as: Linux-user-name
    on-failure: ABORT | CONTINUE
    runtime-versions:
      runtime: version
      runtime: version
    commands:
      - command
      - command
    finally:
      - command
      - command
  pre_build: ```All commands for pre_build can also be run for build and post_build``` 
      run-as: Linux-user-name
    on-failure: ABORT | CONTINUE
    commands:
      - command
      - command
    finally:
      - command
      - command
  build:
    commands:
      - echo Build started on `date`
      - echo Compiling the C++ code...
      - g++ hello.cpp -o hello.out
  post_build:
      commands:
      - echo Build completed on `date`
artifacts:
  files:
    - hello.out


