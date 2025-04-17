# WordPress 开发环境 (Docker Compose)

本项目使用 Docker Compose 快速搭建一个本地 WordPress 开发环境。

## 包含的服务

此 Docker Compose 配置包含以下两个主要服务：

1.  **WordPress**:
    *   使用官方 `wordpress` 镜像。
    *   服务将在宿主机的 `8080` 端口上暴露。
    *   WordPress 的文件存储在名为 `wordpress` 的 Docker 卷中，以实现数据持久化。
2.  **MySQL 数据库 (db)**:
    *   使用 `mysql:8.0` 镜像。
    *   作为 WordPress 的数据库后端。
    *   数据库文件存储在名为 `db` 的 Docker 卷中，以实现数据持久化。
    *   数据库连接信息在 `docker-compose.yml` 文件中通过环境变量配置。

## 配置文件

*   **`docker-compose.yml`**: 定义了上述服务及其配置、网络和卷。这是启动环境的主要文件。
*   **`nginx.conf`**: (当前 `docker-compose.yml` 未使用) 提供了一个基础的 Nginx 配置示例，用于反向代理 WordPress。如果需要使用 Nginx，需要修改 `docker-compose.yml` 文件来包含并配置 Nginx 服务。
*   **`uploads.ini`**: (当前 `docker-compose.yml` 未使用) 提供了 PHP 文件上传相关的配置。如果需要自定义 PHP 上传限制，需要修改 `docker-compose.yml` 文件中的 WordPress 服务，将此文件挂载到容器的 PHP 配置目录 (`/usr/local/etc/php/conf.d/uploads.ini`)。

## 如何运行

### 前提条件

*   确保您的系统已安装 [Docker](https://www.docker.com/get-started) 和 [Docker Compose](https://docs.docker.com/compose/install/)。

### 启动环境

1.  打开命令行或终端。
2.  导航到包含 `docker-compose.yml` 文件的目录 (`f:\A_Hanami\Hanami_wordpress\dokcer-compose-dev\`)。
3.  运行以下命令以后台模式启动服务：
    ```bash
    docker-compose up -d
    ```

### 访问 WordPress

启动成功后，您可以通过浏览器访问 `http://localhost:8080` 来进行 WordPress 的安装和配置。

### 数据库连接信息

WordPress 服务已配置为连接到 `db` 服务。以下是 `docker-compose.yml` 中定义的环境变量：

*   数据库主机: `db`
*   数据库用户: `hanami`
*   数据库密码: `hanami`
*   数据库名称: `wordpress_db`

首次访问 WordPress 时，安装程序可能会要求您输入这些信息（尽管通常会自动配置）。

### 停止环境

要停止并移除由 Docker Compose 创建的容器、网络和卷（注意：移除卷会删除数据，如果需要保留数据请只停止容器），请在 `dokcer-compose-dev` 目录下运行：

*   停止并移除容器、网络（不删除卷）：
    ```bash
    docker-compose down
    ```
*   仅停止容器（保留容器、网络和卷）：
    ```bash
    docker-compose stop
    ```

## 数据持久化

*   WordPress 核心文件、主题和插件位于 `wordpress` Docker 卷中。
*   MySQL 数据库数据位于 `db` Docker 卷中。

这意味着即使您停止和移除了容器 (`docker-compose down`)，只要不手动删除卷，您的网站内容和数据库数据也会被保留。下次 `docker-compose up -d` 时会重新使用这些数据。