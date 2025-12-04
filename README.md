# Antigravity2api-nodejs

## 项目说明

当前项目是基于[liuw1535](https://github.com/liuw1535)大佬的(antigravity2api-nodejs)[https://github.com/liuw1535/antigravity2api-nodejs]做的修改和迭代

## 免责声明

仅供个人学习，出事与项目负责人无关

## 启动方式

编写`docker-compose.yml`文件

> 必须设置的三个环境变量
> 1. `API_KEY`：调用API的KEY
> 2. `PANEL_PASSWORD`:面板登录密码
> 3. `PANEL_USER`:面板用户名

~~~yml
version: '3.8'
networks:
  1panel-network:
    external: true
services:
  antigravity-api:
    image: ghcr.io/zhongruan0522/Antigravity2api-node-js:latest
    container_name: antigravity-api
    restart: unless-stopped
    networks:
      - 1panel-network
    ports:
      - "8045:8045"
    environment:
      - PANEL_USER=admin
      - PANEL_PASSWORD=设置你的密码
      - API_KEY=sk-text
    volumes:
      - ./data:/app/data
    healthcheck:
      test: [ "CMD", "node", "-e", "require('http').get('http://localhost:' + process.env.PORT + '/healthz', (res) => { process.exit(res.statusCode === 200 ? 0 : 1) }).on('error', () => process.exit(1))" ]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
~~~

输入`docker compose up -d`启动，启动成功后访问`http://IP:8045`即可访问面板

## 环境变量配置

详见`.env.example`文件