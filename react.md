# Задачи для практики с собеседованиям на frontend-разработчика - Задачи по React

## React

### Задача

Выполнить рефакторинг

```ts
interface IProps extends React.PropsWithChildren
{
    isOpen: boolean;
    title: string;
    onModalClose: () => void;
    onSubmit: () => void;
}

const Modal: React.FC<IProps> = props => {
    useEffect(() => {
        if (props.isOpen) {
            document.title = props.title + ' - ' + document.title;
        }
        document.body.classList.add('overflow--hidden');
    }, []);

    const didModalClosed = useCallback(() => props.onModalClose(), []);


    return props.isOpen && (
        <>
            <div className="fade" onClick={didModalClosed}>
                <div className="modal">
                    <div className="header">
                        <h3 className="title">
                            {props.title}
                        </h3>
                        <button onClick={didModalClosed}>Close</button>
                    </div>
                    <div className="content">
                        {props.children}
                    </div>
                    <div className="footer">
                        <button onClick={props.onSubmit}>Submit</button>
                    </div>
                </div>
            </div>
        </>
    );
};
```

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### Задача

В App есть инпут, счетчик и дочерний компонент, который выполняет лишние ререндеры, при обновлении счетчика и изменении инпута как избежать лишних ререндеров дочернего

```js
import React,{ useState } from 'react';

export default function App() {
    const [input, setInput] = useState('');
    const [count, setCount] = useState(0);

    const increment = () => setCount(count + 1);

    return (
        <div>
            <input value={input} onChange={(e) => setInput (e.target.value)} />
            <p>Input text: {input}</p>

            <button onClick={increment}>Increment</button>
            <p>Count: {count}</p>

            <ChildComponent count={count} />
        </div>
    );
}

const ChildComponent = ({ count, increment }: any) => {
  console.log(":: render");
  return <div>Child component {count}</div>;
};
```


<details>
  <summary>Решение</summary>

```js
const ChildComponent = memo(({ count }: any) => {
    console.log(":: render");
    return (
        <>
          <div>Child component {count}</div>
        </>
    );
});
```
</details>


 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### Задача

В App есть инпут, счетчик и дочерний компонент, который выполняет лишние ререндеры, при обновлении счетчика и изменении инпута
Также есть функция increment, которая вызывается в App и передается для вызова в дочернем как избежать лишних ререндеров дочернего

```js
import React, { useState } from "react";
import ReactDOM, { createPortal } from "react-dom";



const App = () => {
  const [input, setInput] = useState("");
  const [count, setCount] = useState(0);

  const increment = () => {
	  setCount(count + 1);
  }

  return (
    <div>
      <input value={input} onChange={(e) => setInput(e.target.value)} />
      <p>Input text: {input}</p>

      <button
        onClick={increment}
      >
        Increment
      </button>
      <p>Count: {count}</p>

      <ChildComponent count={count} increment={increment} />
    </div>
  );
};

const ChildComponent = ({ count, increment }) => {
  console.log(":: render");
  return (
    <>
      <div>Child component {count}</div>
      <button onClick={increment}>Increment</button>
    </>
  );
};
```

<details>
    <summary>Решение</summary>

```js
import React, { useState, useEffect, useCallback, FC, memo } from "react";
import ReactDOM from "react-dom";

const App = () => {
  const [input, setInput] = useState("");
  const [count, setCount] = useState(0);

  const increment = useCallback(() => setCount(() => count + 1), []);

  return (
    <div>
      <input value={input} onChange={(e) => setInput(e.target.value)} />
      <p>Input text: {input}</p>

      <button onClick={increment}>Increment</button>
      <p>Count: {count}</p>

      <ChildComponent count={count} increment={increment} />
    </div>
  );
};

const ChildComponent = memo(({ count, increment }) => {
  return (
    <>
      <div>Child component {count}</div>
      <button onClick={increment}>Increment</button>
    </>
  );
});
```
</details>

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### Задача
https://codesandbox.io/p/devbox/trusting-edison-forked-tmkyk4?file=%2Fsrc%2FApp.tsx%3A53%2C1

1) Типизировать данные из файла data/data.json
2) Написать функцию, которая вернёт промис, который зарезолвится через 2 секунды с данными из data/data.json
3) Вывести на интерфейс названия полученных элементов в виде дерева (у каждого следующего уровня должен быть бОльший отступ слева)
4) Реализовать поиск по дереву (если дочерний элемент удовлетворяет условиям поиска, то должен отобразиться он и все его родители)
5) Добавить в поиск функцию debounce
 ```js
Название один
Второе название
    Винни-Пух
    Крокодил Гена
        Я уже устал придумывать названия
        Последнее название
    Чат GPT
Название третьего объекта
Объектов уже четыре
    Выходной
    Раскалённая лава
Пяточок
```

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### Задача

1) Написать компонент, который выводит дату каждую секунду
2) Сделать задание с помощью setInterval и с помощью setTimeout
```js
import React, { useState, useEffect } from 'react';

const Clock = () => {
  return (
    <div>
      <p>Current time: {Здесь должна быть дата}</p>
    </div>
  );
};

export default Clock;
```

<details>
    <summary>Решение</summary>

```js
// setInterval
import React, { useState, useEffect } from 'react';

const Clock = () => {
  const [date, setDate] = useState(new Date());

  useEffect(() => {
    const interval = setInterval(() => {
      setDate(new Date());
    }, 1000);

    return () => {
      clearInterval(interval);
    };
  }, []);

  return (
    <div>
      <p>Current time: {date.toLocaleTimeString()}</p>
    </div>
  );
};

export default Clock;

// setTimeout

import { useEffect, useState } from "react";

const Clock = () => {
  const [time, setTime] = useState<string | null>(null);

  useEffect(() => {
    let timeout = null;
    const update = () => {
      const currentTime = new Date().toLocaleTimeString();
      setTime(currentTime);
      timeout = setTimeout(update, 1000);
    };

    update();

    return () => {
      clearTimeout(timeout);
    };
  }, []);

  return (
    <div>
      <p>Current time: {time}</p>
    </div>
  );
};

export default Clock;
```
</details>

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### Задача
В файле 2 задачи: получение списка и работа с таймером. Нужно провести ревью кода и пофиксить все проблемы

```ts
/* eslint-disable */
import React, { useState, useEffect } from "react";
import ReactDOM from "react-dom";

import "./styles.css";

interface Post {
  id: number;
  userId: number;
  title: string;
  body: string;
}

interface Comment {
  id: number;
  postId: string;
  name: string;
  email: string;
  body: string;
}

// @ts-ignore
const LargeList = ({ commentPrefix }) => {
  const [posts, setPosts] = useState<Post[]>([]);
  const [comments, setComments] = useState<Comment[]>([]);

  /* fetch data */
  // @ts-ignore
  useEffect(async () => {
    const postsResponse = await fetch(
      "https://jsonplaceholder.typicode.com/posts"
    );
    const commentsResponce = await fetch(
      "https://jsonplaceholder.typicode.com/comments"
    );

    const posts = await postsResponse.json();
    const commentsJson = await commentsResponce.json();
    setPosts(posts);
    setComments(commentsJson);
  }, []);

  /* get all comments by postId */
  const findRelatedComments = (postId: number) => {
    const newComments = [];
    for (let i = 0; i < comments.length; i++) {
      if (comments[i].id === postId) {
        newComments.push(comments[i]);
      }
    }
    return newComments;
  };

  return (
    <div>
      {/* render posts */}
      {posts.map(({ title, body, id }) => (
        <div className="post">
          <h1>{title}</h1>
          <p>{body}</p>
          <hr />
          {/* render comments list */}
          <ul>
            {findRelatedComments(id).map((comment) => (
              <div>
                {commentPrefix} {comment.body}
              </div>
            ))}
          </ul>
        </div>
      ))}
    </div>
  );
};

let isLoading = true;

const AppWithCounter = () => {
  const [counter, setCounter] = useState(0);
  const commentPrefix = "*";

  const increase = () => {
    setCounter((prev) => prev + 1);
  };

  useEffect(() => {
    if (isLoading) {
      setInterval(increase, 1000);
    }
    isLoading = false;
  });

  return (
    <div>
      Прошло секунд: {counter}
      <LargeList commentPrefix={commentPrefix} />
    </div>
  );
};

// don't change
ReactDOM.render(<AppWithCounter />, document.getElementById("root"));

setTimeout(() => console.clear(), 1000);

```

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### Задача

Задача: Сделать конвертер BTC в другие валюты.
Список валют для конвертации и курс можно получить из ручки: https://blockchain.info/ticker
1. Актуальный курс должен обновляться каждую минуту.
2. В input можно вводить только цифры.
3. При вводе значения в input BTC должно пересчитываться значение выбранной валюты.

```js
import { useEffect, useState, useMemo } from "react";
import "./styles.css";

export default function App() {
  return (
    <div className="App">
      <h1>Конвертер BTC</h1>

      <div className="form">
        <div className="currency">
          <div>BTC</div>
          <input type="text" />
        </div>

        <div className="currency">
          <select>
            <option>--Выберите валюту--</option>
          </select>
          <input type="text" disabled placeholder="Выберите валюту" />
        </div>
      </div>
    </div>
  );
}

```

<details>
    <summary>Решение</summary>

```ts
// https://codesandbox.io/p/sandbox/interesting-fermat-wqkvqs

import React, { useEffect, useState, useMemo, useCallback } from "react";


type CurrencyCode = string;

type Currency = {
  "15m": number;
  last: number;
  buy: number;
  sell: number;
  symbol: string;
};

type Currencies = Record<CurrencyCode, Currency>;

const fetchCurrencies = async (): Currencies => {
  const response = await fetch("https://blockchain.info/ticker").then((res) =>
    res.json<Currencies>()
  );

  return response;
};

export default function App() {
  const [currency, setCurrency] = useState<CurrencyCode | null>(null);
  const [currenciesObject, setCurrenciesObject] = useState<Currencies | null>(
    null
  );

  const [inputValue, setInputValue] = useState(0);

  const getCurrencies = useCallback(async () => {
    const currenciesData = await fetchCurrencies();
    setCurrenciesObject(currenciesData);
  }, []);

  useEffect(() => {
    getCurrencies();
    const interval = setInterval(() => {
      getCurrencies();
    }, 60000);

    return () => {
      clearInterval(interval);
    };
  }, []);

  const currencies = useMemo(() => {
    if (currenciesObject) {
      return Object.keys(currenciesObject);
    }
    return [];
  }, [currenciesObject]);

  const result = useMemo(() => {
    if (!currency || !currenciesObject) {
      return 0;
    }
    return Number(inputValue) * currenciesObject[currency].buy;
  }, [currency, inputValue, currenciesObject]);

  return (
    <div className="App">
      <h1>Конвертер BTC</h1>

      <ExchangeInput value={inputValue} setValue={setInputValue} />

      <CurrenciesSelect
        result={result}
        currencies={currencies}
        onChange={setCurrency}
      />
    </div>
  );
}

type Props = {
  result: number;
  currencies: CurrencyCode[];
  onChange: (currency: CurrencyCode) => void;
};

const CurrenciesSelect = (props: Props) => {
  const { result, currencies, onChange } = props;

  const handleCurrencyChange = useCallback((event) => {
    onChange(event.target.value);
  }, []);

  return (
    <div className="currency">
      <select onChange={handleCurrencyChange}>
        <option>--Выберите валюту--</option>
        {currencies.map((currency) => (
          <option key={currency} value={currency}>
            {currency}
          </option>
        ))}
      </select>
      <input
        type="text"
        value={result}
        disabled
        placeholder="Выберите валюту"
      />
    </div>
  );
};

const ExchangeInput = ({ value, setValue }: any) => {
  const handleChange = useCallback(
    (event: React.ChangeEvent<HTMLInputElement>) => {
      const inputValue = event.target.value;
      const validatedValue = inputValue.replaceAll(/[^\d]/g, "");

      setValue(validatedValue);
    },
    []
  );

  return (
    <div className="form">
      <div className="currency">
        <div>BTC</div>
        <input type="text" onChange={handleChange} value={value} />
      </div>
    </div>
  );
};
```
</details>

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### Задача
1) При клике на кнопку Increment счетчик должен увеличиваться на величину шага
2) При клике на кнопку Decrement счетчик должен уменьшаться на величину шага
3) Кнопки не должны перерендериваться

```js
import { useCallback, useState, useRef, useEffect, ButtonHTMLAttributes } from "react";

const Button = (props: ButtonHTMLAttributes<HTMLButtonElement>) => {
  console.log("button render", props.children);
  return <button {...props} />;
};


export const App = () => {
  const [step, setStep] = useState(1);
  const [counterValue, setCounterValue] = useState(0);

  return (
    <div className="App">
      <div>Counter value {counterValue}</div>
      <span>Step</span>
      <input
        value={step}
        type="range"
        min="1"
        max="10"
      />
      <span>{step}</span>
      <br />
      <Button>Increment</Button>
      <Button>Decrement</Button>
    </div>
  );
};

```
<details>
    <summary>Решение</summary>

```js
import "./styles.css";
import { useCallback, useState, useRef, useEffect, memo, ButtonHTMLAttributes } from "react";

export const Button = memo((props: ButtonHTMLAttributes<HTMLButtonElement>) => {
  console.log("button render", props.children);
  return <button {...props} />;
});


const initialStep = 1;

export const App = () => {
  const [step, setStep] = useState(initialStep);
  const stepRef = useRef<number>(step);

  const [counterValue, setCounterValue] = useState(0);

  const handleIncrement = useCallback(() => {
    setCounterValue((counter) => counter + stepRef.current);
  }, []);

  const handleDecrement = useCallback(() => {
    setCounterValue((counter) => counter - stepRef.current);
  }, []);

  const handleRangeChange = useCallback(
    (event: React.ChangeEvent<HTMLInputElement>) => {
      setStep(Number(event.target.value));
    },
    []
  );

  useEffect(() => {
    stepRef.current = step;
  }, [step]);

  return (
    <div className="App">
      <div>Counter value {counterValue}</div>
      <span>Step</span>
      <input
        value={step}
        onChange={handleRangeChange}
        type="range"
        min="1"
        max="10"
      />
      <span>{step}</span>
      <br />
      <Button onClick={handleIncrement}>Increment</Button>
      <Button onClick={handleDecrement}>Decrement</Button>
    </div>
  );
};

```
</details>

___
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### Задача
В задании приведен эмулятор вызова запроса с бекенда.
При нажатии на fetch success выводится "Request success" и "Success from App"
При нажатии на fetch failt выводится "Request fail" "Success from App"

1. Исправить баг показа лоадера. Сейчас он показвыается постоянно, а нужно только при получении запроса
2. Сделать так, чтоб при нажатии на success было 2 success'а, а при нажатии на fail было 2 fail'a

```js
import { useState, useCallback } from "react";

export const App = () => {
  const { fetch, loading } = useGetSomething();

  const succesFetch = useCallback(() => {
    fetch(false)
      .then(() => {
        alert("Success from App");
      })
      .catch((e) => {
        alert("Fail from App");
      });
  }, [fetch]);

  const failFetch = useCallback(() => {
    fetch(true)
      .then(() => {
        alert("Success from App");
      })
      .catch((e) => {
        alert("Fail from App");
      });
  }, [fetch]);

  return (
    <div className="App">
      <button onClick={succesFetch}>Success fetch</button>
      <button onClick={failFetch}>Fail fetch</button>
      {loading && <div>Loading...</div>}
    </div>
  );
};

export const useGetSomething = () => {
  const [loading, setLoading] = useState(false);

  const fetch = useCallback(async (fail: boolean) => {
    try {
      await new Promise((resolve) => setTimeout(resolve, 1000));
      if (fail) {
        throw new Error("");
      }

      alert("Request success");
    } catch (e) {
      alert("Request fail");
    }
  }, []);

  return {
    fetch,
    loading,
  };
};
```

<details>
    <summary>Решение</summary>
    1. Лоадер - нужно ставить isLoading = true в начале загрузки и false в конце, в finally, вне зависимости от результата выполнения промиса.
    2. Для показа fail в обоих случаях нужно правильно обработать ошибку - сейчас она попадает в catch и после этого обрабаывается как успех.
    Нужно из catch также вернуть ошибку

```js
import { useState, useCallback } from "react";

export const App = () => {
  const { fetch, loading } = useGetSomething();

  const succesFetch = useCallback(() => {
    fetch(false)
      .then(() => {
        alert("Success from App");
      })
      .catch((e) => {
        alert("Fail from App");
      });
  }, [fetch]);

  const failFetch = useCallback(() => {
    fetch(true)
      .then(() => {
        alert("Success from App");
      })
      .catch((e) => {
        alert("Fail from App");
      });
  }, [fetch]);

  return (
    <div className="App">
      <button onClick={succesFetch}>Success fetch</button>
      <button onClick={failFetch}>Fail fetch</button>
      {loading && <div>Loading...</div>}
    </div>
  );
};

export const useGetSomething = () => {
  const [loading, setLoading] = useState(false);

  const fetch = useCallback(async (fail: boolean) => {
    setLoading(true);
    try {
      await new Promise((resolve) => setTimeout(resolve, 1000));
      if (fail) {
        throw new Error("");
      }

      alert("Request success");
    } catch (e) {
      alert("Request fail");
      throw e;
    } finally {
      setLoading(false);
    }
  }, []);

  return {
    fetch,
    loading,
  };
};

```
</details>

___
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### Задача
Есть обычная модалка с react-modal - принимает в себя children и решерит контен
Имея эту реализацию нужно реализовать ModalWithRender применяя механиз renderProps

```js

import { useState, PropsWithChildren } from "react";
import Modal from "react-modal";

// Это пример обычного использования модалки, править тут не надо
export const ModalUsual = () => {
  const [show, setShow] = useState(false);
  return (
    <>
      <button onClick={() => setShow(true)}>Open modal usual</button>
      <Modal isOpen={show} onRequestClose={() => setShow(false)}>
        Modal content here
      </Modal>
    </>
  );
};

// Тут нужно реализовать компонент
export const ModalWithRender = () => {
  const [show, setShow] = useState(false);
  return null;
};

export const App = () => {
  return (
    <div className="App">
      <ModalUsual />

      {/* Должна отрисоваться кнопка, которая откроет модалку с переденным контентом */}
      <ModalWithRender content="To book a hostel call 88007009988">
        {({ onOpen }) => {
          return <button onClick={onOpen}>Click button to open modal</button>;
        }}
      </ModalWithRender>

      {/* Должна отрисоваться ссылка, которая откроет модалку с переденным контентом */}
      <ModalWithRender content="Information about frontend technology">
        {({ onOpen }) => {
          return <a onClick={onOpen}>Click anchor to open modal</a>;
        }}
      </ModalWithRender>
    </div>
  );
};

```


<details>
<summary>Решение</summary>

```ts
import { useState, PropsWithChildren } from "react";
import Modal from "react-modal";

type RenderProps = {
  onOpen: () => void;
};

type Props = {
  content?: string;
  children: (props: RenderProps) => React.ReactNode;
};

export const ModalWithRender = (props: Props) => {
  const { children, content } = props;

  const [show, setShow] = useState(false);

  const renderProps: RenderProps = {
    onOpen: () => setShow(!show),
  };

  return (
    <>
      {children(renderProps)}
      <Modal isOpen={show} onRequestClose={() => setShow(false)}>
        {content}
      </Modal>
    </>
  );
};

export const App = () => {
  return (
    <div className="App">
      {/* Должна отрисоваться кнопка, которая откроет модалку с переденным контентом */}
      <ModalWithRender content="To book a hostel call 88007009988">
        {({ onOpen }) => {
          return <button onClick={onOpen}>Click button to open modal</button>;
        }}
      </ModalWithRender>

      {/* Должна отрисоваться ссылка, которая откроет модалку с переденным контентом */}
      <ModalWithRender content="Information about frontend technology">
        {({ onOpen }) => {
          return <a onClick={onOpen}>Click anchor to open modal</a>;
        }}
      </ModalWithRender>
    </div>
  );
};
```
</details>

___
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

