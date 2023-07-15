# Разметка\_текста

## Разметка текста

## Абзацы&#x20;

Начнём с простейшего тега `<p>`, с помощью которого создаются абзацы. По умолчанию абзацы начинаются с новой строки и имеют вертикальные отступы, которыми можно управлять с помощью стилей.



## Заголовки и подзаголовки

В языке HTML для выделения заголовков предусмотрено целое семейство тегов: от `<h1>` до `<h6>`. Тег `<h1>` обозначает самый важный заголовок (заголовок верхнего уровня), а тег `<h6>`обозначает подзаголовок самого нижнего уровня.



## Неупорядоченный список

Неупорядоченные (или маркированные) списки создаются с помощью тега `<ul>`, который может содержать внутри себя теги `<li>`, обозначающие «элемент списка».

## Упорядоченный список

&#x20;    Упорядоченный список создаётся с помощью тега `<ol>`, который может содержать внутри себя теги `<li>`.

&#x20;    Для упорядоченного списка можно задать атрибут `start`, который изменяет начало нумерации. Например, код:

```
<ol start="3">
  <li>раз</li>
  <li>два</li>
</ol>
```

Многоуровневый список

Сначала нужно создать список первого уровня, а затем внутрь любого элемента этого списка, между тегами `<li>` и `</li>`, добавить список второго уровня. При этом необходимо аккуратно закрывать все теги.

Пример правильного кода:

```
<ul>
  <li>1
    <ul>
      <li>1.1</li>
      <li>1.2</li>
    </ul>
  </li>
  <li>2</li>
</ul>
```

&#x20;Пример кода с ошибкой:

```
<ul>
  <li>1</li>
  <ul>
    <li>1.1</li>
    <li>1.2</li>
  </ul>
  <li>2</li>
</ul>
```

В примере с ошибкой вложенный список вставлен не внутрь элемента списка, а между элементами, что недопустимо.

Количество уровней в списках не ограничено. В многоуровневом списке можно использовать как упорядоченные, так и неупорядоченные списки.



Список определений

Список определений создаётся с помощью трёх тегов:

1. `<dl>` обозначает сам список определений;
2. `<dt>` обозначает термин;
3. `<dd>` обозначает определение термина.

Теги `<dt>` и `<dd>` пишутся парами внутри `<dl>`.

Например:

```
<dl>
  <dt>Термин</dt>
  <dd>Определение</dd>

  <dt>Второй термин</dt>
  <dd>И его определение</dd>

  <dt>Кошка</dt>
  <dd>Шерстяное изделие развлекательного характера</dd>
</dl>
```



Важность. Теги strong и b&#x20;

**Логическая** разметка текста, поэтому уделяется особое внимание смыслу элементов, их предназначению, а не визуальному форматированию.

Тег `<strong>` определяет **важность** отмеченного текста.

Тег `<b>` предназначен для выделения текста без придания ему особой важности.

Визуально оба тега одинаковы, они выделяют текст полужирным.



**Акцентируем внимание. Теги em и i**

Тег `<em>` определяет текст, на который сделан _особый акцент_, меняющий смысл предложения.

Например, если мы хотим подчеркнуть, что Кекс не любит _питаться_ укропом (он больше за тунца), а любит только _гонять его по полу_, то разметим текст так:

```
Инструктор Кекс любит <em>играть</em> с укропом.
```

Тег `<i>` обозначает текст, который отличается от окружающего текста, но не является более важным. Обычно так выделяют _названия_, _термины_, _иностранные слова_.

Например, если мы хотим указать, что _инспектор_ — это какой-то специальный термин, то разметим текст так:

```
Обычно Кекс пользовался <i>инспектором</i> браузера для поиска ошибок.
```

Визуально оба тега одинаковы, они выделяют текст курсивом.



Переносы и разделители. Теги br и hr

Иногда возникает необходимость вставить в текст перенос строки, не создавая при этом абзац. Например, при разметке стихов или текстов песен.

Для этого в HTML предусмотрен одиночный тег `<br>`.

Иногда этот тег используется для разбиения текста на «как бы абзацы», что является плохим подходом. Используйте для разметки абзацев тег `<p>`.

Одиночный тег `<hr>` используется для того, чтобы создать горизонтальную линию-разделитель. На внешний вид этой линии можно влиять с помощью атрибутов, но правильней делать это с помощью CSS.



Цитаты

В HTML существует несколько тегов для обозначения цитат:

* `<blockquote>` предназначен для выделения длинных цитат, которые могут состоять из нескольких абзацев. Тег выделяет цитату как отдельный блок текста с отступами.
* `<q>` предназначен для выделения коротких цитат в предложениях. Текст внутри этого тега автоматически обрамляется кавычками.
* `<cite>` используется для того, чтобы выделить источник цитаты, название произведения или автора цитаты.



Верхние и нижние индексы

Следующие два тега обычно используются не для выделения слов, а для выделения отдельных символов. Их используют для указания единиц измерения или для написания простых формул.

Например: 20м2, H2O, X3+X2=1

Тег `<sup>` отображает текст в виде верхнего индекса.

Тег `<sub>` отображает текст в виде нижнего индекса.

Стоит отметить, что эти теги являются чисто презентационными и не имеют собственной семантики.

Эти теги можно использовать внутри друг друга для создания более сложных формул.

Если вам нужно вставить очень сложную формулу в HTML-документ, лучше воспользоваться специальным языком разметки [MathML](http://ru.wikipedia.org/wiki/MathML).



Помечаем изменения. Теги del и ins

Любой документ на протяжении своей «жизни» может изменяться. С распространением динамических веб-приложений вносить изменения в HTML-документы стало проще простого.

Иногда возникает вопрос: а что же именно было изменено в документе, что было добавлено, а что удалено?

Как раз для описания изменений предназначены теги `<del>` и `<ins>`.

`<del>` выделяет текст, который был удалён в новой версии документа.

`<ins>` выделяет текст, который был добавлен в новой версии документа.

Оба тега имеют атрибут `datetime`, в котором можно указать дату и время, когда была внесена та или иная правка.

Простейшим примером применения этих тегов может служить список ошибок. Когда ошибка исправлена, её помечают тегом `<del>`, если найдена новая ошибка, то её добавляют в список и помечают тегом `<ins>`.

Атрибут `datetime` предназначен не для людей, а для компьютеров, поэтому дату и время там пишут в стандартизованном формате. При такой разметке программам легче разбирать документы и анализировать, когда произошли те или иные изменения.



Преформатированный текст

Наверное, вы уже заметили, как отличается отображение кода в HTML-редакторе и в мини-браузере.

Вы можете ставить сколько угодно пробелов в HTML-коде, но браузер отобразит их как один. Вы также можете ставить сколько угодно переносов строки в HTML-коде, а в браузере переноса не будет, если только не использовать специальные теги, например `<p>` или `<br>`.

Изменить это поведение браузера можно с помощью тега `<pre>`, который обозначает «предварительно отформатированный текст». Браузер сохраняет и отображает все пробелы и переносы, которые есть внутри тега `<pre>`.

Наиболее часто этот тег используется при отображении примеров кода.



Просто выделенный текст

В HTML5 появился новый тег `<mark>`, который обозначает выделенный текст.

Иногда при работе с объёмными текстами мы используем маркер, чтобы выделять ключевые слова, идеи или что-то другое на что стоит обратить внимание. Такое же назначение и у тега `<mark>`.

В современных браузерах текст внутри `<mark>` подсвечивается жёлтым фоном.