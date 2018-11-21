# laradock地雷合集

**Apache2 up状态却无法访问网站**

```sh
docker-compose exec apache2 bash # 进入容器交互界面
/etc/init.d/apache2 restart # 查看报错信息(可能是虚拟主机配置错误)
```

**解压PHP源码，安装PHP扩展**

```sh
RUN docker-php-source extract \
    # 此处开始执行你需要的操作 \
    && cd /usr/src/php/ext/pgsql\
    && phpize \
    && ./configure \
    && make \
    && make install \
    && docker-php-ext-enable pgsql.so \
    && docker-php-source delete
```

**memcached无法set数据**

***1.查看容器ip***
> docker network ls
> docker network inspect laradock-backend

***2.添加路由***
> route -p add 172.18.12.0 MASK 255.255.255.0 10.0.75.2

***3.new memcached***
```php
$memcache = new Memcached;
$memcache->addServer('10.0.75.2', 11211) or die('Could not connect');
```

**切换mysql版本后，重新build后无法启动mysql**
> 原因：从8切换到5.7，volume数据不兼容

```yaml
处理方案：根据版本volume数据
 volumes:
        - ${DATA_PATH_HOST}/mysql/${MYSQL_VERSION}:/var/lib/mysql
        - ${MYSQL_ENTRYPOINT_INITDB}:/docker-entrypoint-initdb.d
```