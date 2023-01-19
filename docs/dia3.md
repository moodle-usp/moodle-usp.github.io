---
layout: default
title: Dia 3
nav_order: 4
---
1. TOC
{:toc}
---

## Banco de dados

<a href="/assets/cursos.csv">Arquivo csv com cursos. </a>

Resgatando os dados submetidos na view.php:

```php
$data = [
	'uploadform' => $uploadform->render(),
	'records'    => array_values($DB->get_records('block_importstuffs'))
];
```

No mustache apresentamos os dados existentes no banco de dados:

```php
{% raw %}<table class="table">

    <tr>
      <td> <b>Descrição do arquivo</b></td>
      <td> <b>Tipo</b></td>
    </tr>

    {{#records}}
      <tr>
        <td> {{description}}</td>
        <td>
          {{#type}}
              Users
          {{/type}}

          {{^type}}
            Course
          {{/type}}
        </td>
      </tr>
    {{/records}}

  </table>{% endraw %}
```

Criando uma nova página para criação dos cursos *createcourses.php*:

```php
require_once('../../config.php');
global $DB;

$url = new moodle_url("/blocks/importstuffs/createcourses.php");
$PAGE->set_url($url);
$PAGE->set_context(context_system::instance());

$id = required_param('id', PARAM_INT);

die('to aqui');
```

Criando um link para nova página no mustache:

```html
<a href="/blocks/importstuffs/createcourses.php/?id={{ id }}"> Importar no moodle </a>
```

Em *createcourses.php* verificando se registro existe:

```php
$record = $DB->get_record('block_importstuffs', ['id' => $id]);
if(!$record) {
    \core\notification::error('Registro não existe');
    redirect($CFG->wwwroot . '/blocks/importstuffs/view.php');
}
```

Vamos escrever *createcourses.php* para contemplar a importação de cursos,
por enquanto vamos ignorar importação de usuários:

```php
if($record->type == 1){
    \core\notification::error('Sistema só permite importação de cursos');
    redirect($CFG->wwwroot . '/blocks/importstuffs/view.php');
}
```

Em *createcourses.php* abrindo o arquivo csv:

```php
echo "<pre>";
$lines = explode(PHP_EOL,$record->file);
foreach($lines as $line){
    $course = str_getcsv($line);
    print_r($course);
}
```

Inialmente verificar se curso não está cadastrado:

```php
if($DB->get_record('course', ['shortname' => $course[0]])){
    \core\notification::error($course[0] . ' já existe');
    continue;
}
```

No final da página vamos retornar para o formulário:
```php
redirect($CFG->wwwroot . '/blocks/importstuffs/view.php');
```


