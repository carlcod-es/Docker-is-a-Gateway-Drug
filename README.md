# Docker-is-a-Gateway-Drug

Talk given at DDD South West 2022

[Slides](https://docs.google.com/presentation/d/1tXQhdrO5DL73pQaHMZhZ0Tq7D4RdxERoFKPQq6KGamw/edit#slide=id.g1358cb38f49_0_2)


## Database Server

SQL Server 2019

    docker run -d --name ddd_sql_server_2019 -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=SQL_password123' -v ddd_sql_system:/var/opt/mssql -p 1433:1433 mcr.microsoft.com/mssql/server:2019-latest


## Linux Basics

Copy files to container

    docker cp "C:\Work\Talks\Docker-is-a-Gateway-Drug\db\Umbraco.bak" ddd_sql_server_2019:/var/opt/mssql/data/Umbraco.bak

Restore the Database

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


## Preview Framework

- https://hub.docker.com/_/microsoft-dotnet-sdk
- https://hub.docker.com/_/microsoft-dotnet-nightly-sdk/

To run : 

    docker pull mcr.microsoft.com/dotnet/nightly/sdk:7.0
    docker run --name dotnet -p 2010 mcr.microsoft.com/dotnet/nightly/sdk:7.0

    dotnet --info

Create a new  --shm-size=512m -p 6901:690

    dotnet new console --name appTest

     dotnet run --project appTest/ 

## Dooom!

    docker pull kasmweb/doom:1.9.0
    docker run -d --name Doooom --rm  -it --shm-size=512m -p 6901:6901 -e VNC_PW=password kasmweb/doom:1.9.0 

    The container is now accessible via a browser : https://127.0.0.1:6901

    User : kasm_user
    Password: password


# Junk below here

## Doom fallback

    docker pull kasmweb/doom:1.9.0
    docker run --rm  -it --shm-size=512m -p 6901:6901 -e VNC_PW=password kasmweb/doom:1.9.0
