slice:
	docker run -it romainbou/docker-cura-engine:3.4 slice
run:
	docker run -it romainbou/docker-cura-engine:3.4
run-it:
	docker run -it --entrypoint "/bin/bash" romainbou/docker-cura-engine:3.4
run-it-build:
	docker run -it --entrypoint "/bin/bash" romainbou/docker-cura-engine-build:3.4

image:
	docker build -t romainbou/docker-cura-engine:3.4 .
build:
	docker build -t romainbou/docker-cura-engine-build:3.4 -f Dockerfile.build .