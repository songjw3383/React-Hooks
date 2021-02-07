# React-Hooks 10

- [ ] useTitle 
- [ ] useInput
- [ ] useClick
- [ ] useFadeIn
- [ ] useFullscreen
- [ ] useHover
- [ ] useNetwork
- [ ] useNotification
- [ ] useScroll
- [ ] useTabs
- [ ] usePreventLeave
- [ ] useConfirm
- [ ] useAxios
## 0207 ~
### 0.Introduction
**요구사항**
* NodeJS, NPM, React 기초지식 등등..
* codesandebox.io 에서 진행
* functional component에서 state를 가질 수 있게 해준다.
> 함수형 프로그래밍 형식으로 된다는 것.
 
### 1.0 useState
* 항상 2개의 value를 return
> 첫번째는 아이템, 두번째는 아이템을 변경
* useState(1) 의 1은 최초의 value값
* incrementItem 과 decrementItem 을 애로우 함수를 이용하여 정의후 setitem(변경) 을 이용하여 item의 +,- 시켜줌
* onClick 클릭시 {함수} 실행

```
///코드///
const App = () => {
  const [item, setitem] = useState(1);
  const incrementItem = () => setitem(item + 1);
  const decrementItem = () => setitem(item - 1);

  return (
    <div className = "App">
      <h1>Hello {item}</h1>
      <h2>Start editing to see some magic happen!</h2>
      <button onClick={incrementItem}>Increment</button>
      <button onClick={decrementItem}>Increment</button>
    </div>
  )
}
```

### 1.1 useInput
* useInput은 initialValue 값을 받음
* useInput은 value를 리턴하고, name 은 value와 같아질것이고, Input의 value는 name.value를 갖게된다. 
* unpack? {... }을 해줌으로써 코드길이를 짧고 간단하게 해줌
> value={name.value} -> {...name}
* validator 의 default는 true, validator에 대해 function을 만들어주어 value에 대하여 조건을 지정해주어 true/false 값을 매길수 있음
```
(export) const useInput = (initialValue, validator) => {
  const [value, setValue] = useState(initialValue);
  const onChange = (event) => {
    const {
      target: { value }
    } = event;
    let willUpdate = true;
    if(typeof validator ==="function"){
      willUpdate = validator(value);
    }
    if(willUpdate){
      setValue(value);
    };
  };
  return { value, onChange};
};
```
