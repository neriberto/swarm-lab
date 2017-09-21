OPT_GENERIC=-d generic --generic-ip-address
OPT_GENERIC_USER=--generic-ssh-user my_username

default:
	@docker-machine ls

swarm:
	@docker-machine create ${OPT_GENERIC} 172.16.1.31 ${OPT_GENERIC_USER} labvm01
	@docker-machine create ${OPT_GENERIC} 172.16.1.32 ${OPT_GENERIC_USER} labvm02
	@docker-machine create ${OPT_GENERIC} 172.16.1.33 ${OPT_GENERIC_USER} labvm03
	@eval $(docker-machine env labvm02)
	@docker swarm init --advertise-addr 172.16.1.32

uninstall:
	@docker-machine rm labvm01 labvm02 labvm03

token:
	@eval $(docker-machine env labvm02)
	@docker swarm join-token worker

portainer:
	docker service create --name portainer --publish 9000:9000 --constraint 'node.role == manager' --mount type=bind,src=//var/run/docker.sock,dst=/var/run/docker.sock --mount type=bind,src=//srv/laboratorio/portainer_service,dst=/data portainer/portainer -H unix:///var/run/docker.sock
