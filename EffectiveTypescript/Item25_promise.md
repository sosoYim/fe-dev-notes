> 요약
> -  promise, async await
> -  promise보다 async 를 많이 사용하는게 좋다. promise 의 최종 resolve 반환값을 타입으로 알아서 추론해주기 때문


## Intro
> 


```ts
function timeout(millis: number): Promise<never> {
  return new Promise((resolve, reject) => {
     setTimeout(() => reject('timeout'), millis);
  });
}

  

async function fetchWithTimeout(url: string, ms: number) {

  return Promise.race([fetch(url), timeout(ms)]);

}
```

timeout 시간보다 늦게 오면 아예 패치하지 않게 기횔할 경우 흔히 사용하는 패턴

공집합에는 어떤걸 합쳐