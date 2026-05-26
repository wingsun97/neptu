# neptu

PHP 8.3 Docker 镜像仓库，构建于 `ghcr.io/wingsun97/php-cli:8.3-grpc` 之上，托管于 GitHub Container Registry。

## 镜像列表

| 镜像 Tag | 说明 | 扩展 |
|----------|------|------|
| `ghcr.io/wingsun97/neptu:php-8.3-cli` | 基础 CLI 镜像 | bcmath, intl, pdo, pdo_mysql, mysqli, zip, gd, opcache, pcntl + gRPC + redis, igbinary, seaslog, imagick + composer |
| `ghcr.io/wingsun97/neptu:php-8.3-svd` | 增加 supervisor | cli + supervisor (supervisord 作为 CMD) |
| `ghcr.io/wingsun97/neptu:php-8.3-dev` | **DevContainer 专用** | cli + git, curl, sudo, xdebug, php-cs-fixer, phpcs, phpcbf |

## 构建与使用

### php-8.3-dev DevContainer

适合 VS Code Dev Containers 开发的 PHP 环境镜像。

**默认用户**: `dev` (uid=gid=1000)，sudo 免密  
**工作目录**: `/workspace`

#### 推荐 .devcontainer/devcontainer.json 配置

```json
{
    "name": "PHP 8.3 Dev",
    "image": "ghcr.io/wingsun97/neptu:php-8.3-dev",
    "mounts": [
        "source=composer-cache,target=/home/dev/.composer/cache,type=volume"
    ],
    "customizations": {
        "vscode": {
            "extensions": [
                "bmewburn.vscode-intelephense-client",
                "xdebug.php-debug",
                "neilbrayfield.php-docblocker",
                "DEVSENSE.phptools-vscode",
                "esbenp.prettier-vscode",
                "eamodio.gitlens"
            ]
        }
    }
}
```

#### 插件说明

| 扩展 ID | 用途 |
|---------|------|
| `bmewburn.vscode-intelephense-client` | PHP 智能感知 + 补全 |
| `xdebug.php-debug` | PHP 调试 (配合 xdebug) |
| `neilbrayfield.php-docblocker` | 自动生成 PHPDoc |
| `DEVSENSE.phptools-vscode` | PHP 格式化 / 语法检查 |
| `esbenp.prettier-vscode` | 通用格式化 |
| `eamodio.gitlens` | Git 增强 (需镜像中有 git) |

#### 构建 dev 镜像

```bash
docker build -f php-8.3/Dockerfile-Dev -t ghcr.io/wingsun97/neptu:php-8.3-dev .
```

## 构建与使用

构建命令示例 (构建所有 php-8.3 镜像):

```bash
docker build -f php-8.3/Dockerfile-Cli -t ghcr.io/wingsun97/neptu:php-8.3-cli .
docker build -f php-8.3/Dockerfile-Svd -t ghcr.io/wingsun97/neptu:php-8.3-svd .
docker build -f php-8.3/Dockerfile-Dev -t ghcr.io/wingsun97/neptu:php-8.3-dev .
```

## CI 自动构建

推送至 `main` 分支时，GitHub Actions 自动构建并推送至 GHCR。

## 自定义镜像 Tag

修改 `build.yml` 中的 `PHP_*_TAG` 环境变量即可控制输出的 Docker 标签名。如需扩展新版本 (如 PHP 8.4)，复制 `php-8.3/` 目录并更新 Dockerfile 中的基础镜像即可。