> 요약
> -  에디터에서 여러 종류의 제어 흐름을 살펴보며 타입을 좁히는 과정을 이해해야 한다.
> -  태그된/구별된 유니온과 사용자 정의 타입 가드를 사용하여 타입 좁히기 과정을 원활하게 만들 수 있다.


## Intro
> **타입 좁히기**란? null 체크처럼 넓은 타입으로부터 좁은 타입으로 진행하는 과정을 말한다.


### 좁히는 방법

- 조건문
- instance of
- 속성 체크
- 내장 함수 (Array.isArray 등)

주로 조건을 통해서 조건을 좁혀가는데, 이때 잘못된 판단을 하지 않을지 주의해야 한다. 아래는 잘못 사용된 예시다. 


아직 null 의 typeof는 object다. 
`typeof null === 'object'`

따라서 아래의 경우엔 타입이 좁혀지지 않았다.
```ts
const el = document.getElementById('foo'); // type is HTMLElement | null

if (typeof el === 'object') {
  el;  // Type is HTMLElement | null
}

function foo(x?: number | string | null){
	if(!x) {
		x; // 타입이 string, number, null, undefined 다 될 수 있음
	}
} 
```



- 태그 붙이기 (Tagged union, Discriminated union)

명시된 태그를 붙이는 경우가 실제로 많다. (클로이 경험담)
```ts
interface UploadEvent { type: 'upload'; filename: string; contents: string }
interface DownloadEvent { type: 'download'; filename: string; }
type AppEvent = UploadEvent | DownloadEvent;

  

function handleEvent(e: AppEvent) {
  switch (e.type) {
    case 'download':
      e  // Type is DownloadEvent
      break;
    case 'upload':
      e;  // Type is UploadEvent
      break;
  }
}
```


- 사용자 정의 타입 가드: 직접 함수를 만들어 타입가드한다.
	- HTMLElement를 HTMLInputElement로 추론하고 싶은 경우
```ts
function isInputElement(el: HTMLElement): el is HTMLInputElement {

  return 'value' in el;

}

  

function getElementContent(el: HTMLElement) {

  if (isInputElement(el)) {

    el; // Type is HTMLInputElement

    return el.value;

  }

  el; // Type is HTMLElement

  return el.textContent;

} 
```


- filter 예제를 통해 보는 타입 가드
	- 똑같은 역할을 하는 함수여도 타입 가드를 통해 할 수 있다.

```ts
const jackson5 = ['Jackie', 'Tito', 'Jermaine', 'Marlon', 'Michael'];

const members = ['Janet', 'Michael'].map(
  who => jackson5.find(n => n === who)
).filter(who => who !== undefined);  // Type is (string | undefined)[]


function isDefined<T>(x: T | undefined): x is T {
  return x !== undefined;
}

const members = ['Janet', 'Michael'].map(
  who => jackson5.find(n => n === who)
).filter(isDefined);  // Type is string[]
```

```ts
const members = ['Janet', 'Michael'].map(
  who => jackson5.find(n => n === who)!
).filter(who => who !== undefined);  

// Type is string[]로 나오긴 하지만 map 의 결과를 ! 단언으로 거짓?지정해준 것
```

위 예제에서 filter를 사용하기 때문에 결과적으로는 `string[]`이지만 처음 map의 결과가 `(string | undefined)[]` 이기 때문에 members의 결과도 그렇게 나옴. 이걸 string[]으로 해주기 위해 가드 중

### memo
- 왜 null이 object인가??
- falsy값 잘 외워두자