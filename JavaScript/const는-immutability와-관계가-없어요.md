# ES6 const는 불변성(immutability)과 관계 없어요

이건 절대 없어지지 않을, 굉장히 흔한 오해중의 하나입니다. 제가 블로그랑 트위터, 그리고 심지어 책에서도 계속 이야기 하고 있거든요. 이걸 바로잡기 위한 시도를 다시 해보도록 하겠습니다.

## `const`는 불변적 바인딩(immutable binding)을 합니다

ES6는 변수의 값이 '상수'이거나 불변적이라고 말하지 **않습니다**. `const`값은 확실히 변할 수 있어요. 아래의 코드는 예외 처리가 되지 않습니다:
```
const foo = {};
foo.bar = 42;
console.log(foo.bar);
// → 42
```
여기에서 오직 불변적인 것은 바로 바인딩(binding)입니다. `const`는 값(`{}`)을 변수명(`foo`)에 할당하고, 새로운 바인딩이 일어나지 않도록 합니다. 따라서 [할당 연산자(assignment operator)](https://tc39.github.io/ecma262/#sec-assignment-operators), [단항연산(unary)](https://tc39.github.io/ecma262/#sec-unary-operators), [postfix](https://tc39.github.io/ecma262/#sec-postfix-increment-operator)의 `--`또는 `++`연산자를 `const`에 사용하게 되면 `TypeError`를 던지게 되죠:

```
const foo = 27;
// Any of the following uncommented lines throws an exception.
// 코멘트 되지 않은 코드들은 모두 예외처리 됩니다.
// Assignment operators:
// 할당 연산자:
foo = 42;
foo *= 42;
foo /= 42;
foo %= 42;
foo += 42;
foo -= 42;
foo <<= 0b101010;
foo >>= 0b101010;
foo >>>= 0b101010;
foo &= 0b101010;
foo ^= 0b101010;
foo |= 0b101010;
// Unary `--` and `++`:
--foo;
++foo;
// Postfix `--` and `++`:
foo--;
foo++;
```
ES6의 `const`는 값의 불변성(immutability)과는 아무런 관계가 없지요.

## 그럼, 값을 불변적으로 만드는 방법은 무엇일까요?

숫자, 문자열, 불린, 심볼, null, undefined과 같은 원시 자료형의 값들은 언제나 불변적입니다.

```
var foo = 27;
foo.bar = 42;
console.log(foo.bar);
// → `undefined`
```
객체의 값을 불변적으로 만들기 위해서는 [`Object.freeze()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)를 사용하세요. ES5에서부터 존재했었고, 요즘에는 쉽게 사용할 수 있습니다.

```
const foo = Object.freeze({
	'bar': 27
});
foo.bar = 42; // throws a TypeError exception in strict mode;
              // silently fails in sloppy mode
console.log(foo.bar);
// → 27
```
`Object.freeze()`는 얕다는 점에 유의하세요. 얼어버린(frozen, 중첩된 객체) 객체 내부의 객체 값 또한 변경될 수 있습니다. MDN의 [`Object.freeze()`항목](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)에서는 `deepFreeze()`라는 예시를 통해, 객체의 값을 완벽하게 불변적으로 만드는 구현 방법을 설명하고 있죠.

여전히, `Object.freeze()` 는 속성-값 쌍에서만 먹힙니다. 지금 현재로써는 `Date`, `Map`, `Set`과 같은 객체들을 완전히 불변적으로 만드는 방법은 없죠.

뭐, 미래 ECMAScript에 불변적인 자료 구조를 추가하자는 [제안](https://github.com/sebmarkbage/ecmascript-immutable-data-structures)도 있습니다.

## `const` vs `let`

`const`와 `let`의 유일한 차이점은 바로 `const`가 리바인딩(rebinding)을 하지 않을 것이라는 일종의 계약(?)을 한다는 것 입니다.

지금까지 여기에 쓴 것들은 모두 팩트(facts)인데요, 다음으로 이어질 내용은 전적으로 제 주관적인 생각임을 감안해주세요.

위의 내용을 고려했을때, `const`는 코드를 더 쉽게 읽을 수 있도록 한다는 것을 알 수 있습니다. 같은 스코프(scope) 내에서는 `const`변수는 언제나 같은 객체를 참조하지만 `let`은 그럴 것이라는 보장이 없습니다. 그러므로, ES6에서는 다음의 원칙을 따르며 `const`와 `let`을 사용하는 것이 합당합니다:

- 기본적으로 `const`를 사용하세요
- 리바인딩(rebinding)이 필요할때만 `let`을 사용하세요
- (ES6에서는 `var`를 사용하시면 안됩니다)

제 생각에 동의하시나요? 어째서죠? 특히나 저는 `const`보다 `let`을 더 선호하시는(심지어 리바인딩되지 않는 변수에도) 개발자들의 이야기에 관심있습니다. 리바인딩없이 `let`을 쓴다면 애초에 `let`을 왜 쓰는거죠? 이건 "`const`는 constant(상수)를 위한거야"라는 오해 때문인지, 아니면 다른 이유가 있는 건가요? 코멘트에 의견을 달아주세요!

** 원글 : https://mathiasbynens.be/notes/es6-const
