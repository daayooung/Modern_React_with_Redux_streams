# React Router

1. 사용자가 다른 page로 이동하기 위해 'Link' tag를 클릭한다.
2. Browser가 새 page로 이동하고 새 index.html을 가져오는 것을 React Router가 막는다.
3. URL이 바뀐다.
4. 'History'는 업데이트 된 URL을 보고, 그것을 BrowserRouter로 보낸다.
5. browserRouter가 route Components에 URL을 전달한다.
6. 새 component를 보여주기 위해 Route Components 가 render된다.

# type을 변수화 하면 오타로 인한 error를 빨리 잡을 수 있다.

<br/>

> client > src > actions > types.js

<br/>

```js
export const SIGN_IN = 'SIGN_IN';
export const SIGN_OUT = 'SIGN_OUT';
```

<br/>

> client > src > actions > authReducer.js

<br/>

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

<br/>

> reducers > streamReducer.js

<br/>

# Obect에서 특정 key 삭제

### import \_ from 'lodash';

객체를 대상으로 하여 특정 키나 값을 제거하는 함수. 기존 객체에서 새로운 객체를 생성하여 반환한다.

```
\_.omit(state, action.payload)cc
```

<br/>

> [326] Merging Lists Of Records

<br/>

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

<br/>

> [343] Selecting Recodes from State
> client > src > commponents > streams > StreamEdit.js

# mapStateToProps의 parameter

### 첫번째 parameter : state

##### redux store의 모든 state

```js
{auth: {…}, form: {…}, streams: {…}}

auth:
isSignedIn: true
userId: "108666364830521293464"
__proto__: Object
form:
__proto__: Object
streams:
__proto__: Object
__proto__: Object
```

### 두번째 parameter : ownProps

##### component 안의 props

```js
{history: {…}, location: {…}, match: {…}, staticContext: undefined}

// history
history:
action: "POP"
block: ƒ block(prompt)
createHref: ƒ createHref(location)
go: ƒ go(n)
goBack: ƒ goBack()
goForward: ƒ goForward()
length: 10
listen: ƒ listen(listener)

// location
location: {pathname: "/streams/edit/>3", search: "", hash: "", state: undefined}
push: ƒ push(path, state)
replace: ƒ replace(path, state)
__proto__: Object
location:
hash: ""
pathname: "/streams/edit/>3"
search: ""
state: undefined
__proto__: Object

// match
match:
isExact: true
params: {id: ">3"}
path: "/streams/edit/:id"
url: "/streams/edit/>3"
__proto__: Object
staticContext: undefined
__proto__: Object
```

> [344] Component Isolation with React Router
> client > src > commponents > streams > StreamEdit.js

사용자가 애플리케이션을 처음 load 할 때 redux state 객체는 아직 읽어들인 stream 없이 비어있다. 그러다 stream 목록 중 하나를 클릭하면 그 때 state 값이 들어오는데, 이는
streams를 호출하는 함수가 componentDidMount 안에 있기 때문이다. (화면에 stream을 표시하는 순간 모든 다른 stream의 목록을 가져온다.)

따라서 사용자가 북마크를 해서 http://localhost:3000/streams/edit/3 에 바로 접속할 경우 stream 정보가 들어있지 않은 페이지를 보게 된다.

그러므로 React-router를 사용하면 각 component가 격리되어 작동하도록 설계해야 한다. (자체 data를 fetch)

##### StreamList.js

```js
  componentDidMount() {
    this.props.fetchStreams();
  }
```

<br/>

> [348] Setting Inintial Values
> client > src > components > streams > StreamEdit.js

# Edit 화면 초기 data 설정

### reduxForm 의 initialValues로 설정

StreamForm component의 <Field/> 의 name 속성을
StreamEdit component의 <StreamForm/>의 initialValues 속성에서 설정해준다.

##### StreamForm component

```js
  return (
    <form
      onSubmit={this.props.handleSubmit(this.onSubmit)}
      className="ui form error">
      <Field name="title" component={this.renderInput} label="Enter Title" />
      <Field
        name="description"
        component={this.renderInput}
        label="Enter Description"
      />
      <button className="ui button primary">Submit</button>
    </form>
```

<br/>

##### StreamEdit component

```js
<StreamForm
  initialValues={{
    title: this.props.stream.title,
    description: this.props.stream.description,
  }}
  onSubmit={this.onSubmit}
/>
```

> [349] Avoiding Changes to Properties
> client > src > components > streams > StreamEdit.js

formValues : console.log로 찍어보면, 변경하고자 하는 속성인 title, description만 포함되어야 하는데 userId와 id도 같이 넘어오고 있다.

redux state에서 streams의 기본값이 title, description, userId, id이기 때문에 redux form이 userId, id가 있어야 하는 값이라고 간주했기 떄문이다.

우리가 원하는 data인 title, description만 전달하기 위해 다음 두가지 방식을 사용할 수 있다.

##### 1.

```js
<StreamForm
  initialValues={{
    title: this.props.stream.title,
    description: this.props.stream.description,
  }}
  onSubmit={this.onSubmit}
/>
```

<br/>

=

<br/>

##### 2. use lodash

```js
import _ from 'lodash';

<StreamForm
  initialValues={_.pick(this.props.stream, 'title', 'description')}
  onSubmit={this.onSubmit}
/>;
```

> [351] PUT cs PATCH request
> client > src > actions > index.js

### PUT Method : Update all properties of a record

### PATCH Method : Update some propertied of record

PUT으로 요청하면 모든 property들을 update하는데, 수정한 data에 userId와 id 정보가 들어있지 않으므로 더이상 필요하지 않다고 간주하여 수정한 stream의 userId와 id 정보가 사라져 버린다. (edit, delete 버튼이 사라진다.) 그러므로 PUT이 아닌 PATCH로 API를 요청한다.

```js
export const editStream = (id, formValues) => async (dispatch) => {
  const response = await streams.put(`/streams/${id}`, formValues);

  dispatch({ type: EDIT_STREAM, payload: response.data });
  history.push('/');
};
```

<br/>

=>

<br/>

```js
export const editStream = (id, formValues) => async (dispatch) => {
  const response = await streams.patch(`/streams/${id}`, formValues);

  dispatch({ type: EDIT_STREAM, payload: response.data });
  history.push('/');
};
```

> [355] Hiding a Modal
> client > src > components > Modal.js

# Modal : modal창 외부 click시 modal창 닫기

```js
import history from '../history';

const Modal = (props) => {
  return ReactDOM.createPortal(
    <div
      // modal창 외부 click시 url '/' 로 이동
      onClick={() => history.push('/')}
      className="ui dimmer modals visible active">
      <div
        // e.stopPropagation() : event bubbling 방지
        // modal 내부 click시 창 닫히지 않게 처리
        onClick={(e) => e.stopPropagation()}
        className="ui standard modal visivle active">
        <div className="header">Delete Stream</div>
        <div className="content">
          Are you sure you want to delete this stream?
        </div>
        <div className="actions">
          <button className="ui primary button">Delete</button>
          <button className="ui button">Cancle</button>
        </div>
      </div>
    </div>,
    document.querySelector('#modal')
  );
};
```

> [358] OnDismiss From the Parent
> Modal : 하드코딩 => 재사용가능한 component

##### StreamDelete.js (modal의 부모 component)

화면에 보여줄 정보들을 props로 내려준다. StreamDelete.js => Modal.js

```js
import history from '../../history';

const StreamDelete = () => {
  const actions = (
    <>
      <button className="ui button negative">Delete</button>
      <button className="ui button">Cancle</button>
    </>
  );
  return (
    <div>
      StreamDelete
      <Modal
        title="Delete Stream"
        content="Are you sure you want to delete this stream?"
        actions={actions}
        onDismiss={() => history.push('/')}
      />
    </div>
  );
};
```

<br/>

##### Modal.js

```js
const Modal = (props) => {
  return ReactDOM.createPortal(
    <div onClick={props.onDismiss} className="ui dimmer modals visible active">
      <div
        onClick={(e) => e.stopPropagation()}
        className="ui standard modal visivle active">
        <div className="header">{props.title}</div>
        <div className="content">{props.content}</div>
        <div className="actions">{props.actions}</div>
      </div>
    </div>,
    document.querySelector('#modal')
  );
};
```
