## TS_유니언과 리터럴

- 유니언: 값에 허용된 타입을 두 개 이상의 가능한 타입으로 확장하는 것
- 내로잉: 값에 허용된 타입이 하나 이상의 가능한 타입이 되지 않도록 좁히는 것

종합하자면 유니언과 내로잉은 다른 주요 프로그래밍 언어에서는 불가능하지만 타입스크립트에서는 가능한 '코드 정보에 입각한 추론'을 해내는 강력한 개념



### 유니언 타입

```typescript
let mathematician = Math.random() > 0.5
		? undefined
		: 'Mark Goldberg';
```

 mathematician은 어떤 타입일까?

둘 다 잠재적인 타입이긴 하지만 무조건 undefined이거나 혹은 무조건 string인 것도 아니다

mathematiciain은 undefined 이거나 string 일 수 있다. '이거 혹은 저거' 와 같은 타입을 **유니언** 이라고 한다.

유니언 타입은 값이 정확히 어떤 타입인지 모르지만 두 개 이상의 옵션 중 하나라는 것을 알고 있는 경우에 코드를 처리하는 훌륭한 개념입니다.

타입스크립트는 가능한 값 또는 구성 요소 사이에 |(수직선) 연산자를 사용해 유니언 타입을 나타냅니다. 앞에서 mathematician은 string | undefined 타입으로 간주된다. mathematician 변수 위로 마우스를 가져가면 밑에와 같이 타입이 string | undefined로 표시 됨

```typescript
let mathematician: string | undefined
let mathematician = Math.random() > 0.5
		? undefined
		: 'Mark Goldberg';
```



#### 유니언 타입 선언

변수의 초깃값이 있더라도 변수에 대한 명시적 타입 애너테이션을 제공하는 것이 유용할 때 유니언 타입을 사용합니다. 다음 예제에서 thinker의 초깃값은 null 이지만 잠재적으로 null 대신 string이 될 수 있음을 알려 준다. 명시적으로 string | null 타입 애너테이션은 타입스크립트가 thinker의 값으로 string 타입의 값을 할당할 수 있음을 의미한다.

```typescript
let thinker: string| null = null;

if(Math.random() > 0.5) {
	thinker = 'Susanne Langer' //ok
}
```

유니언 타입 선언은 타입 애너테이션으로 타입을 정의하는 모든 곳에서 사용할 수 있습니다.

> 유니언 타입 선언의 순서는 중요하지 않습니다. 타입스크립트에서는 boolean | number 나 
>
> number | boolean 모두 똑같이 취급합니다.



#### 유니언 속성

값이 유니언 타입일 때 타입스크립트는 유니언으로 선언한 모든 가능한 타입에 존재하는 멤버 속성에만 접근할 수 있다. 유니언 외의 타입에 접근하려고 하면 타입 검사 오류가 발생합니다.

다음 스니펫에서 physicist는 number | string 타입으로 두 개의 타입에 모두 존재하는 toString()은 사용할 수 있지만, toUpperCase() 와 toFixd() 는 사용할 수 없다.

```typescript
let phusicist = Math.random() > 0.5
		? 'Marie Curie'
		: 84;
physicist.toString(); //ok

physicist.toUpperCase(); //Error
physicist.toFixed() //Error
```

모든 유니언 타입에 존재하지 않는 속성에 대한 접근을 제한하는 것은 안전 조치에 해당합니다. 객체가 어떤 속성을 포함한 타입으로 학실하게 알려지지 않은 경우, 타입스크립트는 해당 속성을 사용하려고 시도하는 것이 안전하지 않다고 여김. 그런 속성이 존재하지 않을 수도 있으니까

유니언 타입으로 정의된 여러 타입 중 하나의 타입으로 된 값의 속성을 사용하려면 코드에서 값이 보다 구체적인 타입 중 하나라는 것을 타입스크립트에 알려야 한다. 이과정을 **내로잉**이라고 부름



### 내로잉

내로잉은 값이 정의, 선언 혹은 이전에 유추된 것 보다 더 구체적인 타입임을 코드에서 유추하는 것

타입스크립트가 값의 타입이 이전에 알려진 것보다 더 좁혀졌다는 것을 알게 되면 값을 더 구체적인 타입으로 취급

타입을 좁히는데 사용할 수 있는 논리적 검사를 **타입 가드**라고 함

타입스크립트가 코드에서 타입을 좁히는 데 흔히 사용하는 타입 가드는 두 가지가 있음



#### 값 할당을 통한 내로잉

변수에 값을 직접할당하면 타입스크립트는 변수의 타입을 할당된 값의 타입으로 좁힘

다음 admiral 변수는 초기에 number | string으로 선언했지만 'Grace Hopper' 값이 할당된 이후 타입스크립트는 admiral 변수가 string 타입이라는 것을 알게됨

```typescript
let admiral: number | string;
admiral = 'Grace Hopper';
admiral.toUpperCase(); //ok: string
admiral.toFixed(); //Error: 
```

변수에 유니언 타입 애너테이션이 명시되고 초깃값이 주어닐 때 값 할당 내로잉이 작동합니다.

타입스크립트는 변수가 나중에 유니언 타입으로 선언된 타입 중 하나의 값을 받을 수 있지만

처음에는 초기에 할당된 값의 타입으로 시작한다는 것을 이해합니다.

다음 코드에서 inventor는 number | string 타입으로 선언되었지만 초깃값으로 문자열이 할당되었기 때문에 타입스크립트는 즉시 string 타입으로 바로 좁혀졌다는 것을 알고 있습니다.

```typescript
let inventor: number | string = 'Hedy Lamarr';
inventor.toUpperCase(); //ok: string
inventor.toFixed(); //Error
```



#### 조건 검사를 통한 내로잉

일반적으로 타입스크립트에서는 변수가 알려진 값과 같은지 확인하는 if문을 통해 변수의 값을 좁히는 방법을 사용합니다.

타입스크립트는 if문 내에서 변수가 알려진 값과 동일한 타입인지 확인합니다.

```typescript
//scientist: number | string의 타입
let scientis = Math.random() > 0.5
 		? 'Rosalin franklin'
 		: 51;
 		
if (scientis === 'Rosalind Franklin') {
		// scientist: string의 타입
		scientis.toUpperCase(); //ok
}

//scientist: number|string의 타입
scientis.toUpperCase() //Error
```

조건부 로직으로 내로잉할 때, 타입스크립트 타입 검사 로직은 훌륭한 자바스크립트 코딩 패턴을 미러링해 구현함

만약 변수가 여러 타입 중 하나라면, 일반적으로 필요한 타입과 관련된 검사를 원할 것

타입스크립트는 강제로 코드를 안전하게 작성할 수 있도록 하는 고마운 언어

#### typeof 검사를 통한 내로잉

타입스크립트는 직접 값을 확인해 타입을 좁히기도 하지만, typeof 연산자를 사용할 수도 있다

scientist 예제와 유사하게 다음 if문에서 typeof resarcher가 'string'인지 확인해 타입스크립트에 resarcher의 타입이 string임을 나타냄



```typescript
let scientis = Math.random() > 0.5
 		? 'Rosalin franklin'
 		: 51;
 		
if (scientis === 'Rosalind Franklin') {
		// scientist: string의 타입
		scientis.toUpperCase(); //ok
}
```

!를 사용한 논리적 부정과 else 문도 잘 작동합니다.

```typescript
if (!(typeof researcher === 'string')) {
		researcher.toFixed(); // ok: number
} else {
		researcher.toUpperCase(); // ok: string
}
```

이러한 코드 스니펫은 타입 내로잉에서도 지원되는 삼항 연산자를 이용해 다시 작성할 수 있다.

```typescript
typeof researcher === 'string'
		? researcher.toUpperCase() //ok: string
		: researcher.toFixed(); //ok: number
```

어떤 방법으로 작성하든 typeof 검사는 타입을 좁히기 위해 자주 사용하는 실용적인 방법

타입스크립트의 타입 검사기는 이후 장에서 보게 될 더 많은 내로잉 형태를 인식



### 리터럴 타입

두 개 이상의 잠재적 타입이 될 수 있는 값을 다루기 위해 유니언 타입과 내로잉을 살펴봤으니 지금부터는

**리터럴 타입**을 소개하겠습니다. 리터럴 타입은 좀 더 구체적인 버전의 원시타입 입니다.

```typescript
const philosopher = 'Hypatia';
```

philosopher는 어떤 타입인가요? string 타입이지만

philosopher는 단지 string 타입이 아닌 'Hypatia'라는 특별한 값이다. 따라서 변수 philosopher의 타입은 기술적으로 더 구체적인 'Hypatia'이다.

이것이 바로 리터럴 타입의 개념이다. 원시 타입 값 중 어떤 것이 아닌 **특정 원싯값**으로 알려진 타입이 리터럴 타입이다.

원시 타입 string은 존재할 수 있는 모든 가능한 문자열의 집합을 나타냄 하지만 리터럴 타입인 'Hypatia'는 하나의 문자열만 나타냄

만약 변수 const로 선언하고 직접 리터럴 값을 할당하면 타입스크립트는 해당 변수를 할당된 리터럴 값으로 유추합니다. 따라서 VS Code 같은 IDE에서 초기 리터럴 값이 할당된 const 변수 위에 마우스를 가져가면 밑 처럼 표시됨

```typescript
const mathematician = 'Mark Goldberg'
// const mathematician = 'Mark Goldberg'

let mathematician = 'Mark Goldberg'
// let mathematician: string
```

각 원시 타입은 해당 타입이 가질 수 있는 가능한 모든 리터럴 값의 전체 조합으로 생각할 수 있다.

즉 원시타입은 해당 타입의 가능한 모든 리터럴 값의 집합이다.

boolean, null, undefined 타입 외의 number, string 과 같은 모든 원시 타입에는 무한한 수 의 리터럴 타입이 있다. 일반적인 타입스크립트 코드에서 발견할 수 있는 타입은 다음과 같다.

- boolean: true|false
- null 과 undefined: 둘 다 자기 자신, 즉, 오직 하나의 리터럴 값만 가짐
- number: 0 | 1| 2.. | 0.1| 0.2|...
- string: '' | 'a' | 'b' | 'c' | ... | 'aa' |'ab' | 'ac'| ..

유니언 타입 애너테이션에서는 리터럴과 원시 타입을 섞어서 사용할 수 있다.  예를 들어 lifespan은 number 타입이거나 선언된 'ongoing' 혹은 'uncertain' 값 중 하나로 나타낼 수 있다.

```typescript
let liftespan: number | 'ongoing' | "uncertain"
lifespan = 89; //ok
lifespan = 'ongoing'; //ok

lifespan = true; //Error
```



#### 리터럴 할당 가능성

앞서 number 와 string과 같은 서로 다른 원시 타입이 서로 할당되지 못한다는 것을 보았다.

마찬가지로 0과 1처럼 동일한 원시타입일지라도 서로 다른 리터럴 타입은 서로 할당할 수 없다.

specificallAda는 리터럴 타입 'Ada'로 선언했으므로 값에 'Ada'를 할당할 수 있지만, 'Byron'이나 string 타입 값은 할당할 수 없다.

```typescript
let specificallyAda = 'Ada';
specificallyAda = 'Ada' //ok
specificallyAda = 'Byron' //Error

let someString = ''; //타입 스트링

specificallyAda = someString; //Error: type
```

그러나 리터럴 타입 그 값이 해당하는 원시 타입에 할당할 수 있다. 모든 특정 리터럴 문자열은 여전히 string 타입이기 때문



```typescript
someString = ':)';
```

타입 ':)' 의 값 ':)' 는 앞서 string 타입으로 간주된 someString 변수에 할당



#### 엄격한 null 검사

리터럴로 좁혀진 유니언의 힘은 타입스크립트에서 엄격한 null 검사 라 부르는 타입 시스템 영역인 잠재적으로 정의되지 않은 undefined 값으로 작업할 때 특히 두드러진다.

타입스크립트는 두려운 십억 달러의 실수를 바로 잡기 위해 엄격한 null 검사를 사용하며 이는 최신 프로그래밍 언어의 큰 변화중 하나

십억 달러의 실수는 다른 타입이 필요한 위치에서 null 값을 사용하도록 허용하는 많은 타입 시스템을 가리키는 업계 용어

엄격한 null 검사가 없는 언어에서 다음 예제 코드처럼 string 타입 변수에 null을 할당하는 것이 허용

```typescript
const firstName: string = null
```

만약 여러분이 c++이나 자바 같은 십억달럭의 실수를 겪고 있는 타입 언어로 작업한 적이 있다면, 일부 언어에서 null 사용을 허용하지 않는 다는 것이 놀라울 것입니다. 반대로 이전에 엄격한 null 검사를 사용하는 언어로 작업한 적이 없다면, 일부 언어가 애초에 십억 달러의 실수를 허용했다는 사실이 놀라울 것이다.

타입스크립트 컴파일러는 실행바식을 변경할 수 있는 다양한 옵션을 제공

가장 유용한 옵션 중 하나인 strictNullChecks는 엄격한 null 검사를 활성화할지 여부를 결정합니다. 간략하게 설명하면 stricNullChecks를 비활성화면 코드의 모든타입 | null | undefined를 추가해야 모든 변수가 null 또는  undefined를 할당할 수 있다.

strictNullChecks 옵션을 false로 설정하면 다음 코드의 타입은 완벽히 안전하다고 간주된다.

하지만 틀렸다. nameMaybe 변수가 .`toLowerCase` 에 접근할 때 undefined가 되는 것은 잘못된 것

```typescript
let nameMaybe = Math.random() > 0.5
		? 'Tony Hoare'
		: undefined

nameMaybe.toLowerCase()
```

엄격한 null 검사가 활성화 되면 타입스크립트는 다음 코드에서 발생하게 될 잠재적인 충돌을 확인



```typescript
let nameMaybe =Math.random() > 0.5
		? "Tony Hoare"
		:undefined
		
nameMaybe.toLowerCase(); //Error: Object is possibly 'undefined'
```

엄격한 null 검사를 활성화해야만 코드가 null 또는  undefined 값으로 인한 오류로 부터 안전한지 여부를 쉽게 파악할 수 있다.

타입스크립트의 모법 사례는 일반적으로 엄격한 null 검사를 활성화하는 것 그렇게 해야만 충돌을 방지하고 십억 달러의 실수를 제거할 수 있다.



#### 참 검사를 통한 내로잉

자바스크립트에서 참 또는 truthy는 &&연산자 또는 if문처럼 boolean 문맥에서  true로 간주된다는 점을 떠올리자 자바스크립트에서 false, 0, -0n, null, undefined, NaN 처럼 falsy로 정의된 값을 제외한 모든 값은 모두 참입니다.

타입스크립트는 잠재적인 값 중 truthy로 확인된 일부에 한해서만 변수의 타입을 좁힐 수 있다.

geneticist는 string | undefined 타입이며 undefined는 항상 falsy이므로 타입스크립트는 if 문의 코드블록에서 geneticist가 string 타입이 되어야 한다고 추론할 수 있다.

```typescript
let geneticist = Math.random() > 0.5
		? 'barbara McClintock'
		: undefined;
		
if (geneticist) {
		geneticsist.toUpperCase(); //ok:string
}

geneticist.toUpperCase(); //Error
```

논리 연산자인 &&와 ? 는 참 여부를 검사하는 일도 잘 수행한다. 하지만 안타깝게도 참 여부 확인 외에 다른 기능은 제공하지 않는다. string | undefined 값에 대해 알고 있는 것이 falsy라면 그것이 빈 문자열인지 undefined인지는 알 수 없다.

```typescript
geneticist && geneticist.toUpperCase(); //ok: string | undefined
geneticist?.toUpperCase(); //ok: string | undefined
```

