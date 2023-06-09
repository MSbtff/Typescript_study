## TS_배열2



### 배열 멤버

타입스크립트는 배열의 멤버를 찾아서 해당 배열의 타입 요소를 되도려주는 전형적인 인덱스 기반 접근 방식을 이해하는 언어이다.

다음 dfenders 배열은 string[] 타입이므로 defender는 string타입이다.

```typescript
const defenders = ['Clearenz', 'Dina'];
//타입: string
const defender = defender[0];
```

유니언 타입으로 된 배열의 멤버는 그 자체로 동일한 유니언 타입이다.

soliderOrDates는 (string | data)[]  타입이므로 soldierOrDate 변수는 string | Date 타입이다.

```typescript
const soldiersOrDates = ['Deborh Sampson', new Date(1782, 6, 3)] ;

//타입: string | Date
cosnt soldierOrDate = soldierOrDates[0];
```



#### 주의 사항: 불안정한 멤버

타입스크립트 타입 시스템은 기술적으로 불안정하다고 알려져 있다. 대부분 올바른 타입을 얻을 수 있지만, 때로는 값 타입에 대한 타입 시스템의 이해가 올바르지 않을 수 있다.

특히 배열은 타입 시스템에서 불안정한 소스이기에 기본적으로 타입스크립트는 모든 배열의 멤버에 대한 접근이 해당 배열의 멤버를 반환한다고 가정하지만, 자바스크립트에서 조차도 배열의 길이보다 큰 인덱스로 배열 요소에 접근하면 undefined를 제공한다.



다음 코드는 타입스크립트 컴파일러의 기본 설정에서 오류를 표시하지 않는다.

```typescript
function withElements(elements: string[]) {
	console.log(elements[9001[].length) //타입 오류 없음
}

withElements(['It's , 'over'])
```

이 책의 독자라면 런타임 시 Cannot read property 'lengh of undefined' 가 발생하며 충돌할걸라고 유추할 수 있지만,

타입스크립트는 검색된 배열애 멤버가 존재하지는지 의도적으로 확인하지 않는다. 코드 스니펫에서 Elements[9001]은 undefined가 아니라 string 타입으로 간주 된다.

> 타입스크립트에는 배열 조회를 더 제한하고 타입을 안전하게 만드는  noUncheckedIndexedAccess 플래그가 있지만 이 플래그는 매우 엄격해서 대부분의 프로젝트에서 사용하지 않는다.



### 스프레드와 나머지 매개변수

... 연산자를 사용하는 나머지 매개변수와 배열 스프레드는 자바스크립트에서 배열과 상호작용하는 핵심 방법이다.



#### 스프레드 

... 스프레드 연산자를 사용해 배열을 결합한다. 타입스크립트는 입력된 배열 중 하나의 값이 결과 배열에 포함될 것임을 이해한다.

만약에 입력된 배열이 동일한 타입이라면 출력 배열도 동일한 타입이다. 서로 다른 타입의 두 배열을 함께 스프레드해 ㅅ 배열을 ㅅ생성하면 새 배열은 두 개의 원래 타입 중 어느 하나의 요소인 유니언 타입 배열로 이해 된다.

conjoined 배열은 string 타입과 number 타입 값을 모두 포함하므로(string | number)[]  타입으로 유추 된다.

```typescript
// 타입: string[]
const soldiers =['Harriet Tubman', 'Joan of Arc', 'Khutulun'];

// 타입: number []
const soldierAges = [90, 19, 45];

// 타입: (string | number) []
const conjoined = [...soldiers, ...soldierAges];
```

 

#### 나머지 매개변수 스프레드

타입스크립트는 나머지 매개벼누로 배열을 스프레드하는 자바스크립트 실행을 인식하고 이에 대해 타입 검사를 수행한다. 나머지 매개변수를 위한 인수로 사용되는 배열은  나머지 매개변수와 동일한 배열 타입을 가져야 한다.

logWarriors 함수는 ...names 나머지 매개변수로 string 값만 받습니다. string[]타입 배열을 스프레드하는 것은 허용되지만 number[]는 허용되지 않습니다.

```typescript
function logWarriors(greeting: string, ...names: string[]) {
	for(const name of names) {
		console.log('${greeting}, ${name}!');
	}
}

const warriors = ['Cathay Williams', 'Lozen', 'Nzinga'];
logWarrios('Hello', ...warriors);
const birthYears = [1844,1840,1583];
logWarrios('Born in', ...birthYears); //Error: Argument of type 'number' is not assignable to parameter of type 'string'
```



### 튜플

자바스크립트 배열은 이론상 어떤 크기라도 될 수 있다. 하지만 때로는 튜플이라고 하는 고정된 크기의 배열을 사용하는 것이 유용하다. 튜플 배열은 각 인덱스에 알려진 특정 타입을 가지며 배열의 모든 가능한 멤버를 갖는 유니언 타입보다 더 구체적이다. 튜플 타입을 선언하는 구문은 배열 리터럴처럼 보이지만 요소의 값 대신 타입을 적는다.

yaerAndWariior 배열은 인덱스 0에 number 타입 값을 갖고, 인덱스1에 string 값을 갖는 튜플 타입으로 선언되었다.

```typescript
let yearAndWarrior: [number, string];
yearAndWarrior = [530, 'Tomyris']; //ok
yearandWarrior = [false, 'Tomyris'] //Error: Type 'boolean' is not assignable to type 'number'

yearAndWarrior = [530]; //Error: Type '[number]' is not assignable to type '[number, string]'
// Source has 1 elements(s) but target requires 2.
```

자바스크립트에서는 단일 조건을 기반으로 두 개의 변수에 초깃값을 설정하는 것처럼 한 번에 여러 값을 할당하기 위해 튜플과 배열 구조 분해 할당을 함께 자주 사용한다.

예를 들어 타입스크립트는 year는 항상 number 이고, warrior는 항상 string 임을 인식한다.

```typescript
//year 타입: number
// warrior 타입: string
let [year, wariior] = Math.random() > 0.5
	? [340, 'Archidamia']
	:	[1828, 'Rani of Jhansi'];
```



#### 튜플 할당 가능성

타입스크립트에서 튜플 타입은 가변 길이의 배열 타입보다 더 추게적으로 처리된다. 즉, 가변 길이의 배열 타입은 튜플 타입에 할당할 수 없습니다.

코드에서 우리는 pirLoose 내부에 [boolean, number] 가 있는 것을 볼 수 있지만, 타입스크립트는 더 일반적인(boolean | number) [] 타입으로 유추한다.

```typescript
//타입: (boolean | number) []
const pairLoose = [false, 123];
const pairTupeleLoose: [boolean, number] = pairLoose
//Error: type '(number|boolean)[]' is not assignable to type '[boolean, number]'
// Target requires 2 element(s) but source may have fewer.
```

pairLoose가 [boolean, number] 자체로 선언된 경우 pairTupleLoose에 대한 값 할당이 허용되었다. 하지만 타입스크립트는 튜플 타입의 튜플에 얼마나 많은 멤버가 있는지 알고 있기 때문에 길이가 다른 튜플은 서로 할당할 수 없다.

