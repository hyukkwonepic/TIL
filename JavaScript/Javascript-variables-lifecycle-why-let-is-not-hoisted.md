# JavaScript variables lifecycle: why let is not hoisted

Hoisting is the process of virtually moving the variable or function definition to the beginning of the scope, usually for variable statement var and function declaration function fun() {...}.
호이스팅(Hoisting)은 정의된 함수의 변수들을 가상적으로 스코프(scope)의 맨 위로 올리는 과정을 말하는데요, 보통 `var`과 같은 변수문이나 `fun() {...}` 와 같은 함수 선언(function declaration)에 사용됩니다.

When let (and also const and class, which have similar declaration behavior as let) declarations were introduced by ES2015, many developers including myself were using the hoisting definition to describe how variables are accessed. But after more search on the question, surprisingly for me hoisting is not the correct term to describe the initialization and availability of the let variables.
`let`선언(또한 `let`과 비슷한 선언 행동을 보이는 `const` 와 `class`)은 ES2015에서 소개되었는데요, 저를 포함한 많은 개발자들은 호이스팅 선언을 사용하여 변수가 어떻게 접근되는지 표현합니다. 하지만 이 질문에 대해 조사를 해보니, 놀랍게도 호이스팅은 `let`변수를 초기화(initialization)와 가용성(availability)을 설명하는 올바른 용어가 아니었습니다.

ES2015 provides a different and improved mechanism for let. It demands stricter variable declaration practices (you can't use before definition) and as result better code quality.
ES2015는 `let`에 관해 이전과 다른, 개선된 메커니즘을 제공합니다. 이는 보다 엄격한 변수 선언 방법(선언 이전에는 사용할 수 없음)을 요구하기 때문에 더 나은 코드 품질로 이어집니다.
 
Let's dive into more details about this process.
이 과정에 대해 자세히 알아보도록 합시다.

## 1. Error prone var hoisting
## 1. 오류 발생 가능성이 있는 `var` 호이스팅
Sometimes I see a weird practice of variables var varname and functions function funName() {...} declaration in any place in the scope:
저는 가끔씩 스코프(scope) 내의 어떤 부분에서건 `var varname` 및 함수 `function funName() {...}`선언의 이상한 행동들이 보이는데요:

Try in JS Bin
```
// var 호이스팅(hoisting)
num;     // => undefined  
var num;  
num = 10;  
num;     // => 10  
// 함수 호이스팅(function hoisting)
getPi;   // => function getPi() {...}  
getPi(); // => 3.14  
function getPi() {  
  return 3.14;
}
```

The variable num is accessed before declaration var num, so it is evaluated to undefined.
변수 `num`은 `var num`선언 이전에 액세스 되기 때문에 `undefined`로 평가됩니다.
The function function getPi() {...} is defined at the end of file. However the function can be called before declaration getPi(), as it is hoisted to the top of the scope.
`function getPi() {...}`함수는 파일의 끝에 정의됩니다. 하지만 함수는 스코프의 맨 위로 호이스팅되기 때문에 `getPi()`는 선언 전에 호출될 수 있습니다.

This is the classical hoisting.
이것은 전통적인 호이스팅입니다. 

As it turns out, the possibility to first use and then declare a variable or function creates confusion. Suppose you scroll a big file and suddenly see an undeclared variable... how the hell it does appear here and where is it defined? 
결과적으로, 변수나 함수를 먼저 사용한 후에 선언을 하면 혼란이 생길 수 있습니다. 예를들어 여러분이 엄청 큰 파일을 살펴보고 있는데, 갑자기 선언되지 않은 변수가 나타나면... 대체 이 변수가 왜 여기에 있는거지!? 도대체 어디에서 정의 된거지? 라는 의문(짜증)을 품게 될 수 있죠.
Of course a practiced JavaScript developer won't code this way. But in the thousands of JavaScript GitHub repos is quite possible to deal with such code.
물론, 숙련된 JavaScript 개발자는 이런식으로 코드를 짜지 않을 겁니다. 하지만 수 천개의 JavaScript와 관련된 깃헙 레포들에서는 이런 코드를 자주 만나게 됩니다.

Even looking at the code sample presented above, it is difficult to understand the declaration flow in the code.
위에서 보여드린 예제 코드를 살펴보더라도, 코드내에서 선언의 흐름을 이해하기가 까다롭죠.

Naturally first you declare or describe an unknown term. And only later make phrases with it. let encourages you to follow this approach with variables.
물론 이 알 수 없는 코드를 먼저 선언하거나 설명하세요. 그리고 선언을 작성하면 됩니다. `let`은 변수를 사용하여 이러한 접근방식을 따를 수 있게 도와줍니다.

## 2. Under the hood: variables lifecycle
## 2. 표면 아래의 변수 라이프사이클(lifecycle)
When the engine works with variables, their lifecycle consists of the following phases:
JavaScript엔진이 변수를 다룰 때, 변수의 라이프사이클은 아래의 단계로 구성됩니다:


1. Declaration phase is registering a variable in the scope.
1. `선언 단계(Declaration phase)`는 스코프 내에 변수를 등록합니다.
2. Initialization phase is allocating memory and creating a binding for the variable in the scope. At this step the variable is automatically initialized with undefined.
2. `초기화 단계(Initialization phase)`는 메모리를 할당하고 스코프내의 변수에 대한 바인딩(binding)을 만드는 단계입니다. 해당 단계에서 변수는 자동으로 `undefined`값으로 초기화 됩니다. 
3. Assignment phase is assigning a value to the initialized variable.
3. `할당 단계(Assignment phase)`에서 초기화 된 변수에 값을 할당합니다. 

A variable has unitialized state when it passed the declaration phase, yet didn't reach the initilization.
변수는 선언 단계를 통과할 때 초기화되지 않은(uninitialized) 상태를 가지지만, 아직은 초기화에 도달하지 않았습니다.

[img]
Variables lifecycle phases in JavaScript
JavaScript 변수의 라이프사이클 단계

Notice that in terms of variables lifecycle, declaration phase is a different term than generally speaking variable declaration. In simple words, the engine processes the variable declaration in 3 phases: declaration phase, initialization phase and assignment phase.
`변수 라이프사이클` 측면에서는, `선언 단계`가 일반적으로 말하는 `변수 선언`과 다른 용어라는 점에 주목하세요. 간단히 말해, JavaScript엔진은 `선언, 초기화, 할당` 3단계로 `변수 선언` 을 처리한다는 것입니다.

## 3. var variables lifecycle
## 3. var 변수 라이프사이클
Being familiar with lifecycle phases, let's use them to describe how the engine handles var variables.
이제 라이프사이클 단계들에 익숙해 졌으니, 쟈스 엔진이 `var`변수를 처리하는 방법을 설명하겠습니다.

[img]
var statement variables lifecycle
var 선언 변수 라이프사이클

Suppose a scenario when JavaScript encounters a function scope with var variable statement inside. The variable passes the declaration phase and right away the initialization phase at the beginning of the scope, before any statements are executed (step 1). 
var variable statement position in the function scope does not influence the declaration and initialization phases.
자바스크립트가 `var`변수 선언문을 포함하는 함수 스코프를 만나는 상황을 가정해봅시다. 변수는 모든 선언문들이 실행되기 전에, 스코프의 시작 지점에서 선언 단계를 통과한 후 바로 초기화 단계를 거칩니다 (1 단계). 함수 스코프 내의 `var variable`선언문의 위치는 선언 단계와 초기화 단계에 영향을 미치지 않습니다.

After declaration and initialization, but before assignment phase, the variable has undefined value and can be used already.
선언과 초기화 단계를 지난 이후, 하지만 할당 단계 직전에, 해당 변수는 `undefined`를 가지기 때문에 이미 사용할 수 있는 상태입니다.

On assignment phase variable = 'value' the variable receives its initial value (step 2).
할당 단계에서 `variable = var'` 변수는 초기 값을 받습니다 (2단계).

Strictly hoisting consists in the idea that a variable is declared and initialized at the beginning of the function scope. There is no gap between declaration and initialization phases.
엄밀하게 말해서, 호이스팅이란, 변수는 함수 스코프의 맨 앞에서 선언되고 초기화 된다는 개념입니다. 바로 선언 단계와 초기화 단계에는 간격이 없다는 말이죠.

Let's study an example. The following code creates a function scope with a var statement inside:
예제를 한번 살펴봅시다. 아래의 코드는 내부에 `var` 선언문이 포함된 함수 스코프를 생성합니다:

```
Try in JS Bin
function multiplyByTen(number) {  
  console.log(ten); // => undefined
  var ten;
  ten = 10;
  console.log(ten); // => 10
  return number * ten;
}
multiplyByTen(4); // => 40  
```
When JavaScript starts executing multipleByTen(4) and enters the function scope, the variable ten passes declaration and initialization steps, before the first statement. So when calling console.log(ten) it is logged undefined. 
자바스크립트가 `multipleByTen(4)`를 실행하고 함수 스코프에 들어가기 시작할 때, 변수 `ten`은 첫번째 선언 이전에 선언 단계와 초기화 단계를 거칩니다. 따라서 `console.log(ten)`을 호출하면 `undefined`가 기록되죠. 
The statement ten = 10 assigns an initial value. After assignment, the line console.log(ten) logs correctly 10 value.
`ten = 10`선언문은 초기 값을 할당합니다. 할당 후에는, `console.log(ten)`명령어는 정확히 `10`의 값을 기록하게 됩니다.

## 4. Function declaration lifecycle
## 4. 함수 선언 라이프사이클
In case of a function declaration statement function funName() {...} it's even easier.
함수 선언문 `function funName() {...}`의 경우는 오히려 더 쉽습니다.

[img]
Function declaration variables lifecycle
함수 선언 변수 라이프사이클

The declaration, initialization and assignment phases happen at once at the beginning of the enclosing function scope (only one step). funName() can be invoked in any place of the scope, not depending on the declaration statement position (it can be even at the end).
이 경우에는, 함수 스코프의 시작에서 선언, 초기화 그리고 할당 단계가 한번에 일어납니다(오직 한단계 뿐이죠). `funName()`은 스코프 내에서 선언문의 위치에 구애받지 않고 어느 위치에서나 호출 할 수 있습니다(끝 부분에서도 가능하죠).

The following code sample demonstrates the function hoisting:
아래의 코드는 함수 호이스팅을 보여줍니다:

```
Try in JS Bin
function sumArray(array) {  
  return array.reduce(sum);
  function sum(a, b) {
    return a + b;
  }
}
sumArray([5, 10, 8]); // => 23  
```
When JavaScript executes sumArray([5, 10, 8]), it enters sumArray function scope. Inside this scope, immediately before any statement execution, sum passes all 3 phases: declaration, initialization and assignment. 
자바스크립트가 `sumArray([5, 10, 8])`을 실행할 때, 자바스크립트는 sumArray 함수 스코프 안에 들어갑니다. 스코프 내부에서, 어떠한 선언문의 실행 바로 직전에, `sum`은 3가지 단계를 모두 통과합니다: 선언, 초기화 그리고 할당이죠.
This way array.reduce(sum) can use sum even before its declaration statement function sum(a, b) {...}.
이렇게 하면, `array.reduce(sum)`은 `sum`을 `function sum(a,b) {...}` 선언문 이전에도 사용할 수 있습니다.

## 5. let variables lifecycle
## 5. let 변수 라이프사이클
let variables are processed differently than var. The main distinction is that declaration and initialization phases are split.
`let`변수는 `var`과 다르게 처리됩니다. 주요한 차이점은 바로 선언과 초기화 단계가 서로 분리되어 있다는 것이죠.

[img]
let statement variables lifecycle
`let` 선언 변후 라이프사이클

Now let's study a scenario when the interpreter enters a block scope that contains a let variable statement. Immediately the variable passes the declaration phase, registering its name in the scope (step 1). 
이제 인터프리터(interpreter)가 `let variable`선언문을 포함하는 블록 스코프(block scope)에 진입할 경우를 생각해 봅시다. 변수가 선언 단계를 거치자 마자, 해당 변수의 이름은 스코프에 등록합니다 (1단계).
Then interpreter continues parsing the block statements line by line.
그런 다음, 인터프리터는 블록을 한줄 한줄 계속 파싱해 나갑니다.

If you try to access variable at this stage, JavaScript will throw ReferenceError: variable is not defined. It happens because the variable state is uninitialized. 
만일 여러분이 이 단계에서 변수에 접근하려고 하면, 자바스크립트는 `ReferenceError: variable is not defined`에러를 발생합니다. 바로 변수 스테이트가 초기화되지 않았기 때문에 이런 현상이 나타납니다.
variable is in the temporal dead zone.
변수는 바로 템포럴 데드 존(temporal dead zone, TDZ; 역주: 임시 비할당 지역)에 있기 때문입니다. 

When interpreter reaches the statement let variable, the initilization phase is passed (step 2). Now the variable state is initialized and accessing it evaluates to undefined. 
인터프리터가 `let variable`선언문에 도달하면, 초기화 단계를 거칩니다(2단계). 이제 변수는 초기화 되었고 해당 변수에 접근하면 undefined로 평가됩니다.
The variable exits the temporal dead zone.
이제 변수는 TDZ(temporal dead zone)에서 나오게 됩니다.

Later when an assignment statement appears variable = 'value', the assignment phase is passed (step 3).
그 후에, `variable = 'value'`같은 할당문을 만나면, 할당 단계가 통과됩니다 (3단계).

If JavaScript encounters let variable = 'value', then initialization and assignment happen in a single statement.
만약 JavaScript가 `let variable = 'value'`를 만나게 되면, 초기화와 할당이 하나의 선언문에서 이루어지게 되는 것이죠.

Let's follow an example. let variable number is created in a block scope:
아래의 예제를 따라봅시다. `let`변수인 `number`은 블록 스코프에 만들어 졌습니다:

```
Try in JS Bin
let condition = true;  
if (condition) {  
  // console.log(number); // => Throws ReferenceError
  let number;
  console.log(number); // => undefined
  number = 5;
  console.log(number); // => 5
}
```
When JavaScript enters if (condition) {...} block scope, number instantly passes the declaration phase. 
JavaScript가 `if (condition) {...}`라는 블록 스코프에 들어가면, `number`는 그 즉시 선언 단계를 통과합니다.
Because number has unitialized state and is in a temporal dead zone, an attempt to access the variable throws ReferenceError: number is not defined. 
Later the statement let number makes the initialization. Now the variable can be accessed, but its value is undefined. 
The assignment statement number = 5 of course makes the assignment phase.

const and class types have the same lifecycle as let, other than the assignment can happen only once.

5.1 Why hoisting is not valid in let lifecycle
As mentioned above, hoisting is variable's coupled declaration and initialization at the top of the scope. 
let lifecycle however decouples declaration and initialization phases. Decoupling vanishes the hoisting term for let. 
The gap between the two phases creates the temporal dead zone, where the variable cannot be accessed.

In a sci-fi style, the collapsed hoisting in let lifecycle creates the temporal dead zone.

6. Conclusion
The freedom to declare variables using var is error prone. 
Based on this lesson, ES2015 introduces let. It uses an improved algorithm to declare variables and additionally is block scoped.

Because the declaration and initialization phases are decoupled, hoisting is not valid for a let variable (including for const and class). Before initialization, the variable is in temporal dead zone and is not accessible.

To keep the variables declaration smooth, these tips are recommended:

Declare, initialize and then use variables. This flow is correct and easy to follow.
Keep the variables as hidden as possible. The less variables are exposed, the more modular your code becomes.
That's all for today. See you in my next post.

What do you think about variables coding best practices? Feel free to write a comment below!
