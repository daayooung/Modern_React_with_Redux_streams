# React Router

1. 사용자가 다른 page로 이동하기 위해 'Link' tag를 클릭한다.
2. Browser가 새 page로 이동하고 새 index.html을 가져오는 것을 React Router가 막는다.
3. URL이 바뀐다.
4. 'History'는 업데이트 된 URL을 보고, 그것을 BrowserRouter로 보낸다.
5. browserRouter가 route Components에 URL을 전달한다.
6. 새 component를 보여주기 위해 Route Components 가 render된다.

# type을 변수화 하면 오타로 인한 error를 빨리 잡을 수 있다.

- client > src > actions > types.js

```js
export const SIGN_IN = 'SIGN_IN';
export const SIGN_OUT = 'SIGN_OUT';
```

<br/>

- client > src > actions > authReducer.js

```js
export default (state = INITAL_STATE, action) => {
  switch (action.type) {
    case SIGN_IN:
      return { ...state, isSignedIn: true };
    case SIGN_OUT:
      return { ...state, isSignedIn: false };
    default:
      return state;
  }
};
```

# object에 항목 추가

```js
const animalSounds = { cat: 'meow', dog: 'bark' };
const animal = 'lion';
const sound = 'roar';

{...animalSounds, [animal]:sound} // {cat: "meow", dog: "bark", lion: "roar"}
{...object원본, [key]:value}
```

> reducers > streamReducer.js

# Obect에서 특정 key 삭제

### import \_ from 'lodash';

객체를 대상으로 하여 특정 키나 값을 제거하는 함수. 기존 객체에서 새로운 객체를 생성하여 반환한다.

```
\_.omit(state, action.payload)cc
```

> [326] Merging Lists Of Records

# Array -> Obect

### mapKeys

```js
const streams = [
  { id: 12, title: 'My Stream', description: 'My Stream' },
  { id: 35, title: 'My Stream', description: 'My Stream' },
];

_.mapKeys(streams, 'id');

{
  12: { id: 12, title: 'My Stream', description: 'My Stream' },
  35: { id: 35, title: 'My Stream', description: 'My Stream' },
};
```

# idpiframe_initialization_failed 에러

https://wooooooak.github.io/error/2018/07/25/%EC%86%8C%EC%85%9C-%EB%A1%9C%EA%B7%B8%EC%9D%B8-idpiframe_initialization_failed-%EC%97%90%EB%9F%AC/

크롬 설정 -> third party cookies enabled(타사 쿠키 활성화)
