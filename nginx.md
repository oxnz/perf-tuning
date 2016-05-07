# Nginx perf tuning

## Worker

```
worker_processes 24;
worker_connections 4000;
worker_rlimit_nofile 200000;
worker_cpu_affinity 0000001 000010 0000100 000010000 00010000;
```

## Open file cache

```
sendfile on;
open_file_cache max = 200000 inactive = 20s;
open_file_cache_valid 30s;
open_file_cache_min_uses 1;
open_file_cache_errors on;
```

## TCP

```
tcp_nopush on;
tcp_nodelay on;
```

## Client

```
client_header_buffer_size 1k;
client_body_buffer_size 10k;
# bigger than this would get 413(Request Entity Too Large)
client_max_body_size 8m;
large_client_header_buffers 21k;
```

## Timeout

```
# 408(Request timeout)
client_header_timeout 12;
client_body_timeout 12;
keepalive_timeout 30;
keepalive_requests 100000;
reset_timeout_connection on;
send_timeout 10;
```

## Limits

```
limit_conn_zone $binary_re****
limit_req_zone $binary_****
limit_req_zone $cookie_tok****
limit_req_zone $http_x_http_f****
limit_req_zone $binary_remo****
limit_req_zone $request****
```

## Proxy

```
proxy_ignore_client_abort on;
proxy_connect_timeout;
proxy_read_timeout;
proxy_send_timeout;
proxy_buffer_size;
proxy_buffers;
proxy_busy_buffers_size;
proxy_temp_file_write_size;
```

## Misc

```
use epoll;
multi_accept on;
```

## Notes

惊群处理

Nginx通过lock的方式实现多进程互斥，避免了惊群效应。

proxy_pass
* 节省IP资源, 日志集中管理
* 方便实现灵活的访问控制
* 使用upstream实现负载分流以及failover
* 简单的waf(针对部分已经攻击以及扫描进行访问阻止)
* 静态资源的缓存以及压缩，优化访问速度
* 错误页面的重定向(处理不友好的访问页面，阻止部署信息泄露)
* https发布
* ssl offloading nginx使用openssl性能优于tomcat的ssl解决方案，并且通过前端的nginx作ssl处理，让tomcat等后端专注于业务逻辑的处理

## Use

main(global conf)
server(host conf)
upstream(load balance conf)
location(URL match conf)

反向代理，负载均衡，目录保护，IP访问限制，防盗链，下载限速

## Features

热部署 高并发 响应快 内存消耗低 容易配置 bug少
