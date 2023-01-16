---
layout: default
title: Dia 1
nav_order: 2
---
1. TOC
{:toc}
---

## Considerações

- Moodle histórico
- Por que um plugin?
- Tipos de plugin
- Nosso exemplo: importação de cursos e alunos a partir de um arquvo csv, que pode ser adaptado para outros contextos.
- Camadas moodle: apresentação, negócio e dados.

## Preparação do ambiente

Criar o banco de dados no Mariadb

```bash
mariadb -uadmin -psenha
create database moodle;
\q
```

Baixando o moodle:

```bash
wget https://download.moodle.org/download.php/stable401/moodle-latest-401.tgz
tar zxvf moodle-latest-401.tgz
```

Entrar no moodle e seguir o procedimento de instalação:

```bash
cd moodle/
php -S 0.0.0.0:8888
```

## Criação do plugin

Criação do plugin *importstuffs* do tipo bloco:

```bash
cd blocks
mkdir importstuffs
cd importstuffs
```

Arquivo com metadados, *version.php*:

```php
$plugin->component = 'block_importstuffs';
$plugin->version = 2023011401;
```
Arquivo version.php

```php
$plugin->component = 'block_importstuffs';
$plugin->version = 2023011401;
```

Class filha de block_base (*block_importstuffs.php*): 

```php
class block_importstuffs extends block_base {
	public function init () {
		$this->title = 'Importação de cursos e usuários'; 
	}
	
	public function get_content () {	
		$this->content = new stdClass;
		$this->content->text = "Texto do corpo do bloco";
		return $this->content;
	}
}
```

Internacionalização, nome do plugin no sistema moodle:

```bash
mkdir -p lang/en
mkdir -p lang/pt_br
```

*lang/en/block_importstuffs.php*:

```php
$string['pluginname'] = 'Importing courses and users';
```

*lang/pt_br/block_importstuffs.php*:

```php
$string['pluginname'] = 'Importação de cursos e usuários';
```

Neste ponto podemos ativar nosso plugin pela interface e adicionar o bloco ativando
o modo de edição, painel e em adicionar bloco.

Se houve algum erro, pode-se ativar opções de debug no *config.php*:

```php
@error_reporting(E_ALL | E_STRICT);
@ini_set('display_errors', '1');
$CFG->debug = (E_ALL | E_STRICT);
$CFG->debugdisplay = 1;
```

Criando um paǵina, *view.php*:

```php
require_once('../../config.php');
global $PAGE, $OUTPUT;

$url = new moodle_url("/blocks/importstuffs/view.php");
$PAGE->set_url($url);
$PAGE->set_context(context_system::instance());

$PAGE->set_pagelayout('admin');
require_login();

$PAGE->set_pagelayout('standard');

print $OUTPUT->header();
print 'Plugin de importação de coisas';
print $OUTPUT->footer();
```

Voltando ao bloco, podemos fazer um link para a página recém criada:

```php
$url = new moodle_url('/blocks/importstuffs/view.php');
$this->content->text .= html_writer::link($url, 'Upload stuffs');
```

## Templates

O sistema de template evita o código "macarrônico", *templates/view.mustache*:

```php
{% raw %}
<div class="card">
  <div class="card-header">
    {{ title }}
  </div>
  <div class="card-body">
    ainda nada
  </div>
</div>
{% endraw %}
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





