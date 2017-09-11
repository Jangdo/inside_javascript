## 4.4 함수 호출과 this

함수의 기본적인 기능은 당연히 함수를 호출하여 코드를 실행하는 것이다.  
하지만 자바스크립트 언어 자체가 C/C++ 같은 엄격한 문법 체크를 하지 않는 자유로운 특성의 언어이므로 함수 호출 또한 다른 언어와는 달리 자유롭다.  

### 4.4.1 arguments 객체
C와 같은 엄격한 언어와 달리, 자바스크립트에서는 함수를 호출할때 함수 형식에 맞춰 인자를 넘기지 않더라도 에러가 발생하지 않는다.
```js
//함수 형식에 맞춰 인자를 넘기지 않더라도 함수 호출이 가능함을 나타내는 예제코드
function func(arg1, arg2){
    console.log(arg1, arg2);
}

func();          //출력값 undefined undefined
func(1);         //출력값 1 undefined
func(1, 2);      //출력값 1 2
func(1, 2, 3);   //출력값 1 2
```
위 예제에서 func(), func(1) 호출처럼 정의된 함수의 인자보다 적게 함수를 호출했을 경우, 넘겨지지 않은 인자에는 **undefined** 값이 할당된다.  
이와 반대로 정의된 인자 개수보다 많게 함수를 호출했을 경우는 에러가 발생하지 않고,  
초과된 인수는 무시된다.

자바스크립트의 이러한 특성 때문에 함수 코드를 작성할 때, 런타임 시에 호출된 인자의 개수를 확인하고 이에 따라 동작을 다르게 해줘야 할 경우가 있다.  
이를 가능케 하는 게 바로 **arguments 객체** 다.  

자바스크립트에서는 함수를 호출할 때 인수들과 함계 암묵적으로 arguments 객체가 함수 내부로 전달되기 때문이다.  

arguments 객체는 함수를 호출할 때 넘긴 인자들이 배열 형태로 저장된 객체를 의미한다.  
특이한 점은 이 객체는 실제 배열이 아닌 **유사 배열 객체** 라는 점이다.

```js
//arguments 겍체 예제 코드
// add() 함수
function add(a, b){
    //arguments 객체 출력
    console.dir(arguments);
    return a+b;
}
console.log(add(1));    //출력값 NaN
console.log(add(1, 2)); //출력값 3
console.log(add(1, 2, 3)); //출력값 3
```
- 함수를 호출할 때 넘겨진 인자 (배열 형태) : 함수를 호출할 때 첫 번째 인자는 0번 인덱스, 두 번째 인자는 1번 인덱스, ...
- length 프로퍼티 : 호출할 때 넘겨진 인자의 개수를 의미
- callee 프로퍼티 : 현재 실행 중인 함수의 참조값

앞서 얘기했듯이 arguments는 객체이지 배열이 아니다.  
즉, length 프로퍼티가 있으므로 배열과 유사하게 동작하지만, 배열은 아니므로 배열 메서드를 사용할 경우 에러가 발생한다는 것에 주의해야 한다.  
물론 유사 배열 객체에서 배열 메서드를 사용하는 방법이 있다.  
**call과 apply 메서드를 이용한 명시적인 this 바인딩**

arguments 객체는 매개변수 개수가 정확하게 정해지지 않은 함수를 구현하거나, 전달된 인자의 개수에 따라 서로 다른 처리를 해줘야 하는 함수를 개발하는 데 유용하게 사용할 수 있다.
```js
function sum(){
    var result = 0;

    for(var i = 0; i < arguments.length; i++){
        result += arguments[i];
    }

    return result;
}

console.log(sum(1, 2, 3));                    //출력값 6
console.log(sum(1, 2, 3, 4, 5, 6, 7, 8, 9));  //출력값 45
```

### 4.4.2 호출 패턴과 this 바인딩
#### 4.4.2.1 객체의 메서드 호출할 때 this 바인딩
객체의 프로퍼티가 함수일 경우, 이 함수를 메서드라고 부른다.  
이러한 메서드를 호출할 때, 메서드 내부 코드에서 사용된 this는 **해당 메서드를 호출한 객체로 바인딩** 된다.
```js
//메서드 호출 사용 시 this 바인딩
//myObject 객체 생성
var myObject = {
    name : 'foo',
    sayName : function(){
        console.log(this.name);
    }
};

//otherObject 객체 생성
var otherObject = {
    name : 'bar'
};

otherObject.sayName = myObject.sayName;

myObject.sayName();        //출력값 foo
otherObject.sayName();     //출력값 bar
```
#### 4.4.2.2 함수를 호출할 때 this 바인딩
자바스크립트에서는 함수를 호출하면, 해당 함수 내부 코드에서 사용된 **this는 전역 객체에 바인딩** 된다.  
브라우저에서 자바스크립트를 실행하는 경우 전역 객체는 **window 객체** 가 된다.  

자바스크립트의 모든 전역 변수는 실제로는 이러한 전역 객체의 프로퍼티들이다.
```js
var foo = "I'm foo";

console.log(foo);           //출력값 I'm foo
console.log(window.foo);    //출력값 I'm foo
```
따라서 전역 변수는 전역 객체(window)의 프로퍼티로도 접근할 수가 있다.
```js
//함수를 호출할 때 this 바인딩을 보여주는 예제 코드
var test = 'This is test';
console.log(window.test);       //출력값 This is test
//sayFoo() 함수
var sayFoo = function(){
    console.log(this.test);     //sayFoo() 함수 호출 시 this는 전역 객체에 바인딩된다.
};
sayFoo();                       //출력값 This is test
```
이러한 함수 호출에서의 this 바인딩 특성은 **내부 함수를 호출했을 경우** 에도 그대로 적용되므로, 내부 함수에서 this를 이용할 때는 주의해야 한다.
```js
//내부 함수의 this 바인딩 동작을 보여주는 예제 코드
//전역 변수 value 정의
var value = 100;

//myObject 객체 생성
var myObject = {
    value : 1,
    func1 : function(){
        this.value += 1;
        console.log(this.value);
        func2 = function(){
            this.value += 1;
            console.log(this.value);
            func3 = function(){
                this.value += 1;
                console.log(this.value);
            }
            func3();
        }
        func2();
    }
};
myObject.func1();
/*
출력값
2
101
102
*/
```
이렇게 실행결과가 예측했던 것과 다르게 출력된 이유는 자바스크립트에서는 **내부 함수 호출 패턴을 정의해 놓지 않기 때문이다.**  
내부 함수도 결국 함수이므로 이를 호출할 때는 함수 호출로 취급된다. 따라서 함수 호출 패턴 규칙에 따라 내부 함수의 this는 전역 객체 (window)에 바인딩된다.  

이렇게 내부 함수가 this를 참조하는 자바스크립트의 한계를 극복하려면 **부모 함수의 this** 를 내부 함수가 접근 가능한 **다른 변수에 저장** 하는 방법이 사용된다.  
보통 관례상 this 값을 저장하는 변수의 이름을 that이라고 짓는다.  
이렇게 되면 내부 함수에서는 that 변수로 부모 함수의 this가 가리키는 객체에 접근할 수 있다.
```js
//내부 함수의 this 바인딩 문제를 해결한 예제 코드
//내부 함수 this 바인딩
var value = 100;

//myObject 객체 생성
var myObject = {
    value : 1,
    func1 : function(){
        //this를 that 변수에 저장
        var that = this;
        this.value += 1;
        console.log(this.value);
        func2 = function(){
            that.value += 1;
            console.log(that.value);
            func3 = function(){
                that.value += 1;
                console.log(that.value);
            }
            func3();
        }
        func2();
    }
};
myObject.func1();
/*
출력값
2
3
4
*/
```
자바스크립트에서는 이와 같은 this 바인딩의 한계를 극복하려고, this 바인딩을 명시적으로 할 수 있도록 call과 apply 메서드를 제공한다.  
게다가, jQuery, underscore.js 등과 같은 자바스크립트 라이브러리들의 경우 bind라는 이름의 메서드를 통해, 사용자가 원하는 객체를 this에 바인딩할 수 있게 기능을 제공하고 있다.
