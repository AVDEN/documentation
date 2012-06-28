Различные хаки упрощающие разработку портала
============================================

## Модули
##### mod_k2_tools (добавляем класс в виде альяса категории)
`modules\mod_k2_tools\helper.php`
```php
<?php echo $row->alias ?>
```

## Компоненты
##### Вывод дополнительных полей в произвольном месте
В шапку файла `item.php` вставляем следующий код:
```php
<?php
    //setup new array
    $exFields = array();
    //loop thru extrafields and build collection of key and values
    foreach ($this->item->extra_fields as $key=>$extraField):
    $exFields[$extraField->name] = $extraField->value;
    endforeach;
?>
```
Затем в том месте где на нужно вывести значение дополнительно поля вставляем код:
```php
<?php echo $exFields["Телефон"]; ?>
```
где `Телефон` название дополнительного поля


##### Не выводить пустые дополнительные поля
```php
<?php foreach ($this->item->extra_fields as $key=>$extraField): ?>
    <?php if ($extraField->value != ''): ?>
        <li class="<?php echo ($key%2) ? "odd" : "even"; ?> type<?php echo ucfirst($extraField->type); ?> group<?php echo $extraField->group; ?>">
            <span class="catItemExtraFieldsLabel"><?php echo $extraField->name; ?></span>
            <span class="catItemExtraFieldsValue"><?php echo $extraField->value; ?></span>
        </li>
    <?php endif; ?>
<?php endforeach; ?>
```
`<?php if ($extraField->value != ''): ?>` - если дополнительное поле содержит значение (не пустое), то вывести его


## Joomla
##### Обрезание текста вопросами �
В каталоге компонента/модуля/шаблона ищем файл в котором встречается `substr` и заменяем на `JString::substr`
`JString` - это своего рода джумловская обработка строк, которая не знает проблем с UTF-8.