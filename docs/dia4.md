---
layout: default
title: Dia 4
nav_order: 5
---
1. TOC
{:toc}
---

Finalizando *createcourses.php*:

```php
$lines = explode(PHP_EOL,$record->file);
foreach($lines as $line){
    $course = str_getcsv($line);

    if($DB->get_record('course', ['shortname' => $course[0]])){
        \core\notification::error($course[0] . ' já existe');
        continue;
    }

    $newcourse = new \stdClass();
    $newcourse->shortname = $course[0];
    $newcourse->fullname = $course[1];
    $newcourse->idnumber = $course[0];
    $newcourse->category = 1;

    $start_time = strtotime($course[2]);
    $end_time = strtotime($course[3]);

    $newcourse->startdate = $start_time;
    $newcourse->enddate = $end_time;
    $newcourse->timemodified = time();

    $created_course = \create_course($newcourse);
    \core\notification::success($course[1] . ' cadastrado com sucesso');
}

redirect($CFG->wwwroot . '/blocks/importstuffs/view.php');
```

