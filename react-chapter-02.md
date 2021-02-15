# React Chapter 02

[https://react.vlpt.us/basic/11-render-array.html](https://react.vlpt.us/basic/11-render-array.html)

## 배열 렌더링

```text
const users = [
  {
    id: 1,
    username: 'velopert',
    email: 'public.velopert@gmail.com'
  },
  {
    id: 2,
    username: 'tester',
    email: 'tester@example.com'
  },
  {
    id: 3,
    username: 'liz',
    email: 'liz@example.com'
  }
];
```

이러한 배열이 존재할때,

첫번째 출력 방법

```text
 <div>
      <div>
        <b>{users[0].username}</b> <span>({users[0].email})</span>
      </div>
      <div>
        <b>{users[1].username}</b> <span>({users[1].email})</span>
      </div>
      <div>
        <b>{users[2].username}</b> <span>({users[1].email})</span>
      </div>
    </div>
```

재사용 되는 코드를 줄이기 위하여 업그레이드 하면,

```text
function User({ user }) {
  return (
    <div>
      <b>{user.username}</b> <span>({user.email})</span>
    </div>
  );
}

.
.
.

<div>
      <User user={users[0]} />
      <User user={users[1]} />
      <User user={users[2]} />
</div>
```

아예 객체로 처리해버림,

여기에서 배열 하나하나의 인덱스를 조회해가며 렌더링 방법을 업그레이드 하면,

```text
<div>
      {
          users.map(user => (
            <User user={user} />
      ))}
</div>
```

key value를 가지는 map으로 변환하여 출력,

> 리액트에서 배열을 렌더링 할 때에는 `key` 라는 props 를 설정해야합니다. `key` 값은 각 원소들마다 가지고 있는 고유값으로 설정을 해야합니다. 지금의 경우엔 `id` 가 고유 값이지요.
>
> 만약 배열 안의 원소가 가지고 있는 고유한 값이 없다면 `map()` 함수를 사용 할 때 설정하는 콜백함수의 두번째 파라미터 `index` 를 `key` 로 사용하시면 됩니다.
>
> [https://react.vlpt.us/basic/11-render-array.html](https://react.vlpt.us/basic/11-render-array.html)

```text
<div>
      {users.map(user => (
        <User user={user} key={user.id} />
      ))}
</div>
```

user.id를 key 값으로 설정한 경우

```text
<div>
  {users.map((user, index) => (
    <User user={user} key={index} />
  ))}
</div>
```

원소의 고유 값이 존재하지 않을 때 index를 이용하여 key값을 지정한 경우

이러한 배열이 존재한다고 가정할때,

```text
const array = ['a', 'b', 'c', 'd'];
```

```text
array.map(item => <div>{item}</div>);
```

이렇게 렌더링 한다면, 즉 키가 없는 상태로 렌더링을 하게 된다면

![img](https://i.imgur.com/3rkaiY1.gif)

위의 사진과 같이 기존 태그사이에 삽입을 하는 것이 아닌, 이미 존재하는 태그에 있던 값을 바꾸는과정을 거침

키가 있을 경우 렌더링 방식이 달라짐.

```text
array.map(item => <div key={item.id}>{item.text}</div>);
```

수정되지 않는 기존의 값은 그대로 두고 원하는 곳에 내용을 삽입하거나 삭제함.

![img](https://i.imgur.com/yEUS6Bx.gif)

## UseRef 로 컴포넌트 이용하여 배열 다루기

참고

[https://www.daleseo.com/react-hooks-use-ref/](https://www.daleseo.com/react-hooks-use-ref/)

`UseRef` 란 ? 리액트 컴포넌트는 기본적으로 내부 상태가 변할 때 마다 다시 랜더링이 됩니다. 컴포넌트 함수가 다시 호출이 된다는 것은 함수 내부의 변수들이 모두 다시 초기화가 되고 함수의 모든 로직이 다시 실행된다는 것을 의미합니다.그러나 이러한 초기화의 부작용은 바로 함수 내부의 변수들이 기존에 저장하고 있는 값들을 잃어버리고 초기화 된다는 것이다.

**다시 렌더링이 되더라도 기존에 참조하고 있던 컴포넌트 함수내의 값이 그대로 보존되야 하는 상황** 

이러한 상황을 구현하기 위해 나온 것이 바로, UseRef 함수입니다.

> `useRef` 함수는 `current` 속성을 가지고 있는 객체를 반환하는데, 인자로 넘어온 초기값을 `current` 속성에 할당합니다. 이 `current` 속성은 값을 변경해도 상태를 변경할 때 처럼 React 컴포넌트가 다시 랜더링되지 않습니다. React 컴포넌트가 다시 랜더링될 때도 마찬가지로 이 `current` 속성의 값이 유실되지 않습니다.
>
> [https://www.daleseo.com/react-hooks-use-ref/](https://www.daleseo.com/react-hooks-use-ref/)

또한 리액트 컴포넌트에서의 상태는 상태를 바꾸는 함수를 호출하고 나서 그 다음 랜더링 이후로 업데이트 된 상태를 조회 할 수 있는 반면, `useRef`로 관리하고 있는 변수는 설정 후 바로 조회 할 수 있습니다.

이 변수를 사용하여 다음과 같은 값을 관리 할 수 있습니다.

* `setTimeout`, `setInterval` 을 통해서 만들어진 `id`
* 외부 라이브러리를 사용하여 생성된 인스턴스
* scroll 위치

목표 : App 컴포넌트에서 `useRef` 를 사용하여 변수를 관리하는 것 , 배열에 새 항목을 추가할때 새항목에서 사용할 고유 id를 관리하는 용도

준비작업 : userlist.js 내부에 있던 배열을 app으로 위치시키고 app에서 userlist에게 props로 전달해주는 것

```text
 const nextId = useRef(4);
 //nextId 라는 변수 만들기
```

comments

Q.

이런식의 선언은 왜 안되는 건가요?

A.

## 배열에 항목 추가하기

만들고 싶은 것

name 과 e-mail을 적고 등록하면, 현재 존재하던 데이터 다음에 추가되는 페이지

```text
//CreateUser.js
//이 컴포넌트에서는 상태관리를 하지 않습니다
//App.js에서 넘겨주는 것들을 출력하는 역할만 함.
import React from 'react';

function CreateUser({ username, email, onChange, onCreate }) {
  return (
    <div>
      <input
        name="username"
        placeholder="계정명"
        onChange={onChange}
        value={username}
      />
      <input
        name="email"
        placeholder="이메일"
        onChange={onChange}
        value={email}
      />
      <button onClick={onCreate}>등록</button>
    </div>
  );
}

export default CreateUser;
```

```text
//App.js
//배열에 항목 추가하는 기본 뼈대 code

import React, { useRef, useState } from 'react';
import UserList from './UserList';
import CreateUser from './CreateUser';

function App() {
  //추가할 항목을 초기화 하는 객체
  const [inputs, setInputs] = useState({
    username: '',
    email: ''
  });
  const { username, email } = inputs;

  //event Handler 
  // e.target에 있는 값이 name,value로 들어간다 다음 input값을 복사하여 처리
  const onChange = e => {
    const { name, value } = e.target;
    setInputs({
      ...inputs,
      [name]: value
    });
  };
  const [users, setUsers] = useState([
    {
      id: 1,
      username: 'velopert',
      email: 'public.velopert@gmail.com'
    },
    {
      id: 2,
      username: 'tester',
      email: 'tester@example.com'
    },
    {
      id: 3,
      username: 'liz',
      email: 'liz@example.com'
    }
  ]);

  const nextId = useRef(4);
  const onCreate = () => {
    // 나중에 구현 할 배열에 항목 추가하는 로직
    // ...

    setInputs({
      username: '',
      email: ''
    });
    //일단 초기화 먼저

    nextId.current += 1;
  };
  return (
    <>
      <CreateUser
        username={username}
        email={email}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} />
    </>
  );
}

export default App;
```

이제 여기서 배열에 변화를 주어보자,

배열에 변화를 줄 때에는 객체와 마찬가지로 불변성을 지켜주어야 한다. 그렇기 때문에 배열의 `push`, `splice`, `sort` 등의 함수를 사용하면 안된다. 만약에 사용할 경우 기존의 배열을 복사하여 사용하는 것이 바람직하다.

불변성을 지키면서 배열에 새항목을 추가하는 방법은 두가지이다.

### Spread 연산자

```text
const onCreate = () => {
    const user = {
      id: nextId.current,
      username,
      email
    };
    setUsers([...users, user]);

    setInputs({
      username: '',
      email: ''
    });
    nextId.current += 1;
  };
```

spread 문법을 사용하면 기존의 배열을 건드리지 않고도 새로운 배열을 만들어 내어 추가할 수 있기 때문에

```text
const animals = ['개', '고양이', '참새'];
const anotherAnimals = [...animals, '비둘기'];
//['개', '고양이', '참새']
//['개', '고양이', '참새','비둘기']
```

### Concat 함수

```text
const onCreate = () => {
    const user = {
      id: nextId.current,
      username,
      email
    };
    setUsers(users.concat(user));

    setInputs({
      username: '',
      email: ''
    });
    nextId.current += 1;
  };
```

## 배열에 항목 제거하기

```text
//UserList.js

import React from 'react';

function User({ user, onRemove }) {
  return (
    <div>
      <b>{user.username}</b> <span>({user.email})</span>
      <button onClick={() => onRemove(user.id)}>삭제</button>
    </div>
  );
}

function UserList({ users, onRemove }) {
  return (
    <div>
      {users.map(user => (
        <User user={user} key={user.id} onRemove={onRemove} />
      ))}
    </div>
  );
}

//userList 에 컴포넌트마다 onRemove 버튼을 할당 한다

export default UserList;
```

```text
//App.js

 const onRemove = id => {
    // user.id 가 파라미터로 일치하지 않는 원소만 추출해서 새로운 배열을 만듬
    // = user.id 가 id 인 것을 제거함
    setUsers(users.filter(user => user.id !== id));

  };
  return (
    <>
      <CreateUser
        username={username}
        email={email}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} onRemove={onRemove} />
    </>
  );
```

```text
console.log(user.filter(user => user.id !== id))

###
(3) [Object, Object, Object]
0: Object
id: 1
username: "velopert"
email: "public.velopert@gmail.com"
1: Object
id: 5
username: ""
email: "ok@gmail.com"
2: Object
id: 6
username: ""
email: ""
###
```

삭제하는 id가 아닌 것만 가져와서 set 한다.

## 배열에 항목 수정하기

목표 : 클릭하면 계정명이 초록색으로 변하는 기능

```text
//UserList.js

import React from "react";

function User({ user, onRemove, onToggle }) {
  return (
    <div>
      <b
        style={{
          cursor: "pointer",
          color: user.active ? "green" : "black"
          /** user.active === true or false  **/
        }}
        onClick={() => onToggle(user.id)
        /** onClick 함수 실행시 가리키는 user.id 를 param으로 보낸다 **/
        }
      >
        {user.username}
      </b>{" "}
      <span>({user.email})</span>
      <button onClick={() => onRemove(user.id)}> 삭제 </button>
    </div>
  );
}

function UserList({ users, onRemove, onToggle }) {
  return (
    <div>
      {users.map((user) => (
        <User
          user={user}
          key={user.id}
          onRemove={onRemove}
          onToggle={onToggle}
        />
      ))}
    </div>
  );
}

export default UserList;
```

```text
const onToggle = (id) => {
    setUsers(
      users.map((user) =>
        user.id === id
          ? {
              ...user,
              active: !user.active
            }
          : user

      )
    );
  };
```

user.id === id -&gt; True 일 경우 ...user 에 있는 active 라는 변수를 반전시켜주고,

False 일 경우 user를 그대로 냅둔다.

