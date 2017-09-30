# ES6新特性介绍

ECMAScript 6.0（以下简称 ES6）是 JavaScript 语言的下一代标准，已经在2015年6月正式发布了。它的目标，是使得 JavaScript 语言可以用来编写复杂的大型应用程序，成为企业级开发语言。

一种新的语法从提案到变成正式标准，需要经历五个阶段。每个阶段的变动都需要由 TC39 委员会批准。

- Stage 0 - Strawman（展示阶段）
- Stage 1 - Proposal（征求意见阶段）
- Stage 2 - Draft（草案阶段）
- Stage 3 - Candidate（候选人阶段）
- Stage 4 - Finished（定案阶段）

# generate函数
generate函数是es6提供的一种解决异步编程的方案，generate有多种理解角度，从语法上来理解，generate是包含了多种内部状态的状态机，并且可以分布执行。

### generate函数声明
generate函数从形式上看是一个普通的函数，包含两个特征function关键字和函数名中间有个*号，二是，函数内部使用yield表达式，定义内部不同的状态。

**example**  

```javascript
function* generateFn(){ 
    console.log('the first word'); 
    yield 'this is first state';
    yield 'this is second state';
    
    return 'end';
 }
 ```
 
### generate函数调用步骤
  
  generate的调用方式和普通函数相同，也是函数名加一对圆括号；不同的是，generate调用后并不会立即返回函数执行结果，而是返回一个指向内部状态的指针对象，然后调用改指针对象的next方法，执行yield语句，并使的指针对象指向generate函数的下一个状态。也就是说，每次调用next方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个yield表达式（或return语句）为止。
  
 **example**
 
  ```javascript
  const g = generateFn(); // the first word
  g.next();  // this is first state
  g.next();  // this is second state
  g.next()
  ```
  
  ### generate函数常用方法
  
  #### next
  generate函数返回一个遍历器对象，只有通过next方法才能遍历执行函数下一个内部状态
  #### throw
  
  #### return
  return函数可以返回给定的值，并遍历generate函数
  如果generate函数内部部署的有try...finally代码块，那么return函数将会被推迟到finally代码块执行完再执行
  
  ### yield、yield*表达式
  yield*表达式用于在一个generate函数中执行另外一个generate函数，因为如果在 Generator 函数内部，调用另一个 Generator 函数，默认情况下是没有效果的
  **eg**
  ```javascript
    function* innerGenerate(){
      yield 'hello';
      yield 'world';
    }
    
    function* outerGenerate(){
       yield 'i am outer';
       yield* innerGenerate();
       yield 'i am outer ';
    }
    const g = outerGenerate();
    g.next();  //i am outer;
    g.next();  // hello world;
    g.next();  // i am outer;
    g.next();  // undefined;
  ```
  yield* 表达式实际上相当于遍历执行generate函数产生的遍历器对象

  


