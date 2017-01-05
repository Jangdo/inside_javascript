#03 자바스크립트 테이터 타입과 
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
