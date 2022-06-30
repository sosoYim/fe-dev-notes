> 요약
> -  타입 별칭을 사용해도 되지만 일관성 있게 사용해야 한다.
> - 


## Intro
> **타입 좁히기**란? null 체크처럼 넓은 타입으로부터 좁은 타입으로 진행하는 과정을 말한다.

별칭을 썼으면 별칭을 계속 써라.
```ts
interface BoundingBox {
  x: [number, number];
  y: [number, number];
}
  

interface Polygon {
  exterior: Coordinate[];
  holes: Coordinate[][];
  bbox?: BoundingBox;
}

function isPointInPolygon(polygon: Polygon, pt: Coordinate) {

  const box = polygon.bbox;

  if (polygon.bbox) {
    if (pt.x < box.x[0] || pt.x > box.x[1] ||
        //     ~~~                ~~~  Object is possibly 'undefined'
        pt.y < box.y[1] || pt.y > box.y[1]) {
        //     ~~~                ~~~  Object is possibly 'undefined'
      return false;

    }

  }
  // ...
}
```

Polygon 안에 정의된 선택 타입 bbox는 BoundingBox이거나 undefined이다.

if 조건문을 통해 polygon.bbox, 즉 bbox가 BoundingBox인 것으로 타입을 좁혔지만, 외부에서 별개의 식별자로 별칭을 지정한 box는 타입이 좁혀지지 않았다.

따라서 별칭을 쓰려면 별칭만 쓰고, 기본 타입을 사용하려면 기본 타입을 기준으로 다 하라.

이 문제를 해결하기 위한 다른 간단한 방법으로 구조분해 할당도 있다.

(추가)
134p 선택적 속성일 경우 함께 다니는 속성이라면 이렇게 하라
type  coord = {x: number; y: number} | null;

```ts
function fn(p: Polygon) { /* ... */ }

  

polygon.bbox  // Type is BoundingBox | undefined

if (polygon.bbox) {
  polygon.bbox  // Type is BoundingBox
  fn(polygon);
  polygon.bbox  // Type is still BoundingBox

}

```

함수를 거쳐왔을 때 polygon이 바뀔 수도 있으니 위와 같은 상황에서는 별칭을 빼는 것이 좋다.
```ts
function fn(p: Polygon) { /* ... */ }

  

polygon.bbox  // Type is BoundingBox | undefined

const { bbox } = polygon;

if (bbox) {

  polygon.bbox  // Type is BoundingBox
  fn(polygon);
  polygon.bbox  // Type is still BoundingBox

}
```