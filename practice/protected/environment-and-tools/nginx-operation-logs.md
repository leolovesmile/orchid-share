
# start and stop
- 启动： `/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf`
- 重启nginx server： `nginx -s reload`
- 

# reverse proxy

# 注意事项
nginx指定文件路径有两种方式root和alias，root与alias主要区别在于nginx如何解释location后面的uri，这会使两者分别以不同的方式将请求映射到服务器文件上。
- alias 指定的目录是准确的，给location指定一个目录。（alias只能位于location块中）
- root 指定目录的上级目录，并且该上级目录要含有locatoin指定名称的同名目录。
例如：
```conf
location /data/ {
	alias /var/www/data/;
}
# 上述配置，访问/data/时，nginx会去/var/www/data/ 目录找文件，可以理解为用alias的值替换了原来的值

location /assert/ {
	root /var/www/;
}
# 这种配置, 访问/assert/时，nginx将root的值作为base directory，去/var/www/assert/下找文件
```

# a typical front-end and backend seperated application deployment
## nginx
```conf
#前端页面
location / {
    #Linux上的前端工程打包目录
    root   /home/dist/; 

    #防止重定向页面刷新 路由失效
    try_files $uri $uri/ /index.html;
    index index.jsp index.html index.htm;
}

#后端接口地址
location /api/{
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header REMOTE-HOST $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_pass  http://localhost:8080/;
}

```
说明：
- `try_files`最核心的功能是可以替代rewrite；按顺序检查文件是否存在，返回第一个找到的文件。


