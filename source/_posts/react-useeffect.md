---
title: React useEffect常见问题
date: 2023-08-14 15:26:26
tags: [计算机, 学习]
categories:
- [计算机, React]
---
原文：https://www.freecodecamp.org/news/most-common-react-useeffect-problems-and-how-to-fix-them/
# React useEffect常见问题
例子：
```javascript
const Component = () => {
  // useUser custom hook
  const user = useUser({ id: 1 })
  return <div>{user.name}</div>
}

const useUser = (user) => {
  const [userData, setUserData] = useState();
  useEffect(() => {
    if (user) {
      fetch("users.json").then((response) =>
        response.json().then((users) => {
          return setUserData(users.find((item) => item.id === user.id));
        })
      );
    }
  }, []);

  return userData;
};
```
我们会得到一个ESLint warning：
`React Hook useEffect has a missing dependency: 'user'. Either include it or remove the dependency array. (react-hooks/exhaustive-deps)`

如果，我们加上这些dependencies，会发生什么？
```js
const useUser = (user) => {
  const [userData, setUserData] = useState();
  useEffect(() => {
    if (user) {
      fetch("users.json").then((response) =>
        response.json().then((users) => {
          return setUserData(users.find((item) => item.id === user.id));
        })
      );
    }
  }, [user]);

  return userData;
};
```

我们的Component会无止境的re-rendering！
