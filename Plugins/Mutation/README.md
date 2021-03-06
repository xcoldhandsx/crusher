В `Crusher` есть возможность использования пользовательских мутаций.

Пользовательская мутация представляют из себя плагин на языке программирования `Python`.

Для использования пользовательской мутации необходимо выполнить следующие действия:
1. написать плагин;
2. добавить плагин в `Crusher`;
3. добавить новую мутацию в конфиг-файл для запуска `Crusher`;
4. запустить `Crusher` с этим конфиг-файлом.

### 1. Написание плагина
Плагин представляет из себя `.py` файл со следующими необходимыми компонентами:
* функция инициализации `initialization()`;
* класс мутации.

#### Функция `initialization()`
Функция `initialization()` возвращает словарь, который должен содержать следующие поля:

* `mut_name` - имя мутации (имя файла плагина без расширения);

* `class` - имя класса мутации;

* `method` - имя метода класса мутации, реализующий алгоритм мутации.

Пример функции `initialization()`:

```python
def initialization():
    info_mut_alg = {"mut_name": "duplicate_mutator",
                    "class": "DuplicateMutator", 
                    "method": "perform_mutation"}
    return info_mut_alg
```

#### Класс мутации
Имя класса должно соответствовать значению поля `"class"` из словаря, возвращаемого функцией `initialization`.

##### Конструктор

Конструктор класса мутации обязательно должен содержать следующий код (для правильной инициализации плагина в `Crusher`):
```python
    def __init__(self, init=None):
        if not init is None:
            self.init = True
        else:
            self.init = False
```

При этом в конструкторе могут инициализироваться любые другие поля, необходимые для мутации.

##### Метод мутации
Имя метода должно соответствовать значению поля `"method"` из словаря, возвращаемого функцией `initialization`.

Метод принимает в качестве аргумента массив байт (`bytearray`) - буфер, который передаёт ядро `Crusher` для мутаций, и возвращает также массив байт.

Метод должен обязательно содержать следующий код (для правильной его инициализации в `Crusher`):
```python
    def perform_mutation(self, data=None):
        if self.init:
            self.init = False
            return 42
```

Предполагается, что в остальной части тела метода проводятся манипуляции над `data` и возвращается новый массив байт.
Однако, использование `data` не является обязательным.

### 2. Добавление плагина в `Crusher`
Скопируйте плагин мутации (например, [duplicate_mutator.py](duplicate_mutator.py)) в директорию с пользовательскими мутациями `Crusher`:
```shell script
cp duplicate_mutator.py /path/to/crusher/bin_x86-64/Plugins/mutation
```

### 3. Добавление мутации в конфиг-файл для `Crusher`
Конфиг-файл для `Crusher` (наряду с аргументами командной строки) позволяет более тонко настраивать его работу. Используется формат `JSON`.

Для использования новой мутации при фаззинге нужно добавить её имя в список мутаций в поле `"mutation"` в конфиг-файле.

Важно помнить, что по умолчанию программа не будет запускаться фаззером после применения пользовательской мутации к данным. Это оставляет возможность применения сразу нескольких мутаций перед запуском программы.

Для запуска программы после применения пользовательской мутации нужно явно указать это в конфиг-файле - добавить имя мутации в соответствующий список в поле `"run_after_mutation"`.

Пример конфиг-файла с мутацией `duplicate_mutator`:
```json
{
  "mutation": ["duplicate_mutator"],
  "run_after_mutation": ["duplicate_mutator"]
}
```

### 4. Запуск `Crusher`
Пример запуска `Crusher` с пользовательской мутацией, где [config.json](config.json) - конфиг-файл:
```shell script
/path/to/crusher/bin_x86-64/fuzz_manager --start 4 -i in -o out --config-file config.json -- ./test @@
```

Пример плагина мутации - [duplicate_mutator.py](duplicate_mutator.py).

Пример конфиг-файла - файл [config.json](config.json).
