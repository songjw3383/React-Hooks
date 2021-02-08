# React-Hooks 10

- [x] useTitle 
- [x] useInput
- [x] useClick
- [ ] useFadeIn
- [ ] useFullscreen
- [ ] useHover
- [ ] useNetwork
- [ ] useNotification
- [ ] useScroll
- [x] useTabs
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
import React, { useState } from "react";
import ReactDOM from "react-dom";

import "./styles.css";

const content = [
  {
    tab: "Section 1",
    content: "I'm the content of the Section 1"
  },
  {
    tab: "Section 2",
    content: "I'm the content of the Section 2"
  }
];

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
### 2.0 Introduction to useEffect
* useEffect는 componentDidmount의 역할을 해서 새로고침할때 마다 Hello 출력, 또한 componentDidUpdate의 역할도 하기 때문에 number과 aNumber버튼을 누를때마다 sayHello도 실행하게 됨
```
const sayHello = () => console.log("hello");
  useEffect(() => {
    sayHello();
    ...
```    
* useEffect는 2개의 인자를 받는다
1. 첫번째는 function으로써의 effect
2. 두번째는 dependency, 만약 deps가 있다면 effect는 deps리스트에 있는 값일 때만 값이 변하도록 활성화 될것이다. ->ComponentWillUpdate
```
//2번 예시//
const App = () => {
const sayHello = () => console.log("hello");
const [number, setNumber] = useState(0);
const [aNumber, setAnumber] = useState(0);
useEffect(sayHello, [number]);

return (
  <div className = "App">
    <div>Hi</div>
    <button onClick={() => setNumber(number + 1)}>{number}</button>
    <button onClick={() => setAnumber(aNumber + 1)}>{aNumber}</button>
 </div>
);
};
```
> useEffect는 기본적으로 sayHello를 출력, 그리고 deps리스트에 number값만 존재하기때문에 number클릭시에만 hello를 출력. (aNumber은 리스트에 없기때문에 hello를 출력하지 않음)
* 즉, useEffect는 3가지다. **ComponentDidMount, ComponentWillUnMount, ComponentDidUpdate**

### 2.1 useTitle
* 문서의 제목을 업데이트 시켜주는걸 담당하는 hooks
* 제목을 업데이트할 수 있게 setTitle을 리턴
* App내부에서 useTitle의 디폴트 값을 Loading... 으로 해줌
* updateTitle 함수를 만들어서 HTML tage의 title을 얻어옴 -> document.querySelector("title). 그리고 htmlTitle.innerText 는 title(첫번째 인자) 로 해줌.
* 그 후, useEffect는 component가 마운트 될때 updateTitle을 부른다. 그리고 title이 업데이트 되면 updateTitle을 다시부른다.
* **정리하면,**
1. 초기값은 loading이며 title도 loading 이다. 그리고 useEffect가 마운트되면 html.innerText도 loading이 되는 것.
2. 그리고 어디선가 titleUpdater을 실행하여 변화를 주면 deps의 값인 title이 바뀌면서 전체적으로 새로고침해주어 title값이 변경되는것이다.
```
const useTitle = (initialTitle) => {
  const [title, setTitle] = useState(initialTitle);
  const updateTitle = () => {
    const htmlTitle = document.querySelector("title")
    htmlTitle.innerText = title;
  };
  useEffect(updateTitle,[title]);
  return setTitle;
}

const App = () => {
  const titleUpdater = useTitle("Loading...");
  setTimeout(() =>titleUpdater("Home"), 5000); //타임아웃 5초 후 Home으로 바뀌도록 지정
  return (
    <div className="App">
      <div>Hi</div>
    </div>
  )
};
```
### 2.2 useClick
* useClick은 매우 간단한 hooks 
* references란? 기본적으로 component의 어떤 부분을 선택할 수 있는 방법. (document.getElementById()를 사용한것과 동등)
**recap**
1. useClick을 사용해 useRef()를 만들고 같은 refernece를 return (return element)
2. reference를 title에 줌(h1태그에)
3. 그리고 useEffect()는 referent 안에 element.current가 있는지 확인하는 것, 그리고 조건이 만족되면 click이벤트를 부여하기로 설정함

* useEffect() 가 mount 되었을때 첫번째 if문이 실행됨. 그리고 function을 리턴 받았다면 그 function은 componentWillUnMount로 부터 호출된것
* 즉, 첫번째 component가 mount되면  addEvenetListener작동! 그리고 deps가 없기 때문에 componentDidMount때 단 한번만 실행되라는 의미가 된다. 그리고 2번째 function을 return 할건데, 이것은 componentWillUnMount때 호출될것이다. (두번째를 하는 이유는? component가 마운트되지 않았을때 eventListener가 배치되게 하고 싶지 않기 때문)
```
const useClick = (onClick) => {
  if (typeof onClick !== "function") {
    return;
  }
  const element = useRef();
  useEffect(() => {
    if (element.current) {
      element.current.addEventListener("click", onClick);
    }
    return () => {
      if (element.current) {
        element.current.removeEventListener("click", onClick);
      }
    };
  }, []);
  return element;
};

const App = () => {
  const sayHello = () => console.log("say hello");
  const title = useClick(sayHello);
  return (
    <div className="App">
      <h1 ref={title}>Hi</h1>
    </div>
  );
};
```
