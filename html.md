# Задачи для практики с собеседованиям на frontend-разработчика - Верстка

## Верстка

### Задача

Какой фон будет у div'ов

```html
<style>
  .block {
    display: block;
    width: 50px;
    height: 50px;
  }
  .red {
    background: red;
  }
  .blue {
    background: blue;
  }
</style>

<div class="block red blue">
</div>
<div class="block blue red">
</div>
```


<details>
  <summary>Объяснение</summary>

  Важно помнить, что важно не то, как селектора прописаны в HTML, а то, какая очередность их объявления в CSS.
  Так как red и blue - это классы, то их вес равен 100. Если веса селекторов равны, то применяется тот, что находится ниже.

  Итого, оба блока будут синего цвета

</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### Задача

К чему применятся стили каждого из селекторов?

```css
.some div > p {}

.some_div__p {}
```


<details>
  <summary>Объяснение</summary>

**  Селектор 1:**
  - `.some` - селектор по классу
  - `div` - селектор по тегу
  - `>` - обращение к вложенным элементам первого уровня
  - `p` - селектор по тегу

Итого: обращение ко всем тегам p, которые лежат на первом уровне вложенности тега div, который в свою очередь нахзодится в элементе с классов .some

```html
<div class="some">
  // любое количество вложенности HTML
  <div>
   ** <p>Здесь стиль будет применен</p>**

    <div>
      <div>
       ** <p>Здесь стиль НЕ будет применен</p>**
      </div>
    </div>
  </div>
</div>
```

**Селектор 2:**
- `.some_div__p` - селектор по классу, в названии которого присутствует _. Обращения по тегам здесь нет. Следовательно применен стиль будет к любому элементу, у которого есть класс `some_div__p`

</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->