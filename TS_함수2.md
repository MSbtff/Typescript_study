## TS_함수2



### 반환 타입

타입스크립트는 지각적이다. 함수가 반환할 수 있는 가능한 모든 값을 이해하면 함수가 반환하는 타입을 알 수 있다.

```typescript
//타입: (songs: string[]) => number
function singSongs(songs: string[]) [
	for(const song of songs) {
		console.log('${song}');
	}
	return songs.length;
]
```

singSongs 는 타입스크립트에서 number를 반환하는것으로 파악됨

함수에 다른 값을 가진 여러 개의 반환문을 포함하고 있다면, 타입스크립트는 반환 타입을 가능한 모든 반환 타입의 조합으로 유추한다.

```typescript
//타입: (songs: string[], index: number) => string | undefined
function getSongAt(songs: string[], index: number) {
	return index < songs.length
		? songs[index]
		: undefined;
}
```

getSongAt 함수는 string | undefined를 반환하는 것으로 유추된다. 두 가지 간으한 반환값이 각각 string과 undefined이기 때문



#### 명시적 반환 타입

변수와 마찬가지로 타입 애너테이션을 사용해 함수의 반환 타입을 명시적으로 선언하지 않는 것이 좋다. 그러나 특히 함수에서 반환 타입을 명시적으로 선언하는 방식이 매우 유용할 때가 종종 있다.

- 가능한 반환값이 많은 함수가 항상 동일한 타입의 값을 반환하도록 강제함
- 타입스크립트는 재귀함수의 반환 타입을 통해 타입을 유추하는 것을 거부한다.
- 수백 개 이상의 타입스크립트 파일이 있는 매우 큰 프로젝트에서 타입스크립트 타입 검사 속도를 높일 수 있다.

함수 선언 반환 타입 애너테이션은 매개변수 목록이 끝나는 ) 다음에 배치된다. 함수 선언의 경우는 { 앞에 배치된다.

```typescript
function singSongsRecuresive(songs: string[], count = 0): number {
	return songs.length ? singSongsRecursive(songs.slice(1), count + 1) : count;
}
```



화살표 함수의 경우 => 앞에 배치된다.

```typescript
const singSongsRecursiv = (songs: string[], count = 0) : number = > 
	songs.length ? singSongsRecursive(songs.slice(1), count + 1) : count;
```

함수의 반환문이 함수의 반환 타입으로 할당할 수 없는 값을 반환하는 경우 타입스크립트는 할당 가능성 오류를 표시한다.



```typescript
function getSongRecordingDate(song: string):
Date | undefind {
	switch(song) {
		case 'Strange Fruit':
			return new Date('April 20, 1939') //ok
			
		case 'Greensleeves';
			return 'unknown';
			//Error: Type 'String' in not assignable to type 'Date'
    default: return undefined; //ok
	}
}
```



### 함수 타입

자바스크립트에서는 함수를 값으로 전달할 수 있다. 즉 함수를 가지기 위한 매개변수 또는 변수의 타입을 선언하는 방법이 필요하다

함수 타입 구문은 화살표 함수와 유사하지만 함수 본문 대신 타입이 있다.

```typescript
let nothingInGivesString: () = > string;
```

nothingInGivesString 변수 타입은 매개변수가 없고 string 타입을 반환하는 함수임

```typescript
let inputAndOutPut: (songs: string[], count?: number) => number:
```

inputAndOutput 변수 타입은 string[] 매개변수와 count 선택적 매개변수 및 number 값을 반환하는 함수임을 설명한다.



함수타입은 콜백 매개변수(함수로 호출되는 매개변수)를 설명하는 데 자주 사용된다.

```typescript
const songs = ['juice', 'Shake It Off', 'What's up '];
function runOnSongs(getSongAt: (index: number) => string) {
	for(let i = 0; i< songs.length; i += 1) {
		console.log(getSongAt(i))
	}
}

function getSongAt(index: number) {
	return`${songs[index]}`;
}

runOnSongs(getSongAt); //ok

function logSong(song: string) {
	return `${song}`;
}

runOnSongs(logSong); //Error: Argument of type '(song: string) => string' is not
// assignable to parameter of type '(index: number) => string'
//Type of parameter 'song' and 'index' are incompatible.
// Type 'number' is not assignable to type 'string'
```

runOnSongs 함수는 getSongAt 매개변수의 타입을 index: number를 받고 string을 반환하는 함수로 선언했다. getSongAt을 전달하면 해당 타입과 일치하지만, logSong은 매개변수로 number 대신 string을 사용하므로 반환값을 가져오는데 실패한다.

runOnSongs(logsong)에 대한 오류 메시지는 할당 가능성 오류의 예로 몇 가지 상세한 단계까지 제공한다. 두 함수를 서로 할당할 수 없다는 오류를 출력할 때 타입스크립트는 일반적으로 세 가지 상세한 단계를 제공. 

1. 첫 번째 들여쓰기 단계는 두 함수 타입을 출력한다.
2. 다음 들여쓰기 단계는 일치하지 않는 부분을 지정
3. 마지막 들여쓰기 단계는 일치하지 않는 부분에 대한 정확한 할당 가능성 오류를 출력한다.

이전 코드 스니펫에서 단계별로 제공하는 내용은 다음과 같다.

1. logSong: (song: string) => string은 getSongAt: (index: number) => string에 할당되도록 제공된 타입
2. logSong의 song 매개변수는 getSongAt의 index 매개변수로 할당된다.
3. song의 string 타입은 index의 number 타입에 할당할 수 없다.



#### 함수 타입 괄호

함수 타입은 다른 타입이 사용되는 모든 곳에 배치할 수 있다. 여기에는 유니언 타입도 포함된다.

유니언 타입의 애너테이션에서 함수 반환 위치를 나타내거나 유니언 타입을 감싸는 부분을 표시할 때 괄호를 사용한다.

```typsc
//타입은 string | undefined 유니언을 반환하는 함수
let returnsStringOrUndefined: () => string | undefined;

//타입은 undefined나 string을 반환하는 함수
let maybeReturnsString: (()=> string) |undefined
```



#### 매개변수 타입 추론

매개변수로 사용되는 이란인 함수를 포함하여 작성한 모든 함수에 대해 매개변수를 선언해야 한다면 번거로울 것 다행히도 타입스크립트는 선언된 타입의 위치에 제공된 함수의 매개변수 타입을 유추할 수 있다.

```typescript
let singger: (song:string) => string;
singer = function(song) {
	//song: string의 타입
	return `Singing: ${song.toUpperCase()}!`; //ok
}
```

singer 변수는 string 타입의 매개변수를 갖는 함수로 알려져 있으므로 나중에 singer가 할당되는 함수 내의 song 매개변수는 string으로 예측된다.



함수를 매개변수로 갖는 함수에 인수로 전달된 함수는 해당 매개변수 타입도 잘 유추할 수 있다.

```typescript
const songs = ['Call me' , 'jolene', 'The Chain'];

//song: string
//index: number
songs.forEach((song, index)=> {
	console.log(`${song} is at index ${index}`);
})
```



#### 함수 타입 별칭

유니언 리털러에서 별칭처럼 함수 타입도 동일하게 타입 별칭 사용 가능

```typescript
type StringToNumber = (input: string) => number;
let stringToNumber: StringToNumber;
stringToNumber = (input) => input.length; // ok
stringToNumber = (input) => input.toUpperCase(); //Error: Type 'siring' is not assignable to type 'number'
```

StringToNumber 타입은 string 타입을 받고 number 타입을 반환하는 함수의 별칭을 지정 별칭은 이후 변수 타입을 설명하는데 사용한다.



비슷하게 함수 매개변수도 함수타입을 참조하는 별칭을 입력할 수 있다.

```typescript
type NumberToString = (input: number) => string;
function useNumberToString(numberToString: NumberToString) {
	console.log(`The string is: ${numberToString(1234)}`)''
}

usesNumberToString((input)=> `${input}! Hooray!`); //ok
usesNumberToString((input)=> input * 2); //Error Type 'number' is not assignable to type 'string'
```

useNumberToString 함수는 함수 타입 별칭인 NumberToString의 단일 매개변수를 갖는다.

타입 별칭은 특히 함수 타입에 유용하다. 타입 별칭을 이용하면 반복적으로 작성하는 매개변수와 반환 타입을 갖는 코드 공간을 많이 절약할 수 있다.

