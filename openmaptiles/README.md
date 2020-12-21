## 必须文件

1. 地图数据文件，canada.mbtiles，在 config.json 中配置 data 来源
2. styles/* 下必须有一个样式，同样需要在 config.json 中配置 style 来源

[更多可供下载的样式](https://openmaptiles.org/styles/)

自定义样式需要修改样式文件 style.json 的 source 字段和 sprite, glyphs 字段

更多内容参考 [Mapbox Style Specification](https://docs.mapbox.com/mapbox-gl-js/style-spec/)

## 启动服务

在 config.json 同级目录运行，启动地图服务

```
$ docker run -d --rm -it -v $(pwd):/data -p 8080:80 maptiler/tileserver-gl --config ./config.json
```

该服务只有 http 服务，如果想使用 https，使用 nginx 反向代理配置 https

[Running behind a proxy or a load-balancer](https://tileserver.readthedocs.io/en/latest/deployment.html#running-behind-a-proxy-or-a-load-balancer)


## nginx https 配置

```nginx
    location / {
        add_header Content-Security-Policy upgrade-insecure-requests;
        proxy_redirect     off;
        proxy_pass http://map.example.com;

        proxy_set_header  Upgrade-Insecure-Requests 1;
        proxy_set_header  X-Forwarded-Host $host;
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header  Host $http_host;
        proxy_set_header  X-Forwarded-Protocol "https";
    }
```
