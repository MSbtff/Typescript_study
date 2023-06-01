## TS_객체 2



### 사용 검사

객체 타입으로 애너테이션된 위치에 값을 제공할 때 타입스크립트는 값을 해당 객체 타입에 할당할 수 있는지 확인한다.

할당하는 값에는 객체 타입의 필수 속성이 있어야 하기에 객체타입에 필요한 멤버가 객체에 없다면 타입스크립트는 오류를 발생시킴

```typescript
type FirstAndLastNames = {
	first: string;
	last: string;
}

//ok
const hasBoth: FirstAndLastNames = {
	first: 'Sarojini',
	last: 'Nadiu',
};

const hasOnlyOne: FirstAndNames = {
	//Error: Property'last' is missing in type '{first:string;}'
  first: 'Sappho'
  
};
```

다음 별칭 객체 타입인 FirstAndLastName는 first와 last 속성이 모두 있어야 한다. 두 가지 속성을 모두 포함한 객체는 FirstAndLastName 타입으로 선언된 변수에 사용할 수 있지만, 두 가지 속성이 모두 없는 객체는 사용할 수 없음

둘 사이에 일치하지 않는 타입도 허용되지 않는다. 객체 타입은 필수 속성 이름과 해당 속성이 예상되는 타입을 모두 지정한다. 객체의 속성이 일치하지 않으면 타입스크립트는 타입 오류를 발생시킨다.

```typescript
type TimeRange = {
	start: Date;
};

const hasStartString: TimeRange= {
	start: '1879-02-13',
	
	//Error
}
```



### 초과 속성 검사

변수가 객체 타입으로 선언되고, 초깃값에 객체 타입에서 정의된 것보다 많은 필드가 있다면 타입스크립트에서 타입 오류가 발생한다. 따라서 변수를 객체 타입으로 선언하는 것은 타입검사기가 해당 타입에 예상되는 필드만 있는지 확인하는 방법이기도 하다.

```typescript
type Poet = {
	born: number;
	name: string;
}

//ok: poet의 필드와 일치함
const poetMatch: Poet{
	born:1928,
	name: 'Maya Angelou'
}

const extraProperty: Poet = {
	activity: 'walking',
  born: 1935,
  name: 'Mary Oliver',
}
```

poetMatch 변수는 별칭 객체 타입에 정의된 필드가 Poet에 정확히 있지만, 초과 속성이 있는 extraProperty는 타입 오류를 발생시킨다.

초과 속성 검사는 객체 타입으로 선언된 위치에서 생성되는 객체 리터럴에 대해서만 일어난다. 기존 객체 리터럴을 제공하면 초과 속성 검사를 우회한다.

```typescript
const existingObject = {
	activity: 'walking',
	born: 1935,
	name: 'Mary Oliver',
};

const extraPropertuButOk: Poet = existingObject; //ok
```

extraPropertyButOk 변수는 초깃값이 구조적으로 Poet과 일치하기 때문에 이전 예제의 Poet 타입처럼 타입 오류가 발생하지 않는다.

배열 요수, 클래스 필드 및 함수 매개변수가 포함된 객체 타입과 일치할거라 예상되는 위치에서 생성되는, 새로운 객체가 있는 모든 곳에서 초과 속성 검사가 일어난다. 타입스크립트에서 초과 속성을 금지하면 코드를 깨끗하게 유지할 수 있고, 예상한 대로 작동하도록 만들 수 있다. 객체 타입에 선언되지 않은 초과 속성은 종종 잘못 입력된 속성 이름이거나 사용되지 않는 코드일 수 있다.



### 중첩된 객체 타입

자바스크립트 객체는 다른 객체의 멤버로 중첩될 수 있으므로 타입스크립트의 객체 타입도 타입 시스템에서 중청된 객체 타입을 나타낼 수 있어야 한다. 이를 구현하는 구문은 이전과 동일하지만 기본 이름 대신에 {...} 객체 타입을 사용한다.

Poem 타입은 author 속성이 firstName: string 과 lastName: string 인 객체로 선언되었다. poemMatch 변수는 구조가 Poem과 일치하기 때문에 Poem을 할당할 수 있는 반면, pemMismatch는 author 속성에 firsName과 lastName 대신 name을 포함하므로 할당할 수 없다.

```typescript
type Poem = {
	author: {
		firstNmae: string;
		lastName: string;
};
	name: string;
};

//ok
const poemMatch: Poem = {
	author: {
		firstName: 'Sylvia',
		lastName: 'plath',
	},
	naem: 'Lady Lazrus',
};

const poemMismatch: Poem = {
	author: {
		name: 'Sylvia Pllath',
	},
	name: 'Tulips',
};
```

Poem 타입을 작성할 때 author 속성의 형태를 자체 별칭 객체 타입으로 추출하는 방법도 있다. 중첩된 타입을 자체 타입 별칭으로 추출하면 타입스크립트의 타입 오류 메시지에 더 많은 정보를 담을 수 있다. 이 경우에는 {firstName: string, lastName: string;} 대신 Author를 사용할 수 있다.

```typescript
type Aythor = {
	firstName: string;
	lastName: string;
};

type Poem = {
	author: Author;
	name: string;
};

const poemMismatch: Poem = {
	author: {
		name: 'Sylvia Plath',
	},
	name: 'tylips',
}
```

> 이처럼 중첩된 객체 타입을 고유한 타입 이름으로 바꿔서 사용하면 코드와 오류 메시지가 더 읽기 쉬워짐



### 선택적 속성

모든 객체에 객체 타입 속성이 필요한 건 아니다. 타입의 속성 애너테이션에서 :앞에 ?를 추가하면 선택적 속성임을 나타낼 수 있다.

```typescript
type Book = {
	author?: string;
	pages: number;
}

//ok
const ok:Book = {
	author: 'Rita Dove',
	pages: 80,
};

const missing: Book = {
	//Error: Property 'pages' is missing in type
	author: 'Rita Dove'
}
```

Book 타입은 pages 속성만 필요하고 author 속성은 선택적으로 허용한다.  객체가 pages 속성을 제공하기만 하면 author 속성은 제공하거나 생략할 수 있다.



선택적 속성 undefined를 포함한 유니언 타입의 속성 사이에는 차이가 있음을 명심해라. ?를 사용해 선택적으로 선언된 속성은 존재하지 않아도 된다. 필수로 선언된 속성과 |undefined는 그 값이 undefined 일지라도 반드시 존재해야 한다.





```typescript
type Writers = {
	author?: string | undefined;
	editor? : number;
}
//ok: author는 undefined으로 제공됨
const hasRequired: Writers = {
	author: undefined,
}

const missingRequired: Writers = {};
// Error: Property 'author' is missing in type '{} but required in type 'Writers'

```

