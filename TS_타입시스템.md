## 타입 시스템

### 타입의 종류

타입은 자바스크립트에서 다루는 값의 형태에 대한 설명

여기서 형태란 값에 존재하는 속성과 메서드 그리고 내장되어 있는 typeof 연산자가 설명하는 것을 의미

타입스크립트의 가장 기본적인 타입은 자바스크립트의 일곱 가지 기본 원시 타입과 동일함

null, undeinfed, boolean, string, number,bigint, symbol



#### 타입 시스템

프로그래밍 언어가 프로그램에서 가질 수 있는 타입을 이해하는 방법에 대한 규칙 집합입니다

1. 코드를 읽고 존재하는 모든 타입과 값을 이해합니다.
2. 각 값이 초기 선언에서 가질 수 있는 타입을 확인합니다.
3. 각 값이 추후 코드에서 어떻게 사용될 수 있는지 모든 방법을 확인합니다.
4. 값의 사용법이 타입과 일치하지 않으면 사용자에게 오류를 표시합니다.

```typescript
let firstName = 'whitney';
firstName.length();
// Error
// Type 'number'
```

1. 코드를 읽고 firstName 이라는 변수를 이해합니다.
2. 초깃값이 'whitney' 이므로 firstName 이 string 타입이라고 결론 짓습니다.
3. firstName의 .length 멤버를 함수처럼 호출하는 코드를 확인합니다.
4. string의 .length 멤버는 함수가 아닌 숫자라는 오류를 표시합니다. **즉, 함수처럼 호출할 수 없습니다.**

타입스크립트의 타입 시스템에 대한 이해는 타입스크립트 코드를 이해하는데 중요한 기술입니다. 



#### 오류 종류

타입스크립트를 작성하는 동안 가장 자주 접하게 되는 오류 두 가지는 다음과 같다

- 구문 오류: 타입스크립트가 자바스크립트로 변환되는 것을 차단한 경우
- 타입 오류: 타입 검사기에 따라 일치하는 않는 것이 감지된 경우



#### 구문오류

타입스크립트가 코드로 이해할 수 없는 잘못된 구문을 감지할 때 발생

이는 타입스크립트가 타입스크립트 파일에서 자바스크립트 파일을 올바르게 생성할 수 없도록 차단함

```typescript
let let wat;
//error
```



#### 타입 오류

타입 오류는 타입스크립트의 타입 검사기가 프로그램의 타입에서 오류를 감지했을 대 발생

오류가 발생했다고 해서 타입스크립트 구문이 자바스크립트로 변환되는 것을 차단하지는 않음 하지만 코드가 실행되면 무언가 충돌하거나 예기치 않게 작동할 수 있음을 나타냅니다.

```typescript
console.blub("nothing is worth more than laughter")
//Error: property 'blub' does not exist on type 'Console'
```

자바스크립트 코드가 원하는대로 실행되지 않을 가능성이 있다는 신호를 타입 오류로 알려줍니다. 자바스크립트를 실행하기 전에 타입 오류를 확인하고 발견된 문제를 먼저 해결하는 것이 가장 좋다.

대부분의 프로젝트는 tsconfig.json 파일을 사용해 오류가 수정될 때 까지 코드 실행을 차단하는 것을 안 한다



### 할당 가능성

타입스크립트는 변수의 초깃값을 읽고 해당 변수가 허용되는 타입을 결정 나중에 해당 변수에 새로운 값이 할당되면, 새롭게 할당된 값의 타입이 변수의 타입과 동일한지 확인

타입스크립트 변수에 동일한 타입의 다른 값이 할당될 때는 문제가 없음

예를 들어 변수가 처음에 string 값이면 나중에 다른 string 값을 할당하는 것은 문제가 되지 않는다.

```typescript
let firstName = 'Carole'
firstName = 'Joan';
```

하지만 타입스크립트 변수에 다른 타입의 값이 할당되면 타입 오류가 발생 예를 들어 처음에는 string 값으로 변수를 선언한 다음 나중에 boolean 값을 넣을 수 없다.

```typescript
let lastName = 'king';
lastName = true;
//Error
```

타입스크립트에서 함수 호출이나 변수에 값을 제공할 수 있는지 여부를 확인하는 것을 **할당 가능성**이라고 합니다.

즉 전달된 값이 예상된 타입으로 할당 가능한지 여부를 확인합니다.



#### 할당 가능성 오류 이해하기

'Type...is not assignable to type...' 형태의 오류는 타입스크립트 코드를 작성할 때 만나게 되는 가장 일반적인 오류 중 하나

해당 오류 메시지에서 언급된 첫 번째 type은 코드에서 변수에 할당하려고 시도하는 값

두 번째는 type은 첫 번째 즉, 값이 할당되는 변수입니다. 예를 들어 이전 코드 스니펫에서 lastName = true를 작성할 때 boolean 타입인 true 값을 string 타입인 변수 lastName에 할당하려고 했습니다.



#### 타입 애너테이션

때로는 변수에 타입스크립트가 읽어야 할 초깃값이 없는 경우도 있다.

타입스크립트는 나중에 사용할 변수의 초기 타입을 파악하려고 시도하지 않음

그리고 기본적으로 변수를 암묵적인 any타입으로 간주한다. 즉 변수는 세상의 모든 것이 될 수 있음을 나타냄

초기 타입을 유추할 수 없는 변수는 **진화하는 any** 라고 부름 특정 타입을 강제하는 대신 새로운 값이 할당될 때 마다

변수타입에 대한 이해를 발전시킴

```typescript
let rocker; // 타입: any
rocker = 'joan jett' // 타입: string
rocker.toUpPerCase(); //ok

rocker = 19.58// 타입: number
rocker.toPrecision(1); //ok

rocker.toUpperCase();
//error
```

코드를 보면 진화하는 any 변수인 rocker 에는 처음에 문자열이 할당되는데 이는 toUpperCase() 같은 string 메서드를 갖는 것을 의미하지만 그다음에는 number 타입으로 진화되는 것을 확인할 수 있음

타입스크립트는 number 타입으로 진화한 변수가 toUpperCase() 메서드를 호출하는 것을 포착 그러나 변수가 string 타입에서 number 타입으로 진화된 것이 처음부터 의도된 것인지에 대한 여부는 더 일찍 알 수 없다.

일반적으로 any타입을 사용해 any타입으로 진화하는 것을 허용하게 되면 타입스크립트의 타입 검사 목적을 부분적으로 쓸모 없게 만듭니다. 타입스크립트는 값이 어떤 타입인지 알고 있을 때 가장 잘 작동합니다. any 타입을 가진 값에는 타입스크립트의 타임 검사 기능을 잘 적용할 수 없다.

검사를 위해 알려진 타입이 없기 때문

타입스크립트는 초기값을 할당하지 않고도 변수의 타입을 선언할 수 있는 구문인 **타입 애너테이션** 제공

타입 애너테이션은 변수 이름뒤에 배치되며 콜론(:) 과 타입 이름을 차례대로 기재

```typescript
let rocker: string;
rocker = 'Joan Jett'
```



이러한 타입 애너테이션은 타입스크립트에만 존재하며 런타임 코드에 영향을 주지도 않고, 유효한 자바스크립트 구문도 아님

tsc 명령어를 실행해 타입스크립트 소스 코드를 자브스크립트로 컴파일하면 해당 코드가 삭제됨

```typescript
//출력된 js 파일
let rocker;
rocker = 'Joan Jett'
```

변수에 타입 애너테이션으로 정의한 타입 외의 값을 할당하면 타입 오류가 발생함

다음 코드는 string 타입으로 선언된 rocker 변수에 숫자를 할당해 타입 오류가 발생한 상황

```typescript
let rocker: string;
rocker = 19.58;
//Error
```

> 타입스크립트 타입은 컴파일을 통해 생성된 자바스크립트에 어떠한 영향도 주지 않는다.



#### 불필요한 타입 애너테이션

타입 애너테이션은 타입스크립트가 자체적으로 수집할 수 없는 정보를 타입스크립트에 제공할 수 있습니다.

타입을 즉시 유추할 수 있는 변수에도 타입 애너테이션을 사용할 수 있다.

하지만 타입스크립트가 아직 알지 못하는 것은 알려주지 못한다.

```typescript
let firstName: string = 'Tina' //타입 시스템은 변경되지 않음
```

string 타입 애너테이션은 중복입니다. 타입스크립트가 이미 firstName이 string 타입임은 유추할 수 있기 때문

초기값이 있는 변수에 타입 애너테이션을 추가하면 타입스크립트는 변수에 할당된 값의 타입이 일치하는지 확인한다.



```typescript
let firstName: string = 42;
//error
```

firstName은 string 타입으로 선언되었지만, number 값인 42로 초기화 되었다. 그렇게 되면 타입스크립트가

호환되지 않는다는 것을 보여준다.

많은 개발자는 아무것도 변하지 않는 변수에는 타입 애너테이션을 추가하지 않기를 선호함

타입 애너테이션을 수동으로 작성하는 일은 번거롭다. 특히 타입이 변경되거나 복잡한 타입일 때 더욱 그렇다.

코드를 명확하게 문서화하거나 실수롤 변수 타입이 변경되지 않도록 타입스크립트를 보호하기 위해 변수에 명시적으로 타입 애너테이션을 포함하는 것이 경우에 따라서는 유용할 수 있음



### 타입 형태

타입스크립트는 변수에 할당된 값이 원래 타입과 일치하는지 확인하는 것 이상을 수행한다.

타입스크립트는 객체에 어떤 멤버 속성이 존재하는지 알고 있다. 만약 코드에서 변수의 속성에 접근하려고 한다면

타입스크립트는 접근하려는 속성이 해당 변수의 타입에 존재하는지 확인한다.

string 타입의 rapper 변수를 선언한다고 가정시 rapper변수를 사용할 때 타입스크립트가 string 타입에서 사용가능한 작업만을 허용한다.

```typescript
let rapper = 'Queen atifah';
rapper.length //ok
```



타입스크립트가 string 타입에서 작동하는지 알 수 없는 작업은 허용되지 않는다.

```typescript
rapper.push('!');
//Error: Property
```

타입은 더 복잡한 형태, 특히 객체일 수도 있다. 다음 스니펫에서 타입스크립트는 cher 객체에 middleName 키가 없다는 것을 알고 오류를 표시함

```typescript
let cher = {
	firsName = 'Cherilyn',
	lastNmae: 'sarkisian',
}

cher.middleName;
//Error:property
```

타입스크립트는 객체의 형태에 대한 이해를 바탕으로 할당 가능성뿐만 아니라 객체 사용과 관련된 문제도 알려준다.



### 모듈

자바스크립트는 비교적 최근까지 서로 다른 파일에 작성된 코드를 공유하는 방법과 관련된 사양을 제공하지 않았다. ECMA스크립트 2015에는 파일 간에 가져오고 내보내는 구문을 표준화하기 위해 ECMA스크립트 모듈(ESM) 추가되였다.

참고로 다음 모듈 파일은 ./values 파일에서 value를 가져오고 변수 doubled를 내보냅니다.

```typescript
import {value} from './values';
export const doubled = value * 2;
```



ECMA 스크립트 사양과 일치 시키기 위해 다음 명명법을 사용

모듈: export 또는 import가 있는 파일

스크립트: 모듈이 아닌 모든 파일

타입스크립트는 최신 모듈 파일을 기존 파일과 함께 실행할 수 있다. 모듈 파일에 선언된 모든 것은 해당 파일에서 명시한 export 문에서 내보내지 않는 한 모듈 파일에서만 사용할 수 있다. 한 모듈에서 다른 파일에 선언된 변수와 동일한 이름으로 선언된 변수는 다른 파일의 변수를 가져오지 않는 한 이름 충돌로 간주하지 않는다.

다음은 a.ts 와 b.ts 파일 모두 모듈이고 이름이 동일한 shared 변수를 문제없이 내보내는 코드

```typescript
//a.ts
export const shared = 'Cher';
```

```typescript
//b.ts
export const shared = 'Cher';
```



```typescript
//c.ts
import {shared} from './a' //Error: import declaration

export const shared = 'Cher'; //Error: Individual
```

그러나 파일이 스크립트면 타입스크립트는 해당 파일을 전역 스코프로 간주하므로 모든 스크립트가 파일의 내용에 접근할 수 있다. 즉 스크립트파일에 선언된 변수는 다른 스크립트 파일에 선언된 변수와 동일한 이름을 가질 수 없다.

다음 a.ts와 b.ts 파일은 모듈 스타일의 export 또는 import 문이 없기 때문에 일반 스크립트로 간주된다. 따라서 동일한 이름의 변수가 동일한 파일에 선언된 것처럼 서로 충돌합니다.

```typescript
//a.ts
const shared = 'Cher'; //Error
```

```typescript
//b.ts
const shared = 'Cher' //Error
```

타입스크립트 파일에 Cannot redeclare... 라는 오류가 표시되면 파일에 아직 export 또는 import 문을 추가하지 않았기 때문일 수 있다. ECMA 스크립트 사양에 따라 export 또는 import 문 없이 파일을 모듈로 만들어야 한다면 파일의 아무 곳에나 export{}를 축가해 강제로 모듈이 되도록 만든다.

```typescript
//a.ts and b.ts
const shared = 'Cher' 
export {};
```

타입스크립트는 CommonJs 와 같은 이전 모듈을 사용해서 작성된 타입스크립트 파일의 import, export 형태는 인식하지 못합니다. 타입스크립트는 일반적으로 CommonJs 스타일의 require 함수에서 반환된 값을 any타입으로 인식합니다.



