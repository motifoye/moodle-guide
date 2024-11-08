# Moodle examples

## Создание блока

Чтобы создать собственный блок в Moodle, нужно разработать плагин типа "block". Блоки — это элементы, которые могут быть добавлены на страницы сайта Moodle для отображения различной информации и функций. Приведу шаги, которые помогут вам создать базовый блок:

### Шаг 1. Создание структуры плагина

1. В корневой директории Moodle найдите папку `blocks`.

2. Внутри `blocks` создайте новую папку с уникальным именем для вашего блока, например, `my_custom_block`.
3. Внутри этой папки создайте следующие файлы и папки:
   - `block_my_custom_block.php`: основной файл плагина блока.
   - `version.php`: файл с информацией о версии.
   - `db/access.php`: файл для настройки прав доступа.
   - `lang/en/block_my_custom_block.php`: файл для языковых строк (например, для английского языка).

### Шаг 2. Основной файл блока (`block_my_custom_block.php`)

Этот файл содержит основную логику блока. Создайте файл block_my_custom_block.php и добавьте в него следующий базовый код:

```php
<?php
// Проверка на безопасность доступа
defined('MOODLE_INTERNAL') || die();

class block_my_custom_block extends block_base {
    
    // Инициализация блока
    public function init() {
        $this->title = get_string('pluginname', 'block_my_custom_block');
    }

    // Содержимое блока
    public function get_content() {
        if ($this->content !== null) {
            return $this->content;
        }
        
        $this->content = new stdClass();
        $this->content->text = 'Hello, this is my custom block!';
        $this->content->footer = '';

        return $this->content;
    }
}
```

Здесь:

- Метод init() устанавливает заголовок блока.
- Метод get_content() возвращает содержимое блока, которое будет отображено на странице.

### Шаг 3. Создание файла версии (`version.php`)

В `version.php` укажите информацию о версии и зависимостях плагина. Создайте файл `version.php` и добавьте следующий код:

```php
<?php
defined('MOODLE_INTERNAL') || die();

$plugin->component = 'block_my_custom_block';
$plugin->version = 2023110600; // Дата в формате ГГГГММДДЧЧ
$plugin->requires = 2021051700; // Минимальная версия Moodle (3.11)
$plugin->release = '1.0';
```

### Шаг 4. Настройка прав доступа (`access.php`)

Если ваш блок будет иметь права доступа, создайте файл access.php в папке db и укажите доступные права. Например:

```php
<?php
defined('MOODLE_INTERNAL') || die();

$capabilities = [
    'block/my_custom_block:addinstance' => [
        'captype' => 'write',
        'contextlevel' => CONTEXT_BLOCK,
        'archetypes' => [
            'editingteacher' => CAP_ALLOW,
            'manager' => CAP_ALLOW,
        ]
    ],
    'block/my_custom_block:myaddinstance' => [
        'captype' => 'write',
        'contextlevel' => CONTEXT_SYSTEM,
        'archetypes' => [
            'student' => CAP_PREVENT,
        ]
    ]
];
```

Эти права позволят учителям и менеджерам добавлять блок на страницы.

### Шаг 5. Добавление языковых строк

Создайте папку `lang/en/` и добавьте файл `block_my_custom_block.php` для языковых строк:

```php
<?php
$string['pluginname'] = 'My Custom Block';
$string['my_custom_block:addinstance'] = 'Add a new custom block';
$string['my_custom_block:myaddinstance'] = 'Add a new custom block to My Moodle page';
```

### Шаг 6. Установка блока

1. После создания файлов перейдите на страницу администратора Moodle, чтобы запустить установку.
2. Moodle автоматически обнаружит новый блок и предложит установить его.
3. Нажмите «Продолжить» и завершите установку.

### Шаг 7. Добавление блока на страницу

1. Перейдите к странице, где хотите разместить блок (например, на главную страницу или в курс).
2. Включите режим редактирования и выберите ваш блок в списке доступных блоков.
3. После добавления блок будет отображать текст «Hello, this is my custom block!», заданный в `get_content()`.

### Дополнительные функции

Вы можете добавлять в блок HTML-контент, настраиваемые параметры, форму обратной связи и динамическое обновление данных.
