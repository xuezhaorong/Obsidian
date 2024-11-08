
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

