# docker-usage-instructiona

檢查前10名當前資料夾的檔案大小
du -hsx * | sort -rh | head -10

/var/lib/containers 底下會存放各個容器的 log 可以設定小一點或是定期清除，避免太多。

docker-compose system prune 
docker-compose system prune -a 
docker-compose system prune -af 直接刪除所有無用的 images, volume, container,
networks


