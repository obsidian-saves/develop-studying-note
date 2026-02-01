useCallback은 [[메모이제이션]]된 콜백 함수를 반환하는 Hook으로, 이는 자식 컴포넌트에 함수를 props 형태로 전달할 때 유용하다.

## 예시

```jsx
const memoizeCallback = useCallback(
    () => {
        doSomething(a, b);
    },
    [a, b],
)
```
위 코드에서, a와 b가 변경되지 않으면 useCallback이 반환하는 콜백 함수의 참조가 바뀌지 않으므로, 매 렌더링마다 새로 생성되지 않는다. 이는 useCallback이 [[디펜던시 배열]]을 기반으로 메모이제이션된 함수를 반환하기 때문이다.

## 특징

- **함수 메모이제이션**
```jsx
const memoizedFunction = useCallback(함수, 배열);
```
위 코드에서는 아무 의존성이 없기 때문에, `memoizedFunction`은 처음 생성된 이후 계속 같은 참조값을 유지한다.

- **함수 동등성**

```shell
> const add1 = () => x + y;
undefined
> const add2 = () => x + y;
undefined
> add1 === add2
false
```
동일한 로직을 가진 함수라도 각각 생성될 때 서로 다른 참조값을 갖게 될 수 있다. 이로 인해 `React.memo`같은 최적화 도구에서 불필요한 렌더링이 발생할 수 있다.

## 사용 예제
```jsx
const Parent = () => {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("Clicked!");
  }, []);

  return <Child onClick={handleClick} />;
};

const Child = React.memo(({ onClick }) => {
  console.log("Child Rendered");
  return <button onClick={onClick}>Click Me</button>;
});

```
useCallback을 사용하면 Child는 onClick의 참조가 바뀌지 않아 불필요한 렌더링을 피할 수 있다.

## 주의점

useCallback은 다음과 같은 경우에만 사용하는 것이 좋다.

- 자식 컴포넌트가 `React.memo`로 최적화가 되어 있고, props로 콜백을 전달할 때
- 렌더링마다 콜백 함수가 새로 생성되어 성능에 영향을 줄 수 있을 때

다음과 같은 경우에는 굳이 사용할 필요가 없다.

- 콜백 함수 내부에 의존성이 많아 자주 바뀌는 경우
- props로 전달하지 않고 내부에서만 사용하는 콜백일 경우