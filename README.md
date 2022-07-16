# Lsky-Pro Docker镜像

每天自动拉取最新代码构建Docker镜像

## 使用方法

```docker
docker run -d \
    --name lsky-pro \
    --restart unless-stopped \
    -p 9080:80 \
    -v /path-to-data:/var/www/html \
    halcyonazure/lsky-pro-docker:latest
```

## 反代HTTPS

如果使用了Nginx反代后，如果出现无法加载图片的问题，可以根据原项目 [#317](https://github.com/lsky-org/lsky-pro/issues/317) 执行以下指令来手动修改容器内`AppServiceProvider.php`文件对于HTTPS的支持

***Tips：将lskypro改为自己容器的名字***

```bash
docker exec -it lskypro sed -i '32 a \\\Illuminate\\Support\\Facades\\URL::forceScheme('"'"'https'"'"');' /var/www/html/app/Providers/AppServiceProvider.php
```

## Docker-Compose部署

使用`MySQL`来作为数据库的话参考内容如下

```yaml
version: '3'
services:
  lskypro:
    image: halcyonazure/lsky-pro-docker:latest
    container_name: lskypro
    restart: unless-stopped
    volumes:
      - /root/lskypro/html:/var/www/html
    ports:
      - 9080:80
    
    depends_on:
      - db
    
 db:
    image: mysql:5.7.22
    container_name: mysql
    restart: unless-stopped
    command: --default-authentication-plugin=mysql_native_password
    volumes:
      - /root/lskypro/mysql/data:/var/lib/mysql
      - /root/lskypro/mysql/conf:/etc/mysql
      - /root/lskypro/mysql/log:/var/log/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=TWqiVWMqN7mSTu
      - MYSQL_DATABASE=lskydb   

    
```

原项目：[兰空图床](https://github.com/lsky-org/lsky-pro)
