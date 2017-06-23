# site_id: 231
# site_name: h5.dianping.com
# publish_user: yanfang.chang
# uuid: dbae632c4615



upstream h5.dianping.com.customerinfo-certification-web {
    server 10.1.126.154:80 max_fails=3 weight=1 fail_timeout=2s;  # customerinfo-certification-web01.nh
    server 10.1.124.11:80 max_fails=3 weight=1 fail_timeout=2s;  # customerinfo-certification-web02.nh
    
    keepalive 20;
    keepalive_timeout 60;
    
    check interval=1000 fall=3 rise=2 timeout=3000 default_down=false type=tcp;
}

upstream h5.dianping.com.points-mall-web {
    server 10.6.6.5:80 max_fails=3 weight=1 fail_timeout=2s;  # points-mall-web01.tx
    server 10.6.6.20:80 max_fails=3 weight=1 fail_timeout=2s;  # points-mall-web03.tx
    server 10.3.14.14:80 max_fails=3 weight=1 fail_timeout=2s;  # points-mall-web01.gq
    server 10.3.17.58:80 max_fails=3 weight=1 fail_timeout=2s;  # points-mall-web03.gq
    
    keepalive 20;
    keepalive_timeout 60;
    
    check interval=3000 fall=3 rise=2 timeout=3000 default_down=false type=http;
    check_http_send 'GET /index.jsp HTTP/1.0\r\n\r\n';
    check_http_expect_alive http_2xx;
}

upstream h5.dianping.com.event-static-web {
    server 10.1.108.193:80 max_fails=3 weight=1 fail_timeout=2s;  # event-static-web01.nh
    server 10.1.108.177:80 max_fails=3 weight=1 fail_timeout=2s;  # event-static-web02.nh
    
    keepalive 20;
    keepalive_timeout 60;
    
    check interval=5000 fall=3 rise=2 timeout=3000 default_down=false type=tcp;
}

upstream h5.dianping.com.dp-storage-web {
    server 10.1.11.13:80 max_fails=3 weight=1 fail_timeout=2s;  # dp-storage-web01.nh
    server 10.1.11.14:80 max_fails=3 weight=1 fail_timeout=2s;  # dp-storage-web02.nh
    
    keepalive 5;
    keepalive_timeout 60;
    
    check interval=5000 fall=3 rise=2 timeout=3000 default_down=false type=tcp;
}

upstream h5.dianping.com.wed-activity-web {
    server 10.1.119.156:80 max_fails=3 weight=1 fail_timeout=2s;  # wed-activity-web01.nh
    server 10.69.173.228:80 max_fails=3 weight=1 fail_timeout=2s;  # wed-activity-web01.gq
    server 10.69.82.140:80 max_fails=3 weight=1 fail_timeout=2s;  # wed-activity-web03.gq
    
    keepalive 20;
    keepalive_timeout 60;
    
    check interval=3000 fall=3 rise=2 timeout=3100 default_down=false type=http;
    check_http_send 'GET /index.jsp HTTP/1.0\r\n\r\n';
    check_http_expect_alive http_2xx;
}


# http
server {
    listen          80;
    server_name     h5.dianping.com h5.meituan.com osx.dpfile.com osx.dianping.com h5.meituan.net;
    access_log      logs/h5.dianping.com.access.log main;
    
    log_by_lua_file "/usr/local/nginx/conf/lua/nginx_qps.lua";
    set $site   "h5.dianping.com";
    set $cluster "dengine-web-b";
    set $locationurl "default";

    if ($request_uri ~ "^\/.*(_memberAccess|OgnlContext|ProcessBuilder|debug=browser).*$") {
       return 404 "not found";
    }
    if ($content_type ~* ".*(_memberAccess|OgnlContext|ProcessBuilder|debug=browser).*"){
      return 404 "not found";
    }

    location / {
        set $dp_upstream "h5.dianping.com.dp-storage-web";
        
        # 私有规则
        more_set_headers 'Access-Control-Allow-Origin:*';
        more_set_headers 'Timing-Allow-Origin:*';
        if ($request_uri ~ "^\/.*(_memberAccess|OgnlContext|ProcessBuilder|debug=browser).*$") {
            access_log logs/struts_deny.access.log main;more_set_headers 'Content-Type : text/plain';return 404 "not found";
        }

        # 公共规则
        header_filter_by_lua " if ngx.req.get_headers()['dpop'] == 'y' and (ngx.var.remote_addr == '180.166.152.90' or ngx.var.remote_addr == '210.22.122.2' or ngx.var.remote_addr == '180.153.132.38' or ngx.var.remote_addr == '180.153.132.6') then ngx.header.x_upstream = ngx.var.remote_addr .. '|' .. ngx.var.server_addr .. '|' .. (ngx.var.dp_upstream or '') .. '|' .. (ngx.var.upstream_addr or '') end ";
        

        # Location
        location ~* ^/crm/ {
            set $dp_upstream "h5.dianping.com.customerinfo-certification-web";
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/tuan {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            if ($uri ~* .*\.(css|js|appcache|html|gif|png|bmp|jpeg|jpg)$) {
                more_set_headers 'Cache-Control : max-age=600';
            }
            if ($uri ~* .*\.json$) {
                more_set_headers 'Cache-Control : max-age=300';
            }
            if ($uri ~*  \.appcache$) {
                more_set_headers 'Cache-Control : max-age=300'; more_set_headers 'Content-Type: text/cache-manifest';
            }
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/\w+/json/ {
            set $dp_upstream "h5.dianping.com.event-static-web";
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/platform/(easylife|fitness)/ {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            more_set_headers more_set_headers 'Cache-Control : max-age=60';
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/pointmall/ {
            set $dp_upstream "h5.dianping.com.points-mall-web";
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/pointsmall/ {
            set $dp_upstream "h5.dianping.com.points-mall-web";
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/wed/event/other/wizardAjaxAction {
            set $dp_upstream "h5.dianping.com.wed-activity-web";
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/app/app-overseas-peon/ {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            more_set_headers 'Cache-Control:max-age=2592000, s-maxage=3600';
            if ($uri ~* .*\.html$) {
                more_set_headers 'Cache-Control : max-age=300'; more_set_headers 'Content-Security-Policy: default-src \'self\' *.meituan.com https://*.meituan.com *.meituan.net https://*.meituan.net *.maoyan.com https://*.maoyan.com wvjbscheme://* imeituan://* *.dianping.com  https://*.dianping.com *.dpfile.com https://*.dpfile.com *.51ping.com https://*.51ping.com http://sentry7.sankuai.com https://*.qq.com https://sentry7.sankuai.com *.amap.com ws://* https://fecsp.sankuai.com *.google-analytics.com csi.gstatic.com maps.gstatic.com maps.google.cn maps.googleapis.com ditu.google.cn *.weibo.com js://* dplocalresource://* http://api.map.baidu.com http://*.map.bdimg.com http://operate.osportal.dp https://ss0.bdstatic.com fonts.googleapis.com fonts.gstatic.com mtnblocalresouce://* meituan-media://* knb-media://* \'unsafe-inline\' \'unsafe-eval\' blob: data:; report-uri https://fecsp.sankuai.com/report/touchoverseas';
            }
            if ($uri ~* .*\.appcache$) {
                more_set_headers 'Cache-Control : no-cache'; more_set_headers 'Content-Type: text/cache-manifest';
            }
            if ($uri = /app/app-overseas-peon/channel.html) {
                more_set_headers 'Cache-Control : no-cache'; more_set_headers 'Content-Security-Policy: default-src \'self\' *.meituan.com https://*.meituan.com *.meituan.net https://*.meituan.net *.maoyan.com https://*.maoyan.com wvjbscheme://* imeituan://* *.dianping.com  https://*.dianping.com *.dpfile.com https://*.dpfile.com *.51ping.com https://*.51ping.com http://sentry7.sankuai.com https://*.qq.com https://sentry7.sankuai.com *.amap.com ws://* https://fecsp.sankuai.com *.google-analytics.com csi.gstatic.com maps.gstatic.com maps.google.cn maps.googleapis.com ditu.google.cn *.weibo.com js://* dplocalresource://* http://api.map.baidu.com http://*.map.bdimg.com http://operate.osportal.dp https://ss0.bdstatic.com fonts.googleapis.com fonts.gstatic.com mtnblocalresouce://* meituan-media://* knb-media://* \'unsafe-inline\' \'unsafe-eval\' blob: data:; report-uri https://fecsp.sankuai.com/report/touchoverseas';
            }
            if ($uri = /app/app-overseas-peon/unionpay/shopping.html) {
                more_set_headers 'Cache-Control : no-cache'; more_set_headers 'Content-Security-Policy: default-src \'self\' *.meituan.com https://*.meituan.com *.meituan.net https://*.meituan.net *.maoyan.com https://*.maoyan.com wvjbscheme://* imeituan://* *.dianping.com  https://*.dianping.com *.dpfile.com https://*.dpfile.com *.51ping.com https://*.51ping.com http://sentry7.sankuai.com https://*.qq.com https://sentry7.sankuai.com *.amap.com ws://* https://fecsp.sankuai.com *.google-analytics.com csi.gstatic.com maps.gstatic.com maps.google.cn maps.googleapis.com ditu.google.cn *.weibo.com js://* dplocalresource://* http://api.map.baidu.com http://*.map.bdimg.com http://operate.osportal.dp https://ss0.bdstatic.com fonts.googleapis.com fonts.gstatic.com mtnblocalresouce://* meituan-media://* knb-media://* \'unsafe-inline\' \'unsafe-eval\' blob: data:; report-uri https://fecsp.sankuai.com/report/touchoverseas';
            }
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/app/app-overseas-public-static/ {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            more_set_headers 'Cache-Control:max-age=2592000, s-maxage=3600';
            if ($uri ~* .*\.html$) {
                more_set_headers 'Cache-Control : max-age=300';
            }
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/app/peon-test/static/ {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            if ($uri ~* .*\.appcache$) {
                more_set_headers 'Cache-Control : no-cache'; more_set_headers 'Content-Type: text/cache-manifest';
            }
            if ($uri = /app/app-overseas-test/static/test.min.e77ba0b0a31e3f5b54217d2afbee4b92.js) {
                more_set_headers 'Cache-Control : no-cache';
            }
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/app/peon-test/ {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            if ($uri ~* .*\.html$) {
                more_set_headers 'Content-Security-Policy: default-src \'self\' *.meituan.com https://*.meituan.com *.meituan.net https://*.meituan.net *.maoyan.com https://*.maoyan.com wvjbscheme://* imeituan://* *.dianping.com  https://*.dianping.com *.dpfile.com https://*.dpfile.com *.51ping.com https://*.51ping.com http://sentry7.sankuai.com https://*.qq.com https://sentry7.sankuai.com *.amap.com ws://* https://fecsp.sankuai.com *.google-analytics.com *.weibo.com js://* dplocalresource://* http://api.map.baidu.com http://*.map.bdimg.com http://operate.osportal.dp https://ss0.bdstatic.com \'unsafe-inline\' \'unsafe-eval\' blob: data:; report-uri https://fecsp.sankuai.com/report/touchoverseas';
            }
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/app/overseas-info/ {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            more_set_headers 'Cache-Control:max-age=2592000, s-maxage=3600';
            if ($uri ~* .*\.html$) {
                more_set_headers 'Cache-Control : max-age=300'; more_set_headers 'Content-Security-Policy: default-src \'self\' *.meituan.com https://*.meituan.com *.meituan.net https://*.meituan.net *.maoyan.com https://*.maoyan.com wvjbscheme://* imeituan://* *.dianping.com  https://*.dianping.com *.dpfile.com https://*.dpfile.com *.51ping.com https://*.51ping.com http://sentry7.sankuai.com https://*.qq.com https://sentry7.sankuai.com *.amap.com ws://* https://fecsp.sankuai.com *.google-analytics.com csi.gstatic.com maps.gstatic.com maps.google.cn maps.googleapis.com ditu.google.cn *.weibo.com js://* dplocalresource://* http://api.map.baidu.com http://*.map.bdimg.com http://operate.osportal.dp https://ss0.bdstatic.com fonts.googleapis.com fonts.gstatic.com mtnblocalresouce://* meituan-media://* knb-media://* \'unsafe-inline\' \'unsafe-eval\' blob: data:; report-uri https://fecsp.sankuai.com/report/touchoverseas';
            }
            if ($uri ~* .*\.(appcache|map)$) {
                more_set_headers 'Cache-Control : no-cache'; more_set_headers 'Content-Type: text/cache-manifest';
            }
            proxy_pass  http://$dp_upstream;
        }
        
        if ( !-f $request_filename )  {
            proxy_pass http://$dp_upstream;
            break;
        }
    }
}



# https
server {
    listen          443 ssl;
    server_name     h5.dianping.com h5.meituan.com osx.dpfile.com osx.dianping.com h5.meituan.net;
    access_log      logs/h5.dianping.com.https.access.log main;
    

    set $site   "h5.dianping.com_https";
    set $cluster "dengine-web-b_https";
    set $locationurl "default";

    ssl_prefer_server_ciphers on;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;
    ssl_certificate /usr/local/nginx/conf/ssl/dianping.com.crt;
    ssl_certificate_key /usr/local/nginx/conf/ssl/dianping.com.key;

    if ($request_uri ~ "^\/.*(_memberAccess|OgnlContext|ProcessBuilder|debug=browser).*$") {
       return 404 "not found";
    }
    if ($content_type ~* ".*(_memberAccess|OgnlContext|ProcessBuilder|debug=browser).*"){
      return 404 "not found";
    }

    location / {
        set $dp_upstream "h5.dianping.com.dp-storage-web";
        
        # 私有规则
        more_set_headers 'Access-Control-Allow-Origin:*';
        more_set_headers 'Timing-Allow-Origin:*';
        if ($request_uri ~ "^\/.*(_memberAccess|OgnlContext|ProcessBuilder|debug=browser).*$") {
            access_log logs/struts_deny.access.log main;more_set_headers 'Content-Type : text/plain';return 404 "not found";
        }

        # 公共规则
        header_filter_by_lua " if ngx.req.get_headers()['dpop'] == 'y' and (ngx.var.remote_addr == '180.166.152.90' or ngx.var.remote_addr == '210.22.122.2' or ngx.var.remote_addr == '180.153.132.38' or ngx.var.remote_addr == '180.153.132.6') then ngx.header.x_upstream = ngx.var.remote_addr .. '|' .. ngx.var.server_addr .. '|' .. (ngx.var.dp_upstream or '') .. '|' .. (ngx.var.upstream_addr or '') end ";
        

        # Location
        location ~* ^/crm/ {
            set $dp_upstream "h5.dianping.com.customerinfo-certification-web";
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/tuan {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            if ($uri ~* .*\.(css|js|appcache|html|gif|png|bmp|jpeg|jpg)$) {
                more_set_headers 'Cache-Control : max-age=600';
            }
            if ($uri ~* .*\.json$) {
                more_set_headers 'Cache-Control : max-age=300';
            }
            if ($uri ~*  \.appcache$) {
                more_set_headers 'Cache-Control : max-age=300'; more_set_headers 'Content-Type: text/cache-manifest';
            }
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/\w+/json/ {
            set $dp_upstream "h5.dianping.com.event-static-web";
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/platform/(easylife|fitness)/ {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            more_set_headers more_set_headers 'Cache-Control : max-age=60';
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/pointmall/ {
            set $dp_upstream "h5.dianping.com.points-mall-web";
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/pointsmall/ {
            set $dp_upstream "h5.dianping.com.points-mall-web";
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/wed/event/other/wizardAjaxAction {
            set $dp_upstream "h5.dianping.com.wed-activity-web";
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/app/app-overseas-peon/ {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            more_set_headers 'Cache-Control:max-age=2592000, s-maxage=3600';
            if ($uri ~* .*\.html$) {
                more_set_headers 'Cache-Control : max-age=300'; more_set_headers 'Content-Security-Policy: default-src \'self\' *.meituan.com https://*.meituan.com *.meituan.net https://*.meituan.net *.maoyan.com https://*.maoyan.com wvjbscheme://* imeituan://* *.dianping.com  https://*.dianping.com *.dpfile.com https://*.dpfile.com *.51ping.com https://*.51ping.com http://sentry7.sankuai.com https://*.qq.com https://sentry7.sankuai.com *.amap.com ws://* https://fecsp.sankuai.com *.google-analytics.com csi.gstatic.com maps.gstatic.com maps.google.cn maps.googleapis.com ditu.google.cn *.weibo.com js://* dplocalresource://* http://api.map.baidu.com http://*.map.bdimg.com http://operate.osportal.dp https://ss0.bdstatic.com fonts.googleapis.com fonts.gstatic.com mtnblocalresouce://* meituan-media://* knb-media://* \'unsafe-inline\' \'unsafe-eval\' blob: data:; report-uri https://fecsp.sankuai.com/report/touchoverseas';
            }
            if ($uri ~* .*\.appcache$) {
                more_set_headers 'Cache-Control : no-cache'; more_set_headers 'Content-Type: text/cache-manifest';
            }
            if ($uri = /app/app-overseas-peon/channel.html) {
                more_set_headers 'Cache-Control : no-cache'; more_set_headers 'Content-Security-Policy: default-src \'self\' *.meituan.com https://*.meituan.com *.meituan.net https://*.meituan.net *.maoyan.com https://*.maoyan.com wvjbscheme://* imeituan://* *.dianping.com  https://*.dianping.com *.dpfile.com https://*.dpfile.com *.51ping.com https://*.51ping.com http://sentry7.sankuai.com https://*.qq.com https://sentry7.sankuai.com *.amap.com ws://* https://fecsp.sankuai.com *.google-analytics.com csi.gstatic.com maps.gstatic.com maps.google.cn maps.googleapis.com ditu.google.cn *.weibo.com js://* dplocalresource://* http://api.map.baidu.com http://*.map.bdimg.com http://operate.osportal.dp https://ss0.bdstatic.com fonts.googleapis.com fonts.gstatic.com mtnblocalresouce://* meituan-media://* knb-media://* \'unsafe-inline\' \'unsafe-eval\' blob: data:; report-uri https://fecsp.sankuai.com/report/touchoverseas';
            }
            if ($uri = /app/app-overseas-peon/unionpay/shopping.html) {
                more_set_headers 'Cache-Control : no-cache'; more_set_headers 'Content-Security-Policy: default-src \'self\' *.meituan.com https://*.meituan.com *.meituan.net https://*.meituan.net *.maoyan.com https://*.maoyan.com wvjbscheme://* imeituan://* *.dianping.com  https://*.dianping.com *.dpfile.com https://*.dpfile.com *.51ping.com https://*.51ping.com http://sentry7.sankuai.com https://*.qq.com https://sentry7.sankuai.com *.amap.com ws://* https://fecsp.sankuai.com *.google-analytics.com csi.gstatic.com maps.gstatic.com maps.google.cn maps.googleapis.com ditu.google.cn *.weibo.com js://* dplocalresource://* http://api.map.baidu.com http://*.map.bdimg.com http://operate.osportal.dp https://ss0.bdstatic.com fonts.googleapis.com fonts.gstatic.com mtnblocalresouce://* meituan-media://* knb-media://* \'unsafe-inline\' \'unsafe-eval\' blob: data:; report-uri https://fecsp.sankuai.com/report/touchoverseas';
            }
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/app/app-overseas-public-static/ {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            more_set_headers 'Cache-Control:max-age=2592000, s-maxage=3600';
            if ($uri ~* .*\.html$) {
                more_set_headers 'Cache-Control : max-age=300';
            }
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/app/peon-test/static/ {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            if ($uri ~* .*\.appcache$) {
                more_set_headers 'Cache-Control : no-cache'; more_set_headers 'Content-Type: text/cache-manifest';
            }
            if ($uri = /app/app-overseas-test/static/test.min.e77ba0b0a31e3f5b54217d2afbee4b92.js) {
                more_set_headers 'Cache-Control : no-cache';
            }
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/app/peon-test/ {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            if ($uri ~* .*\.html$) {
                more_set_headers 'Content-Security-Policy: default-src \'self\' *.meituan.com https://*.meituan.com *.meituan.net https://*.meituan.net *.maoyan.com https://*.maoyan.com wvjbscheme://* imeituan://* *.dianping.com  https://*.dianping.com *.dpfile.com https://*.dpfile.com *.51ping.com https://*.51ping.com http://sentry7.sankuai.com https://*.qq.com https://sentry7.sankuai.com *.amap.com ws://* https://fecsp.sankuai.com *.google-analytics.com *.weibo.com js://* dplocalresource://* http://api.map.baidu.com http://*.map.bdimg.com http://operate.osportal.dp https://ss0.bdstatic.com \'unsafe-inline\' \'unsafe-eval\' blob: data:; report-uri https://fecsp.sankuai.com/report/touchoverseas';
            }
            proxy_pass  http://$dp_upstream;
        }
        
        location ~* ^/app/overseas-info/ {
            set $dp_upstream "h5.dianping.com.dp-storage-web";
            more_set_headers 'Cache-Control:max-age=2592000, s-maxage=3600';
            if ($uri ~* .*\.html$) {
                more_set_headers 'Cache-Control : max-age=300'; more_set_headers 'Content-Security-Policy: default-src \'self\' *.meituan.com https://*.meituan.com *.meituan.net https://*.meituan.net *.maoyan.com https://*.maoyan.com wvjbscheme://* imeituan://* *.dianping.com  https://*.dianping.com *.dpfile.com https://*.dpfile.com *.51ping.com https://*.51ping.com http://sentry7.sankuai.com https://*.qq.com https://sentry7.sankuai.com *.amap.com ws://* https://fecsp.sankuai.com *.google-analytics.com csi.gstatic.com maps.gstatic.com maps.google.cn maps.googleapis.com ditu.google.cn *.weibo.com js://* dplocalresource://* http://api.map.baidu.com http://*.map.bdimg.com http://operate.osportal.dp https://ss0.bdstatic.com fonts.googleapis.com fonts.gstatic.com mtnblocalresouce://* meituan-media://* knb-media://* \'unsafe-inline\' \'unsafe-eval\' blob: data:; report-uri https://fecsp.sankuai.com/report/touchoverseas';
            }
            if ($uri ~* .*\.(appcache|map)$) {
                more_set_headers 'Cache-Control : no-cache'; more_set_headers 'Content-Type: text/cache-manifest';
            }
            proxy_pass  http://$dp_upstream;
        }
        
        if ( !-f $request_filename )  {
            proxy_pass http://$dp_upstream;
            break;
        }

    }
}
