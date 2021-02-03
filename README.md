### JS继承

这里简单的介绍一下继承，在我们的日常开发中，通过继承可以更好的复用以前的开发代码，提升开发效率。

继承实现的方式有很多中，这里我们分 使用Object.create() 和不使用Object.create()的分开来讲

#### 不使用Object.create()的继承

+ 原型链继承。

  ```jsx
    function Parent1() {
      this.name = 'parent';
      this.friends = [1, 2, 3];
    }
  
    Parent1.prototype.getName = function () {
      return this.name;
    };
  
    function Child1() {
      this.type = 'child1';
    }
  
    Child1.prototype = new Parent1();
  
    const per1 = new Child1();
    const per2 = new Child1();
    per1.friends.push(4);
  
    console.log(per1.friends, per2.friends); // [1,2,3,4] [1,2,3,4]
  ```

  原型链继承缺点：两个实例使用的是同一个原型对象，内存空间是共享的，当一个发生变化时，另外一个也会随之变化

+ 构造函数继承

  ```jsx
    function Parent1() {
      this.name = 'parent';
      this.friends = [1, 2, 3];
    }
  
    Parent1.prototype.getName = function () {
      return this.name;
    };
  
    function Child1() {
      Parent1.call(this)
      this.type = 'child1';
    }
  
    const per1 = new Child1();
    const per2 = new Child1();
    per1.friends.push(4);
  
    console.log(per1.friends, per2.friends); // [1,2,3] [1,2,3,4]
    console.log(per1.getName); // undefined
  ```

  构造函数继承可以解决上面内存共享的问题，但是有个缺点，只能继承父类的实例属性和方法，不会继承原型的属性和方法。

+ 组合继承（结合前两种）

  ```jsx
    function Parent1() {
      this.name = 'parent';
      this.friends = [1, 2, 3];
    }
  
    Parent1.prototype.getName = function () {
      return this.name;
    };
  
    function Child1() {
      Parent1.call(this); // 第二次调用
      this.type = 'child1';
    }
  
    Child1.prototype = new Parent1(); // 第一次调用
    Child1.prototype.constructor = Child1;
  
    const per1 = new Child1();
    const per2 = new Child1();
    per1.friends.push(4);
    console.log(per1.friends, per2.friends); // [1,2,3,4] [1,2,3]
    console.log(per1.getName()); // 'parent'
  ```

  组合继承可以很好的解决上面的两个问题，但是呢，也有一些小瑕疵。调用了两次父类的构造函数，那这个问题怎么优化呢，下面最后一种继承方式可以更好的解决这个问题

  

  #### 使用Object.create()的继承

  + 原型式继承

    ```jsx
    const obj = {
      name:'jack',
      age:18,
      friends:[1,2,3],
      getName:function() {
        return this.name
      }
    }
    const newObj1 = Object.create(obj)
    const newObj2 = Object.create(obj)
    
    newObj1.friends.push(4)
    
    console.log(newObj1.friends,newObj2.friends) // [1,2,3,4] [1,2,3,4]
    
    ```

    原型式继承类似于浅拷贝，对于引用类型：多个实例的引用类型属性指向相同的内存。

  + 寄生式继承

    ```jsx
    const obj = {
      name:'jack',
      age:18,
      friends:[1,2,3],
      getName:function() {
        return this.name
      }
    }
    function clone(obj) {
      const newObj = Object.create(obj)
      newObj.sayHello = function() {
        return 'hello'
      }
      return newObj
    }
    const newObj = clone(obj)
    
    console.log(newObj.sayHello) // 'hello'
    ```

    寄生式继承就是在继承的同时，给新的对象增加一些属性和方法

  + 寄生组合式继承

    ```jsx
    function clone(parent,child) {
      child.prototype = Object.create(parent.prototype)
      child.prototype.constructor = child
    }
    
    function Parent() {
      this.name = 'parent';
      this.friends = [1,2,3]
    }
    
    Parent.prototype.getName = function() {
      return this.name
    }
    
    function Child() {
      Parent.call(this)
      this.type = 'type'
    }
    
    clone(Parent,Child)
    
    const person = new Child()
    
    console.log(person.getName())
    ```

    这种寄生组合式继承方式，基本可以解决前几种继承方式的缺点，较好地实现了继承想要的结果，同时也减少了构造次数.class中的extends关键字用的就是寄生组合式继承。

  

