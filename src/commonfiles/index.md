# Общие файлы

### version.php

**Метаданные версии** `Обязательный`  
**Расположение**: `/version.php`

В файле version.php содержатся метаданные о плагине.

Он используется при установке и обновлении плагина.

Этот файл содержит метаданные, используемые для описания плагина, и включает в себя такую информацию, как:

- номер версии
- список зависимостей
- необходимая минимальная версия Moodle
- срок службы плагина

<details>
<summary>Шаблон для файла version.php</summary>

```php
<?php
// This file is part of Moodle - http://moodle.org/
//
// Moodle is free software: you can redistribute it and/or modify
// it under the terms of the GNU General Public License as published by
// the Free Software Foundation, either version 3 of the License, or
// (at your option) any later version.
//
// Moodle is distributed in the hope that it will be useful,
// but WITHOUT ANY WARRANTY; without even the implied warranty of
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
// GNU General Public License for more details.
//
// You should have received a copy of the GNU General Public License
// along with Moodle.  If not, see <http://www.gnu.org/licenses/>.

/**
 * @package   plugintype_pluginname
 * @copyright 2020, You Name <your@email.address>
 * @license   http://www.gnu.org/copyleft/gpl.html GNU GPL v3 or later
 */

defined('MOODLE_INTERNAL') || die();

$plugin->version = 2024123100; // YYYYMMDDXX
$plugin->requires = 2022041900.00; // Moodle 4.0.
$plugin->supported = [400, 400];
$plugin->incompatible = 401;
$plugin->component = 'tool_example';
$plugin->maturity = MATURITY_STABLE;
$plugin->release = '41.3-lemmings-1.0';

$plugin->dependencies = [
    'mod_forum' => 2022041900,
    'mod_data' => 2022041900,
];
```
</details>

### plugintype_pluginname.php

**Языковые файлы** `Обновляется при очистке кэша` `Обязательный`  
**Расположение**: `/lang/en/plugintype_pluginname.php`

Каждый плагин должен определить набор языковых строк, как минимум, с английским переводом. Они указываются в директории `lang/en` плагина, в файле с именем плагина. Например, плагин аутентификации LDAP:

```php
// Plugin type: `auth`
// Plugin name: `ldap`
// Frankenstyle plugin name: `auth_ldap`
// Plugin location: `auth/ldap`
// Language string location: `auth/ldap/lang/en/auth_ldap.php`
```

> ВНИМАНИЕ
> Каждый плагин должен определять собственное имя в строке `pluginname`.

Функция `get_string` используется для преобразования строкового идентификатора в переведенную строку.  

```php
get_string('pluginname', '[plugintype]_[pluginname]');
```

- Документация по языковым файлам [String API](https://docs.moodle.org/dev/String_API#Adding_language_file_to_plugin) 

> ОТЛИЧИЯ МОДУЛЕЙ АКТИВНОСТИ
>
> Модули активности не исаользуют формат наименования файла **frankenstyle**, вместо этого они используют **имя плагина**. Пример, плагин форума:
>
> ```php
> // Plugin type: `mod`
> // Plugin name: `forum`
> // Frankenstyle plugin name: `mod_forum`
> // Plugin location: `mod/forum`
> // Language string location: `mod/forum/lang/en/forum.php`
> ```

<details>
<summary>Шаблон для файла</summary>

```php
<?php
// This file is part of Moodle - http://moodle.org/
//
// Moodle is free software: you can redistribute it and/or modify
// it under the terms of the GNU General Public License as published by
// the Free Software Foundation, either version 3 of the License, or
// (at your option) any later version.
//
// Moodle is distributed in the hope that it will be useful,
// but WITHOUT ANY WARRANTY; without even the implied warranty of
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
// GNU General Public License for more details.
//
// You should have received a copy of the GNU General Public License
// along with Moodle.  If not, see <http://www.gnu.org/licenses/>.

/**
 * Languages configuration for the plugintype_pluginname plugin.
 *
 * @package   plugintype_pluginname
 * @copyright Year, You Name <your@email.address>
 * @license   http://www.gnu.org/copyleft/gpl.html GNU GPL v3 or later
 */

$string['pluginname'] = 'The name of my plugin will go here';
```

</details>

### lib.php

**Глобальные функции плагина** `Legacy`  
**Расположение**: `/lib.php`

Файл `lib.php` является устаревшим файлом, который действует как мост между ядром Moodle и плагином. В последних плагинах он должен использоваться только для определения обратных вызовов и связанных с ними функций, которые в настоящее время не поддерживаются как автозагружаемые классы.

Все функции, определенные в этом файле, должны отвечать требованиям, изложенным в соответствующем разделе [Coding style](https://moodledev.io/general/development/policies/codingstyle#functions-and-methods).

> ВЛИЯНИЕ НА ПРОИЗВОДИТЕЛЬНОСТЬ
>
> Ядро Moodle часто загружает все файлы плагинов определенного типа. По соображениям производительности настоятельно рекомендуется сделать этот файл как можно меньше и реализовать в нем только необходимый код. Вся внутренняя логика плагина должна быть реализована в автозагружаемых классах.

### classes/

**Автоматически загружаемые классы** `Обновляется при очистке кэша`  
**Расположение:** `/classes/`

Moodle поддерживает и рекомендует использовать автозагружаемые PHP-классы.

Размещая файлы в каталоге `classes` или соответствующих подкаталогах, с правильным пространством имен PHP и именем класса, Moodle может автоматически загружать классы без необходимости вручную требовать или включать их.

Подробнее об этих правилах и соглашениях можно прочитать в следующей документации:

- [Coding style - namespace conventions](https://moodledev.io/general/development/policies/codingstyle#namespaces)
- [Automatic class loading](https://docs.moodle.org/dev/Automatic_class_loading)

<!-- TODO Дописать про общие файлы -->