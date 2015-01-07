This plugin was forked from Igor Borodikhin (https://github.com/iborodikhin/munin-plugin-nginx-upstream-multi).
The primary difference is in this version, on the graphs are shown on the hosts graph page. Igor's only shows
the # of Requests graph in the main listing, and displays the rest nested under it.

Munin plugin to monitor requests number, cache statuses, http status codes and average request times of
specified nginx upstreams.

## Configuration parameters:
* *env.graphs* — which graphs to produce (optional, list of graphs separated by spaces, default — cache http time request)
* *env.log* — log file path (mandatory, ex.: /var/log/nginx/upstream.log)
* *env.upstream* — list of upstreams to monitor (mandatory, including port numbers separated by space, ex.: 10.0.0.1:80 10.0.0.2:8080)
* *env.statuses* — list of http status codes to monitor (optional, default — all statuses, ex.: 200 403 404 410 500 502)
* *env.percentiles* — which percentiles to draw on time graphs (optional, list of percentiles separated by spaces, default - 80)

## Installation
Copy file to directory /usr/share/munin/pligins/ and create symbolic link to /etc/munin/plugins/nginx_upstream_multi_<name of upstream>.

Specify log_format at /etc/nginx/conf.d/upstream.conf:
```
log_format upstream "ua=[$upstream_addr] ut=[$upstream_response_time] us=[$upstream_status] cs=[$upstream_cache_status]"
```

Use it in your site configuration (/etc/nginx/sites-enabled/anything.conf, or if you want to specify for all hosts /etc/nginx/nginx.conf):
```
access_log /var/log/nginx/upstream.log upstream;
```

And specify some options in munin-node.conf:
```
[nginx_upstream_multi_<name of upstream>]
env.graphs cache http time request
env.log /var/log/nginx/upstream.log
env.upstream 10.0.0.1:80 10.0.0.2:8080 unix:/var/run/php5-fpm.sock
env.statuses 200 403 404 410 500 502
env.percentiles 50 80
```
