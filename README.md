# Salary API

## Table of Contents 
1. [Purpose](#purpose)
2. [Pre-Requisites](#Pre-Requisites)
3. [Key Components](#Key-Components)
4. [System Requirements](#system-requirements)
5. [Important Ports](#Important-Ports)
6. [Dependencies](#Dependencies)
7. [Architecture](#Architecture)
8. [Setup API](#Setup-API)
9. [Results](#Results)
10. [API Security](#API-Security)
11. [Troubleshooting](#Troubleshooting)
12. [Contact Information](#Contact-Information)
13. [References](#References)


## Purpose

Salary API is a Java-based microservice which is responsible for all the salary-related transactions and records in **[OT-Microservices](https://github.com/OT-MICROSERVICES/salary-api)** stack. The application is platform-independent and can be run on multiple operating systems. **[Java Runtime](https://www.java.com/en/download/manual.jsp)** would be required to run this application.

## Pre-Requisites

The Salary API application has some database, cache manager and package dependencies. Some of the dependencies are optional and some are mandatory. To compile the application, we only need `maven` as a build tool, but for running the application following things are required:-

- **[ScyllaDB](https://www.scylladb.com/)**
- **[Redis](https://redis.io/)**
- **[Migrate](https://github.com/golang-migrate/migrate)**
- **[Maven](https://maven.apache.org/)**
- **[Java 17](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-22-04)**
- **[migrate](https://github.com/golang-migrate/migrate)**

Maven will be used as a package manager to download specific versions of dependencies to run the Salary API.

## Key Components  

| Tool/Software | Usage  |
| ------------- | --------------------------------------------------------- | 
| `Spring boot` | based web framework, which uses Tomcat as a web server.  | 
| `ScyllaDB`    | is used as the primary database for storing all the salary data. |
| `Redis`       | as cache manager to store the cache response.|
| `Prometheus and Open-telemetry` | metrics support for monitoring and observability. |
| `migrate`     | Database migration |



## System Requirements

|   System Requirement              |             Minimum                        |
|-----------------------------------|--------------------------------------------|
| Processor/Instance Type           |             Dual Core/t2.medium            | 
| RAM                               |               4GB                          |
| Disk Space                        |               16GB                         |            
| OS Required (Linux Distributions) | Ubuntu 20.04 LTS, Debian, or CentOS 7/8    |

## Important Ports

### Inbound Ports 

|   Port        |    Description     |
| ----------    |    -----------     |
|    8080       |   Salary  API port | 

### Outbound Port

|   Port        |    Description    |
| ----------    |    -----------    |
|    9042       |    Scylla DB      |
|    6379       |    Redis          |

### Others
|   Port        |    Description    |
| ----------    |    -----------    |
|    22         |    SSH            |



## Dependencies

### Runtime Dependencies

|   Tool/ Software    |    Version    |
| ------------------- |  -----------  |
|        Java         |       17      |

### Build Dependencies

|   Tool/ Software    |    Version    |
| ------------------- |  -----------  |
|        Java         |       17      |
|        Maven        |      3.6.3    |
|        Make         |      4.2.1    |
## Architecture

![image](https://github.com/avengers-p7/Documentation/assets/156056444/6992da87-fbb3-4eb6-972c-a02d031bbb90)

## Setup API

### Step1: Installation of API Dependencies
1. _ScyllaDB_

   Here is a step-by-step **[documentation (in Ubuntu)](https://github.com/avengers-p7/Documentation/blob/main/OT%20Micro%20Services/Software/ScyllaDB.md)** to install ScyllaDB in your system/server.
   Make sure the databases (ScyllaDB and Redis) are up and running. Access your ScyllaDB.
   ```shell
   cqlsh <IP>
   ```
   Now, create the keyspace mentioned `employee_db` manually in your ScyllDB.
   
   ```shell
    CREATE KEYSPACE IF NOT EXISTS employee_db WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
   ```
3. _Redis_

   Here is a step-by-step **[documentation (in Ubuntu)](https://github.com/avengers-p7/Documentation/blob/main/OT%20Micro%20Services/Software/Redis.md)** to install Redis in your system/server.

> [!NOTE]
> If the documentation repo is not accessible then please refer to the official page link shared above _(in the Pre-requisites section)_.

3. _Maven_
    ```shell
    sudo apt install maven -y
    ```
4. _JAVA 17_
    ```shell
    sudo apt install openjdk-17-jre-headless -y
    ```
5. _Make_
   ```shell
   sudo apt install make -y 
   ```
6. _Migrate_
    ```shell
      curl -s https://packagecloud.io/install/repositories/golang-migrate/migrate/script.deb.sh | sudo bash
      sudo apt update
      sudo apt install migrate -y  
    ```


### Step2: Build/Artifact Generation 

1. Clone Salary API repo

   ```shell
   git clone https://github.com/OT-MICROSERVICES/salary-api.git
   ```
   
> [!NOTE]
> 1. Configuration properties will be configured inside **[application.yml]([./src/main/resources/application.yml](https://github.com/OT-MICROSERVICES/salary-api/blob/main/src/main/resources/application.yml))** file in main and test directory.
> 2. Also, once the property file is defined and configured properly, we need to run migrations to create a database, schema etc. The connection details for migration are available in **[migration.json](https://github.com/OT-MICROSERVICES/salary-api/blob/main/migration.json)**.
>    ```shell
>     make run-migrations
>    ```
![image](https://github.com/avengers-p7/Documentation/assets/156056444/a0245aa4-bcf0-4e22-81e1-0b567fd9715c)


Once the schema, table and database are configured, we can start our application using Java runtime.
   
2. For building the Salary API application, we can use `make` commands with our **[Makefile](./Makefile)**. But first, we need to install the dependencies which can be simply done using the `make` command.

   ```shell
   make build
   ```

3. Salary API also contains different test cases and code quality-related integrations. To check the code quality, we will use `checkstyle` plugin with Maven.

   ```shell
   make fmt
   make test
   ```
   The test cases are present in **[src/test/java/com/opstree/microservice/salary](./src/test/java/com/opstree/microservice/salary)**. For dev testing, the Swagger UI can be used for sample          payload generation and requests. The swagger page will be accessible on http://localhost:8080/salary-documentation.

### Step3: Running API

   ```shell
   java -jar target/salary-0.1.0-RELEASE.jar
   ```
   You can check your API 
   
   ```shell
   http://localhost:8080/salary-documentation
   ```
![image](https://github.com/avengers-p7/Documentation/assets/156056444/d1ed67d6-a0d7-498f-8c42-76222f8863c0)

## API Endpoint Information

| **Endpoint**                   | **Method** | **Description**                                                                               |
|--------------------------------|------------|-----------------------------------------------------------------------------------------------|
| `/api/v1/salary/create/record` | POST       | Data creation endpoint which accepts certain JSON body to add salary information in database  |
| `/api/v1/salary/search`        | GET        | Endpoint for searching data information using the params in the URL                           |
| `/api/v1/salary/search/all`    | GET        | Endpoint for searching all information across the system                                      |
| `/actuator/health`             | GET        | Endpoint for providing shallow health check information about application health and readiness|


### Results


![image](https://github.com/avengers-p7/Documentation/assets/156056444/925e8f75-4247-48dc-8f53-bc2722b433f7)

![image](https://github.com/avengers-p7/Documentation/assets/156056444/48b6ccff-c668-4dc6-b834-7cffb2fcc8df)

![image](https://github.com/avengers-p7/Documentation/assets/156056444/3c2c0835-c1f3-44a2-964e-bc8892b05057)

## API Security

1. **Identify Vulnerabilities:**
   - Understand and secure all stages of the API lifecycle.
   - Consider the API as a software artifact from planning to production.

2. **Leverage OAuth:**
   - Implement OAuth for robust access control, authentication, and authorization.
   - Use token-based authentication to allow secure third-party access without exposing credentials.

3. **Encrypt Data:**
   - Encrypt sensitive data, especially personally identifiable information (PII).
   - Implement encryption at rest and in transit (TLS) to protect against unauthorized access and modifications.

4. **Use Rate Limiting and Throttling:**
   - Set rate limits to prevent denial-of-service (DoS) attacks.
   - Protect against peak traffic to maintain performance and security.
   - Regulate user connections to balance access and availability.

## Troubleshooting

1. **Error-01** 

    ![image](https://github.com/avengers-p7/Documentation/assets/156056444/97c10d42-6f4a-44bf-8362-0a0f331bc928)

   1.1. install jq

       sudo apt install jq -y 

   1.2. install Migrate

         curl -s https://packagecloud.io/install/repositories/golang-migrate/migrate/script.deb.sh | sudo bash
         sudo apt update
         sudo apt install migrate -y
 2. **Error-02**

      ![image](https://github.com/avengers-p7/Documentation/assets/156056444/d0c21a64-1072-4585-9ae5-79b038439905)

    For this error provide your **DB information**  in files **[migration.json](https://github.com/OT-MICROSERVICES/salary-api/blob/main/migration.json)**, **[src/main/resources/application.yml](https://github.com/OT-MICROSERVICES/salary-api/blob/main/src/main/resources/application.yml)** and **[src/test/resources/application.yml](https://github.com/OT-MICROSERVICES/salary-api/blob/main/src/test/resources/application.yml)** then move forward.

       ![image](https://github.com/avengers-p7/Documentation/assets/156056444/8b021e21-bb6e-405b-b734-cf8c8a11d5ac)

       ![image](https://github.com/avengers-p7/Documentation/assets/156056444/14a60863-bb8d-421c-b34d-d726e85903e8)

 4. After this, you may also face a **CORS error**.

       ![image](https://github.com/avengers-p7/Documentation/assets/156056444/75cdfe88-4c38-4300-9dcd-3797e63864fa)

       To solve this error add CORS config in **[src/main/java/com/opstree/microservice/salary/contollers/SpringDataController.java](https://github.com/OT-MICROSERVICES/salary-api/blob/main/src/main/java/com/opstree/microservice/salary/contollers/SpringDataController.java)** as per your requirement.



## Contact Information

|     Name         | Email  |
| -----------------| ------------------------------------ |
| Harshit Singh    | harshit.singh.snaatak@mygurukulam.co |                                                                                      

## References

|     Description                  | References  
| ---------------------------------| ------------------------------------------------------------------- |
|     Documentation Template       | https://github.com/OT-MICROSERVICES/documentation-template/wiki/Application-Template |
|     Salary Documentation         | https://github.com/OT-MICROSERVICES/salary-api/blob/main/README.md |  
|     Error Handling               | 1. https://www.youtube.com/watch?v=uMJnAxapF7E |
|                                  | 2. https://github.com/serlesen/fullstack-cors/tree/chapter_1 |
|                                  | 3. https://github.com/OT-MICROSERVICES/salary-api/blob/main/README.md | 
|       API security               | https://brightsec.com/blog/api-security/

