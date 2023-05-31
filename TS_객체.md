## TS_객체



### 객체 타입

{...} 구문을 사용해서 객체 리터럴을 생성하면, 타입스크립트는 해당 속성을 기반으로 새로운 객체 타입 또는 타입 형태를 고려한다.

해당 객체 타입은 객체의 값과 동일한 속성명과 원시 타입을 갖는다. 값의 속성에 접근하려면 value.멤버 또는 value['멤버'] 구문을 사용해한다

다음 poet 변수의 타입은 number 타입인 born 과 string 타입인 name 으로 이루어진 두 개의 속성을 갖는 객체

이 두개의 속성에 접근하는 것은 허용되지만, 다른 속성 이름으로 접근하려고 하면 해당 이름이 존재하지 않는다는 타입 오류가 발생

```typescript
const poet = {
		born: 1935;
		name: 'mary oliver',
}

poet['born']; //타입 number
poet.name; //타입: string

poet.end; //Error
```

객체 타입은 타입스크립트가 자바스크립트 코드를 이해하는 방법에 대한 핵심 개념

null 과 undefined를 제외한 모든 값은 그 값에 대한 실제 타입의 멤버 집합을 가지므로 타입스크립트는 모든 값의 타입을 확인하기 위해 객체 타입을 이해해야 한다.



### 객체 타입 선언

기존 객체에서 직접 타입을 유추하는 방법도 굉장히 좋지만, 결국에는 객체의 타입을 명시적으로 선언하고 싶다.

명시적으로 타입이 선언된 객체와는 별도로 객체의 형태를 설명하는 방법이 필요

객체 타입은 객체 리터럴과 유사하게 보이지만 필드 값 대신 타입을 사용해 설명한다.

타입스크립트가 타입 할당 가능성에 대한 오류 메시지에 표시하는 것과 동일한 구문

poetLater 변수는 born: number와 name:  string으로 이전과 동일한 타입

```typescript
let poetLater : {
		born: number;
		name: string;
}

//ok
poetLater= {
		born: 1935,
		name: 'mary oliver'
}
poetLater = 'Sappho' //error
```



### 별칭 객체 타입

{born: number, name: string} 과 같은 객체 타입을 계속 작성하는 일은 매우 귀찮기에 각 객체 타입에 타입 별칭을 할당해 사용하는 방법이 더 일반적

다음과 같이 이전 코드 스니펫 poet 타입으로 다시 작성할 수 있으며, 타입스크립트의 할당 가능성 오류 메시지를 좀 더 직접적으로 읽기 쉽게 만드는 추가 이점이 있다.

```typescript
type Poet = {
		born: nuber;
		name: string;
};

let poetLater: Poet;

//ok
poetLater= {
		born: 1935,
		name: 'Sara Tesdale'
}
poetLater = 'Emily Dickinson' //error 
```

> 대부분의 타입스크립트 프로젝트는 객체 타입을 설명할 때 인터페이스 키워드를 사용하는 것을 선호
>
> 별칭 객체 타입과 인터페이슨느 거의 동일함



이 시점에서 객체 타입을 살펴보는 이뉴는 타입스크립트의 타입 시스템을 배울 때, 타입스크립트가 객체 리터럴을 해석하는 방법을 이해하는 것이 매우 중요하기 때문



### 구조적 타이핑

타입스크립트의 타입 시스템은 구조적 타입화 되어 있음 즉 타입을 충족하는 모든 값을 해당 타입의 값으로 사용할 수 있다.

매개변수나 변수가 특정 객체타입으로 선언되면 타입스크립트에 어떤 객체를 사용하든 해당 속성이 있어야 한다고 말해야 한다.

다음 별칭 객체인 WithFirstName 과 WithLastName은 오직 string 타입의 단일 멤버만 선언한다. hasBoth 변수는 명시적으로 선언되지 않았음에도 두 개의 별칭 객체 타입을 모두 가지므로 두 대그의 별칭 객체타입 내에서 선언된 변수를 모두 제공할 수 있다.

```typescript
type WithFirstName = {
	firstName: string;
}

type WithLastName = {
	lastName: string;
}

const hasBoth ={
	firstName = 'lucille'
	lastName = 'Clifton',
}

//ok: hasboth는 string 타입의 firstname을 포함
let withFirstName: WithFristName = hasBoth;

//ok: hasboth는 string 타입의 lastName을 포함함
let withLastName: WithLastName = hasBoth
```

구조적 타이핑은 덕 타이핑과는 다름 덕 타이핑은 오리처럼 보이고 오리처럼 꽥꽥 거리면, 오리일 것이다라는 문구에서 유래했는데

- 타입스크립트의 타입 검사기에서 구조적 타이핑은 정적 시스템이 타입을 검사하는 경우이다.
- 덕 타이핑은 런타임에서 사용될 때 까지 객체 타입을 검사하지 않는 것을 말한다.

요약하면 자바스크립트는 덕 타입인 반면 타입스크립트는 구조적으로 타입화 된다.







