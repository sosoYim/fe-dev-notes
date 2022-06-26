> 요약
> -  자주 편집기를 통해 추론 결과를 확인하자
> - TS는 명확성(충분히 구체적으로 타입 추론)과 유연성(잘못된 추론을 할 정도로 구체적이이 않음) 사이의 균형을 맞추려 한다.

## Intro
> 타입 넓히기(widening)이란, 지정된 단일 값을 가지고 할당 가능한 값들의 집합을 유추하는 과정이다. 넓히기의 과정을 이해하여 오류의 원인을 파악하고 타입 구문을 더 효과적으로 사용해보자.

### 타입이 넓히기 예제
`'"x"'|'"y"'|'"z"'`  형식의 매개변수이 `string` 형식의 변수를 할당하여 타입 에러가 발생했다.

```ts
interface Vector3 {x: number; y:number; z:number;}
function getComponent(vector: Vector3, axis: 'x'|'y'|'z'){
	return vertor[axis];
}

let x = 'x'; // string으로 타입 추론
let vec = {x: 10, y: 20, z:30};

getComponent(vec, x); // ~'string' 형식의 인수는 '"x"'|'"y"'|'"z"' 형식의 매개변수에 할당될 수 없습니다.
```

하나의 경우에 타입이 될 수 있는 후보가 굉장히 많다.
```ts
const mixed = ['x', 1];

// 다양한 타입을 지정할 수 있지만, 추론한다면 다음과 같이 추론한다.
// (string | number)[]
```
- `('x'|1)[]`
- `['x'',1]`
- `[string, number]`
- `readonly [string, number]`
- `(string|number)[]`
- `readonly (string|number)[]`
- `[any, any]`
- `any[]`

> 기억하자. 타입스크립트가 아무리 똑똑해도 개발자 마음까지 읽어낼 수는 없다. 

`let x = 'x'` 타입을 추론할 떄, 일반적인 규칙은 변수가 선언된 후로는 타입이 바뀌지 않아야 하므로 `string` 을 사용하는 것이 좋다. 

### 타입 넓히기 과정 제어
1. const 사용
let 대신 const를 사용하면 더 좁은 타입이 된다. 단, 재할당이 불가능한 원시값만.
```ts
let x = 'x'; // type: string
const x = 'x'; // type: 'x'
```


2. 객체에서의 타입 체크
	1. 타입 체크 과정
		- 타입 넓히기 알고리즘은 객체 내부의 각 요소를 let으로 할당된 것 처럼 다룬다.
	2. TS 기본 동작 재정의를 통한 타입 추론 강도 직접 제어
		1. 명시적 타입 구문 제공: 타스에게 모든 것을 맡기지 않고 x에 올 수 있는 값들을 제시해줬다.
			```ts
			const v: {x: 1|3|5} = {
			  x: 1,
			};  // Type is { x: 1 | 3 | 5; }
			```
		1. 타입 체커에 추가적인 문맥 제공 
			- 함구의 매개 변수로 값 전달
		1. const 단언문 사용 `as const`
		```ts
		interface Vector3 { x: number; y: number; z: number; }
		
		function getComponent(vector: Vector3, axis: 'x' | 'y' | 'z') {
		  return vector[axis];
		}
		
		const v1 = {
		  x: 1,
		  y: 2,
		};  // Type is { x: number; y: number; }
		
		  
		
		const v2 = {
		  x: 1 as const,
		  y: 2,
		};  // Type is { x: 1; y: number; }
		
		  
		
		const v3 = {
		  x: 1,
		  y: 2,
		} as const;  // Type is { readonly x: 1; readonly y: 2; }
		```

```ts
const v = {
	x: 1, // let x = 1 => x: number
}

v.x = 4; // 정상
v.x = '4'; // ~~ '"4"' 형식은 number에 할당할 수 없습니다.
v.y = 3; // ~~ '{x: number;}' 형식에 'y' 속성이 없습니다.
```


- 튜플 사용
```ts
const a1 = [1, 2, 3];  // Type is number[]

const a2 = [1, 2, 3] as const;  // Type is readonly [1, 2, 3] 
```


### memo
- readonly는 값에 지정할 수 없다. (타입의 프로퍼티 선언, 인덱스 시그니처에 사용)