#03 자바스크립트 테이터 타입과 연산자
##3.1 자바스크립트 기본타입
숫자, 문자열, boolean, null, undefined
변수에는 어떤 타입의 데이타라도 저장이 가능 <br>
따라서, 변수에 어떤 형태의 데이터를 저장하느냐에 따라 해당 변수의 타입이 결정된다.

<pre>
예제

var intNum = 10;
console.log(typeof intNum);
</pre>
###null 과 undefined
<pre>
var nullVar = null;
console.log(typeof nullVar === null);
console.log(nullVar === null);
</pre>
null 의 타입은 object 이다.
undefined 는 타입이자 값을 나타낸다.

##3.2 자바스크립트 참조타입(객체타입)
기본타입을 제외한 모든 값은 객체<br>
연산 역시 참조에 의한 연산
##3.3 객체생성
Object 생성방법, 객체 리터널 생성방법, 생성자 함수 생성방법 <br>
생성된 객체의 프로퍼티 접근 - . 연산자 및 ['property name'] 으로 접근 <br>
객체 프로퍼티는 읽기/쓰기/갱신이 가능
##3.4 객체연산
객체의 모든연산은 실제 값이 아닌 참조값으로 처리된다.
<pre>
var objA = {
  var : 40
}
var objB = objA;
console.log(objA, objB);
objB.val = 50;
console.log(objA, objB);
</pre>

##3.5 참조에 의한 함수 호출 방식
기본타입의 호출 방식 : 값에 의한 호출(call by value)<br>
참조타입의 호출 방식 : 참조에 의한 호출(call by reference)
<pre>
var a= 100;
  var objA = {value:100};
  function bbb(num, obj){
    num = 200;
    obj.value = 200;
    console.log(num);
    console.log(obj);
  };
  bbb(a, objA);
  console.log(a);
  console.log(objA);
</pre>
### call by value
기본타입의 경우 함수를 호출할 때 인자로 기본 타입의 값을 넘길 경우, 호출된 함수의 매개변수로 <strong style="color:#ff0000">복사된 값</strong>이 전달된다.<br>
