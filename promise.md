# Задачи для практики с собеседованиям на frontend-разработчика - Promise

## Promise

### Задача

Сделать аналог Promise.all и Promise.allSettled

```ts
const resolveFunc = async (val, timeout) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(val);
    }, timeout);
  });
};

const rejectFunc = async (val, timeout) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      reject(val);
    }, timeout);
  });
};

const requests = [
	resolveFunc(1, 100),
	resolveFunc(2, 900),
	resolveFunc(3, 800),
	resolveFunc(4, 1500),
	resolveFunc(5, 3500),
	resolveFunc(6, 1500)
];

promiseAll(requests)
  .then(console.log)
  .catch(console.error);

promiseSettled(requests)
  .then(console.log)
  .catch(console.error);
```

<details>
  <summary>Решение</summary>

Promise.all
```ts
const promiseAll = async promises => {
  return new Promise((resolve, reject) => {
    const result = new Array(promises.length);

    let finishedPomisesCount = 0;

    for (let index in promises) {
      promises[index]
        .then(data => {
          finishedPomisesCount++;
          result[index] = data;

          if (promises.length === finishedPomisesCount) {
            resolve(result);
          }
        })
        .catch(data => {
          reject(data);
        });
    }
  });
};

promiseAll(requests)
  .then(console.log)
  .catch(console.error);

```

Promise.allSettled
```ts
const promiseAllSettled = async promises => {
  return new Promise((resolve, reject) => {
    const result = new Array(promises.length);

    let finishedPomisesCount = 0;

    for (let index in promises) {
      let tempPromiseResult = null;
      promises[index]
        .then(data => {
          tempPromiseResult = { status: 'fulfilled', data };
        })
        .catch(data => {
          tempPromiseResult = { status: 'rejected', data };
        })
        .finally(() => {
          result[index] = tempPromiseResult;
          finishedPomisesCount++;

          if (promises.length === finishedPomisesCount) {
            resolve(result);
          }
        });
    }
  });
};
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->




 ### Задача

Есть GET запрос https://winry.khashaev.ru/posts/ID, который возвращает JSON вида:
```json
{
   "userId": 5,
   "id": 42,
   "title": "commodi ullam sint et excepturi error explicabo praesentium voluptas",
   "body": "odio fugit voluptatum ducimus earum autem est incidunt voluptatem"
}
```

Нужно сделать функцию, которая принимает массив ID постов, параллельно их запрашивает и отдает результат

```ts
const fetchData = async (array: any): any => {
}

fetchData([42, 2, 3])
  .then(value => console.log(value))
  .catch(err => console.log(err))
```

<details>
  <summary>Решение</summary>

```ts
interface Data {
  userId: number;
  id: number;
  title: string;
  body: string;
}

const fetchData = async(array: number[]): Promise<Data[]> => {
  const res = array.map(num => fetch(`https://winry.khashaev.ru/posts/${num}`).then(res => res.json()))

  const response = await Promise.all(res);

  return response
}

fetchData([42, 2, 3]).then(value => console.log(value)).catch(err => console.log(err))
```

</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->



