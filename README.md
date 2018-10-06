# Ark v1 Docker deployment (mainnet)

## What is this?

Start an Ark v1 node using a pre-built Ark and customized Postgres image. Current implementation can
only be used with mainnet!


## How to use it?

1. Copy `docker-compose.yml` into your server
2. Start the DB: `docker-compose up -d db`
3. Download fresh DB image and restore it: `docker-compose exec -d db make restore`
4. DO NOT START THE NODE - wait for the database to be fully restored. This can easily take up to 20
minutes, depending on your specs and how many other nodes are downloading the DB image. You can
check if the database is still restoring by checking if `pg_restore` process is running:
`docker-compose exec -T db /bin/bash -c 'ps aux | grep pg_restore'`
5. Start the node: `docker-compose up -d --no-recreate node`


## Node is in a weird state, how can I rebuild it?

1. Stop all running container: `docker-compose kill`
2. Remove database container: `docker-compose rm db`
3. Restore the database: `docker-compose exec -d db make restore`
4. DO NOT START THE NODE - wait for the database to be fully restored. This can easily take up to 20
minutes, depending on your specs and how many other nodes are downloading the DB image. You can
check if the database is still restoring by checking if `pg_restore` process is running:
`docker-compose exec -T db /bin/bash -c 'ps aux | grep pg_restore'`
5. Start the node: `docker-compose up -d --no-recreate node`


## How can I restart the node?

1. Restart the node container: `docker-compose restart node`

## Why so many steps?

Given the nature of Ark v1 you can't just start the node by running `docker-compose up`, this is
because the database needs to be restored before the node gets started. That's why it needs to be
start separately.


## Few things to note

1. Both arkv1 and postgres images are built by roks0n and are not built automatically on every new
update release which can result in an outdated version.
2. `db` container does not have a persistent database.
3. Despite it's difficult to do so, do not even try running a forging node.
4. Current implementation can only be used for mainnet.
