# Задачи для практики с собеседованиям на frontend-разработчика - Event Loop

## Event Loop

### Задача

Что будет выведено в консоль?

```ts
setTimeout(() => console.log('setTimeout 1'), 0);

new Promise((resolve, reject) => {
    console.log('Promise 1');
    resolve();
    console.log('Promise 2');
}).then(() => console.log('Promise 3'));

Promise.resolve().then(() => setTimeout(() => console.log('setTimeout 2'), 0));

Promise.resolve().then(() => console.log('Promise 4'));

setTimeout(() => console.log('setTimeout 3'), 0);

console.log('final');
```

<details>
  <summary>Решение</summary>

```ts
Promise 1

Promise 2

final

Promise 3

Promise 4

setTimeout 1

setTimeout 3

setTimeout 2
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


 ### Задача
Логика решения в прошлой задаче

```ts
setTimeout(() => console.log(1));

new Promise(function (resolve, reject) {
    resolve();
  })
  .then(() => console.log(2))
  .then(() => console.log(3))
  .catch(() => console.log("err"))
  .then(() => setTimeout(() => console.log(4)));

console.log(5);

```

<details>
  <summary>Решение</summary>

```ts
5
2
3
1
4
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### Задача

```ts
// Задача 1
Promise.reject("a")
.catch((p)=> p + "b")
.catch((p)=> p + "c")
.then((p)=>p + "d")
.then((p)=>console.log(p))

// Задача 2
Promise.reject("a")
    .then((p)=>p + "2")
    .then((p)=>p + "3")
    .then((p)=> {
        throw new Error('4')
    })
    .catch((p)=> p + "5")
    .catch((p)=> p + "7")
    .then((p)=>p + "8")
    .catch((p)=> p + "9")
```

<details>
  <summary>Решение</summary>

Первый catch обрабатывает первую ошибку, выборшенную через reject.
Дальше в catch не проваливаемся и можно обрабатывать полученное значение

```ts

// Задача 1
Promise.reject("a")
.catch((p)=> p + "b") // отловили reject
.catch((p)=> p + "c") // пропускаем, так как уже отловили ошибку и получили результат
.then((p)=>p + "d") // обработали результат
.then((p)=>console.log(p)) // обработали результат

// Результат abd


// Задача 2
Promise.reject("a")
.then((p)=>p + "2") // пропускаем, так как нужно отловить ошибку
.then((p)=>p + "3") // пропускаем, так как нужно отловить ошибку
.then((p)=> { throw new Error('4') }) // отловили reject. И отдали еще одну ошибку
.catch((p)=> p + "5") // обрабатываем выброшенную ошибку
.catch((p)=> p + "7") // пропускаем, так как уже отловили ошибку и получили результат
.then((p)=>p + "8") // обработали результат
.catch((p)=> p + "9")  // пропускаем, так как уже отловили ошибку и получили результат

// Результат a58
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### Задача

```ts
console.log('A')

const p = new Promise((resolve) => {
  resolve('');
  console.log('B')
});

p.then(() => {
  p.then(() => console.log('C'));
  console.log('D');
});

setTimeout(() => {
   console.log('E')
}, 0)

p.then(() => console.log('F'));

```

<details>
    <summary>Решение</summary>

```ts
A
B
D
F
C
E
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->
