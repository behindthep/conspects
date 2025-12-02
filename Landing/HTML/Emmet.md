<section class="cards">
  <article class="card">
    <div class="card-title">Название товара</div>
    <img src="/images/card-item-1.png" alt="Товар-1">
    <div class="card-price">
      <span class="price-sum">24 000</span><span class="price-cur">руб.</span>
    </div>
    <button>Заказать</button>
  </article>
</section>

section.cards>article.card>div.card-title{Название товара}+img[src="/images/card-item-1.png"][alt="Товар-1"]+(.card-price>span.price-sum{24 000}+span.price-cur{руб.})+button{Заказать}

Выделить всю строку и Ctrl + Shift + P. Во всплывающем окне набирать >Emmet: Expand Abbreviation.

section.cards — CSS-селектор. создать элемент <section> с классом cards.
> — как и в CSS, на вложенность. section.cards> все, что указано далее, располагаться внутри секции с классом cards.
article.card — элемент <article> с классом card.
div.card-title — блочный элемент с классом card-title. создать элемент <div>, не обязательно его указывать напрямую. не указан тег перед классом, Emmet создаст элемент <div>. вместо div.card-title указать .card-title.
{Название товара} — в фигурных скобках текст, помещен внутрь блока.
+ — правую часть расположить рядом с левой, а не внутри нее, как в случае с символом >. расположить картинку рядом с блоком card-title, а не внутри.
img[src="/images/card-item-1.png"][alt="Товар-1"] — создаем элемент <img>. В квадратных скобках атрибуты, которые добавить. атрибут src, значением путь до изображения, а второй атрибут alt за мета-информацию о картинке. Атрибуты записывать как по отдельности, так и вместе. img[src="/images/card-item-1.png" alt="Товар-1"] результат тем же.
() — скобки группировать информацию. все, что внутри скобок, располагалось рядом с картинкой, созданной в прошлом пункте.

header>.logo+nav>ul>li*3

<header>
  <div class="logo"></div>
  <nav>
    <ul>
      <li></li>
      <li></li>
      <li></li>
    </ul>
  </nav>
</header>

*3 — умножение. взять элемент и разметить его три раза.

table>tr*3>td*3

<table>
  <tr>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>

header+main+footer

<header></header>
<main></main>
<footer></footer>

div.item-$*3

<div class="item-1"></div>
<div class="item-2"></div>
<div class="item-3"></div>

$ счетчиком. При разворачивании строчки все символы $ заменяются на числа от 1 с шагом единица. доп конструкций, расширить счетчик:
Если использовать несколько идущих подряд символов $, столько разрядов в счетчике. div.item-$$$*3 начнет счетчик с двух нулей. Первым элементом <div class="item-001"></div>.
Счетчик начать с произвольного числа @, после которого идет начальное число. div.item-$@5*3 начнет счетчик с числа пять. Первым элементом <div class="item-5"></div>.

Счетчик внутри конструкции {}:

div.item{Работа номер $}*3

<div class="item">Работа номер 1</div>
<div class="item">Работа номер 2</div>
<div class="item">Работа номер 3</div>
