# Run Postgres in Docker

## Download and Run the image

`sudo docker run --name local-pg-1 -e POSTGRES_PASSWORD=mysecretpassword -d postgres`

## Connect to the container

`sudo docker exec -it local-pg-1 psql -U postgres -p5432`


##

sudo docker run --rm -it --name local-pg-1 --volume $(pwd)/tmp/stcpay/issue123:/var/tmp/issue -e POSTGRES_PASSWORD=mysecretpassword -d postgres
