[Whitepaper](https://d1.awsstatic.com/whitepapers/DevOps/running-containerized-microservices-on-aws.pdf)
[12 factor app](https://12factor.net/)

### ECS ###
- Supports Windows containers and Linux docker containers

### To login to an ECR REPO and upload an image###
$(aws ecr get-login --no-include-e-mail --region us-east-1)
docker build -t mydeckrepo .
docker tag mydockerrepo:latest 111111111111.dkr.ecr.us-east-1.amazonaws.com/mydockerrepo:latest
docker push 111111111111.dkr.ecr.us-east-1.amazonaws.com/mydockerrepo:latest

https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html
