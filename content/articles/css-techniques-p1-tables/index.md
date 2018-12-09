---
date: "27 июня 2013"
title: "Основные способы вёрстки. Часть первая: таблица"
---

# Основные способы вёрстки. Часть первая: таблица

[Лев Солнцев](http://my.opera.com/GreLI/blog/) 27 июня 2013

Для профессионального оформления сайтов необходимо знать не только основы CSS, но и понимать, как работает браузер, знать правила, которым он следует. Именно они определяют основные способы и приёмы вёрстки.

Только имея такое понимание, можно выбрать наиболее подходящий способ решения задачи из нескольких возможных, с учётом их достоинств и ограничений. Только так можно наиболее полно задействовать возможности браузера и предупредить потенциальные ошибки.

Существует немало описаний различных приёмов. В этой статье предпринята попытка собрать вместе самые важные приёмы и систематизировать их, чтобы дать представление как об основных возможностях, так и об ограничениях CSS, актуальных в настоящее время.

Статья рассчитана на людей, которые знакомы с основами HTML и CSS и имеют представление об основных свойствах и базовых принципах работы каскадных таблиц стилей.

## Таблица

Исторически, первым и единственным способом раскладки страницы были таблицы. Описанию поведения таблиц посвящена [целая глава](http://www.w3.org/TR/CSS2/tables.html) в спецификации CSS 2.1. Несмотря на такой объем, некоторые моменты описаны скудно или вообще не определены и отданы на усмотрение браузеров.

### Достоинства и недостатки

Таблица служит для отображения упорядоченных данных в строках и столбцах, имеющих смысловую связь по горизонтали или вертикали. Отсюда следует главное достоинство: ячейки таблиц выравниваются по сетке, что позволяет простым и очевидным образом создать _модульную сетку_.

Это неотъемлемое свойство таблиц позволяет заполнить плоскость окна браузера и создавать «резиновые сайты». Но, как при малых, так и больших размерах окна просмотра браузера структура таблицы не меняется, она не может гибко адаптироваться под доступное пространство.

При использовании таблицы для раскладки, то есть размещения в сетке данных, не имеющих смысловой связи, нарушается семантичность. Применение таких таблиц ухудшает доступность для людей, использующих специальные программы, и снижает положение в поисковой выдаче, поскольку поисковому движку, предположительно, сложнее разобраться в структуре страницы. Как следствие, сайт работает менее эффективно.

### Особенности

Ячейки таблиц идут в коде строго друг за другом, слева направо или справа налево в зависимости от направления языка, заданного CSS-свойством `direction` или его аналогом в HTML, атрибутом `dir`.

Если, к примеру, требуется, чтобы основное содержимое в центральной колонке шло в начале, перед содержимым других колонок в исходном HTML-коде, таблица — неподходящее решение.

Структура таблицы довольно сложна, она описывается большим количеством тегов, что приводит к усложнению исходного кода. Негативный эффект проявляется ещё больше, когда несколько таблиц вложены друг в друга.

### Имитация

Появившаяся в CSS 2.1 группа свойств `display: table-*` позволяет создать таблицу из произвольных элементов, имеющих соответствующую структуру.

Согласно спецификации, достаточно только одного объявления вроде `display: table` или `display: table-cell` — недостающие элементы должны автоматически достраиваться браузером.

Однако будет надёжнее создать минимальную структуру `таблица > ряд > ячейка`, аналогично обязательным тегам `<table>`, `<tr>`, `<td>`, с соответствующими значениями свойства `display`: `table`, `table-row` и `table-cell`.

В противном случае может возникнуть нерегулярно проявляющаяся ошибка, замеченная в Firefox и браузерах на основе Webkit, когда ряд таблицы без элемента с `display: table-cell` случайным образом разбивается на несколько ячеек. Возможное объяснение может состоять в попадании границы сетевых пакетов среди ячеек при передаче HTML-кода.

Таким образом, блочная разметка с `display: table-*` почти не отличается от обычной HTML-таблицы ни в чем, кроме имён тегов, однако обычная таблица лучше поддерживается браузерами (а именно в Internet Explorer 7 и ниже) и имеет больше возможностей, таких как объединение ячеек.

Стоит отметить, что, несмотря на необязательность тега `<tbody>` в HTML, браузер обязательно создаст этот элемент, если только документ не обрабатывается в режиме XHTML, при отсутствии группирующих элементов `<tbody>`, `<thead>` и `<tfoot>`. Этим можно пользоваться при оформлении, и обязательно следует иметь в виду при использовании родительского селектора, который может иметь запись вида `table > tbody > tr > td`. Селектор `table > tr > td` работать не будет.

Анонимные элементы при `display: table-*`, воссоздающие структуру таблицы согласно CSS 2.1, не влияют на дерево элементов. Им нельзя задать CSS-правила, действуют только наследуемые свойства.

### Семантичность

Существует мнение, что использование `display: table` более семантично, так как используются теги, лучше соответствующие содержимому, и это поможет различным движкам в обработке страницы. Нередко при этом приводятся в пример программы чтения с экрана.

Однако есть [исследования](http://www.456bereastreet.com/archive/201110/using_displaytable_has_semantic_effects_in_some_screen_readers/), которые показывают, что некоторые такие программы учитывают оформление страницы и воспринимают элементы с `display: table` точно так же, как и обычную таблицу, размеченную стандартными тегами. Тем не менее такой способ имеет право на существование, когда использование тегов таблицы неуместно.

Рекомендуется использовать методы [WCAG](http://www.w3.org/TR/WCAG-TECHS/html.html) для семантичного оформления таблиц, задействуя дополнительные возможности разметки, такие как краткое описание с помощью тега `<caption>` или атрибута `summary` и указание области действия заголовков `<th>` атрибутом `scope`.

### Ширина

Ширина таблицы, будучи не задана, рассчитывается браузером с учётом содержимого. Если ширина таблицы меньше ширины родительского элемента, таблица может быть отцентрирована с помощью свойства `margin: auto` или выровнена по левому или правому краю.

При `table-layout: auto` (значение по умолчанию) ширина таблицы расчитывается так, чтобы поместилось всё её содержимое. Если ширина элемента, в котором находится таблица, недостаточна, она выйдет за его пределы.

Это может привести к появлению горизонтальной прокрутки на странице даже тогда, когда казалось бы достаточно места, из-за слишком длинного слова (например, какой-то интернет-адрес) или широкого изображения.

В CSS 2.1 не определено действие свойств `min-width`, `max-width`, `min-height` и `max-height` на элементы таблицы.

Так как ячейка не может иметь ширину меньшую, чем позволяет её содержимое, вместо `min-width` можно использовать достаточно широкую «распорку». Для этого можно использовать пустой блок нужной ширины.

С оговоркой, о которой будет сказано далее, ограничить ширину ячейки может задание свойства `width`. Этот способ работает в современных браузерах, включая Internet Explorer 8 и даже в старших версиях Internet Explorer в режиме Internet Explorer 7, но не в настоящем браузере Internet Explorer 7. Это один из тех случаев, когда поведение в режиме эмуляции отличается от настоящей версии браузера.

Ширина ячейки может не соответствовать предписанному значению в том случае, когда заданная для всей таблицы ширина не равна сумме заданных ширин всех ячеек, или их сумма не равна 100%. Тогда доступное место распределяется среди ячеек пропорционально значениям их ширины.

В соответствии со стандартной блочной моделью CSS ширина ячейки задаётся по области содержимого, исключая толщину рамки и величину отступа _(padding)_. Однако, если ширина колонки таблицы устанавливается через элемент `<col>` или `<colgroup>`, то она задаётся уже с учётом ширины рамок и отступа ячеек.

### Гибкость

Одним из преимуществ является то, что таблица позволяет гибко управлять соотношением ширин колонок через процентные значения. Ширина ячеек при этом учитывает размер содержимого.

Примечательным свойством таблицы среди возможностей CSS 2.1 является возможность установить такую ширину ячейке, которая осталась ей доступна от остальных ячеек в ряду.

### Высота

По спецификации свойство `height` задаёт только минимальную высоту ячейки. Если с шириной ячеек браузеры следуют модели CSS, то установку высоты ячейки разные браузеры трактуют по-разному.

Firefox до 15-й версии и Opera до 12-й включительно считают высоту вместе с отступом и рамкой, другими словами, ведут себя как при `box-sizing: border-box`, что соответствует поведению в режиме обратной совместимости _(Quirks Mode)_.

Internet Explorer версии 8 и выше и браузеры на основе WebKit задают высоту только для содержимого аналогично `box-sizing: content-box`, что является правильным поведением с точки зрения модели CSS.

На текущий момент свойство `box-sizing` влияет на задание высоты ячейки только в браузере Internet Explorer. А Firefox до 16 версии не учитывал `box-sizing` даже для ширины ячеек таблиц.

### Раскладка

#### Жёсткая

При `table-layout: fixed` ширина ячеек таблицы задаётся непосредственно, либо равномерно распределяется среди колонок. Таблица теряет возможность автоматического расчёта ширины с учётом заполненности ячеек. Переполнение ячеек обрабатывается в соответствии со значением свойства `overflow`. Спецификация оставляет браузерам возможность всегда использовать этот режим. Его могут применять мобильные браузеры, ограниченные в ресурсах для сложной обработки таблиц.

#### Автоматическая

При `table-layout: auto` содержимое не может выйти за границы ячейки, и свойство `overflow` не имеет действия. При этом нельзя заставить таблицу всегда оставаться в заданных рамках. Из-за автоматического расчёта ширины, она не уменьшится, даже если есть `overflow: hidden` у элемента с неопределенной шириной внутри ячейки.

<figure>
	<img src="images/logo-n-menu.svg" alt="">
	<figcaption>Дизайнер может желать, чтобы логотип и меню всегда умещались в одной строчке. Таблица не может обрезать длинные надписи для этой цели.</figcaption>
</figure>

### Быстродействие

Cвойство `table-layout: fixed` предназначено для того чтобы отображать большие таблицы по мере загрузки данных — так происходит, потому что размеры ячеек указаны заранее.

Если установлено `table-layout: auto`, браузер должен рассчитывать размеры таблицы, рекурсивно обрабатывая всё её содержимое, что не может не сказаться на быстродействии. При этом в большинстве браузеров таблица будет показана лишь после того, как загрузится полностью.

### Выравнивание

Таблица позволяет выравнивать содержимое своих ячеек как по горизонтали (`text-align`), так и по вертикали при помощи `vertical-align`.

Несмотря на то, что выравнивание по вертикали как строчных элементов, так и содержимого ячеек таблиц задаётся одним и тем же свойством, его действие отличается в каждом случае.

Для строчных элементов оно действует на сами элементы и имеет больше вариантов, тогда как в случае таблиц выравнивается содержимое ячеек, включая блочные элементы.

Возможные значения свойства `vertical-align` для ячеек таблиц: `top`, `bottom`, `middle` и `baseline` — выравнивание по верхней границе, нижней границе, середине и базовой линии, соответственно. Выравнивание по базовой линии производится по первой строчке текста, а если таковой нет, то по самой нижней границе блока в ячейке.

Последний вариант уникален: таблица позволяет выровнять строки текста по базовой линии в разных колонках даже при различных свойствах шрифта.

Другими методами в рамках CSS 2.1 такого эффекта добиться практически невозможно из-за погрешностей округления значений и особенностей отрисовки текста операционными системами и браузерами. В лучшем случае можно только подогнать параметры под наиболее вероятную ситуацию: вроде распространённой гарнитуры и стандартного размера шрифта.

<figure>
	<img src="images/table-form.svg" alt="">
	<figcaption>Таблица позволяет легко и надёжно выровнять заголовки и элементы форм по базовой линии.</figcaption>
</figure>

### Проблемы позиционирования

В старых версиях WebKit и текущих версиях Mozilla Firefox нельзя позиционировать элементы таблицы, и потому не получится позиционировать внутренние элементы относительно ячейки. Хотя спецификация не определяет действие свойства `position` на элементы таблиц, имеется открытое [сообщение об ошибке](https://bugzilla.mozilla.org/show_bug.cgi?id=35168) браузера Firefox.

Из-за этой особенности нет работающего во всех браузерах способа разместить что-либо сверху и снизу ячейки одновременно. Иногда проблему можно обойти, разбив ряд таблицы на два, но такое решение ведёт к усложнению кода, а полученная сетка может уже скорее мешать, чем помогать.

В соответствии с принципами CSS установить высоту для элементов внутри таблицы, зависящую от высоты ячейки, можно только в том случае, когда самой ячейке задана высота. Также из-за этого не получится расположить элементы одновременно сверху и снизу с помощью позиционирования относительно внутреннего элемента. Ведь если высота не известна точно заранее, никогда нет гарантии, что соседняя ячейка, а значит и весь ряд, не имеет большую высоту.

### Заключение

В будущем к нам придут на помощь новые модули CSS, но до окончательной разработки и полноценного внедрения, когда ими можно будет реально пользоваться, пройдёт ещё как минимум несколько лет.

Один из таких модулей — [CSS Grid Layout](http://www.w3.org/TR/css3-grid-layout/) — не только позволяет создать модульную сетку, аналогичную таблице, но и даёт большие возможности по оформлению и использованию. При совместном использовании с [медиавыражениями](http://www.w3.org/TR/css3-mediaqueries/), модуль позволяет произвольно адаптировать раскладку под размеры окна одними лишь средствами CSS. Он избавит нас в будущем от необходимости использовать таблицу для раскладки и оставить её, наконец, для своего основного предназначения: разметки табличных данных.

О других приёмах и возможностях CSS 2.1 без использования таблиц читайте во второй части статьи «[Бестабличная вёрстка](/articles/css-techniques-p2-alternate/)».