Эффект ховера (покрас кнопки) не сбрасывается на мобилках после клика. Как убрать покраску после клика? Решение проблемы:
```CSS
@media (hover: hover) {
	button:hover {
		background: blue;
	}
}

@media (hover: none) {
	button:hover {
		background: blue;
	}
}
```
На десктопе ничего не изменится.