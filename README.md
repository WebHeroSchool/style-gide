# Руководства по стилю (JS)

**1) Избегайте forEach**
    Избегайте forEach при изменении данных. Используйте map, reduce или filter вместо forEach для изменять данные.
   
    // bad
    users.forEach((user, index) => {
      user.id = index;
    });
    
    // good
    const usersWithId = users.map((user, index) => {
      return Object.assign({}, user, { id: index });
    });
    
**2) Ограничить количество параметров**
    Если ваша функция или метод имеет более 3 параметров, тогда используйте объект в качестве параметра.
    // bad
    function a(p1, p2, p3) {
      // ...
    };
    
    // good
    function a(p) {
      // ...
    };
**3) Избегайте побочных эффектов в конструкторах**
Избегайте асинхронных вызовов, запросов API или манипуляций с DOM в constructor. Вместо этого переместите их в отдельные функции. Это облегчит написание тестов и поддержку кода.
    
    // bad
    class myClass {
      constructor(config) {
        this.config = config;
        axios.get(this.config.endpoint)
      }
    }
    
    // good
    class myClass {
      constructor(config) {
        this.config = config;
      }
    
      makeRequest() {
        axios.get(this.config.endpoint)
      }
    }
    const instance = new myClass();
    instance.makeRequest();    
    
**4) Избегайте классов для обработки событий DOM**
Если единственная цель класса - связать событие DOM и обработать обратный вызов, используйте функцию.

    // bad
    class myClass {
      constructor(config) {
        this.config = config;
      }
    
      init() {
        document.addEventListener('click', () => {});
      }
    }
    
    // good
    
    const myFunction = () => {
      document.addEventListener('click', () => {
        // handle callback here
      });
    }
**5) Передать контейнер элемента в конструктор**
Когда ваш класс манипулирует DOM, получите контейнер элемента в качестве параметра. Это легче поддерживать и производительность будет выше.
    
    // bad
    class a {
      constructor() {
        document.querySelector('.b');
      }
    }
    
    // good
    class a {
      constructor(options) {
        options.container.querySelector('.b');
      }
    }
**6) Используйте ParseInt**
Используется ParseInt при преобразовании числовой строки в число.

    // bad
    Number('10')
    
    // good
    parseInt('10', 10);    
    
**7) CSS селекторы - использовать js-префикс**
    Если CSS-класс используется только в JavaScript как ссылка на элемент, добавьте префикс имени класса js-.
    
    // bad
    <button class="add-user"></button>
    
    // good
    <button class="js-add-user"></button>
    
**8) Абсолютные и относительные пути для модулей**
Используйте относительные пути, если модуль, который вы импортируете, находится ниже двух уровней.

    // bad
    import GitLabStyleGuide from '~/guides/GitLabStyleGuide';
    
    // good
    import GitLabStyleGuide from '../GitLabStyleGuide';

Если модуль, который вы импортируете, находится на два или более уровней выше, используйте вместо него абсолютный путь:

    // bad
    import GitLabStyleGuide from '../../../guides/GitLabStyleGuide';
    
    // good
    import GitLabStyleGuide from '~/GitLabStyleGuide';

Кроме того, не добавляйте их в глобальное пространство имен.
 
**9) Защита XSS**
  Не используйте innerHTML, append() или html() для установки контента. Это открывает слишком много уязвимостей
  
**10) Функции**
Стремитесь написать много маленьких чистых функций и минимизируйте, где происходят мутации.
   
    // bad
    const values = {foo: 1};
    
    function impureFunction(items) {
      const bar = 1;
    
      items.foo = items.a * bar + 2;
    
      return items.a;
    }
    
    const c = impureFunction(values);
    
    // good
    var values = {foo: 1};
    
    function pureFunction (foo) {
      var bar = 1;
    
      foo = foo * bar + 2;
    
      return foo;
    }
    
    var c = pureFunction(values.foo);  