# How to deploy

1. kubectl create -f voting-app-pod.yml
2. kubectl create -f voting-app-service-lb.yml
3. kubectl create -f redis-pod.yml
4. kubectl create -f redis-service.yml
5. kubectl create -f postgres-pod.yml
6. kubectl create -f postgres-service.yml
7. kubectl create -f worker-app-pod.yml
8. kubectl create -f result-app-pod.yml
9. kubectl create -f result-app-service-lb.yml
