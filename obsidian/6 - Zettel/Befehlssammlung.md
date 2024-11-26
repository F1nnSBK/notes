#Note
2024-11-06

Tags: [[Java]] [[Python]] [[macOS]] [[Programmierung]] [[Docker]]

#### Google Cloud
- Project ändern
	- `gcloud config set project <project_id>` Projekt ändern.
	- `gcloud auth application-default set-quota-project <project_id>` setzt die richtigen Berechtigungen für das neue Projekt.
- Cloud Run Service 
	- `gcloud storage buckets notifications list gs://example-bucket
	- `gcloud storage buckets notifications describe projects/_/buckets/svg-dcc-raw-tst/notificationConfigs/92`
	- `gcloud run deploy sdd-file-dispatch-service --image europe-west3-docker.pkg.dev/svg-dcc-tst-cloudservices-aa07/docker/sdd-file-dispatch-service@sha256:c3f3831919c5a86e27b56aa689ccb29b43d931552e43a85cd084ec811d8d101b --set-env-vars SENDER_EMAIL_PASSWORD=dN04d2023+Sv,EMAIL_API_ENDPOINT=https://sdd-email-sending-service-dev-396758236115.europe-west4.run.app/v1/send-mail`

#### Python
#### Airflow 
- Start airflow docker container:
	`source /Users/finnhertsch/venv/composer-local-venv/bin/activate && cd /Users/finnhertsch/composer && composer-dev restart composer-local-instance`

#### Tools 
#### Git
+ Branch löschen
	+ `git branch -d <branch_name>` löscht den lokalen branch
	+ `git push -d <remote_name> <branch_name>` löscht den remote branch. Oft ist `<remote_name>` origin.

#### Docker
+ Docker build
	+ `docker build -t <image_name> .` baut das Docker image mit der dockerfile im aktuellen Verzeichnis.
	+ `docker run -p 8080:8080 <image_name>` startet das Docker Image auf einem bestimmten Port. Kann auch ohne `-p` Flag genutzt werden.
- Docker compose command:
	- `docker compose build --no-cache` use local Dockerfile to build a container without cache
	- `docker compose up` start docker container
	- `docker compose down` stop docker container
- Show running docker containers:
	`docker ps` shows all running containers and their ports
- Show docker stats:
	`docker stats`  shows stats of running containers

##### Tree
- Tree with full recursion depth:
	`tree <directory>`
	example: `tree . ` current directory
- Tree with custom recursion depth:
	`tree -L <depth: number> <directory>
	example: `tree -L 2 .` tree of current directory with depth = 2


---
## Info