---
layout: default
title: Dia 1
nav_order: 2
---
1. TOC
{:toc}
---

## Preparação do ambiente


Criar o banco de dados no Mariadb

```
mariadb -uadmin -psenha
create database moodle;
\q
```

Baixar o composer:

``` 
curl -s https://getcomposer.org/installer | php
```

Baixar o moosh para o moodle:

```
git clone https://github.com/tmuras/moosh.git
```

Instalar o moosh

```
cd moosh/
../composer.phar install
```

Instalar o moodle

```
cd ..
moosh/moosh.php download-moodle
tar zxvf moodle-latest-401.tgz
```


Entrar no moodle e seguir o procedimento de instalação:

```
cd moodle/
php -S 0.0.0.0:8888
```

## Criação do plugin


