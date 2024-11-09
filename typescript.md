# Задачи для практики с собеседованиям на frontend-разработчика - TypeScript

#### Такие задачи невозможно решить самому. Обязательно нужно готовиться к ним

## TypeScript

### ✅ Задача

Написать свою реализацию Partial (Объект, где все поля опциональные)

```ts
type User = {
  id: number
}

type PartialUser = NewPartial<User>;

type NewPartial = ....
```

<details>
  <summary>Решение</summary>

```ts
type NewPartial<T> = {
  [key in keyof T]?: T[key];
}
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

Записать правильный тип для MYType, чтоб переменные, которым это тип будет присвоен имели тип состоящий из ключей объекта obj

```ts
const obj = {
  name: "Nik",
  age: 25,
};

type MYType = any;
// Вместо any нужный тип

//---------

/** Тут не должно быть ошибок типов */

const var1: MYType = "name";

const var2: MYType = "age";

//----------

/** Тут должны быть ошибки типов */

const var3: MYType = "test";

const var4: MYType = 25;

```

<details>
  <summary>Решение</summary>

```ts
  type MYType = keyof obj // "name" |"age"
```
</details>

  ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


 ### ✅ Задача

 Написать generic Pick

 ```ts
type User = {
  id: number;
  name: string;
  surname: string;
}

type RequiredUser = NewPick<User, 'id' | 'name'>

type NewPick = ....

```

<details>
  <summary>Решение</summary>

```ts
type NewPick<T, K extends keyof T> = {
    [P in K]: T[P];
}
```
</details>
