## TS_함수3



### void 반환 타입

일부 함수는 어떤 값도 변환하지 않는다. 예를 들면 return 문이 없는 함수이거나 값을 반환  하지 않는 

return 문을 가진 함수일 경우 이다. 타입스크립트는 void 키워드를 사용해 반환 값이 없는 함수의 반환 타입을 확인할 수 있다.

반환 타입이 void인 함수는 값을 반환하지 않을 수있다. 

```typescript
function logSong(song:string | undefined) : void {
	if(!song) {
		return; //ok
	}
	console.log('${song}');
	return true; //Error: type 'boolean' is not assignable to type 'void'
}
```

logSong 함수는 void를 반환하도록 선언되었으므로 값 반환을 허용하지 않는다.

함수 타입 선언 시 void 반환 타입은 매우 유용하다. 함수 타입을 선언할 때 void 를 사용하면 함수에서 반환되는 모든 값은 무시 된다.

```typescript
let songLogger: (song:string) => void;

songLogger = (song) => {
	console.log('${songs}');
}
songLogger('Heart of Glass') //ok
```

songLogger 변수는 song:string을 받고, 값을 반환하지 않는 함수이다.

자바스크립트 함수는 실젯값이 반환되지 않으면 기본으로 모두 undefined를 반환하지만 void는 undeined와 동일하지 않는다. void는 함수의 반환 타입이 무시된다는 것을 의미하고 undfined는 반환되는 리터럴 값이다. undfined를 포함하는 대신 void 타입의 값을ㄹ 할당하려고 하면 타입 오류가 발생한다.

```typescript
function returnVoid() {
	return;
}
let lazyValue: string| undefinde;
lazyValue = rturnsVoid(); //Error: Type'void' is not assignable to type 'string | undefined'
```

undefined와 void를 구분해서 사용하면 매우 유용하다. 특히 void 를 반환하는 콜백을 받는다.

forEach에 제공되는 함수는 원하는 모든 값을  반환할 수 있습니다. 

```
const records: string[] = [];

function saveRecords(newRecodrds: string[]) {
	newRecords.forEach(record => records.push(record));
}
saveRecords(['21', 'Come on Over', 'The Bodyguard'])
```

saveRecords 함수의 records.push(record)는 number(배열의 .push()에서 반환된 값)를 반환하지만, 여전히 newRecords.forEach에 전달된 화살표 함수에 대한 반환값이 허용된다. 

void 타입은 자바스크립트가 아닌 함수의 반환 타입을 선언하는데 사용하는 타입스크립트 키워드 입니다. void 타입은 함수의 반환값이 자체적으로 반환될 수 있는 값도 아니고, 사용하기 위한 것도 아니라는 표시임을 기억하세요. 

#### never 반환 타입

일부 함수는 값을 반환하지 않을 뿐만 아니라 반환할 생각도 전혀 ㅇ없다. never 반환 함수는 (의도적으로) 항상 오류를 발생시키거나 무한 루프를 실행하는 함수이다.

함수가 절대 반환하지 않도록 의도하려면 명시적: never 타입 애너테이션을 추가해 해당 함수를 호출한 후 모든 코드가 실행되지 않음을 나타낸다. 

```typescript
function fail(message:string): never {
	throw new Error('Invariant failur: ${message},');
}

function workWithUnsafeParam(param: unknown) {
	if(typeof para !== 'string') {
		fail('para should be a string, not ${typeof para}');
	}
	
	param.toUpperCase(); //ok
}
```

fail 함수는 오류만 발생시키므로 para의 타입을 string으로 좁혀서 타입스크립트의 제어 흐름 분석을 도와준다.



### 함수 오버로드

일부 자바스크립트 함수는 선택적 매개변수와 나머지 매개변수만으로 표현할 수 없는 매우 다른 매개변수들로 호출될 수 있다. 이러한 함수는 오버로드 시그니처라고 불리는 타입스크립트 구문으로 설명할 수 있다. 즉 하나의 최종 구현 시그니쳐와 그 함수의 본문 앞에 서로 다른 버전의 함수 이름, 매개변수, 반환    타입을 여러번 선언한다.

오버로드된 함수 호출에 대해 구문 오류를 생성할지 여부를 결정할 때 타입스크립트는 함수의 오버로드 시그니처만 확인한다. 구현 시그니처는 함수의 내부 로직에서만 사용된다.

```typescript
function createDate(timestamp: number): Date;
function createDate(month: number, day: number, year: number):Date;
function createDate(monthOrTimesstamp: number, day?: number, year?: 	number) {
	return day == undefined || year === undefined
		? new Date(monthOrTimestamp)
		: new Date(year, monthOrTimestamp, day);
}

createDate(554356800); //ok
createDate(7, 27, 1987); //ok

createDate(4, 1) //Error: No overload expects 2 arguments, but overloads
// do exist that expect either 1 or 3 arguments.
```

createDate 함수는 1개의 timestamp 매개변수 또는 3개의 매개변수(month, day, year)를 사용해 호출한다. 허용된 수의 인수를 사용해 호출할 수 있지만 2개의 인수를 사용해 호출하면 2개의 인수를 허용하는 오버로드 시그니처가 없기 때문에 타입 오류가 발생한다.



타입스크립트를 컴파일해 자바스크립트로 출력하면 다른 타입 시스템 구문처럼 오버로드 시그니처도 지워진다.

```typescript
function createDate(monthOrTimestamp, day, year) {
	return day === undefined || year === undefined
		? new Date(monthOrTimestamp)
		: new Date(year, monthOrTimestamp, day);
}
```

> 함수 오버로드는 복잡하고 설명하기 어려운 함수 타입에 사용하는 최후의 수단이다. 함수를 단순하게 유지하고 가능하면 함수 오버로드를 사용하지 않는 것이 좋다.



### 호출 시그니처 호환성

오버로드된 함수의 구현에서 사용되는 구현 시그니처는 매개변수 타입과 반환 타입에 사용하는 것과 동일하다. 따라서 함수의 오버로드 시그니처에 있는 반환 타입과 각 매개변수는 구현 시그니처에 있는 동일한 인덱스의 매개변수에 할당할 수 있어야 한다. 즉. 구현 시그니처는 모든 오버로드 시그니처와 호환되어야 한다.

```typescript
 function format(data: string): string; //ok
 function format(data: string, needle: string, haystack: string): string; //ok
 
 function format(getData: ()=> string): string; //Error: This overload signatrue is not compatible with its implementation
 
 function format(data: string, neelde?: string, haystack?: string) {
	return needle && haystack ? data.replace(needle, haystack): data;
}
```

format 함수의 구현 시그니처는 첫 번째 매개변수를 string으로 선언한다. 처음 두 개의 오버로드 시그니처는 string 타입과 호환되지만, 세 번째 오버로드 시그니처의 () => string 타입과는 호환되지 않는다.



