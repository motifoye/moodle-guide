# Расписание

**Задача:** Отобразить расписание для студентов в lms moodle.
Расписание хранится в мастер системе 1С.

> Текущее решение направлено на размещение расписания в календаре.

**Возможные способы решения задачи:**

1. Calendar API: Стандартное средство Moodle, предоставляет API для работы с календарем. Может использоваться для программной интеграции.

2. LTI External Tool: Встроенный модуль Moodle поддерживает LTI. Если в 1С есть поддержка стандарта LTI, можно использовать этот механизм для передачи данных в Moodle.

   > LTI - Learning Tools Interoperability - стандарт, который интегрирует обучающие платформы с обучающими инструментами. По сути, стандарт LTI обеспечивает безопасный двунаправленный обмен информацией между Moodle и внешними обучающими инструментами.

3. CDO ([cdo-global.ru](https://cdo-global.ru/services/moodle-razrabotka/))
