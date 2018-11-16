# Docker-app plugin

Examples using docker-app experimental (gotemplate) with the goal of create docker-compose file more reusable


## Cases, Problems or error

#### 1. Use environment variables with docker-app and docker stack deploy
------------------

   * **Case:** Set environment variables using docker-app and docker stack deploy
   * **System:** 
      * **OS:** Widnows 10
      * **docker:** 18.06
      * **docker-app:** 0.6.0-14
   * **error-messages:**
      * failed to create service alexa_alexandria: Error response from daemon: Mount denied The source path "D:\\develop" is not a valid Windows path
   
   * **Process:**
      1.  Init a docker-app with the next command: 
      ```
      docker-app init env-variables
      ```
      2.  Modify the docker-compose.yml like example in this respository: https://github.com/mcadac/docker/tree/master/docker-app/gotemplate/env-variables
      
      3.  The volumes must be defined the verbose form and it must be the type "bind", because the current docker for windows version when see a env variable, it sets the tipe like "volume" and this is the error:
      ```
      volumes:
      - type: bind
        source: {{.volumes.logs}}
        target: /opt/docker/logs
      ```
      4. Render the docker compose file
      ```
      docker-app render > docker-compose-result.yml
      ```
      5. Deploy service (This example is with a swarm cluster, before to it you need to execute docker swarm init)
      ```
      docker stack deploy -c docker-compose-result.yml hello
      ```
      6. To see that all service deployed ok you can execute the next commands
      ```
      - docker stack ps hello
      - docker service ls
      - docker service logs {hello-service-id-print-last-command}
      ```
#### 2. Other case
------------------ 

#### 3. Other case
-----------------
