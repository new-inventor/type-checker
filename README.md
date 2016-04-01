#Type checker

Проверяет совпадение типа переменной с указанными типами. Может бросить исключение если необходимо.

##Установка

через composer

`composer require new-inventor/type-checker`

##Принцип работы

Так как класс реализует шаблон проектирования "Одиночка" то сначала необходимо получить его экземпляр.

`$checker = TypeChecker::getInstance();`

Если переменная должна иметь строго 1 тип то можно воспользоваться функциями проверки простых типов.

Например для типа Integer:

`$checker->isInt($var, 'var');`

здесь первый параметр - значение переменной, второй - имя переменной 

Если переменная может быть нескольких типов, то следует воспользоваться методом `check`:

`$checker->check($var, [SimpleTypes::BOOL, '\Another\Class\Name'], 'var');`

здесь вторым параметром передаются набор правильных классов.

Если необходимо проверить елементы **одномерного** массива:

`$checker->checkArray($array, [SimpleTypes::BOOL, '\Another\Class\Name'], 'name');`

После проверки можно бросить исключение:

```$checker->throwTypeErrorIfNotValid();```

при этом исключение возникнет если параметр не отвечает заданным типам.

Так же можно просто получить результат проверки:

```$res = $checker->result();```

##Немного usecase'ов

При проверке пожно пользоваться цепочными вызовами:

```
TypeChecker::getInstance()
    ->isInt($var, 'var')
    ->throwTypeErrorIfNotValid();
```

При проверке элементов **одномерного** массива, для выбрасывания исключения стоит использовать метод `throwCustomErrorIfNotValid($message)`, так как при использовании другого метода ошибка будет некорректной.

Можно использовать связку `result()` и `throwTypeError`/`throwCustomError`, например:

```
if(!$checker->isInt($var, 'var')->result()){
    ...
    $checker->throwTypeError();
}
```