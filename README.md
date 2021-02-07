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
### 1.2 useTabs 
* content의 정보를 tab/content로 구성된 배열로 각각 지정후 <button>{section.tab}</button>을 사용하여 버튼표현
* 각각의 section content를 보여주기 위함이 목적
* changeItem으로 자동으로 버튼을 클릭하게 되면 section이 바뀌게 만들고 기본적으로 setCurrentIndex 를 가짐
* content.map 에서 section과 index가 있음, 그리고 버튼의 onClick에 changeItem을 함수로 만들어줬음
> 클릭할시 index가 무엇이든간에 changeItem(index)를 실행시켜주고 이것은 setCurrentIndex의 item을 바꿔줄것이다. 또한 이는 State를 바꿔줌 그리고 currentItem의 currentIndex를 바꿔줄것이다.
```
///코드///
const useTabs = (initialTab, allTabs) => {
  if(!allTabs || !Array.isArray(allTabs)) {
    return;
  }
  const [currentIndex, setCurrentIndex] = useState(initialTab);
  return {
    currentItem: allTabs[currentIndex],
    changeItem: setCurrentIndex
  };
};

const App = () => {
  const { currentItem, changeItem } = useTabs(0, content);
  return (
    <div className="App">
      {content.map((section, index) => (
        <button onClick={() => changeItem(index)}>{section.tab}</button>
      ))}
      <div>{currentItem.content}</div>
    </div>
  );
};
```
