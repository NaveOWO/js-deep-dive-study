## ⭕️ **프로미스.. 넌 뭐냐?**

자바스크립트에서 비동기적 동작을 다루는 대표적인 방법은 콜백함수를 실행하는 것이다.
콜백함수는 비동기적으로 동작하는 코드중, 전 task가 끝나가 전에 실행되지 않고 전 task가 끝남과 동시에 실행될 것을 보장한다.

![](https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/08/%E1%84%8F%E1%85%A9%E1%86%AF%E1%84%87%E1%85%A2%E1%86%A8%E1%84%8C%E1%85%B5%E1%84%8B%E1%85%A9%E1%86%A8.png?w=640&ssl=1)

- **콜백 지옥(callback hell)** 이란 콜백 함수를 익명 함수로 전달하는 과정에서 또 다시 콜백 안에 함수 호출이 반복되어 코드의 들여쓰기 순준이 감당하기 힘들 정도로 깊어지는 현상을 말한다.

- 주로 이벤트 처리나 서버 통신과 같은 비동기 작업을 제어하기 위해서 사용되는데 이러한 프로그래밍은 가독성이 떨어지고 코드 수정을 어렵게한다.

## ⭕️ **프로미스의 생성**

`Promise` 생성자 함수를 `new 연산자`와 함께 호출하면 프로미스객체를 생성한다. 프로미스 생성자 함수는 비동기 처리를 수행할 콜백함수를 인수로 전달받는데, 이 콜백함수는 `resolve`와 `reject` 함수를 인수로 전달받는다.

```javascript
const promise = new Promise((resolve, reject) => {
  // 콜백함수 내부에서 비동기 처리를 수행함
  return response; // 콜백함수의 결과
  if (response) {
    resolve(response);
  } else {
    reject("에러");
  }
});
```

-> Promise 생성자 함수가 인수로 전달받은 콜백 함수의 내부에서는 비동기 처리를 수행한다.

-> 비동기 처리가 성공하면 resolve함수가 호출되고,

-> 비동기 처리가 실패하면 reject함수가 호출된다.

```javascript
function promiseGet(url) {
  return new Promise((resolve, reject) => {
    const response = axios.get(url).then((data) => {
      return data;
    });

    if (response.status === 200) {
      // 서버에서 온 데이터가 정상적으로 받아졌을 때.
      resolve(response);
    } else {
      // 서버에서 온 데이터가 정상적이지 않을 때
      reject(new Error(response.status));
    }
  });
}

promiseGet("서버 URL");
```

비동기 함수인 promiseGet가 함수 내부에서 프로미스르 생성하고 반환한다고 가정.

비동기처리는 Promise 생성자 함수가 인수로 전달받은 콜백함수 내부에서 수행한다.

이때 만약 비동기처리가 성공하면 비동기 처리 결과를 resolve함수에 전달하면서 호출하고,

비동기 처리가 실패하면 에러를 reject 함수에 인수로 전달하면서 호출한다.

## ⭕️ **프로미스의 상태**

| 프로미스의 상태정보 | 의미                                  | 상태 변경 조건                    |
| ------------------- | ------------------------------------- | --------------------------------- |
| pending             | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후의 기본상태 |
| fulfilled           | 비동기 처리가 수행된 상태 (성공)      | resolve 함수 호출                 |
| rejected            | 비동기 처리가 수행된 상태 (실패)      | reject 함수 호출                  |
|                     |                                       |                                   |

![](https://velog.velcdn.com/images%2Flimes%2Fpost%2F8c3d30f5-6135-4c01-a520-bf4bf91d2152%2Fimage.png)

![](https://velog.velcdn.com/images/jjkim0104/post/82ce434e-305c-4e03-b4b2-f5b44c8514ba/image.png)

![](../../../../Desktop//%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-03-09%20%EC%98%A4%ED%9B%84%2011.46.15.png)

-> 비동기 처리가 성공하면 프로미스는 pending 상태에서 fulfilled상태로 변화한다. 그리고 비동기 처리 결과를 값을 갖는다.

-> 비동기 처리가 실패하면 프로미스는 pending상태에서 rejected상태로 변화한다. 그리고 비동기 처리 결과인 Error를 값을 갖는다.

✅ _**즉 프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체이다.**_

## ⭕️ **프로미스의 사용**

프로미스의 비동기 처리상태가 변화하면 이에 따른 후속 처리를 해야 한다.

이를 위해 프로미스는 후속 메서드 `then`, `catch`, `finally`를 제공한다.

### 🙌🏻 **Promise.then**

then메서드는 두 개의 콜백 함수를 인수로 전달받는다.

- 첫번째 콜백 함수는 프로미스가 `fulfilled`상태가 되면 호출된다. 이때 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달받는다.
- 두 번째 콜백 함수는 프로미스가 `rejected` 상태가 되면 호출된다. 이때 콜백 함수는 프로미스의 에러를 인수로 전달받는다.

```javascript
// fulfilled 일 때
new Promise((response) => resolve("fulfilled")).then(
  (success) => {
    console.log(success);
  },
  (error) => {
    console.log(error);
  }
); // fulfilled

//rejected 일 때
new Promise((response) => reject("error")).then(
  (success) => {
    console.log(success);
  },
  (error) => {
    console.log(error);
  } // error
);
```

-> `then`메서드는 언제나 프로미스를 반환한다.

만약 `then` 메서드의 콜백함수가 프로미스를 반환하면 그 프로미스를 그대로 반환하고, 콜백함수가 프로미스가 아닌 값을 반환하면 그 값을 암묵적으로 `resolve` 또는 `reject하여` 프로미스를 생성해 반환한다.

### 🙌🏻 **Promise.catch**

`catch메서드는` 한 개의 콜백 함수를 인수로 전달받는다. `catch` 메서드의 콜백 함수는 프로미스가 `rejected` 상태인 경우에만 호출된다.

```javascript
new Promise((error) => {
  reject(error);
}).catch((e) => {
  console.log(new Error(e));
});
// e 가 출력됨
```

### 🙌🏻 **Promise.finally**

`finally` 메서드는 한 개의 콜백 함수를 인수로 전달받는다. `finally` 메서드의 콜백 함수는 프로미스의 성공 또는 실패와 상관없이 무조건 한 번 호출된다.

```javascript
new Promise(() => {}).finally(() => console.log("finally"));
```

## ⭕️ **프로미스 체이닝**

비동기 처리를 위한 콜백 패턴은 콜백 헬이 발생하는 문제가 있다. 프로미스는 `then`, `catch`, `finally` 후속 처리 메서드를 통해 콜백 헬을 해결한다.

그렇다면 프로미스에서 순차적으로 처리되어야 하는 비동기 작업이 있다면 어떻게 하나?

프로미스는 `.then`을 연속으로 붙여서 비동기 작업을 순차적으로 처리할 수 있다.

```javascript
//서버에서 여러 게시물들 중 특정 게시물의 id에 짝지어져있는 userId 정보를 가져오고 그 userId를 이용해서 유저정보를 받아야 할 때
//그런데 게시물의 id를 주는 서버경로와, 유저정보를 주는 서버의 경로가 다르고, 유저정보를 얻으려면 게시물의 id가 들어간 url이 필요할 때

function promiseGet(url) {
  return new Promise((resolve, reject) => {
    const response = axios.get(url).then((data) => {
      return data;
    });
  });
}

promiseGet(`baseUrl/post/${게시물 아이디}`)
// return : 게시물 ID에 해당하는 userId
.then(({userId}) => promiseGet(`baseUrl/user/${userId}`))
// then의 첫번째 콜백함수 (resolve)의 인자로 promiseGet의 리턴값을 받고 결과를 얻음
.then(({userData}) => console.log(usdrData))
// 유저의 데이터가 출력됨
.catch((errer) => console.log(errer))
// 위의 여러개의 .then중에서 프로미스의 이행상태가 reject가 된다면 이곳에서 에러가 처리됨
```

`then`, `catch`, `finally` 후속처리 메서드는 콜백함수가 반환한 프로미스를 반환한다.

만약 콜백함수가 프로미스를 반환하지 않더라도 그 값을 암묵적으로 `resolve`, `reject하여` 프로미스를 생성해 반환한다.

이처럼 프로미스는 프로미스 체이닝을 통해 비동기 처리 결과를 반환받아 후속처리를 하기 때문에 비동기 처리를 위해 뎁스가 깊어지는 콜백지옥이 발생하지 않는다.

## ⭕️ **프로미스의 정적 메서드**

`Promise는` 주로 생성자 함수로 사용되지만 함수도 객체이기 때문에 메서드를 가질 수 있다.
`Promise는` 정적 메서드를 제공한다.

### 🙌🏻 **Promise.resolve & Primise.reject**

`Promise.resolve` & `Primise.reject`는 이미 존재하는 값을 리팽하여 프로미스를 생성하기 위해 사용된다.

- `Promise.resolve` 메서드는 인수로 전달받은 값을 `resolve하는` 프로미스를 생성한다.

```javaScript
Promise.resolve({프로미스가 fulfilled 상태일 때의 반환값})

new Promise((resolve) => resolve({프로미스가 fulfilled 상태일 때의 반환값}))
```

- `Promise.reject` 메서드는 인수로 전달받은 값을 `reject` 프로미스를 생성한다.

```javaScript
Promise.reject({프로미스가 rejected 상태일 때의 반환값})

new Promise((reject) => reject({프로미스가 rejected 상태일 때의 반환값}))
```

이 외에도 `Promise.all`, `Promise.race`, `Promise.allSettled` 등의 정적 메서드가 있다.
