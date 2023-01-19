---
layout: default
title: Dia 4
nav_order: 5
---
1. TOC
{:toc}
---

Criando o curso no moodle dentro do foreach do *createcourses.php*:

```php
require_once("$CFG->dirroot/course/lib.php");

$newcourse = new \stdClass();
$newcourse->shortname = $course[0];
$newcourse->fullname = $course[1];
$newcourse->category = 1;

$created_course = \create_course($newcourse);
\core\notification::success($course[1] . ' cadastrado com sucesso');
```

Se quisermos especificar as data de início e fim do curso:

```php
$start_time = strtotime($course[2]);
$end_time = strtotime($course[3]);

$newcourse->startdate = $start_time;
$newcourse->enddate = $end_time;
```

Link para contemplar os usuários:


```php
{% raw %}<td>
    {{#type}} <a href="/blocks/importstuffs/createcourses.php/?id={{id}}"> Importar no Curso </a> {{/type}}
    {{^type}} <a href="/blocks/importstuffs/createusers.php/?id={{id}}"> Importar no Usuários </a> {{/type}}
</td>{% endraw %}
```

