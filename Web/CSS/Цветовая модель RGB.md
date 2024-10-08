## Шестнадцатеричная запись

При использовании такой записи используется шестнадцатеричная система счисления Запись делится на три блока по два числа:
1. от `00` до `ff` — количество красного цвета
2. от `00` до `ff` — количество зелёного цвета
3. от `00` до `ff` — количество синего цвета
```css
/* Белый цвет */ 
color: #ffffff; 

/* Чёрный цвет */ 
color: #000000;
```
## Функция rgb

Второй способ указать цвет с помощью цветовой модели RGB — использование специальной функции `rgb()`. Она принимает три числа от _0_ до _255_, где первое число определяет количество красного цвета, второе число — количество зелёного цвета, а третье — количество синего. Ничего не напоминает?

Если вам показалось, что это похоже на шестнадцатеричную систему, то будете правы — суть точно такая же. Только записываем цвета в привычных нам цифрах. В остальном всё то же самое, а значит можно создать фиолетовый цвет, используя функцию `rgb()`:
```css
.text { 
	color: rgb(255, 0, 255); 
}
```
## Прозрачность и функция rgba()

При использовании фонового цвета часто используют не просто цвет, но и добавляют ему прозрачность. В цветовой модели RGB для этого используется понятие _«альфа»-канал_. Он определяет насколько прозрачный цвет должен выводиться и задаётся числом от _0_ до _1_, где _0_ — полностью прозрачный цвет, а _1_ — полностью непрозрачный.

Чтобы использовать альфа-канал создаётся функция `rgba()`, где `a` — `alpha`. В остальном всё точно так же, как и было изучено ранее. Сделаем полупрозрачный фиолетовый фон:
```css
.background { 
	background-color: rgba(255, 0, 255, 0.5); 
}
```