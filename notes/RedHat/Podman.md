### Naming Convention
- {image registry}/{account}/{image}:{tag}
- example: quay.io/rdacosta/pastebin:latest

### Podman Commands
- For help
```shell
podma --help
man podman-<command>
example: man podman-run
```
- Check running containers
  ```shell
  podman ps
```
- Check available images
  ```shell
  podman images
```
- To login into ur account
  ```shell
  podman login -u <account name> <registry name>
  example: podman login -u rdacosta quay.io
```
- To pull the image from a registry
  ```shell
  podman pull <image location>
```
- To run an image
  ```shell
  podman run <image name or path>
  example  podman run pastebin (pastebin already pulled form   registry) 
```
- To do portforwarding and run image in background
  ```shell
  podman run -d -p 8081:5000 --name pastebin1 quay.io/rdcista.pastebin:latest
  # here -d specifies to run in background
  # -p flag specifies portforwarding from os port 8081 traffic to containers 5000 port
```
- Show all the containers that are running and were running once upon a time
  ```shell
  podman -a ps
```
- To remove a running container
  ```shell
  podman rm <image name>
```
- To execute a cmmand inside the container
  ```shell
  # -it flag for interactive terminal 
  podman exec -it pastebin bash
```
- To see all the process running under a container
  ```shell
  # Run this in the container shell
  ps -ef
```
-  To externally specify enviornment variables 
  ```shell
  podman run <image> -e MY_STRING=foo
```

### Network Basics
- For help
  ```shell
  podman network --help
```

- To Inspect just the network settings
  ```shell
  podman inspect friendly_jowalevski | jq '.[0].NetworkSettings'
```
- Network inspect command
  ```shell
  podman network inspect <id/netowrk name>
```
- To view network list
  ```shell
  podman network ls
```
- To create a new network
  ```shell
  podman network create webapps-net
```
- Run a container image in specified network
  ```shell
podman run -d --name my-container --network example-net container-image:latest
```
- Remove a network
  ```shell
  podman remove <network name>
```

-  To see details about port forwarding 
  ```shell
  podman port <name of container>
```