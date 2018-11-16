# Docker-app plugin

Examples using docker-app experimental (gotemplate) with the goal of create docker-compose file more reusable


# Cases, Problems or error

### 1.Use environment variables with docker-app and docker stack deploy
------------------

   * **Case:** Set environment variables using docker-app and docker stack deploy
   * **System:** 
      * **OS:** Widnows 10
      * **docker:** 18.06
      * **docker-app:** 0.6.0-14
   * **error-messages:**
      * failed to create service alexa_alexandria: Error response from daemon: Mount denied The source path "D:\\develop" is not a valid Windows path
   
   * **Process:**
      a.  Init a docker-app with the next command: 
      ```
      docker-app init test-env
      ```
      b.  Modify the docker-compose.yml
      c.  The volumes must be defined the verbose form
    
    
