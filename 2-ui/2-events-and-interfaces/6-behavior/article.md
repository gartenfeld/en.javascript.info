# Приём проектирования "поведение"

Шаблон проектирования "поведение" (behavior) позволяет задавать хитрые обработчики на элементе *декларативно*, установкой специальных HTML-атрибутов и классов.
[cut]

Например, хочется, чтобы при клике на один элемент показывался другой. Конечно, можно поставить обработчик в атрибуте `onclick` или даже описать его где-нибудь в JS-коде страницы.

Но есть решение другое, и очень изящное.

## Описание 

Приём проектирования "поведение" состоит из двух частей:
<ol>
<li>Элементу ставится атрибут, описывающий его поведение.</li>
<li>При помощи делегирования ставится обработчик на документ, который ловит все клики и, если элемент имеет нужный атрибут, производит нужное действие.
</ol>

## Пример

Например, я хочу, чтобы при клике на один элемент скрывался/показывался другой.

Конечно, можно написать соответствующий обработчик в JavaScript.

А что, если подобная задача возникает часто? 

Хотелось бы получить более простой способ задания такого *поведения*, например -- *декларативно*, при помощи особого атрибута.

**Сделаем так, что при клике на элемент с атрибутом `data-toggle-id` будет скрываться/показываться элемент с заданным `id`.**

```html
<!--+ autorun run height=100 -->
<button *!*data-toggle-id="subscribe-mail"*/!*>
  Показать форму подписки
</button>
 
<form id="subscribe-mail" style="display:none">
  Ваша почта: <input type="email">
</form>

<script>
*!*
  document.onclick = function(event) {
    var target = event.target;

    var id = target.getAttribute('data-toggle-id');
    if (!id) return;

    var elem = document.getElementById(id);

    elem.style.display = (elem.style.display == 'none') ? "" : "none";
  };
*/!*
</script>
```

**При помощи JavaScript мы добавили "поведение" -- возможность через атрибут указывать, что делает элемент.**

[smart header="Не только атрибут"]
Для своих целей мы можем использовать в HTML любые атрибуты, но стандарт рекомендует для своих целей называть атрибуты `data-*`.

В обработчике `document.onclick` мы могли бы проверять не атрибут, а класс или что-то ещё.
[/smart]

Обратите внимание: обработчик поставлен на `document`, то есть клик на любом элементе страницы пройдёт через него. Теперь для того, чтобы добавить скрытие-раскрытие любому элементу -- даже не надо знать JavaScript, можно просто написать атрибут.

Также для добавления обработчиков на `document` рекомендуется использовать `addEventListener`, чтобы можно было добавлять несколько различных поведений на документ.

## Итого

Шаблон "поведение" удобен тем, что сколь угодно сложное JavaScript-поведение можно "навесить" на элемент одним лишь атрибутом. А можно -- несколькими атрибутами на связанных элементах.

Здесь мы рассмотрели базовый пример, который можно как угодно модифицировать и масштабировать. Важно не переусердствовать.

**Приём разработки "поведение" рекомендуется использовать для расширения возможностей разметки, как альтернативу мини-фрагментам JavaScript.**

Далее у нас ещё будут задачи, где мы реализуем этот приём разработки.