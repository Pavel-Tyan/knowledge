Теги: #React #Frontend
## Содержание заметки
**Бэтчинг** - это процесс, при котором React собирает несколько вызовов обновления состояния в один батч (пакет) для однократного обновления, что позволяет уменьшить количество ререндеров, что положительно влияет на производительность. К примеру при многократном вызове setState для одного и того же элемента ререндер выполнится всего 1 раз.
Также при вызове setState для разных элементов ререндер также выполнится 1 раз.