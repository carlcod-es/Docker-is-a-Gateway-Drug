# Docker-is-a-Gateway-Drug

Talk given at the following events

- DDD South West 2022
- .NET Cheltenham and Gloucester 2023

[Slides](https://docs.google.com/presentation/d/1tXQhdrO5DL73pQaHMZhZ0Tq7D4RdxERoFKPQq6KGamw/edit#slide=id.g1358cb38f49_0_2)

Also as a lightning talk at DotNet South West 

[Slides](https://docs.google.com/presentation/d/1TLiaYMO5733RNAkDaAa74AgXPE6cu_xbR4a-YqWzBNc/edit#slide=id.g13779d8b915_0_12)

## Database Server

SQL Server

    docker run -d --name dgd_sql_server_2017 -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=SQL_password123' -p 6517:1433 mcr.microsoft.com/mssql/server:2017-latest
    docker run -d --name dgd_sql_server_2019 -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=SQL_password123' -p 6519:1433 mcr.microsoft.com/mssql/server:2019-latest
    docker run -d --name dgd_sql_server_2022 -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=SQL_password123' -p 6522:1433 mcr.microsoft.com/mssql/server:2022-latest

To connect to these servers using the ports configred, you need to use the following connection string

    Server=localhost,6517;Database=master;User Id=sa;Password=SQL_password123;

SQL Server Azure Sql Edge - for ARM Cpus

    docker run -d --name dgd_sql_server_edge -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=SQL_password123' -p 1433:1433 mcr.microsoft.com/azure-sql-edge

Or from Image

    docker build --tag=my_db_image_empty ./DBContainer/Empty-Database
    docker build --tag=my_db_image_filled ./DBContainer/Server-with-Database

Then Run the image

    docker run --name SqlDBEmpty -p 6617:1433 --volume mydb_sqlserver:/var/opt/sqlserver -it -d my_db_image_empty
    docker run --name SqlDBEmpty -p 6618:1433 --volume mydb_sqlserver:/var/opt/sqlserver -it -d my_db_image_filled


## Blazor Server app

Or from Image

    docker build --tag=my_blazor_app ./BlazorInABox

Then Run the image

    docker run --name blazor_app -p 5080:80 -d my_blazor_app


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

- https://hub.docker.com/_/microsoft-dotnet-nightly-sdk/

To run : 

    docker pull mcr.microsoft.com/dotnet/nightly/sdk:7.0
    docker run -it -d --name dotNetPreview mcr.microsoft.com/dotnet/nightly/sdk:7.0
    docker exec -it dotNetPreview bash

Create a new app

    dotnet --info

    dotnet new console --name appTest

    dotnet run --project appTest/ 

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
