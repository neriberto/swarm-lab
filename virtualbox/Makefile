token=0123456789abcdef

default:
	@docker-machine ls

swarm:
	@docker-machine create -d virtualbox --swarm --swarm-master --swarm-discovery token://${token} manager
	@docker-machine create -d virtualbox --swarm --swarm-discovery token://${token} worker-01
	@docker-machine create -d virtualbox --swarm --swarm-discovery token://${token} worker-02
	@eval $$(docker-machine env manager) && docker swarm init --advertise-addr `(docker-machine ip manager)`

join:
	@eval $$(docker-machine env worker-01) && docker swarm join --token ${token} `(docker-machine ip manager)`:2377
	@eval $$(docker-machine env worker-02) && docker swarm join --token ${token} `(docker-machine ip manager)`:2377
	@eval $$(docker-machine env manager) && docker node promote worker-01 worker-02

uninstall:
	@docker-machine rm manager worker-01 worker-02

portainer:
	@docker service create --name portainer --publish 9000:9000 --constraint 'node.role == manager' --mount type=bind,src=//var/run/docker.sock,dst=/var/run/docker.sock portainer/portainer -H unix:///var/run/docker.sock
