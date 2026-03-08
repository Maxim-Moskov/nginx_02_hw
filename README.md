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

### Задание 3*:

![Задание 3*](img/py_8.png)

![Задание 3*](img/py_9.png)

![Задание 3*](img/py_4.png)

![Задание 3*](img/py_5.png)

![Задание 3*](img/py_6.png)

![Задание 3*](img/py_7.png)

### Задание 4*:

## Проверяем сайт example1.local:

![Задание 4*](img/py_10.png)

## Проверяем сайт example2.local:

![Задание 4*](img/py_11.png)

## haproxy.cfg

```haproxy
frontend multi_domain_frontend
    bind *:8090
    mode http

    # Создаем правила (ACL): проверяем, какой домен запросил клиент
    acl is_example1 hdr(host) -i example1.local
    acl is_example2 hdr(host) -i example2.local

    # Направляем трафик в нужный бэкенд в зависимости от сработавшего правила
    use_backend backend_example1 if is_example1
    use_backend backend_example2 if is_example2

backend backend_example1
    mode http
    balance roundrobin
    server ex1_s1 127.0.0.1:8001 check
    server ex1_s2 127.0.0.1:8002 check

backend backend_example2
    mode http
    balance roundrobin
    server ex2_s1 127.0.0.1:8003 check
    server ex2_s2 127.0.0.1:8004 check
```