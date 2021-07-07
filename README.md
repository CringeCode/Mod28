## Проверка файла index.html:
### Проверка SEO с помощью Google Chrome Lighthouse:
* Присутствует метатег `<meta name="viewport">` со свойством width или initial-scale
* Документ содержит элемент `<title>`
* Код статуса HTTP действителен
* У ссылок есть описания
* Ссылки можно просканировать
* Страница доступна для индексации
* Для документа указан действительный атрибут hreflang
* В документе нет плагинов
* **<span style="color: red">В документе нет метаописания</span>**

**Исправление:**
```
    <meta name="description" content="Практическая работа по использованию сортировки и фильтрации массивов в JS">
    <meta name="keywords" content="JavaSript, массивы,сортировка, фильтрация">
```

### Проверка сайта на валидаторе W3C:
Ответ валидатора: <span style="color: green">Document checking completed. No errors or warnings to show.</span>

### Проверка на соответсвие БЭМ (VS Code BEM Helper и ручная проверка):
* `<button class="sort__change__btn line">Сменить алгоритм сортировки</button>` — <span style="color: red">Класс может иметь только один наследуемый блок или элемент</span>. Исправлено на <span style="color: green">sort__change-btn</span>
* `<button class="sort__action__btn line">Сортировать</button>` — <span style="color: red">Класс может иметь только один наследуемый блок или элемент</span>. Исправлено на <span style="color: green">sort__action-btn</span>
* `<button class="add__action__btn line">Добавить фрукт</button>` — <span style="color: red">Класс может иметь только один наследуемый блок или элемент</span>. Исправлено на <span style="color: green">add-action__btn</span>
* `<button class="addX100__action__btn line">Добавить 100 случайных</button>` — <span style="color: red">Класс может иметь только один наследуемый блок или элемент</span>. Исправлено на <span style="color: green">add-action__btn-x100</span>
* <span style="color: green">(ручная проверка) добавлены недостающие классы</span>

### Применено форматирование документа с помощью плагина **Beautify**

## Проверка файла index.js:
### Соответствие принципу DRY:
<span style="color: red">Найдены повторяющиеся действия</span>

```
function comparationColor(a, b) {
  const codeA = [...a].forEach(char => {
    code += char.charCodeAt(0);
  });;
  const codeB = [...b].forEach(char => {
    code += char.charCodeAt(0);
  });;

  if (codeB < codeA)
    return true;

}
```

**Исправление:**
```
function comparationColor(a, b) {

  function stringToCode(str) {
    let code = "";
    [...str].forEach(char => {
      code += char.charCodeAt(0);
    });
    return parseInt(code);
  }

  const codeA = stringToCode(a);
  const codeB = stringToCode(b);

  if (codeB < codeA)
    return true;

}
```
### Соответсвие принципу KISS:
<span style="color: red">Найдено нерациональное решение</span>

```
function colorToClass(color) {
  switch (color) {
    case 'фиолетовый': return 'fruit_violet';
    case 'зеленый': return 'fruit_green';
    case 'розово-красный': return 'fruit_carmazin';
    case 'желтый': return 'fruit_yellow';
    case 'светло-коричневый': return 'fruit_lightbrown'
  }
}
```
**Исправление:**

Оптимальным решением будет использование Map:

```
const colorClasses = new Map([
  ['фиолетовый', 'fruit_violet'],
  ['зеленый', 'fruit_green'],
  ['розово-красный', 'fruit_carmazin'],
  ['желтый', 'fruit_yellow'],
  ['светло-коричневый', 'fruit_lightbrown']
])

function colorToClass(color) {
  return colorClasses.get(color);
}
```
### Соответствие принципу YAGNI:
<span style="color: red">В исходной задаче не было требования создавать функцию "Добавить 100 случайных"</span>

**Исправление:**

<span style="color: green">Соответсвующие части кода удалены</span>

### Соответствие принципам SOLID:
<span style="color: red">Замечены случаи нарушения принципа единственной ответственности:</span>

```
filterButton.addEventListener('click', () => {
  if ((isNaN(minWeight.value)) || (isNaN(maxWeight.value)) || (minWeight.value == "") || (maxWeight.value == "")) {
    warning('Задайте минимальный и максимальный вес в виде целого числа');
    return false;
  }

  filterFruits();
  display();
});
```

```
addActionButton.addEventListener('click', () => {
  if ((kindInput.value == "") || (weightInput.value == "")) {
    warning('Заполните поля kind и weight');
    return false;
  }

  addCard(kindInput.value, colorInput.value, weightInput.value);
  display();
});
```

**исправление:**

<span style="color: green">Валидация полей вынесена в отдельные функции</span>

```
function validateFilter() {
  if ((isNaN(minWeight.value)) || (isNaN(maxWeight.value)) || (minWeight.value == "") || (maxWeight.value == "")) {
    warning('Задайте минимальный и максимальный вес в виде целого числа');
    return false;
  } else return true;
}

filterButton.addEventListener('click', () => {
  if (validateFilter() === true) {
    filterFruits();
    display();
  }
});
```

```
function validateAdd() {
  if ((kindInput.value == "") || (weightInput.value == "")) {
    warning('Заполните поля kind и weight');
    return false;
  } else return true;
}

addActionButton.addEventListener('click', () => {
  if (validateAdd() === true) {
    addCard(kindInput.value, colorInput.value, weightInput.value);
    display();
  }
});
```