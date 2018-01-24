## Docker 学习笔记

1. 批量删除docker容器
sudo docker ps -a | grep 'Exited' | awk '{print $1}' | xargs sudo docker stop | xargs sudo docker rm
