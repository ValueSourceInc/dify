Dify 部署说明
=============

Dify 原有的Docker 自带 11 个 container: 
此库其中把 `nginx` 和 `db-1` (postgresql) 的container禁掉了

<<<<<<< Updated upstream
<p align="center">
  <a href="https://cloud.dify.ai">Dify Cloud</a> ·
  <a href="https://docs.dify.ai/getting-started/install-self-hosted">Self-hosting</a> ·
  <a href="https://docs.dify.ai">Documentation</a> ·
  <a href="https://dify.ai/pricing">Dify edition overview</a>
</p>
=======
 ✔ Network docker_ssrf_proxy_network  Created                                                                 0.1s 
 ✔ Network docker_default             Created                                                                 0.0s 
 ✔ Container docker-redis-1           Started                                                                 2.4s 
 ✔ Container docker-ssrf_proxy-1      Started                                                                 2.8s 
 ✔ Container docker-sandbox-1         Started                                                                 2.7s 
 ✔ Container docker-web-1             Started                                                                 2.7s 
 ✔ Container docker-weaviate-1        Started                                                                 2.4s 
 X Container docker-db-1              Disabled                                                                 2.7s 
 ✔ Container docker-api-1             Started                                                                 6.5s 
 ✔ Container docker-worker-1          Started                                                                 6.4s 
 X Container docker-nginx-1           Disabled   
>>>>>>> Stashed changes

端口也改了

## 关键文件
- `docker/.env.vs.example`
- `docker/docker-compose.yaml`
- `vs-nginx.example.conf`

## 部署方式:

### Git
```
git clone git@github.com:ValueSourceInc/dify.git
git checkout vs
```

### Nginx
复制 `vs-nginx.example.conf` 到 `/etc/nginx/conf.d`
检查dify.conf 的配置
```
cp vs-nginx.example.conf /etc/nginx/conf.d/dify.conf
nginx -t
systemctl restart nginx
```


### Docker
```
cd docker
cp .env.vs.example .env
```

填写 `.env` 文件里数据库信息
```
DB_USERNAME={数据库用户名}
DB_PASSWORD={数据库密码}
```


#### 启动
启动前确保数据库中有 `dify` db instance
```
docker-compose up -d
```

### 关闭
```
docker-compose down
```

### Troubleshooting

##### DB 中 schema 没生成 服务器报 500 怎么办？
确保数据库中 `dify` db 存在

*重新生成schema*
```
docker compose restart api
```