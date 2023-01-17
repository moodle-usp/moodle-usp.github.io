---
layout: default
title: Dia 2
nav_order: 3
---
1. TOC
{:toc}
---

## Internacionalização

Pode-se criar quantas strings forem necessárias no diretório de idiomas.
Exemplo, em *lang/en/block_importstuffs.php*:

```php
$string['blocktitle'] = 'Import Stuffs';
```

Essa string pode ser acessada em qualquer lugar com o função *get_string*:

```php
get_string('blocktitle','block_importstuffs')
```

Importante: ao criar novas strings, sempre limpe o cache em: *admin/purgecaches.php* ou pela linha de comando:

```bash
php admin/cli/purge_caches.php
```

## Templates

O sistema de template evita o código "macarrônico", *templates/view.mustache*:

```php
{% raw %}<div class="card">
  <div class="card-header">
    {{ title }}
  </div>
  <div class="card-body">
    ainda nada
  </div>
</div>{% endraw %}
```

Podemos passar variáveis da view para o template:

```php
$data = [
    'title' => 'Plugin de importação de coisas',
];

print $OUTPUT->header();
echo $OUTPUT->render_from_template('block_importstuffs/view', $data);
print $OUTPUT->footer()
```

## Form API

uploadform.php

```php
require_once("$CFG->libdir/formslib.php");
class uploadform extends moodleform {
    public function definition() {
        $this->_form->addElement('text', 'description', 'identificação do arquivo:');
        $this->_form->addElement('select', 'type', 'Tipo', ['users', 'courses']);
        $this->_form->addElement('file', 'file', 'arquivo csv');
        $this->_form->addElement('submit', 'button', 'Enviar');
        
        $this->_form->addRule('description', null, 'required');
        $this->_form->addRule('type', null, 'required');
        $this->_form->addRule('type', null, 'required');
    }
}
```
na view.php

```php
require_once('uploadform.php');
$uploadform = new uploadform();

$data = [
    'title' => 'Plugin de importação de coisas',
    'uploadform' => $uploadform->render(),
];
```

No mustache:

```php
{% raw %}{{{ uploadform }}}{% endraw %}
```

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

## Criação de tabela no banco de dados

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<XMLDB PATH="blocks/importstuffs/db" VERSION="20200213" COMMENT=""
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:noNamespaceSchemaLocation="../../../lib/xmldb/xmldb.xsd"
>
  <TABLES>
    <TABLE NAME="block_importstuffs" COMMENT="">
      <FIELDS>
        <FIELD NAME="id" TYPE="int" LENGTH="10" NOTNULL="true" SEQUENCE="true"/>
        <FIELD NAME="intro" TYPE="text" NOTNULL="false" SEQUENCE="false"/>
        <FIELD NAME="type" TYPE="text" NOTNULL="false" SEQUENCE="false"/>
        <FIELD NAME="file" TYPE="text" NOTNULL="false" SEQUENCE="false"/>
      </FIELDS>
      <KEYS>
        <KEY NAME="primary" TYPE="primary" FIELDS="id"/>
      </KEYS>
    </TABLE>
  </TABLES>
</XMLDB>
```

Criando tabela:

```bash
php admin/cli/uninstall_plugins.php --plugins=block_importstuffs --run
php admin/cli/upgrade.php
```