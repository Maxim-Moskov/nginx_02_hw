# Домашнее задания "Кластеризация и балансировка нагрузки" — Моськов Максим

### Задание 1:

![Задание 1](img/py_1.png)

### Задание 2:

### С доменом

![Задание 2](img/py_2.png)

### Без домена

![Задание 2](img/py_3.png)

## haproxy.cfg

```haproxy
listen web_tcp
    bind *:1325
    mode tcp
    balance roundrobin
    server s1 127.0.0.1:8888 check
    server s2 127.0.0.1:9999 check

frontend example_frontend
    bind *:8088
    mode http
    # Проверяем домен
    acl is_example_local hdr(host) -i example.local
    # Пускаем трафик, если домен совпал
    use_backend web_servers if is_example_local

backend web_servers
    mode http
    balance roundrobin
    # Указываем веса 2, 3 и 4
    server s1 127.0.0.1:8888 weight 2 check
    server s2 127.0.0.1:9999 weight 3 check
    server s3 127.0.0.1:7777 weight 4 check
    ```