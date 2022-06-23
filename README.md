# Docker-is-a-Gateway-Drug

Talk given at DDD South West 2022

## Dooom!



    docker pull kasmweb/doom:1.10.0
    docker run -d --name Doooom --rm  -it --shm-size=512m -p 6901:6901 -e VNC_PW=password kasmweb/doom:1.10.0 

    The container is now accessible via a browser : https://127.0.0.1:6901

    User : kasm_user
    Password: password


# Junk below here

## Doom fallback

    docker pull kasmweb/doom:1.9.0
    docker run --rm  -it --shm-size=512m -p 6901:6901 -e VNC_PW=password kasmweb/doom:1.9.0
