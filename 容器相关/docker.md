# 基本操作

```
docker ps #查看
docker rm -f $(docker ps -aq) #删除所有
docker exec -it ${name} /bin/bash
#批量清除
docker container prune
docker image prune
docker network prune
docker volume prune

```

