### Плагины для инструмента Crusher
В данной директории представлены плагины, которые можно использовать для расширения функциональных возможностей инструмента `Crusher`.

#### Плагины отправки данных
У пользователя есть возможность написания плагинов отправки данных по сети на языке программирования `Python`. 
В директории [Network](Network) размещены примеры пользовательских плагинов передачи данных по сети и инструкции по их использованию
и созданию новых плагинов.

#### Плагины мутаций  
У пользователя есть возможность использования при фаззинге собственных алгоритмов мутаций, реализованных в виде плагинов на языке программирования `Python`. 
В директории [Mutation](Mutation) размещена инструкция по их написанию и использованию, а также пример плагина.

#### Плагины пост-обработки данных  
Перед подачей данных приложению иногда требуется произвести над ними дополнительную обработку, например: добавить некоторые заголовки, сжать, зашифровать.
Эти действия выполняются после мутаций над данными с помощью скриптов пост-обработки данных, реализованных в виде плагинов на языке программирования `Python`.
У пользователя есть возможность использования собственных скриптов пост-обработки данных.
В директории [PostProcessing](PostProcessing) размещена инструкция по их написанию и использованию, а также пример плагина.
