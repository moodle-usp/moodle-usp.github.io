---
layout: default
title: Extra
nav_order: 7
---
1. TOC
{:toc}
---

## Settings

Indicar no arquivo *block_atpc.php* que esse plugin terá área de adiministração. 

```php
function has_config(){
    return true;
}
```

Criar um arquivo na raiz do plugin com o nome *settings.php*

```php
<?php

defined('MOODLE_INTERNAL') || die();
if ($ADMIN->fulltree) {
    $setting = new admin_setting_configtext('block_atpc/texto', 'texto','um texto qualquer', 'texto default', PARAM_TEXT);
    $settings->add($setting);
}
```

Para usar o campo criado em qualquer lugar do plugin:

```php
get_config('block_atpc','texto');
```
