# modal-window
https://dmitriy-1986.github.io/modal-window/.

План работы скрипта:
Зарегистрировать событие клика на элементы с классом js-open-modal
При клике на картинку, ищем модальное окно с таким же атрибутом data-modal, добавляем класс .active подложке и этому модальному окну
При клике на крестик удаляем класс у родительского модального окна и подложки

Пишем JavaScript
В начале, повесим на document событие DOMContentLoaded. Это событие сработает когда страница будет загружена.
document.addEventListener('DOMContentLoaded', function() {
});
Затем запишем массив кнопок в переменную используя метод querySelector. Здесь же определим еще 2 переменные: элемент подложки и массив кнопок-крестиков.
document.addEventListener('DOMContentLoaded', function() {
   var modalButtons = document.querySelectorAll('.js-open-modal'),
       overlay      = document.querySelector('#overlay-modal'),
       closeButtons = document.querySelectorAll('.js-modal-close');
});
Заметьте, modalButtons и CloseButtons мы получаем через querySelectorAll, который возвращает массив, мы сделали это потому что нужно обрабатывать клики по всем кнопка, а вот overlay мы получаем через querySelector, он возвращает один элемент.
В html добавим кнопкам классы .js-open-modal. Мы специально будем использовать новый класс с приставкой js, чтобы не путать стили и интерактивность. Все кто будет работать с кодом увидит, что у класса есть приставка js, значит этот класс используется для интерактивности и его лучше не трогать.
<!-- Элементы для вызова модальных окон, могут быть любые -->
<a href="#" class="js-open-modal">Открыть окно </a>
После этого нужно повесить событие клика на каждую кнопку. Для этого мы переберем полученный массив кнопок и повесим обработчик на каждый элемент. Перебирать массив мы будем с помощью forEach:
/* Перебираем массив кнопок */
modalButtons.forEach(function(item){
});
В переменной item у нас будет храниться текущий элемент цикла. Повесим обработчик на него:
/* Перебираем массив кнопок */
modalButtons.forEach(function(item){
    /* Назначаем каждой кнопке обработчик клика */
    item.addEventListener('click', function(event) {
     });
});
event или e — объект текущего события. В этом объекте хранятся различные методы и данные. При вызове любого события указание аргумента у функции будет ссылаться на этот объект. Зачем нам нужен этот объект? Об этом чуть ниже.
Что нам нужно сделать теперь?
Для начала нужно предусмотреть то, что кнопка, которая вызывает наше модальное окно, может быть ссылкой, кнопкой в форме или другим элементом, который стандартно выполняет какие-то интерактивные действия. Нам нужно отменить эти действия для нормальной работы нашего кода.
Для этого в объекте события есть метод, который предотвращает стандартное действие элемента.
event.preventDefault();
С предотвращением вопрос решили.
У каждой кнопки есть атрибут data-modal, в нем хранится значение, которое находится у модального окна в таком же атрибуте. Наши действия:
Получить значение атрибута текущей кнопки
Найти модальное окно с помощью этого значения
var modalId = this.getAttribute('data-modal'),
    modalElem = document.querySelector('.modal[data-modal="' + modalId + '"]');
Для того чтобы найти модальное окно, мы пишем определенный селектор, а там где нужно вставить значение, делаем разрыв строки и вставляем значение переменной, соединяя строки используя оператор сложения, в случае со строками, он выполняет их соединение(конкатенацию).
В итоге получается такой селектор — ‘.modal=[data-modal=”значение переменной”]’ , который и находит наше модальное окно.
Давайте добавим нашему окну и подложке класс active.
modalElem.classList.add('active');
overlay.classList.add('active');
Напишем стили для классов .active:
.modal.active,
.overlay.active{
   opacity: 1;
   visibility: visible;
}
Весь javascript код который получился:
document.addEventListener('DOMContentLoaded', function() {
   var modalButtons = document.querySelectorAll('.js-open-modal'),
       overlay      = document.querySelector('#overlay-modal'),
       closeButtons = document.querySelector('.js-modal-close');
   
   
   modalButtons.forEach(function(item){
      
      item.addEventListener('click', function(e) {
         
         e.preventDefault();
         var modalId = this.getAttribute('data-modal'),
             modalElem = document.querySelector('.modal[data-modal="' + modalId + '"]');
         
         modalElem.classList.add('active');
         overlay.classList.add('active');
      }); // end click
   }); // end foreach
}); // end ready
Картинка должна открывать то модальное окно, к которой привязана.
