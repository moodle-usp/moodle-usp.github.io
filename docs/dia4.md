---
layout: default
title: Dia 4
nav_order: 5
---
1. TOC
{:toc}
---

<a href="/assets/users.csv">Arquivo csv com usuários. </a>

Criando o curso no moodle dentro do foreach do *createcourses.php*:

```php
require_once("$CFG->dirroot/course/lib.php");

$category = $DB->get_record('course_categories', array('name'=>'Categoria de Cursos'));
if($category) return $category;

$parentid = 0;
$newcategory = new \stdClass();
$newcategory->name = 'Categoria de Cursos';
$newcategory->description = 'descrição';
$newcategory->sortorder = 999;
$newcategory->parent = $parentid;
$newcategory->save();

$newcourse = new \stdClass();
$newcourse->shortname = $course[0];
$newcourse->fullname = $course[1];
$newcourse->category = $newcategory->id;

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

Possível implementação do delete.php:

```php
require_once('../../config.php');

global $DB;
$id = required_param('id',PARAM_INT);
$DB->delete_records('block_importstuffs',[ 'id' => $id ]);
redirect($CFG->wwwroot . '/blocks/importstuffs/view.php');
```

Iniciando arquivo createusers.php

```php
require_once('../../config.php');
require_once("$CFG->dirroot/user/lib.php");

global $DB;
$id = required_param('id', PARAM_INT);

$record = $DB->get_record('block_importstuffs', ['id' => $id]);
if(empty($record)) {
    \core\notification::error('Registro não existe');
    redirect($CFG->wwwroot . '/blocks/importstuffs/view.php');
}

$lines = explode(PHP_EOL,$record->file);
foreach($lines as $line){
    $user = str_getcsv($line);

    $userdb = $DB->get_record('user',['email'=>$user[3]]);

}
redirect($CFG->wwwroot . '/blocks/importstuffs/view.php');
```

Criando usuários no moodle:

```php
$newuser = new stdClass();
$newuser->username = $user[1];
$newuser->firstname = $user[2];
$newuser->email = $user[3];

$userdb = $DB->get_record('user',['email'=>$user[3]]);

if($userdb){
    $newuser->id = $userdb->id;
    user_update_user($newuser);
} else {
    user_create_user($newuser);
}
```
