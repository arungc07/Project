# Project with Frontend and Backend which is to give details from the H2 internal database

## Project
You can create the configuration with the spring boot application and use the same config to run.


## Prerequisites

- JDK 1.7 or later
- Maven 3 or later

## Run
### local set up
Go to project (frontend or backend) directory and run below command

```mvn clean spring-boot:run```
```mvn clean install```

java -jar fronend/target/frontend-1.0-RELEASE.jar

java -jar backend/target/backend-1.0-RELEASE.ja

Then browse

http://localhost:8080/ for front end
http://localhost:8080/docs for back end

## Run Docker
### local set up
Go to project (frontend or backend) directory and run below command

```docker build .```

for Backend
```docker run -p 8085:8080 -d <imageName> -p```


for Frontend
```export BACKEND=http:\\localhost:8085```
```docker run -p 8084:8080 -d <imageName>```

Then browse

http://localhost:8084/ for front end
http://localhost:8085/docs for back end
