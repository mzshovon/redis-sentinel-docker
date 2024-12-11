# redis-sentinel-docker
Redis Sentinel Setup in Docker with multiple slave

## To access redis-master or redis-slave
```bash
docker exec -it redis-master redis-cli -h 172.19.0.2 -p 6479 -a yourmastersecretpassword
docker exec -it redis-slave-1 redis-cli -h 172.19.0.3 -p 6479 -a yourmastersecretpassword
docker exec -it redis-slave-2 redis-cli -h 172.19.0.4 -p 6479 -a yourmastersecretpassword
```

## To get the IP list of redis instances
```bash
docker inspect -f '{{.Name}}: {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -q)

You can see in the console:

/redis-sentinel-2: 172.19.0.7
/redis-sentinel-3: 172.19.0.6
/redis-sentinel-1: 172.19.0.5
/redis-slave-1: 172.19.0.4
/redis-slave-2: 172.19.0.3
/redis-master: 172.19.0.2
```

## To check redis connectivity and access in redis server
```bash
docker exec -it redis-master redis-cli -h 172.19.0.2 -p 6479 -a yourmastersecretpassword 

172.19.0.2:6479> SELECT 1 // Database select
172.19.0.2:6479> KEYS * // Get All keys
172.19.0.2:6479> AUTH yourmastersecretpassword // Access the database
```

## To get the current master
```bash
docker exec -it redis-sentinel-1 redis-cli -p 26479 SENTINEL get-master-addr-by-name mymaster

You can get the current master:
1) "172.19.0.2"
2) "6479"
```

## Set failover to watch the impact of redis switch-over
```bash
docker exec -it redis-sentinel-1 redis-cli -p 26479 SENTINEL failover mymaster
```