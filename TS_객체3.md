## TS_객체3

### 객체 타입 유니언

타입스크립트 코드에서는 속성이 조금 다른, 하나 이상의 서로 다른 객체 타입이 될 수 있는 타입을 설명할 수 있어야 한다. 또한 속성값을 기반으로 해당 객체 타입 간에 타입을 좁혀야 할 수 있다.



#### 유추된 객체 타입 유니언

변수에 여러 객체 타입 중 하나가 될 수 있는 초깃값이 주어지면 타입스크립트는 해당 타입을 객체 타입 유니언으로 유추한다. 유니언 타입은 가능한 각 객체 타입을 구성하고 있는 요소를 모두 가질 수 있다. 객체 타입에 정의된 각각의 가능한 속성은 비록 초깃값이 없는 선택적 타입이지만 각 객체 타입의 구성 요소로 주어진다.

```typescript
const poem = Math.random() > 0.5
	? {name: 'The Double Image', pages: 7}
	: {name: 'Her Kind', rhymes: true}
// 타입:
{
	name: string;
	pages: number;
	rhymes?: undefined;
}
|
{
	name: string;
	pages?: undefined;
	rhymes: boolean;
}
poem.name; //string
poem.pages // number | undefined
poem.rhymes; //boleans | unefined
```

poem 값은 항상 string 타입인 name  속성을 가지며 pages와 rhymes는 있을 수도 있고, 없을 수도 있다.



#### 명시된 객체 타입 유니언

객체 타입의 조합을 명시하면 객체 타입을 더 명확히 정의할 수 있다. 코드를 조금 더 작성해야 하지만 객체 타입을 더 많이 제어할 수 있다는 이점이 있다. 특히 값의 타입이 객체 타입으로 구성된 유니언이라면 타입스크립트의 타입 시스템은 이런 모든 유니언 타입에 존재하는 속성에 대한 접근만 허용한다.

앞에 poem 변수는 pages 또는 rhymes와 함께 필수 속성인 name을 항상 갖는 유니언 타입으로 명시적으로 작성되었다. 속성 name에 접근하는 것은 name 속성이 항상 존재하기 때문에 허용되지만 pages와 rhymes는 항상 존재한다는 보장이 없다.

```typescript
type PoemWithPages = {
	name: string;
	pages:number;
};

type PoemWithRhymes = {
	name: string;
	thymbes: boolean;
}

type Poem = PoemWithPages | PoemWithRhymes;

const poem: Poem = Math.random() > 0.5
	? {name: 'the Double Image', pages: 7}
	: {name: 'Her Kind', rhymes: true};
	
	poem.name; //ok
	poem.pages; //Error
	
	poem.rhymes
//Error
```



잠재적으로 존재하지 않는 객체의 멤버에 대한 접근을 제한하면 코드의 안전을 지킬 수 없다. 값이 여러 타입 중 하나일 경우, 모든 타입에 존재하지 않는 속성이 객체에 존재할 거라 보장할 수 없다.

리터럴 타입이나 원시 타입 모두, 혹은 둘 중 하나로 이루어진 유니언 타입에서 모든 타입에 존재하지 않은 속성에 접근하기 위해 타입을 좁혀야 하는 것처럼 객체 타입 유니언도 타입을 좁혀야 한다.



#### 객체 타입 내로잉

타입 검사기가 유니언 타입 값에 특정 속성이 포함된 경우에만 코드 영역을 실행할 수 있음을 알게 되면, 값의 타입을 해당 속성을 포함하는 구성 요소로만 좁힙니다. 즉 코드에서 객체의 형태를 확인하고 타입 내로잉이 객체에 적용된다.

명시적으로 입력된 poem 예제를 계속 살펴보면 poem의 pages가 타입스크립트의 타입 가드 역할을 해 poemWithPages임을 나타내는지 확인한다. 만일 Poem이 PoemWithPages가 아니라면 PoemWithRhymes 이어야 한다.

```typescript
if('pages' in poem){
	poem.pages; // ok: poem은 PoemWithPages로 좁혀짐
} else {
	poem.rhymes; //ok: poem은 PoemWithRhymes로 좁혀짐
}
```

타입스크립트는 if(poem.pages) 와 같은 형식으로 참 여부를 확인하는 것을 허용하지 않는다. 존재하지 않는 객체의 속성에 접근하려고 시도하면 타입 가드처럼 작동하는 방식으로 사용되더라도 타입 오류로 간주 된다.



```typescript
if (poem.pages) {/**/} 
//Error: Property
```



#### 판별된 유니언

자바스크립트와 타입스크립트에서 유니언 타입으로 된 객체의 또 다른 인기 있는 형태는 객체의 속성이 객체의 형태를 나타내도록 하는 것이다. 이러한 타입 형태를 **판별된 유니언** 이라 부르고, 객체의 타입을 가리키는 속성이 **판별값**이다. 타입스크립트는 코드에서 판별 속성을 사용해 타입 내로잉을 수행한다.

```typescript
type PoemWithPages = {
	name: string;
	pages: number;
	type: 'pages';
}

type PoemWithRymes = {
	name: string;
	rhymes: boolean;
	type: 'rhymes';
}

type Poem = PoemWithPages | PoemWithRymes;

const poem: Math.random() > 0.5
	? {name: 'the double image', pages: 7, type: 'pages'}
	: {name: 'her kind', rhymes: true, type: 'rhymes'};
	
	if(poem.type === 'pages') {
		console.log('It's get pages: ${poem.pages}') //ok
	} else {
		console.log('It ryhmes: ${poem.rhymes}');
}

poem.type; //타입: 'pages' : 'rhymes'

poem.pages; //Error
```

Poem 타입은 PoemWithPages 타입 또는 PoemWithRhymes 타입 둘 다 될 수 있는 객체를 설명하고 type 속성으로 어느 타입인지를 나타낸다. 만일 poem.type이 pages이면, 타입스크립트는 poem을 PoemWithPages로 유추한다. 타입 내로잉 없이는 값에 존재하는 속성을 보장할 수 없다.

판별된 유니언은 우아한 자바스크립트 패턴과 타입스크립트의 타입 내로잉을 아름답게 결합하므로 타입스크립트에서 필자가 가장 좋아하는 기능이다.



### 교차타입

타입스크립트 유니언 타입은 둘 이상의 다른 타입 중 하나의 타입이. 될 수 있음을 나타낸다.

자바스크립트의 런타임 | 연산자가 &연산자에 대응하는 역할을 하는 것처럼, 타입스크립트에서도 & 교차타입을 사용해 여러 타입을 동시에 나타낸다. 교차 타입은 일반적으로 여러 기존 객체 타입을 별칭 객체 타입으로 결합해 새로운 타입을 생성한다.

```typescript
type Artwork = {
	genre: string;
	name: string;
};

type Writing = {
	pages:number;
	name: sting;
}

type WrittenArt = Artwork & Writing;

//다음과 같음:
{
	genre: string;
	name: string;
	pages:number;
}
```

Artwork와 Writing 타입은 genre, name, pages 속성을 결합한 WrittenArt 타입을 형성하는데 사용된다.

교차 타입은 유니언 타입과 결합할 수 있으며, 이는 하나의 타입으로 판별된 유니언 타입을 설명하는 데 유용하다.

```typescript
type ShortPoem = {author: string} & ( 
	| {kigo: string; type: 'haiku';}
	| {meter: number; type: 'Villanelle';}
);

//ok
const moringGlory: ShortPoem = {
	author: 'Fukuda Chiyo-ni',
	kigo: 'Moring Glory',
	type: 'haiku'
};

const oneArt: ShortPoem = {
	//Error
	author: 'Elizabeth Bishop',
	type: 'villanelle',
}
```



#### 교차 타입의 위험성

교차 타입은 유용한 개념이지만, 타입스크립트 컴파일러를 혼동시키는 방식으로 사용하기 쉽다. 교차 타입을 사용할 때는 가능한 코드를 간결하게 유지해야 한다.



##### 긴 할당 가능성 오류

유니언 타입과 결합하는 것처럼 복잡한 교차 타입을 만들게 되면 할당 가능성 오류 메시지는 읽기 어려워 진다. 다시 말해 복잡할수록 타입 검사기의 메시지도 이해하기 더 어려워 진다. 이 현상은 타입스크립트의 타입 시스템, 그리고 타입을 지정하는 프로그래밍 언어에서 공통적으로 관측된다.

이전 코드 스니펫의 ShortPoem의 경우 타입스크립트가 해당 이름을 출력하도록 타입을 일련의 별칭으로 된 객체 타입으로 분할하면 읽기가 훨씬 쉬워진다.

```typescript
type ShortPoemBase = {author: string};
type Haiku = ShortPoemBase & {kigo: string; type: 'haiku'};
type Villanelle = ShortPoemBase & {meter: number; type: 'villanelle'};
type ShortPoem = haiku | Villanelle

const oneArt: ShortPoem = {
	//Error
  
  author: 'Elizabet Bishop',
  type: 'villanelle',
}
```



##### never

교차 타입은 잘못 사용하기 쉽고 불가능한 타입을 생성한다. 원시 타입의 값은 동시에 여러 타입이 될 수 없기 때문에 교차 타입의 구성 요소로 함께 결합할 수 없다. 두 개의 원시 타입을 함께 시도하면 never 키워드로 표시되는 never 타입이 된다.

```typescript
type NotPossible = number & string; //타입: never
```

never 키워드와 never 타입은 프로그래밍 언어에서 bottom 타입 또는 empty 타입을 뜻한다. bottom 타입은 값을 가질 수 없고 참조할 수 없는 타입이므로 bottom 타입에 그 어떠한 타입도 제공할 수 없다.

```typescript
let notNumber: NotPossilbe = 0; //Error

let notString: never ='' //Error
```

대부분의 타입스크립트 프로젝트는 never 타입을 거의 사용하지 않지만 코드에서 불가능한 상태를 나타내기 위해 가끔 등장한다. 하지만 대부분 교차 타입을 잘못 사용해 발생한 실수일 가능성이 높다.