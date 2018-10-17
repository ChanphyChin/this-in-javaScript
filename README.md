# 告别模糊不清的this指向  
*created by Chanphy on 2018.10.17*

***remember：this的指向在函数定义的时候是无法确定的，this在函数被调用的时候指向调用这个函数的对象***  
- this是什么？  
   1. this跟var、new、return一样，是JavaScript的一个关键字。
   2. this就是JavaScript中的一个对象。
   3. this就是包含它的函数被调用时生成的某个具体对象的映射。
   4. function和箭头函数里面的this指向是有差异的。
- examples  
  1. 如果调用的函数没有上级对象，那么this指向window
  ```javascript
  function person () {
    var name = 'chanphy';
    console.log(this.name) // undefine
  }
  person(); // 这里f()其实就是Window.fn(); 所以this指向Window
  ```
  2. 如果调用的函数存在上一级对象，那么this指向该对象
  ```javascript
  var person = {
    name: 'chanphy',
    fn: function () {
      console.log(this.name);// 'chanphy'
    }
  }
  obj.fn(); // 调用方法的是obj，所以this指向obj
  ```  
  3. 如果函数不止被一级对象包含，那么this指向该函数的上一级对象
  ```javascript
  var person = {
    name: 'chanphy',
    do: {
      name: 'sing',
      fn: function () {
        console.log(this.name); // 'sing'
      }
    }
  }
  person.do.fn(); // 调用方法的对象如果外层还有被其他对象包含，this指向它的上一级对象
  ```
  4. 使用new示例过的构造函数中的this指向它的实例对象
  ```javascript
  function Person () {
    this.name = 'chanphy'
  }
  var person1 = new Person();
  console.log(person1.name) // 'chanphy'
  ```
  5. 每个函数都有call、apply和bind方法，使用call和apply可以改变this的指向
  ```javascript
  // call和apply
  var personA = {
    name:'chanphy'
  }
  var personB = {
    name: 'bigRich',
    fn: function (argument) {
      return this.name
    }
  };
  // 对象personA调用了personB的fn方法,这里如果方法里面涉及到this，那么this指向personA
  personB.fn.call(personA, '参数1','参数2'); // 'chanphy'
  personB.fn.call(personA, ['参数1', '参数2']); // 'chanphy'
  // bind 将函数绑定在第一个参数对象上，无论何时被调用
  function person () {
    console.log(this.name)
  }
  var personA = person.bind({name:'chanphy'});
  personA(); // chanphy
  ```
  6. 严格模式下，函数内部this指向undefine
  ```javascript
  function person () {
    "use strict"
    console.log(this);
  }
  person(); // undefine
  ```
  7. 箭头函数this的指向，指向当前的函数的作用域链上的this，***不因call,aplly,bind而改变this指向***
  ```javascript
  var person = {
    name: 'chanphy',
    fn: () => {
      console.log(this.name);
    }
  }
  person.fn(); //undefine  向上找函数作用域链，this指向Window
  //////////////////////////////////////////////////////////////////////华丽的分割线
  var person = {
    name: 'chanphy',
    fn:function (){
      var getName = () => console.log(this.name);
      getName();
    }
  }
  person.fn(); // chanphy   向上找函数作用域链，this指向person
  ```
  8. dom事件中的this
  ```javascript
  // 内链形式
  <button onclick="console.log(this);"></button>  // this为dom节点button
  <button onclick="(function(){console.log(this)})()"></button> //this为window
  <button onclick="(function(){'use strict';console.log(this)})()"></button> //this为undefine
  //事件监听器
  function fn (){
    console.log(this);
  }
  var btn = document.getElementById('btn');
  btn.addEventListener('click',fn); // this指向 btn
  



  
