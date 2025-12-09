## docker issuse https://www.docker.com/blog/docker-engine-version-29/

$env:DOCKER_API_VERSION=1.44
PS C:\Users\sathish.thangavel> echo $env.DOCKER_API_VERSION
PS C:\Users\sathish.thangavel> $env:DOCKER_API_VERSION= "1.44"
PS C:\Users\sathish.thangavel> echo $env.DOCKER_API_VERSION

## kubectl alias for minikube

<!-- function kubectl { minikube kubectl -- @args } -->

1. Build start to end like to story
2. Give summary for each slide
3. Review all the yaml files as its is not working
4. Say what to watchout in production & best practices wherever applicable
5. QA Break Points 5 mins wherever applicable

## notes

1. minikube delete --all --purge
2. minikube start --driver=docker --cpus=8 --memory=12000
3. minikube node add --worker
4. minikube node add --worker

