#Note
2024-11-06

Tags: [[Java]] [[Python]] [[macOS]] [[Programmierung]] [[Docker]]

#### Docker
- Docker compose command:
	- `docker compose build --no-cache` use local Dockerfile to build a container without cache
	- `docker compose up` start docker container
	- `docker compose down` stop docker container
- Show running docker containers:
	`docker ps` shows all running containers and their ports
- Show docker stats:
	`docker stats`  shows stats of running containers

#### Python
#### Airflow 
- Start airflow docker container:
	`source /Users/finnhertsch/venv/composer-local-venv/bin/activate && cd /Users/finnhertsch/composer && composer-dev restart composer-local-instance`

#### Tools 
##### Tree
- Tree with full recursion depth:
	`tree <directory>`
	example: `tree . ` current directory
- Tree with custom recursion depth:
	`tree -L <depth: number> <directory>
	example: `tree -L 2 .` tree of current directory with depth = 2


---
## Info