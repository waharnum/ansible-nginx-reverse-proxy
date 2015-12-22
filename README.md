Role Name
=========

Creates and deploys nginx reverse proxy configurations from supplied variables, optionally making use of the load balancing features as well.

Requirements
------------

nginx with reverse proxy support installed on the server to be managed.

Role Variables
--------------

### Expected to Be Configured

* `nginx_reverse_proxy_proxies`:  list of reverse proxy configurations; each configuration needs the following variables
  * `nginx_reverse_proxy_backend_name:` string, name nginx config uses to refer to the backend
  * `nginx_reverse_proxy_domains`: list of public-facing domains to be proxied
  * `nginx_reverse_proxy_backends`: list of backend services, including port
  * `nginx_reverse_proxy_config_name`: name to use for the proxy file (do not include the '.conf' extension, role will add this)

### Global Defaults

`nginx_reverse_proxy_config_directory:` location to store config files for nginx (default: `/etc/nginx/conf.d`)

Dependencies
------------

An nginx role with a handler called "restart nginx" that... restarts nginx :)

Example Playbook
----------------

```
- hosts: nginx
  remote_user: vagrant
  become: yes
  vars:
    - nginx_reverse_proxy_proxies:
      - nginx_reverse_proxy_config_name: app1proxy
        nginx_reverse_proxy_backend_name: my-backend-1
        nginx_reverse_proxy_backends:
          - localhost:1880
          - localhost:1881
        nginx_reverse_proxy_domains:
          - app1.192.168.88.10.xip.io
      - nginx_reverse_proxy_config_name: app2proxy
        nginx_reverse_proxy_backend_name: my-backend-2
        nginx_reverse_proxy_backends:
          - localhost:1882
          - localhost:1883
        nginx_reverse_proxy_domains:
          - app2.192.168.88.10.xip.io          

  roles:
    - nginx
    - ansible-nginx-reverse-proxy
```    

License
-------

BSD

Author Information
------------------
