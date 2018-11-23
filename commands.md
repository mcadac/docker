# Docker commands

1. Docker info
2. Docker swarm init
3. Docker node ls
4. Docker service  --> replace docker run (docker run never recover a container)
	docker service create alpine ping 8.8.8.8
	docker service ps {service_name or id}
	docker service rm {service_name}
	
5. docker swarm leave --force

7. docker swarm init
8. docker network create --dirver overlay mydrupal
9. docker network ls
10.docker service create --name psql --network mydrupal -e POSTGRES_PASSWORD=mypass postgres
11.docker service ps psql
12.docker container ls
13.docker container logs {container_name}
14.docker service create --name drupal --network mydrupal -p 80:80 drupal
15.docker service ls
16.open browser with you ip or http://localhost
17. postgres -> db passdvance options --> {psql_service_name} "psql" -> default port
18. docker service ps drupal
19. docker service inspect drupal
20. docker service create --name search --replicas 3 -p 9200:9200 elasticsearch:2
21. docker service ps {service_name}	



Practice:

	#network
	docker network create --driver overlay backend
	docker network create --driver overlay frontend
	 
	#vote
	docker service create --name vote --network frontend -p 80:80 --replicas 2 dockersamples/examplevotingapp_vote:before
	 
	#redis
	docker service create --name redis --network frontend redis:3.2
	 
	#worker
	docker service create --name worker --network frontend --network backend dockersamples/examplevotingapp_worker
	 
	# db
	docker service create --name db --network backend --mount type=volume,source=db-data,target=/var/lib/postgresql/data -e POSTGRES_PASSWORD=mypass postgres:9.4
	 
	# result
	docker service create --name result --network backend -p 5001:80 dockersamples/examplevotingapp_result:before


22. docker swarm leave
23. docker swarm init 
24. got to directory swarm-stack-1
25. docker stack deploy -c {compose-file} {app}
26. docker stack ls
27. docker stack services voteapp

28. go to firectory secrets-sample-1
29. docker secret create psql_user psql_user.txt
30. echo "myDBpassWord" | docker secret create psql_pass -
31. docker secret ls
	
	If there is a problem with the next command, attempt to use powershell
32. docker service create --name psql --secret psql_user --secret psql_pass -e POSTGRES_PASSWORD_FILE=/run/secrets/psql_pass -e POSTGRES_USER_FILE=/run/secrets/psql_user postgres
33. docker exec -it {container_name} bash


Practice:
	docker-compose.yml:
		# create your drupal and postgres config here, based off the last assignment
		version: '3.1'

		services:
			drupal:
				image: drupal:8.2
				ports:
					- "8081:80"
				volumes:
					- drupal-modules:/var/www/html/modules
					- drupal-profiles:/var/www/html/profiles
					- drupal-sites:/var/www/html/sites
					- drupal-themes:/var/www/html/themes
					
			postgres:
				image: postgres:9.6
				environment:
					- POSTGRES_PASSWORD_FILE=/run/secrets/psql-pw
				secrets:
					- psql-pw
				volumes:
					- drupal-data:/var/lib/postgresql/data
					
		volumes:
			drupal-data:
			drupal-modules:
			drupal-profiles:
			drupal-sites:
			drupal-themes:
			
		secrets:
			psql-pw:
				external: true

	docker secret:
		echo "testDocker" | docker secret create psql-pw -
	docker stack deploy -c docker-compose.yml drupal
	docker stack ps drupal
	docker stack ls


34. docker swarm leave
35. go to secrets-sample-2 directory
36. docker-compose up -d
37. docker-compose exec psql cat /run/secrets/psql_user

38. got to swarm-stack-3 directory
39. docker-compose up -d
40. docker inspect {}
41. docker-compose down

42. docker service create -p 8088:80 --name web nginx:1.13.7
43. docker service ls
44. docker service scale web=5
45. docker service update --image nginx:1.13.6 web
46. docker service update --publish-rm 8088 --publish-add 9090:80 web
47. docker service rm web

48. docker container run --name p2 -d --health-cmd="pg_isready -U postgres || exit 1" postgres
	
49. Go to swarm-stack-5 directory
50. docker stack deploy -c example-voting-app.yml vote
51.	docker exec $(docker ps --filter name=vote_vote -q) ./generate-some-votes.sh
52. docker service ls
53. docker service logs {service_name}
54. docker service logs --tail 5 --follow vote_worker

55. got to swarm-stack-6 directory
55. docker stack deploy -c .\example-voting-app-stack.yml vote
56. docker config create vote-nginx-20181031
57. docker config ls
58. docker service create --config source=vote-nginx-20181031,target=/etc/nginx/conf.d/default.conf -p 9000:80 --network vote_fronted --name proxy nginx
59. docker config inspect vote-nginx-20181031
60. docker service inspect proxy

61. docker exec -it {name_container} bash
	1. psql -h postgres -U postgres
	2. SELECT 1;

62. docker tag {docker_image} registry01.payulatam.com:5000/payu/sonar:1.0.0
63. sed -i 's/\r//g' filename    "cambia el formato del archivo crlf to lf"
64. git config core.autocrlf input "No cambia los archivos sh"
65. docker-app render | docker stack deploy -c - hello-docker-app  "Print docker compose file and docker stack deploy command use that information"
66. for instances docker-app experimental:
	- https://github.com/docker/app/tree/master/e2e/testdata/templates/gotemplate
	- https://github.com/docker/app/issues/411

67. docker run -it --rm -v "$(pwd)/challenge.php:/home/challenge.php" php:latest /bin/bash


DOCKER-APP:
	e.g. docker-app init --single-file testcamilo or docker-app init testcamilo2

		Create using default settings:
		
			1. docker-app init --single-file hello-world
			2. nano hello-world.dockerapp -> add docker-compose and default settings
			3. 
				3.1 docker-app render > docker-hello-compose.yml 
					or 
				3.2 docker-app render | docker stack deploy -c - hello-docker-app
				
			4. docker stack deploy -c  docker-hello-compose.yml hello-docker-app

		Create using param settings:

		Create using settings file:
	

	MERGE
		1. docker-app init testapp
		2. From directory to app-name.dockerapp file 
		3. docker-app merge
		
	SPLIT
		From app-name.dockerapp file to directory with (docker-compose, metadata and settings files)
		1. docker-app split
		
	INSPECT
		See app docker compose and defaults settings
		1. docker-app inspect {app-docker-name}
			
	
 192.168.65.3:2377
 docker visualizer -> reasearch abaout this
 docker service create --name=viz --publish=8080:8080/tcp --constraint=node.role==manager --mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock bretfisher/visualizer
 
 
 
67. docker run -it --rm php:latest /bin/bash
68. docker build . -t {image-repository-name}
69. docker run -it --rm {image} /bin/bash
70. docker run -it -P --link "{container_name:{alias_host}}" {image}
71. docker stop $(docker ps -q)
72. docker stats    ---> **Allow see docker resource usages**
73. **On windows is '(^\[|Download\\w+:)' and on linux is '(^\\[|Download\\w+:)'**
	export JAR_VERSION=$(mvn org.apache.maven.plugins:maven-help-plugin:2.1.1:evaluate -Dexpression=project.version|grep -Ev '(^\[|Download\\w+:)')

	docker image build \
		--tag registry01.payulatam.com:5000/payu/credibanco-adapter:latest \
		--build-arg APP_JAR=credibanco-adapter-${JAR_VERSION}.jar ./
	

## Docker files
1. RUN apt-get update && apt-get install -y --no-install-recommends vim && apt-get clean
2. HEALTHCHECK --interval=20s --retries=5 --timeout=30s --start-period=5s CMD curl -I -f "http://localhost:8080" || exit 1
