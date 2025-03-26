[![docker pulls](https://img.shields.io/docker/pulls/haxqer/jira.svg)](https://hub.docker.com/r/haxqer/jira/)  [![docker stars](https://img.shields.io/docker/stars/haxqer/jira.svg)](https://hub.docker.com/r/haxqer/jira/) [![image metadata](https://images.microbadger.com/badges/image/haxqer/jira.svg)](https://microbadger.com/images/haxqer/jira "haxqer/jira image metadata")

[kubernetes helm charts](https://github.com/haxqer/charts)


# jira

[README](README.md) | [中文文档](README_zh.md)

+ Long Term Support Version(支持 arm64&amd64): v9.12.18
+ Latest Version(支持 arm64&amd64): v9.17.5
+ Beta Version(支持 arm64&amd64): v10.4.1
+ [新的使用方式](https://github.com/haxqer/jira/tree/build-your-own) ，您可方便自行升级、修改各参数，支持https (感谢 [xsharp](https://github.com/xsharp)).

新的Confluence/Jira版本仅支持数据中心许可证

默认端口: `8080`


视频教程:


[![Jira Complete Installation Guide](https://img.youtube.com/vi/en-h7m2Xv5I/0.jpg)](https://www.youtube.com/watch?v=en-h7m2Xv5I "Jira Complete Installation Guide")


## 环境要求
- docker: 17.09.0+
- docker-compose: 1.24.0+

## 使用 docker-compose 启动 jira

-  启动 jira & mysql

```
    git clone https://github.com/haxqer/jira.git \
        && cd jira \
        && git checkout rm \
        && docker-compose pull \
        && docker-compose up
```

- 以守护进程的方式启动 jira & mysql

```
    docker-compose up -d
```

- 默认的 数据库(mysql8.0) 配置:

```bash
    driver=mysql8.0
    host=mysql-jira
    port=3306
    db=jira
    user=root
    passwd=123456
```

## 使用 docker 启动

- 启动 jira

```
    docker volume create jira_home_data && docker network create jira-network && docker run -p 8080:8080 -v jira_home_data:/var/jira --network jira-network --name jira-srv -e TZ='Asia/Shanghai' haxqer/jira:9.17.5
```

- 然后配置你的数据库


## 破解 jira

```
docker exec jira-srv java -jar /var/agent/atlassian-agent.jar \
    -d \
    -p jira \
    -m Hello@world.com \
    -n Hello@world.com \
    -o your-org \
    -s you-server-id-xxxx
```

初次安装 jira 时，安装页面会显示你的 server-id

## 破解 jira 的插件

- 例如: 你想要破解 BigGantt 插件
1. 从 jira marketplace 中安装 BigGantt 插件
2. 查看 BigGantt 的 `App Key` 是 : `eu.softwareplant.biggantt`
3. 然后执行 :

```
docker exec jira-srv java -jar /var/agent/atlassian-agent.jar \
    -d \
    -p eu.softwareplant.biggantt \
    -m Hello@world.com \
    -n Hello@world.com \
    -o your-org \
    -s you-server-id-xxxx
```

在 `System` -> `System Info` 页面的 `Server ID` 字段即为你的 server-id

例如:
```
docker exec jira-srv java -jar /var/agent/atlassian-agent.jar \
    -d \
    -p eu.softwareplant.biggantt \
    -m Hello@world.com \
    -n Hello@world.com \
    -o https://jira.haxqer.cm \
    -s BSFN-Q264-VGCZ-0AOE
```

4. 最后粘贴生成的 licence


## 破解 Jira Service Management(jsm)

感谢:
+ [d1m0nstr](https://github.com/d1m0nstr) for [Jira Service Management](https://github.com/haxqer/jira/issues/11)

```
docker exec jira-srv java -jar /var/agent/atlassian-agent.jar \
    -d \
    -p jsm \
    -m Hello@world.com \
    -n Hello@world.com \
    -o your-org/ \
    -s you-server-id
```

