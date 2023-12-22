# ud-sk-springboot-ecs
Spring Boot Services on AWS ECS

### Running Apps
```sh
cd users-microservice
mvn clean package
mvn spring-boot:run -Dspring-boot.run.arguments=--spring.profiles.active=dev

# Same for Albums

```

### Push to Docker Hub
```sh
cd users-microservice
mvn clean package

docker build --tag=arvindrukmaji/users-microservice --force-rm=true .
docker images
docker push arvindrukmaji/users-microservice

cd albums-microservice
mvn clean package

docker build --tag=arvindrukmaji/albums-microservice --force-rm=true .
docker images
docker push arvindrukmaji/albums-microservice

```

### AWS IAM ECR Permissions
- Create IAM  User for Programmatic Access
- Attach Permission Policies
  - AmazonEC2ContainerRegistryFullAccess - for private registry
  - AmazonElasticContainerRegistryPublicFullAccess - for public registry

Configure above user's accesskey
```sh
aws configure 
```
### Push Image to ECR 
- Create Public/Private Repositories
- users-microservice
- albums-microservice
- ECR -> Private Repositories -> Select Repo -> View Push Commands

**Note:** Make sure aws cli credentials and repositories are configured for the same region.

```sh
cd users-microservice

aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 169703780415.dkr.ecr.us-east-1.amazonaws.com

docker build -t users-microservice .

docker tag users-microservice:latest 169703780415.dkr.ecr.us-east-1.amazonaws.com/users-microservice:latest

docker push 169703780415.dkr.ecr.us-east-1.amazonaws.com/users-microservice:latest

```
```sh
cd albums-microservice

aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 169703780415.dkr.ecr.us-east-1.amazonaws.com

docker build -t albums-microservice .

docker tag albums-microservice:latest 169703780415.dkr.ecr.us-east-1.amazonaws.com/albums-microservice:latest

docker push 169703780415.dkr.ecr.us-east-1.amazonaws.com/albums-microservice:latest

```