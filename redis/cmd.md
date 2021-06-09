### pull image
```
docker pull redis:5.0.3
```

### 启动镜像
```
docker run -d --name redis -p 6379:6379 -v ~/workspace/code/devops/docker/redis/conf/redis.conf:/redis.conf -v redis:5.0.3 redis-server --appendonly yes
```

