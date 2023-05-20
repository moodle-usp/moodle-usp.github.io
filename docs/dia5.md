---
layout: default
title: Dia 5
nav_order: 6
---
1. TOC
{:toc}
---

## Atribuindo papéis:

Procurando o curso:

```php
$course = $DB->get_record('course',['shortname' => $user[0]]);
```

Jogando um erro caso curso não exista:

```php
if(empty($course)) {
    \core\notification::error("Não existe um curso de código $user[0]");
    continue; 
}
```

Instância moodle:

```php
$instances = enrol_get_instances($course->id, true);
$instance = array_values($instances)[0];
$plugin = enrol_get_plugin('manual');
```

Atribuindo papéis:

```php
// TODO fazer uma consulta na tabela role para pegar o $role
$userdb = $DB->get_record('user',['email'=>$user[3]]);
$student_role = $DB->get_record('role', ["shortname" => 'student']);
$plugin->enrol_user($instance, $userdb->id, $student_role->id);

// Não é necessário
//$context = context_system::instance();
//role_assign($student->id, $userdb->id, $context->id);
```




