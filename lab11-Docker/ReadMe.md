# Lab 11 - Part I: Hello Docker (Flask + Redis client)

Build and run the image from this folder:

```bash
docker run hello-world                 # quick check that Docker works
docker build --tag=hellodocker .       # build the image from the Dockerfile
docker image ls                        # the hellodocker image is listed
docker run -p 4000:80 hellodocker      # run it, mapping host port 4000 to container port 80
curl http://localhost:4000             # in a second terminal (or open the URL in a browser)
docker container ls                    # find the running container's ID
docker container stop CONTAINER_ID     # stop it (or Ctrl+C in the terminal running it)
docker container ls                    # confirm nothing is running
```

The page shows `Hello Yash!`, the container's hostname (its container ID) and a
Redis message, because no Redis service is running yet.
