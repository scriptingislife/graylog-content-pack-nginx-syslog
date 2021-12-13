# graylog-content-pack-nginx-syslog

This content pack supports the NGINX syslog feature. It only requires modification of the nginx.conf file. There are no bells and whistles. The goal is to start logging as quickly as possible.

To create this content pack I modified the [nginx + Docker](https://marketplace.graylog.org/addons/d81a73dd-79bf-4a1f-bb76-89e9a3df6b11) content pack by modifying the extractors and `nginx.conf` file and removing all other Graylog components like dashboards.

# Setup
Install NGINX and replace the `Logging Settings` section in `/etc/nginx/nginx.conf` with the lines below. Replace `logging.example.com` with the domain or IP address of your Graylog server.

```
log_format graylog_json escape=json '{ "timestamp": "$time_iso8601", '
        '"remote_addr": "$remote_addr", '
        '"connection": "$connection", '
        '"connection_requests": $connection_requests, '
        '"pipe": "$pipe", '
        '"body_bytes_sent": $body_bytes_sent, '
        '"request_length": $request_length, '
        '"request_time": $request_time, '
        '"response_status": $status, '
        '"request": "$request", '
        '"request_method": "$request_method", '
        '"host": "$host", '
        '"upstream_cache_status": "$upstream_cache_status", '
        '"upstream_addr": "$upstream_addr", '
        '"http_x_forwarded_for": "$http_x_forwarded_for", '
        '"http_referrer": "$http_referer", '
        '"http_user_agent": "$http_user_agent", '
        '"http_version": "$server_protocol", '
        '"remote_user": "$remote_user", '
        '"http_x_forwarded_proto": "$http_x_forwarded_proto", '
        '"upstream_response_time": "$upstream_response_time", '
        '"nginx_access": true }';

access_log syslog:server=logging.example.com:12401 graylog_json;

access_log /var/log/nginx/access.log;
error_log /var/log/nginx/error.log;
```