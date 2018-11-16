# Examples about docker

### 1. How to connect a docker container to a service or database deployed in my host

  * **Docker version:** 18.06.1-ce
  * **OS:** Windows 10
  * **Description:** To connect a docker container to my postgres database configurated and deployed in my pc (host), you cannot use 
  **"localhost"** or **"your own ip"**.Then you have to use the next property **"host.docker.internal"** to replace the ip where is our database (For more inforamtion read the references).
  * **Example:** "jdbc:postgresql://host.docker.internal:5432/{database_name}"
  * **References:** https://docs.docker.com/docker-for-windows/networking/#there-is-no-docker0-bridge-on-windows

