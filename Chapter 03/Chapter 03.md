# Chapter 03

## 자바스크립트 3. 데이터타입, data types, let vs var, hoisting | 프론트엔드 개발자 입문편 (JavaScript ES5+)

1. Variable(변수)<br>
   변경될 수 있는 값을 변수라고 한다.<br>

   1. let(added in ES6 / read & write)

   ```
   let name = 'ellie';
   console.log(name);
   name = 'hello';
   console.log(name);

   result ->
   ellie
   hello
   ```

   우리가 let이라는 키워드를 통해 name을 정의하게 되면 메모리의 한 공간을 가리킬 수 있는 포인터가 생기게 된다<br>따라서 메모리 어딘가에 있는 한 공간에 name값을 ellie로 저장할 수 있게 된다.<br>또한 name이라는 공간에 다른 값을 넣어서 저장할 수도 있게 된다.<br><br>

   ```
   let globalName = 'global name';
   {
   let name = 'ellie';
   console.log(name);
   name = 'hello';
   console.log(name);
   console.log(globalName);
   }
   console.log(name);
   console.log(globalName);

   result ->
   ellie
   hello
   global name

   global name
   ```

   {}을 Block Scope라고 하는데 블럭 안에서 변수 선언을 하게 되면 밖에서는 안을 더 이상 볼 수 없게된다.<br>즉, 밖에서 콘솔로그를 찍어봐도 안에 있는 값이 나오지 않게된다.<br>반대로 파일안에다가 변수를 정의하는 것을 global scope라고 하는데 이것은 어느곳에서나 보이기 때문에 블럭 안이나 바깥에서 콘솔로그를 찍어보면 다 보이게 된다.<br>단, `글로벌 변수`는 시작부터 끝까지 항상 메모리에 탑재되어있기 때문에 항상 최소한으로 사용하는 것이 좋다.<br><br>

   2. var(ES6 이전 / don't ever use this!)

   ```
   console.log(age);
   age = 4;
   console.log(age);
   var age;

   age = 4;
   let age;

   {
   age = 4;
   var age;
   }
   console.log(age);

   undefined
   4
   error code
   4
   ```

   var는 ES6이전에 사용되는 것으로 값을 할당하기도 전에 위의 코드처럼 사용하는 것이 가능하다.<br>이것은 굉장한 위험을 발생하는데 var hoisting이라고도 한다.<br>`호이스팅`이 무엇인가? => `어디에 선언했느냐에 상관없이 항상 제일 먼저 위로 선언을 끌어올려주는 것을 말함`<br>따라서 제일 위에서 콘솔로그로 age를 출력하게 되면 값이 할당되지 않았지만 선언이 되어있기 때문에 undefined로 나오게 됩니다.<br>이렇기 때문에 var를 사용하면 안되지만 또 사용하면 안되는 이유 중 하나는 var는 block scope를 철저히 무시한다.<br>따라서 밖에서 console.log를 찍어보면 `4`라는 결과값이 찍히게 된다.<br>이것이 왜 위험을 초래하냐면 작은 프로젝트에서는 간간히 사용가능하지만 큰 프로젝트에서는 선언하지도 않았는데 할당되어 있어 오류를 찾기 힘든 경우가 발생하기 때문이다.<br>

   3. Constant(read)<br>

   ```
   // favor immutabel data types always for a few reasons:
   // - security
   // - thread safety
   // - reduce human mistakes

   const daysInWeek = 7;
   const maxNumber = 5;
   ```

   Constants는 값이 한 번 할당되면 절대 바뀌지 않는 것으로 메모리를 가리키는 포인터가 let과 다르게 잠겨있다.<br>한 번 설정되면 바꿀 수 없기 때문에 좋은 점은 `보안상의 이유` + `프로세스 안의 여러 개의 스레드에서 동시에 변수가 변경이 가능한데 동시에 값을 변경한다는 것은 위험하다.` + `변경되어야 할 이유가 없다면 const를 사용해서 실수를 방지할 수 있다.`<br>

   결론 -> 변경될 수 있는 `let`과 변경이 불가능한 `const`외에는 사용하지 말자!<br>

2. Variable Types<br>

   ```
   // Primitive, single item: number, string, boolean, null, undefined, symbol(가장 작은 단위)
   // Object, box container(single itme들을 묶어서 한 단위로 표현)
   // Function, first-class function(first-class function이란 말은 function도 다른 데이터타입처럼 변수 할당이 가능하고, 그렇기 때문에 함수의 인자로도 사용가능하며 함수의 return 타입으로 function이 오기도 한다.)

   single item: null
   let nothing = null;

   single item: undefined;
   let x;
   결론: null = "공익" / undefined = "미필"

   single item: symbol
   const symbol1 = Symbol("id");
   const symbol2 = Symbol("id");
   console.log(symbol1 === symbol2);

   false
   고유한 식별자를 만들 때 사용함

   Object
   const ellie = {name: 'ellie', age: '20'};
   ellie.age = 21;
   객체 자체는 const로 지정되어 있기 때문에 변경이 불가하지만 안의 키들을 참조가 가능하다.

   single item의 자세한 내용들은 다 아는 내용이므로 생략하도록 한다.
   ```

3. Dynamic Typing(dynamic typed language)<br>
   C나 Java 같은 언어는 Statically Type으로 변수를 선언할 때 어떤 타입인지 명시해줘야 하는 것을 말한다.<br>반면에 JavaSciprt는 선언할 때 어떤 타입인지 선언하지 않고 `Run Time`(프로그래밍이 동작할 때 할당된 값에 따라서 타입이 변경될 수 있다는 것을 말함)이다.<br>따라서 JavaScript는 런타임에서 변수 타입이 정해지기 때문에 에러 역시 런타임으로 발생하게 된다.<br>이 때문에 JavaScript를 보완하기 위해 나온 것이 `TypeScript`로 JavaScript 위에 Type이 더해진 방식이다.<br>그렇다면 자바스크립트가 아닌 타입스크립트를 배워야 하는 것이 아닌가 하는 의문이 들 수 있는데 우리는 자바스크립트를 먼저 배우는 것을 추천한다.<br>

   > 1. 타입스크립트는 최신 버전의 자바스크립트로 런타임으로 작동하는 것을 보기 위해서는 Babel을 통해 ES6이하로 변환해야하는 번거로움이 있다<br>
   > 2. 반대로 자바스크립트는 브라우저가 잘 이해할 수 있는 언어이기 때문에 런타임으로 실행되는 것을 바로바로 볼 수가 있기 때문에 현재로서는 개발하는데 자바스크립트를 먼저 배우는 것을 추천한다.

이 글은 [유튜브 드림코딩 by 엘리 채널](https://www.youtube.com/watch?v=OCCpGh4ujb8&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=3)을 통해 리뷰를 작성한 것이며 어떠한 상업적 목적으로도 사용되지 않았습니다. 추후 문제가 되는 점을 발견하시면 댓글을 통해 남겨주시는대로 수정하겠습니다 :)
