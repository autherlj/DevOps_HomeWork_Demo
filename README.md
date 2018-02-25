# 如何完成一个 基于DCOS + Jenkins 的PHP + MySQL 的 持续集成。

> 目标：基于DCOS ，在 DCOS的Jenkins里面用 Docker 镜像的方式Build，然后通过Marathon Deployment。

>本项目代码维护在 **[Liang Jun / php-mysql](https://gitlab.nlabs.sg/liangjun/php-mysql.git)** 项目中。

### 创建 PHP 应用容器

> 因所有官方镜像均位于境外服务器，为了确保所有示例能正常运行，用到了DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

首先，选择官方的 PHP 镜像作为项目的基础镜像。

```dockerfile
FROM daocloud.io/php:5.6-apache
```

接着，用官方 PHP 镜像内置命令`docker-php-ext-install`安装 PHP 的 MySQL 扩展依赖。

```dockerfile
RUN docker-php-ext-install pdo_mysql
```

* 依赖包通过 `docker-php-ext-install` 安装，如果依赖包需要配置参数则通过 `docker-php-ext-configure` 命令。
* 安装 `pdo_mysql` PHP 扩展。

然后，将代码复制到目标目录。

```dockerfile
COPY . /var/www/html/
```
#Docker registry URL 
https://docker.dcos.demo01.nlabs.sg

因为基础镜像内已经声明了暴露端口和启动命令，此处可以省略。

至此，包含 PHP 应用的 Docker 容器已经准备好了。PHP 代码中访问数据库所需的参数，是通过读取环境变量的方式声明的。

```php
$serverName =   env("MYSQL_PORT_3306_TCP_ADDR", "localhost");
$databaseName = env("MYSQL_INSTANCE_NAME", "homestead");
$username =     env("MYSQL_USERNAME", "homestead");
$password =     env("MYSQL_PASSWORD", "secret");

/**
 * 获取环境变量
 * @param $key
 * @param null $default
 * @return null|string
 */
function env($key, $default = null)
{
    $value = getenv($key);
    if ($value === false) {
        return $default;
    }
    return $value;
}
```

这样做是因为在 Docker 化应用开发的最佳实践中，通常将有状态的数据类服务放在另一个容器内运行。

### 与DCOS MySQL 数据容器（绑定）

首先，在DCOS创建一个 MySQL 容器。

```参考
http://nlabs-lj187m.southeastasia.cloudapp.azure.com/#/services/
mysql 
```

之后，通过 DCOS  容器间Environment Variables，便可将 MySQL 暴露给应用容器。
```参考
php-mysql.json


### php-apache-mysql-sample 应用截图

![php-apache-mysql-sample](/php-apache-mysql.png "php-apache-mysql")

### php-apache-mysql-sample 应用入口
http://nlabs-lj187a.southeastasia.cloudapp.azure.com/index.php

