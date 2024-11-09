# Задачи для практики с собеседованиям на frontend-разработчика - задачи, приближенные к реальной жизни

## Задачи из реальной жизни

### Задача

Нужно написать чистый читаемый код
Рассчитать оплату за проживание в отеле с учетом рабочих и праздничных дней

Функция может принимать 2 аргумента
1. Количество ночей проживания в отеле (обязательный параметр)
2. Дата заселения (необязательный параметр). Если значение не указано, то отсчет ведется от текущего дня
- Стоимость проживания в будние дни (с понедельника по пятницу) стоит 1500 руб
- Стоимость проживания в выходные дни (суббота, воскресенье) стоит 2200 руб

```ts
const prices = {
  weekday: 1500,
  holiday: 2200,
};

function bookingCalculate() {

}

console.log(bookingCalculate(7)); // 11900
console.log(bookingCalculate(3, new Date('2023-11-10'))); // 5900
```

<details>
  <summary>Решение</summary>

```ts
const prices = {
  weekday: 1500,
  holiday: 2200,
};

const addDays = (date, days) => {
  const result = new Date(date);
  result.setDate(result.getDate() + days);
  return result;
};

const checkIsHoliday = day => {
  return !checkIsWeekdaty(day);
};

const checkIsWeekdaty = date => {
  const day = new Date(date).getDay();
  return day > 0 && day < 6;
};

function bookingCalculate(nights, startDate = new Date()) {
  const endDate = addDays(startDate, nights);

  let tempStartDate = startDate;
  let weekDays = 0;
  let hoilDays = 0;

  while (tempStartDate < endDate) {
    if (checkIsWeekdaty(tempStartDate)) {
      weekDays += 1;
    }
    if (checkIsHoliday(tempStartDate)) {
      hoilDays += 1;
    }
    tempStartDate = addDays(tempStartDate, 1);
  }
  return weekDays * prices.weekday + hoilDays * prices.holiday;
}

console.log(bookingCalculate(7)); // 11900
console.log(bookingCalculate(3, new Date('2023-11-10'))); // 5900

```
</details>


 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->




### Задача

Реализовать и типизировать собственный pick

```ts
const pick = (obj, ...keys) => {

}
```

<details>
  <summary>Решение</summary>

```ts
const pick = (obj, ...keys) => {
  return keys.reduce((acc, key) => {
    acc[key] = obj[key];
    return acc;
  }, {});
};

console.log(pick({ a: 1, b: 2, c: 3 }, 'a', 'c'));


type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
}
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->



### Задача

1) Написать аналог функции map
```ts
const customMap = () => {}

const array = [{ id: 1 }, { id: 2 }];
const format = (el, index) => `${el.id}|${index}`;
const formattedArray = customMap(array, format);

```

2) Написать полифил для map, используя метод из предыдущего пункта (обращаться к Array.prototype)
```ts
const format = (el, index) => `${el.id}|${index}`;
const array = [{ id: 1 }, { id: 2 }];

const formattedArray = array.customMap(format);

```

<details>
  <summary>Решение</summary>

1) Написать аналог функции map

```ts
const customMap = (array, callback) => {
  const result = [];

  for (let i = 0; i < array.length; i++) {
    result.push(callback(array[i], i));
  }

  return result;
};
```

2) Написать полифил для map, используя метод из предыдущего пункта (обращаться к Array.prototype)

```ts
Array.prototype.customMap = function(callback) {
  const result = [];

  for (let i = 0; i < this.length; i++) {
    result.push(callback(this[i], i));
  }

  return result;
};
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->



### Задача

1. Вернуть объект с ключами type из масива, значение - массив элементов с таким type
2. Вернуть объект с ключами type, а значение - объект вида {count: количество, weight: суммарный вес}

```ts
const arr = [
  { type: "banana", weight: 32 },
  { type: "apple", weight: 24 },
  { type: "kiwi", weight: 55 },
  { type: "banana", weight: 44 },
  { type: "orange", weight: 5 },
];

const groupByType = (arr) => {

};

console.log(groupByType(arr))
```

<details>
  <summary>Решение</summary>

1. Вернуть объект с ключами type из масива, значение - массив элементов с таким type

```ts
const groupByType = (arr) => {
  const result = {};

  arr.forEach((item) => {
    const { type, ...rest } = item;

    if (result[type]) {
      result[type].push(rest);
    } else {
      result[type] = [rest];
    }
  });

  return result;
};

```

2. Вернуть объект с ключами type, а значение - объект вида {count: количество, weight: суммарный вес}

```ts
const groupByType = (arr) => {
  const result = {};

  arr.forEach((item) => {
    const { type, weight } = item;

    if (result[type]) {
      result[type].count++;
      result[type].weight += weight;
    } else {
      result[type] = { count: 1, weight };
    }
  });

  return result;
};
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->



### Задача

Имеется исходный массив плоских данных
  - Необходимо преобразовать его в структуру, где данные будут сгруппированы по одному из полей (кроме id)
  - Внутри сформированной группы должен лежать объект, ключами в которого должно стать поле (к примеру id)
  - Значением должен быть объект из исходного массива с соответствующем полем id, не включая само поле id
```ts
  const data = [
    { id: 1, age: 20, name: 'Иван', country: 'Russia' },
    { id: 2, age: 20, name: 'Дмитрий', country: 'USA' },
    { id: 3, age: 20, name: 'Алексей', country: 'Russia' },
    { id: 4, age: 20, name: 'Александр', country: 'USA' },
    { id: 5, age: 20, name: 'Иван', country: 'Russia' },
  ];

const groupBy = () => {

}

console.log(groupBy(data, 'country', 'id'));


// Должно получиться
  const result = {
    Russia: {
      1: { age: 20, name: 'Иван', country: 'Russia' },
      3: { age: 20, name: 'Алексей', country: 'Russia' },
      5: { age: 20, name: 'Иван', country: 'Russia' },
    },
    USA: {
      2: {age: 20, name: 'Дмитрий', country: 'USA' },
      4: { age: 20, name: 'Александр', country: 'USA' },
    }
  }
```

<details>
  <summary>Решение</summary>

```ts

const groupBy = (data, key, innerKey) => {
  return data.reduce((acc, current) => {
    const fieldByKey = current[key];
    console.log(current);

    if (!acc[fieldByKey]) {
      acc[fieldByKey] = {};
    }

    const innerKeyValue = current[innerKey];

    const rest = omit(current, 'id', 'age');

    acc[fieldByKey][innerKeyValue] = rest;

    return acc;
  }, {});
};

const omit = (obj, ...keys) => {
  const objectOfKeys = keys.reduce((acc, current) => {
    acc[current] = true;
    return acc;
  }, {});

  return Object.keys(obj).reduce((acc, key) => {
    if (objectOfKeys[key]) {
      acc[key] = obj[key];
    }

    return acc;
  }, {});
};

console.log(groupBy(data, 'country', 'id'));

```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->



### Задача

Написать класс EventEmmiter, аналог addeventlistener.
- Метод метод on, который вызывается в именем события и функцией
- Метод off, который вызывается также с названием события и функцией
- Метод emit, который триггерит все функции

```ts

class EventEmitter {

}

// Пример использования
const myEventEmitter = new EventEmitter();

const greetListener = (name: string) => {
  console.log(`Hello, ${name}!`);
};

myEventEmitter.on('greet', greetListener);
myEventEmitter.emit('greet', 'Alice'); // Output: Hello, Alice!

myEventEmitter.off('greet', greetListener);
myEventEmitter.emit('greet', 'Bob'); // No output
```

<details>
  <summary>Решение</summary>

- Создаем поле класса, где будем хранить функции по каждому названию собития - это будет объект, типа `Record<string, Function[]>`
- В on, если по такому типа нет событий создается пустой массив и пушим в него функцию
- В off делаем фильтр и удаляем найденную функцию
- В emit проходим через forEach по функциям и вызываем их

```ts
class EventEmitter {
  private events: Record<string, Function[]> = {};

  on(eventName: string, listener: Function) {
      if (!this.events[eventName]) {
          this.events[eventName] = [];
      }
      this.events[eventName].push(listener);
  }

  off(eventName: string, listener: Function) {
      if (this.events[eventName]) {
          this.events[eventName] = this.events[eventName].filter((eventListener) => eventListener !== listener);
      }
  }

  emit(eventName: string, ...args: any[]) {
      if (this.events[eventName]) {
          this.events[eventName].forEach((listener) => {
              listener(...args);
          });
      }
  }
}

// Пример использования
const myEventEmitter = new EventEmitter();

const greetListener = (name: string) => {
  console.log(`Hello, ${name}!`);
};

myEventEmitter.on('greet', greetListener);
myEventEmitter.emit('greet', 'Alice'); // Output: Hello, Alice!

myEventEmitter.off('greet', greetListener);
myEventEmitter.emit('greet', 'Bob'); // No output
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### Задача

Сделать функцию-обертку для кэширования, которая не будет повторно вызывать фунцию при передаче одних и тех же параметров

```ts
function cache(callback) {
}

const getSum = (a, b) => {
  console.log(':: calculation');
  return a + b;
};
const cachedSum = cache(getSum);

console.log(cachedSum(1, 2)); // должен быть вызов sum
console.log(cachedSum(2, 3)); // должен быть вызов sum
console.log(cachedSum(1, 2)); // не вызываем функцию sum и берем значение из кэша
console.log(cachedSum(2, 3)); // не вызываем функцию sum и берем значение из кэша
```

<details>
 <summary>Решение</summary>

 Кэширование будет работать, но `'2'` и 2 будут считаться одинаковым значением

```ts
function cache(callback) {
  const cache = {};

  return function(...args) {
    const key = args.join('_');

		if (!cache[key]) {
      const result = callback(...args);
      cache[key] = result;
    }

    return cache[key];
  }
}

```

Кэширование разных по типу, но одинаковых по значению параметров

```ts
console.log(cachedSum(1, 2));
console.log(cachedSum(1, 2));

function cache(callback) {
  const cache = {};

  return function(...args) {
    const key = args.sort().join('_');

    if (!cache[key]) {
      const result = callback(...args);
      cache[key] = result;
    }

    return cache[key];
  };
}
```

Если нужна возможность передавать не только примитивы, но и объекты, то нужно серриализовать объект с помощью JSON.stringify

```ts

function cache(callback) {
  const cache = {};

  return function(...args) {
    const key = JSON.stringify(args);

    if (!cache[key]) {
      const result = callback(...args);
      cache[key] = result;
    }

    return cache[key];
  };
}

const getSum = (a, b) => {
  console.log(':: calculation');
  return a.a + b.b;
};

const cachedSum = cache(getSum);

cachedSum({ a: 1 }, { b: 2 }); // no cache
cachedSum({ a: 2 }, { b: 3 }); // no cache
cachedSum({ a: 1 }, { b: 2 }); // cache
cachedSum({ a: 2 }, { b: 3 }); // cache
```
</details>

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### Задача

Сделать вызов функции sum с двумя значениями через `(param1, param2)` и `(param1)(param2)` способоами с использованием каррирования

```ts
const sum = (val1, val2) => {

}

sum(1, 2);
sum(1)(2);
```

<details>
  <summary>Решение</summary>

```ts
const sum = (val1, val2) => {
	if (val2) {
    return val1 + val2;
  }

  return (_val2) => val1 + _val2;
}
```
</details>

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### Задача

Продолжение предыдущей задачи
Сделать каррирование с возможность передавать бесконечное количество параметров

```ts
const sum = () => {

};

sum(1)(); // 1
sum(1)(2)(3)(); // 6
```

<details>
  <summary>Решение</summary>

```ts
const sum = (a) => {
  let res = a;
  const cb = (b) => {
    if (b === undefined) {
      return res;
    }
    res += b;

    return cb;
  };

  return cb;
};

console.log(sum(1)(3)(5)());

```
</details>

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### Задача

Задача: Создайте игру "Память", в которой пользователи должны находить пары совпадающих карточек.

Правила:
1. Пользователям представляют 10 пар совпадающих карточек (всего 20 карточек) в случайном порядке.
2. Карточки отображаются в течение 3 секунд, затем все они переворачиваются, скрывая значения.
3. Пользователи могут переворачивать по 2 карточки за раз:
   - Если карточки совпадают, они должны показывать совпадение и остаться перевёрнутыми.
   - Если карточки не совпадают, они должны быть перевернуты обратно.
4. Пользователь продолжает этот процесс, пока все пары карточек не будут отгаданы.

Задачи для выполнения:
1. Отобразите на экране 10 пар совпадающих карточек в случайном порядке.
2. Реализуйте "переворачивание" карточек.
3. Отобразите карточки (лицом вверх) в течение 3 секунд в начале и затем переверните их все лицом вниз.
4. Разрешите переворачивать не более двух карточек за раз и:
   - если они совпадают, показывайте совпадение и оставляйте их перевёрнутыми.
   - если они не совпадают, переверните их обратно.
5. Кнопка перезапуска должна перезапустить игру.

[Архив](resources/memory-game/Task.zip) (Чтобы скачать файл, нажмите ... справа сверху -> Download)

<details>
  <summary>Решение</summary>

  [Итоговый архив](resources/memory-game/Result.zip) (Чтобы скачать файл, нажмите ... справа сверху -> Download)
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->
