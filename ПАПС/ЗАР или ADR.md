Architecture Decision Records или Запись Архитектурных решений - минималистичный способ записи информации о принятом архитектором и проектной командой решении, который позволяет удобно систематизировать и сохранять такие решения без лишних дополнительных церемоний и документов. Обычно ЗАР оформляется в виде простого текстового документа, которые собирают в отдельный каталог репозитория или вики-системы. Но также ЗАР может содержать и нужные схемы, изображения, ссылки.

Запись об архитектурном решении (ADR) содержит (кроме номера): 
|> Дата создания, изменения 
|> Статус (принято/предложено/отклонено/заменено) 
|> Контекст решения 
|> Решение (Мы будем делать вот так ...) 
|> Последствия (Решение, вероятно, приведет к …) 
|>>>> Риски (Основные риски, которые могут быть вызваны решением …) 
|>>>> Оценка результативности (Заполняется позже)

Пример ЗАР
_Use Plain JUnit5 for advanced test assertions _
_Context and Problem Statement_ 
How to write readable test assertions? 
How to write readable test assertions for advanced tests? 
_Considered Options_ 
* Plain JUnit5 
* Hamcrest 
_Decision Outcome_ 
Chosen option: "Plain JUnit5", because comes out best (see "Pros and Cons of the Options" below). 
_Consequences_ 
* Good, because tests are more readable 
* Good, because more easy to write tests 
* Good, because more readable assertions 
* Bad, because more complicated testing leads to more complicated assertions 
_Confirmation_ 
* Check project dependencies, JUnit5 should appear (and be the only test assertion library). 
* Collect experience with JUnit5 in sprint reviews and retrospectives: does the gained experience match the pros and cons evaluation below? 
* Decide whether and when to review the decision (this is the 'R' in the [ecADR definition of done]
_Pros and Cons of the Options_
Plain JUnit5 Homepage: JabRef testing guidelines: Example: 
```java 
String actual = markdownFormatter.format(source); assertTrue(actual.contains("Markup  ")); 
assertTrue(actual.contains("- list item one")); assertTrue(actual.contains("- list item 2")); assertTrue(actual.contains("> rest")); assertFalse(actual.contains("\n")); 
``` 
* Good, because Junit5 is "common Java knowledge" 
* Bad, because complex assertions tend to get hard to read 
* Bad, because no fluent API
_Pros and Cons of the Options_ 
_Hamcrest Homepage:_ 
* Good, because offers advanced matchers (such as `contains`) 
* Bad, because not full fluent API 
* Bad, because entry barrier is increased