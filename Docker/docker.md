# 2022-05-24
## remove <none> images
```
docker rmi $(docker images -f "dangling=true" -q)
```

