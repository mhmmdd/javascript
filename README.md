# Airbnb JavaScript Stil Rehberi() {

*JavaScript'e daha mantıklı yaklaşım için*


## İçindekiler

  1. [Tipler](#types)
  1. [Object'ler](#objects)
  1. [Diziler](#arrays)
  1. [String'ler](#strings)
  1. [Fonksiyonlar](#functions)
  1. [Nitelikler](#properties)
  1. [Değişkenler](#variables)
  1. [Hoisting](#hoisting)
  1. [Koşullu İfadeler & Eşitlik](#conditional-expressions--equality)
  1. [Bloklar](#blocks)
  1. [Yorumlar](#comments)
  1. [Boşluk](#whitespace)
  1. [Virgüller](#commas)
  1. [Noktalı Virgüller](#semicolons)
  1. [Tip Dönüşümü](#type-casting--coercion)
  1. [İsimlendirmeler](#naming-conventions)
  1. [Erişimciler](#accessors)
  1. [Kurucular](#constructors)
  1. [Olaylar](#events)
  1. [Modüller](#modules)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 Uyumluluğu](#ecmascript-5-compatibility)
  1. [Test Etme](#testing)
  1. [Performans](#performance)
  1. [Kaynaklar](#resources)
  1. [In the Wild](#in-the-wild)
  1. [Çeviriler](#translation)
  1. [The JavaScript Style Guide Guide](#the-javascript-style-guide-guide)
  1. [Katkıda Bulunanlar](#contributors)
  1. [Lisans](#license)

## Tipler

  - **İlkeller**: Bir ilkel tipe erişmek istediğinizde doğrudan onun değeri ile çalışın

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1,
        bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Kompleks**: Bir kompleks tipe erişmek istediğinizde onun değeri ile ilişkili bir referans ile çalışın

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2],
        bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ yukarı git](#table-of-contents)**

## Object'ler

  - Object oluşturmak için değişmez aslına uygun sözdizimini kullanın.

    ```javascript
    // kötü
    var item = new Object();

    // iyi
    var item = {};
    ```

  - Key'ler için [rezerve edilmiş kelimeleri](http://es5.github.io/#x7.6.1) kullanmayın. IE8'de çalışmaz. [Daha fazla](https://github.com/airbnb/javascript/issues/61)

    ```javascript
    // kötü
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // iyi
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - Rezerve edilmiş kelimelerin yerine okunabilir eşanlamlılarını kullanın.

    ```javascript
    // kötü
    var superman = {
      class: 'alien'
    };

    // kötü
    var superman = {
      klass: 'alien'
    };

    // iyi
    var superman = {
      type: 'alien'
    };
    ```

**[⬆ yukarı git](#table-of-contents)**

## Diziler

  - Dizi oluşturmak için değişmez sözdizimini kullanın.

    ```javascript
    // kötü
    var items = new Array();

    // iyi
    var items = [];
    ```

  - Dizi uzunluğunu bilmiyorsanız Array#push kullanın.

    ```javascript
    var someStack = [];


    // kötü
    someStack[someStack.length] = 'abracadabra';

    // iyi
    someStack.push('abracadabra');
    ```

  - Bir diziyi kopyalamaya ihtiyaç duyarsanız Array#slice kullanın. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length,
        itemsCopy = [],
        i;

    // kötü
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // iyi
    itemsCopy = items.slice();
    ```

  - Bir dizi-benzeri object'i araya çevirmek için, Array#slice kullanın.

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ yukarı git](#table-of-contents)**


## String'ler

  - String'ler için tek tırnak `''` kullanın

    ```javascript
    // kötü
    var name = "Bob Parr";

    // iyi
    var name = 'Bob Parr';

    // kötü
    var fullName = "Bob " + this.lastName;

    // iyi
    var fullName = 'Bob ' + this.lastName;
    ```

  - String'ler 80 karakterden daha uzunsa, `string concatenation` ile bölünmüş satırlar şeklinde yazılmalıdır.
  - Not: Gerekli kullanımda, uzun string birleştirme işlemleri performansı etkileyebilir. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40)

    ```javascript
    // kötü
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // kötü
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // iyi
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - Programlı bir şekilde string oluşturmak için, `string concatenation` yerine Array#join kullanın. Çoğunlukla IE için: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items,
        messages,
        length,
        i;

    messages = [{
      state: 'success',
      message: 'This one worked.'
    }, {
      state: 'success',
      message: 'This one worked as well.'
    }, {
      state: 'error',
      message: 'This one did not work.'
    }];

    length = messages.length;

    // kötü
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // iyi
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = messages[i].message;
      }

      return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
    }
    ```

**[⬆ yukarı git](#table-of-contents)**


## Fonksiyonlar

  - Fonksiyon ifadeleri:

    ```javascript
    // anonim fonksiyon ifadesi
    var anonymous = function() {
      return true;
    };

    // adlandırılmış fonksiyon ifadesi
    var named = function named() {
      return true;
    };

    // direk-çağrılan fonksiyon ifadesi (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Bir `non-function` blok (if, while, vb.) içinde asla bir fonksiyon oluşturmayın. Onun yerine fonksiyonu bir değişkene atayın. Tarayıcılar onu yapmana izin verir, fakat farklı yorumlarlar.
  - **Not:** ECMA-262 bir ifadelerin listesi gibi `block` tanımlar. Bir fonksiyon bildirimi bir ifade değildir. [Bu konu hakkında ECMA-262 notunu okuyun](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // kötü
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // iyi
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - Bir parametreyi asla `arguments` olarak isimlendirmeyin, her fonksiyon kapsamında verilen `argümanlar` objesi üzerinde öncelik alacaktır.

    ```javascript
    // kötü
    function nope(name, options, arguments) {
      // ...bir şeyler...
    }

    // iyi
    function yup(name, options, args) {
      // ...bir şeyler...
    }
    ```

**[⬆ yukarı git](#table-of-contents)**



## Nitelikler

  - Niteliklere erişmek için nokta `.` işaretini kullanın.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // kötü
    var isJedi = luke['jedi'];

    // iyi
    var isJedi = luke.jedi;
    ```

  - Niteliklere bir değişken ile erişmek için köşeli parantez `[]` simgesini kullanın .

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

**[⬆ yukarı git](#table-of-contents)**


## Değişkenler

  - Değişken bildirimleri için her zaman `var` ifadesini kullanın. Bunu yapmamak, değişkenin global değişkenler içinde olmasına neden olacaktır. Global isim uzayını kirletmeyi önlemek istiyoruz. Captain Planet uyardı.

    ```javascript
    // kötü
    superPower = new SuperPower();

    // iyi
    var superPower = new SuperPower();
    ```

  - Çoklu değişkenlerde tek bir `var` bildirimi kullanın ve her değişkeni yeni bir satırda bildirin.

    ```javascript
    // kötü
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';

    // iyi
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';
    ```

  - Atanmamış değişkenleri en sonda bildirin. Önceki atanmış değişkenlerin birine bağlı olan bir değişkene atama yapmanız gerektiği zaman bu faydalıdır .

    ```javascript
    // kötü
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // kötü
    var i, items = getItems(),
        dragonball,
        goSportsTeam = true,
        len;

    // iyi
    var items = getItems(),
        goSportsTeam = true,
        dragonball,
        length,
        i;
    ```

  - Kendi kapsama alanının en üstünde değişkenleri atayın. Bu, değişken bildirimi ve atama kaldırma ile ilgili sorunları önlemek için yardımcı olur.

    ```javascript
    // kötü
    function() {
      test();
      console.log('doing stuff..');

      //..diğer şeyler..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // iyi
    function() {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..diğer şeyler..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // kötü
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // iyi
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

**[⬆ yukarı git](#table-of-contents)**


## Hoisting

  - Değişken bildirimleri kapsama alanının en üstünde hoitsting edildi, onların ataması yapılmamış.

    ```javascript
    // Bunun çalışmayacağını biliyoruz(notDefined global
    // değişkenin bildirilmediğini varsayarsak)
    function example() {
      console.log(notDefined); // => bir ReferenceError fırlatır
    }

    // değişkene başvuru yaptıktan sonra  
    // değişken hoisting nedeniyle 
    // bir değişken bildirimi oluşturma çalışacaktır.
    // Not: `true` değerinin ataması hoisting edilmemiştir.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // Çevirmen değişken bildirimini  
    // kapsamın en üstünde hoisting ediyor. Bizim örneğimizin
    // şu şekilde yeniden yazılabileceği anlamına geliyor:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Anonim fonksiyon ifadeleri onların değişken adıyla hoisting edilir, ama fonksiyon ataması yok.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Named function expressions hoist the variable name, not the function name or the function body.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // the same is true when the function name
    // is the same as the variable name.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/)

**[⬆ yukarı git](#table-of-contents)**



## Koşullu İfadeler & Eşitlik

  - `==` ve `!=` yerine `===` ve `!==` kullanın.
  - Koşullu ifadeler `ToBoolean` metodu ile baskı kullanılarak değerlendirilir ve her zaman bu basit kurallarla uyun:

    + **Object'Ler** -> **true**
    + **Undefined** -> **false**
    + **Null** -> **false**
    + **Booleans** -> **boolean'ın değeri**
    + **Numbers** -> **+0, -0, or NaN** ise **false**, diğer durumlarda **true**
    + **Strings** -> `''` boş string olursa **false** , diğer durumlarda **true**

    ```javascript
    if ([0]) {
      // true
      // Bir dizi bir object'dir, object'ler true olarak kabul edilir
    }
    ```

  - Kısa yolları kullanın.

    ```javascript
    // kötü
    if (name !== '') {
      // ...bir şeyler...
    }

    // iyi
    if (name) {
      // ...bir şeyler...
    }

    // kötü
    if (collection.length > 0) {
      // ...bir şeyler...
    }

    // iyi
    if (collection.length) {
      // ...bir şeyler...
    }
    ```

  - Daha fazla bilgi için bknz: [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll

**[⬆ yukarı git](#table-of-contents)**


## Block'lar

  - Tüm çok satırlı block'lar için süslü parantezleri kullanın.

    ```javascript
    // kötü
    if (test)
      return false;

    // iyi
    if (test) return false;

    // iyi
    if (test) {
      return false;
    }

    // kötü
    function() { return false; }

    // iyi
    function() {
      return false;
    }
    ```

**[⬆ yukarı git](#table-of-contents)**


## Yorumlar

  - Çok satırlı yorumlar için `/** ... */` kullanın. Bir açıklama yazın; tipleri, tüm parametreler için değerleri ve dönen değerleri belirtin.

    ```javascript
    // kötü
    // verilen tag ismine göre
    // make() yeni bir element döndürür
    //
    // @param <String> tag
    // @return <Element> element
    function make(tag) {

      // ...bir şeyler...

      return element;
    }

    // good
    /**
     * verilen tag ismine göre
     * make() yeni bir element döndürür
     *
     * @param <String> tag
     * @return <Element> element
     */
    function make(tag) {

      // ...bir şeyler...

      return element;
    }
    ```

  - Tek satırlı yorumlar için `//` kullanın. Yorumun konusu yukarda gelecek şekilde, tek satırlık yorumları yeni bir satırda yerleştirin. Yorumdan önce boş bir satır bırakın.

    ```javascript
    // kötü
    var active = true;  // aktif olan tab

    // iyi
    // aktif olan tab
    var active = true;

    // kötü
    function getType() {
      console.log('fetching type...');
      // varsayılan tipi 'no type' olarak ayarlar
      var type = this._type || 'no type';

      return type;
    }

    // iyi
    function getType() {
      console.log('fetching type...');

      // varsayılan tipi 'no type' olarak ayarlar
      var type = this._type || 'no type';

      return type;
    }
    ```

  - Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME -- need to figure this out` or `TODO -- need to implement`.

  - Use `// FIXME:` to annotate problems

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - Use `// TODO:` to annotate solutions to problems

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
  ```

**[⬆ yukarı git](#table-of-contents)**


## Boşluk

  - Soft tab'ları 2 karakter boşluğa ayarlanmış şekilde kullanın 

    ```javascript
    // kötü
    function() {
    ∙∙∙∙var name;
    }

    // kötü
    function() {
    ∙var name;
    }

    // iyi
    function() {
    ∙∙var name;
    }
    ```

  - Place 1 space before the leading brace.

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - Set off operators with spaces.

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - End files with a single newline character.

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - Use indentation when making long method chains.

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .class('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

**[⬆ back to top](#table-of-contents)**

## Commas

  - Leading commas: **Nope.**

    ```javascript
    // bad
    var once
      , upon
      , aTime;

    // good
    var once,
        upon,
        aTime;

    // bad
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // good
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it's in quirksmode. Also, in some implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in ES5 ([source](http://es5.github.io/#D)):

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // bad
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // good
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

**[⬆ back to top](#table-of-contents)**


## Semicolons

  - **Yup.**

    ```javascript
    // bad
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // good
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // good (guards against the function becoming an argument when two files with IIFEs are concatenated)
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [Read more](http://stackoverflow.com/a/7365214/1712802).

**[⬆ back to top](#table-of-contents)**


## Type Casting & Coercion

  - Perform type coercion at the beginning of the statement.
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    var totalScore = this.reviewScore + '';

    // good
    var totalScore = '' + this.reviewScore;

    // bad
    var totalScore = '' + this.reviewScore + ' total score';

    // good
    var totalScore = this.reviewScore + ' total score';
    ```

  - Use `parseInt` for Numbers and always with a radix for type casting.

    ```javascript
    var inputValue = '4';

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // bad
    var val = inputValue >> 0;

    // bad
    var val = parseInt(inputValue);

    // good
    var val = Number(inputValue);

    // good
    var val = parseInt(inputValue, 10);
    ```

  - If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    var val = inputValue >> 0;
    ```

  - **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](http://es5.github.io/#x4.3.19), but Bitshift operations always return a 32-bit integer ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```

**[⬆ back to top](#table-of-contents)**


## Naming Conventions

  - Avoid single letter names. Be descriptive with your naming.

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - Use camelCase when naming objects, functions, and instances

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}
    var u = new user({
      name: 'Bob Parr'
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Use PascalCase when naming constructors or classes

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // good
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - Use a leading underscore `_` when naming private properties

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - When saving a reference to `this` use `_this`.

    ```javascript
    // bad
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // bad
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // good
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - Name your functions. This is helpful for stack traces.

    ```javascript
    // bad
    var log = function(msg) {
      console.log(msg);
    };

    // good
    var log = function log(msg) {
      console.log(msg);
    };
    ```

  - **Note:** IE8 and below exhibit some quirks with named function expressions.  See [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/) for more info.

**[⬆ back to top](#table-of-contents)**


## Accessors

  - Accessor functions for properties are not required
  - If you do make accessor functions use getVal() and setVal('hello')

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - If the property is a boolean, use isVal() or hasVal()

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - It's okay to create get() and set() functions, but be consistent.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

**[⬆ back to top](#table-of-contents)**


## Constructors

  - Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // bad
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // good
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20) // => undefined

    // good
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

**[⬆ back to top](#table-of-contents)**


## Events

  - When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

    ```js
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    prefer:

    ```js
    // good
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ back to top](#table-of-contents)**


## Modules

  - The module should start with a `!`. This ensures that if a malformed module forgets to include a final semicolon there aren't errors in production when the scripts get concatenated. [Explanation](https://github.com/airbnb/javascript/issues/44#issuecomment-13063933)
  - The file should be named with camelCase, live in a folder with the same name, and match the name of the single export.
  - Add a method called `noConflict()` that sets the exported module to the previous version and returns this one.
  - Always declare `'use strict';` at the top of the module.

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

**[⬆ back to top](#table-of-contents)**


## jQuery

  - Prefix jQuery object variables with a `$`.

    ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = $('.sidebar');
    ```

  - Cache jQuery lookups.

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Use `find` with scoped jQuery object queries.

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ back to top](#table-of-contents)**


## ECMAScript 5 Compatibility

  - Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/)

**[⬆ back to top](#table-of-contents)**


## Testing

  - **Yup.**

    ```javascript
    function() {
      return true;
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Performance

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

**[⬆ back to top](#table-of-contents)**


## Resources


**Read This**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Other Styleguides**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)

**Other Styles**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52)
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript)
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**Further Reading**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban

**Books**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/)
  - [Third Party JavaScript](http://manning.com/vinegar/) - Ben Vinegar and Anton Kovalyov

**Blogs**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

**[⬆ back to top](#table-of-contents)**

## In the Wild

  This is a list of organizations that are using this style guide. Send us a pull request or open an issue and we'll add you to the list.

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **Avalara**: [avalara/javascript](https://github.com/avalara/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **Intent Media**: [intentmedia/javascript](https://github.com/intentmedia/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **Muber**: [muber/javascript](https://github.com/muber/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **REI**: [reidev/js-style-guide](https://github.com/reidev/js-style-guide)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **Userify**: [userify/javascript](https://github.com/userify/javascript)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

## Translation

  This style guide is also available in other languages:

  - :de: **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - :jp: **Japanese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - :br: **Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - :cn: **Chinese**: [adamlu/javascript-style-guide](https://github.com/adamlu/javascript-style-guide)
  - :es: **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - :kr: **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - :fr: **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - :ru: **Russian**: [uprock/javascript](https://github.com/uprock/javascript)
  - :bg: **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)

## The JavaScript Style Guide Guide

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## Contributors

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## License

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**

# };
