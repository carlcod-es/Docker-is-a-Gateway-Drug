# Docker-is-a-Gateway-Drug

Talk given at the following events

- DDD South West 2022
- .NET Cheltenham and Gloucester 2023

[Slides](https://docs.google.com/presentation/d/1tXQhdrO5DL73pQaHMZhZ0Tq7D4RdxERoFKPQq6KGamw/edit#slide=id.g1358cb38f49_0_2)

Also as a lightning talk at DotNet South West 

[Slides](https://docs.google.com/presentation/d/1TLiaYMO5733RNAkDaAa74AgXPE6cu_xbR4a-YqWzBNc/edit#slide=id.g13779d8b915_0_12)

## Story 1 : Database Server directly from Iamge

SQL Server

    docker run -d --name dgd_sql_server_2017 -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=SQL_password123' -p 6517:1433 mcr.microsoft.com/mssql/server:2017-latest
    docker run -d --name dgd_sql_server_2019 -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=SQL_password123' -p 6519:1433 mcr.microsoft.com/mssql/server:2019-latest
    docker run -d --name dgd_sql_server_2022 -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=SQL_password123' -p 6522:1433 mcr.microsoft.com/mssql/server:2022-latest

To connect to these servers using the ports configred, you need to use the following connection string

    Server=localhost,6517;Database=master;User Id=sa;Password=SQL_password123;

SQL Server Azure Sql Edge - for ARM Cpus

    docker run -d --name dgd_sql_server_edge -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=SQL_password123' -p 1433:1433 mcr.microsoft.com/azure-sql-edge
### Blazor Server app

Build from Image

    docker build --tag=my_blazor_app ./BlazorInABox

Then Run the image

    docker run --name blazor_app -p 5080:80 -d my_blazor_app

You can then test the app by going to http://localhost:5080


## Story 2 : Database Server from a Dockerfile, and learning Linux basics

### To run empty database server

To build from a docker file, with an empty database

    docker build --tag=mydb_17_empty ./DBContainer/Empty-Database

To see al list of images you can run
    
    docker images

Then Run the image

    docker run --name Sql_17_Empty -p 6617:1433 --volume mydb_17_empty:/var/opt/sqlserver -it -d mydb_17_empty

### Copy files to container

Run this to copy the file

    docker cp ./db/Umbraco.zip Sql_17_Empty:/var/opt/mssql/data/Umbraco.zip

Then connect to the container

    docker exec -it Sql_17_Empty bash

Then run this to unzip the file

    unzip /var/opt/mssql/data/Umbraco.zip -d /var/opt/mssql/data/

Connect to the sql server with the following connectionstring

    Server=localhost,6617;Database=master;User Id=sa;Password=SQL_password123;

Restore the Database with the following SQL

    ----Restore Database
    declare @dbName nvarchar(50)
    declare @bakFile nvarchar(100)
    declare @mdfFile nvarchar(100)
    declare @ldfFile nvarchar(100)
    set @dbName = 'Umbraco'

    set @mdfFile = '/var/opt/mssql/data/' + @dbName + '.mdf'
    set @ldfFile = '/var/opt/mssql/log/' + @dbName + '.ldf'

    set @bakFile = '/var/opt/mssql/data/'+ @dbName + '.bak'
    RESTORE DATABASE @dbName
    FROM DISK = @bakfile
    WITH MOVE 'DrinkstuffUmbraco' TO @mdfFile,
    MOVE 'DrinkstuffUmbraco_log' TO  @ldfFile

### To run a pre-populated database server

To build from a docker file, with an empty database

    docker build --tag=mydb_17_filled ./DBContainer/Server-with-Database

To see al list of images you can run
    
    docker images

Then Run the image

    docker run --name Sql_17_Filled -p 6618:1433 --volume mydb_17_filled:/var/opt/sqlserver -it -d mydb_17_filled


## Preview Framework

- https://hub.docker.com/_/microsoft-dotnet-nightly-sdk/

To run : 

    docker run -it -d --name dotNetPreview mcr.microsoft.com/dotnet/nightly/sdk:8.0.100-preview.4-jammy-amd64
    docker exec -it dotNetPreview bash

Create a new app

    dotnet --info

    dotnet new console --name appTest

    dotnet run --project appTest/ 


## Story 3 : Running a bunch of other services

To Run a container with Rabbit MQ

    docker run -d --hostname my-rabbit --name some-rabbit -p 5672:5672 -p 15672:15672 -e RABBITMQ_DEFAULT_USER=user -e RABBITMQ_DEFAULT_PASS=password rabbitmq:3-management

To Run a container with Redis

    docker run -d --name redis -p 6379:6379 redis

To Run a container with MongoDb

    docker run -d --name mongo -p 27017:27017 mongo

## Story 4 - Onboarding a new developer

Imagine you've cloned the project to ./UmbracoDemo

- Set your current folder to the UmbracoDemo folder
- Make sure all the docker files in the UmbData container have the right line endings (LF)
- run Docker compose up

## Dooom!

    docker pull kasmweb/doom:1.9.0
    docker run -d --name Doooom --rm -it --shm-size=512m -p 6901:6901 -e VNC_PW=password kasmweb/doom:1.9.0 

    The container is now accessible via a browser : https://127.0.0.1:6901

    User : kasm_user
    Password: password


# Junk below here

## Doom fallback

    docker pull kasmweb/doom:1.9.0
    docker run --rm  -it --shm-size=512m -p 6901:6901 -e VNC_PW=password kasmweb/doom:1.9.0
