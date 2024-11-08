
## 变量
一个变量，就是一个用于存放数值的容器。这个数值可能是一个用于累加计算的数字，或者是一个句子中的字符串。变量的独特之处在于它存放的数值是可以改变的。
```html
<button>Press me</button>

```

```js
const button = document.querySelector("button");

button.onclick = function () {
  let name = prompt("What is your name?");
  alert("Hello " + name + ", nice to see you!");
};

```

### 声明变量
要想使用变量，你需要做的第一步就是创建它——更准确的说，是声明一个变量。声明一个变量的语法是在 `var` 或 `let` 关键字之后加上这个变量的名字：
```js
let myName;
let myAge;

```

### 初始化变量
一旦你定义了一个变量，你就能够初始化它。方法如下，在变量名之后跟上一个“=”，然后是数值：
```html
myName = "Chris";
myAge = 37;

```

可以像这样在声明变量的时候给变量初始化：
```js
let myName = "Chris";

```

### 变量类型
* Number
可以在变量中存储数字，不论这些数字是像 30（也叫整数）这样，或者像 2.456 这样的小数（也叫做浮点数）。
```js
let myAge = 17;

```

* String
字符串是文本的一部分。当你给一个变量赋值为字符串时，你需要用单引号或者双引号把值给包起来，否则 JavaScript 将会把这个字符串值理解成别的变量名。
```js
let dolphinGoodbye = "So long and thanks for all the fish";

```

* Boolean
Boolean 的值有 2 种：true 或 false。它们通常被用于在适当的代码之后，测试条件是否成立。
```js
let iAmAlive = true;

```

然而实际上通常是以下用法：
```js
let test = 6 < 3;

```

* Array
数组是一个单个对象，其中包含很多值，方括号括起来，并用逗号分隔。
```js
let myNameArray = ["Chris", "Bob", "Jim"];
let myNumberArray = [10, 15, 40];

```

当数组被定义后，你可以使用如下所示的语法来访问各自的值，
```js
myNameArray[0]; // should return 'Chris'
myNumberArray[2]; // should return 40

```

* Object
```js
let dog = { name: "Spot", breed: "Dalmatian" };

```

### 动态类型
JavaScript 是一种“动态类型语言”，这意味着不同于其他一些语言 (译者注：如 C、JAVA)，你不需要指定变量将包含什么数据类型（例如 number 或 string）

例如，如果你声明一个变量并给它一个带引号的值，浏览器就会知道它是一个字符串：
```js
let myString = "Hello";

```

即使它包含数字，但它仍然是一个字符串
```js
let myNumber = "500"; // oops, this is still a string
typeof myNumber;
myNumber = 500; // much better — now this is a number
typeof myNumber;

```