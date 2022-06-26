# 1 회차

## 리팩토링
- 인증 관련 비즈니스 로직 리팩토링
	- 상수로 빼기
		- /auth/
		- expires 
	- 중복 코드 및 공통 설정 뽑기
		- _setCookies()
		- headers 에 들어갈 공통 속성들 객체로 반환
			- 개별적으로 추가될 속성이 있다면 디스트럭처링하여 섞어 쓸 수 있다.
	-  RESTFUL  메서드
	- axios 타입스크립트
		- https://github.com/axios/axios/blob/7d6bddba2d8de29c263feaef4c40daa50cb4b176/index.d.ts
- 모듈 인터페이스 고민



## 노트
### [js-cookie](https://github.com/js-cookie/js-cookie)

```js
import Cookies from 'js-cookie'

Cookied.get('name');
Cookies.set('name', 'value', { expires: 7, path: '' });
Cookies.remove('name');
```

### http
[# 토근 기반 인증에서 bearer는 무엇일까?](https://velog.io/@cada/%ED%86%A0%EA%B7%BC-%EA%B8%B0%EB%B0%98-%EC%9D%B8%EC%A6%9D%EC%97%90%EC%84%9C-bearer%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)

### axios with typescript
```ts
export interface AxiosResponse<T = never> {
	data: T;
	status: number;
	statusText: string;
	headers: Record<string, string>;
	config: AxiosRequestConfig<T>;
	request?: any;
}
```



## ??
- 로그인 성공 여부에 상관 없이 무조건 setCookies 해주면 되나?
- 상수로 뺀 url 슬래시 포함?


