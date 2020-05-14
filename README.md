# my-quarkus project

This project uses Quarkus, and the propose is help to demonstrate the kubernetes service routing between pods.

If you want to learn more about Quarkus, please visit its website: https://quarkus.io/ .

## Running the application in dev mode

You can run your application in dev mode that enables live coding using:
```
./mvnw quarkus:dev
```

## Packaging and running the application

The application can be packaged using `./mvnw package`.
It produces the `my-quarkus-1.0-SNAPSHOT-runner.jar` file in the `/target` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `target/lib` directory.

The application is now runnable using `java -jar target/my-quarkus-1.0-SNAPSHOT-runner.jar`.

## Creating a native executable

You can create a native executable using: `./mvnw package -Pnative`.

Or, if you don't have GraalVM installed, you can run the native executable build in a container using: `./mvnw package -Pnative -Dquarkus.native.container-build=true`.

You can then execute your native executable with: `./target/my-quarkus-1.0-SNAPSHOT-runner`

If you want to learn more about building native executables, please consult https://quarkus.io/guides/building-native-image.

## Creating the Docker Image

### 1. JAR File

You must execute the command in home directory project: 

```sh
docker build -f ./src/main/docker/Dockerfile.jvm -t <IMAGE-NAME>:<TAG> . 
```
More details can found in comments of Dockerfile.jvm file.

### 2. Native 

You must execute the command in home directory project: 

```sh
docker build -f ./src/main/docker/Dockerfile.native -t <IMAGE-NAME>:<TAG> . 
```
More details can found in comments of Dockerfile.native file.

## Executing the Docker Image

You must execute the command:

```sh
docker run -i --rm -p 8080:8080 <IMAGE-NAME>:<TAG>
```

The command above can bu used for `JAR File` or `Native`

## Publish Docker Image in Dockerhub

First, you need to create the image before this step.

With image created, you must execute the commands to TAG your image, and upload the your image to Docker Hub.

```sh
docker tag <IMAGE-NAME>:<TAG> <REPOSITORY-NAME>/<IMAGE-NAME>:<TAG>
docker push
``` 
## Publish workload in Kubernetes to test routing

First, you need your image uploaded at your repository before this step.

you must adjust the image repository name on file `./src/main/kubernetes/deployment.yaml`.

After, you must execute the commands to publish the Deployment and the Service in your cluster Kubernetes. In my case, I used the [Okteto](https://okteto.com/) service.

```sh
kubectl -f ./src/main/kubernetes/deployment.yaml
kubectl -f ./src/main/kubernetes/service.yaml
```