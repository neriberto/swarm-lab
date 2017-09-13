default:
	@docker-machine ls

swarm:
	@docker-machine create -d virtualbox manager
	@docker-machine create -d virtualbox worker-01
	@docker-machine create -d virtualbox worker-02
init:
	@eval $(docker-machine env manager)
	@docker swarm init --advertise-addr `(docker-machine ip manager)`

uninstall:
	@docker-machine rm manager worker-01 worker-02

portainer:
	@docker service create --name portainer --publish 9000:9000 --constraint 'node.role == manager' --mount type=bind,src=//var/run/docker.sock,dst=/var/run/docker.sock portainer/portainer -H unix:///var/run/docker.sock
