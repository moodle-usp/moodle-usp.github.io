---
layout: default
title: Dia 3
nav_order: 4
---
1. TOC
{:toc}
---

## Banco de dados

## Configurações do Plugin


Tratando os dados submetidos:

```php
$request = $uploadform->get_data();
if (!empty($request) and !is_null($request))  {
    $file = $uploadform->get_file_content('file');
    print_r($request->description);
    print_r($request->type);
    print_r($file);
    die();
} else {
    \core\notification::info('Nada submetido ainda');
}
```

## Mensagem de alerta (Bootstrap)

Podemos lançar uma mensagem de alerta usando a moodle:

```php
else {
   \core\notification::error('Nada submetido ainda');
}
```


<!---
Não sei se vamos abordar?

- hooks em lib.php ?
- redirect($CFG->wwwroot . '/blocks/importstuffs/view.php', message='redirecionado');

- Inserir no banco de dados:

$row = new stdClass();
$row->campo1 = 'abc';
$row->campo2 = 'Maria';

$DB->insert_record('blocks_importstuffs',$row);
$DB->get_records('blocks_importstuffs');  array_values()

-->
