# React-Hooks 10

- [x] useTitle 
- [x] useInput
- [x] useClick
- [x] useFadeIn
- [x] useFullscreen
- [x] useHover
- [x] useNetwork
- [ ] useNotification
- [x] useScroll
- [x] useTabs
- [x] usePreventLeave
- [x] useConfirm
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

### 2.3 useConfirm & usePreventLeave
> useState와 useEffect를 사용하지 않는 hooks
**1.) useConfirm**
* 보통 useConfirm 은 사용자가 무언가를 하기전에 확인하는 것, 사용자가 버튼을 클릭하는 작업을 하면 이벤트를 실행하기 전에 메세지를 보여주고 싶은 것 (확인메세지 같은것)
* 순서
1. Delete the world 버튼을 클릭하게되면 onClick으로 부터 confirmDelete를 실행하게됨.
2. confirmDelete는 실제로는 confirmAction 이고 confirmAction은 브라우저에 있는 confirm을 실행하게됨.
3. true라고 하면 callback이 실행되면서 deleteWorld의 메세지가 콘솔에 나오게 된다.
* 또한 false일시 console에 aborted출력을 위해 abort함수를 만들어주었다.
* onConfirm와 onCancel 에 대한 조건문 작성
```
const useConfirm = (message = "", onConfirm, onCancel) => {
  if(!onConfirm || typeof onConfirm !== "function") {
    return;
  }
  if(onCancel && typeof onCancel !== "function"){
    return;
  }
  const confirmAction = () => {
    if(confirm(message)){
      onConfirm();
    } else {
      onCancel();
    }
  }
  return confirmAction;
}
```
**2.) usePreventLeave**
* 보통 웹사이트에 볼수 있고 window에서 나갈때 주의 메세지임
* beforeunload는 window가 닫히기 전에 function이 실행되는 것을 허락함 -> 메세지를 표시해주기 위해 사용됨.
* Protect는 addEventListener로 beforeunload와 listener을 실행하고, Unprotect는 removeEventListener로 event를 삭제시켜 메세지를 안나오게한다.
* event.returnValue = ""; 를 적어줘야 메세지가 실행됨(Chrome에서는..)
```
const usePreventLeave = () => {
  const listener = (event) => {
    event.preventDefault();
    event.returnValue = "";
  }
    const enablePrevent = () => window.addEventListener("beforeunload", listener)
    const disablePrevent = () => window.removeEventListener("beforeunload", listener)
    return { enablePrevent, disablePrevent};
  }

const App = () => {
  const {enablePrevent, disablePrevent} = usePreventLeave();
  return (  
    <div className="App">
      <button onClick = {enablePrevent}>Protect</button>
      <button onClick = {disablePrevent}>unprotect</button>
    </div>
  );
};
```
### 2.4 useBeforeLeave
* 기본적으로 탭을 닫을때 실행되는 function.
* 문서에 eventListener를 추가, mount가 되지 않았을 땐 return 해준다.
* handle 함수에 event 내부에 clientY를 event로 하고 브라우저 위로 벗어날때만 메세지가 나오게 한다.(조건 : clientY <= 0)
```
const useBeforeLeave = (onBefore) => {
  if(typeof onBefore !=="function"){
    return;
  }
  const handle = (event) => {
    const { clientY } = event;
    if(clientY <= 0){
    onBefore();
    }
  };
  useEffect(() => {
    document.addEventListener("mouseleave",handle)
    return () => document.removeEventListener("mouseleave",handle)
  }, []);

}

const App = () => {
  const begForLife = () => console.log("plz dont leave");
  useBeforeLeave(begForLife);
  return (
    <div className="App">
      <h1>Hello</h1>
    </div>
  );
};
```
### 2.5 useFadeIn & useNetwork
** 1.) useFadeIn **
* 서서히 나타나게 하는 효과, CSS로도 가능하지만 여기선 hooks + animation 으로 표현.
* element의 opactiy의 기본값은 0으로 설정.
> return {ref : element, style: {opacity: 0}};
* reference를 만들고, 리턴해준다
* App 내부에 faceInH1과 faceInP 생성 후 useFadeIn() 함수 호출
* return 할때 props 처럼 ref와 style을 반환해준다. 그리고 h1과 p 내부에서 {... } 를 사용하여 리턴값을 받아온다.
* useEffect() 내부에선 componentDidMount 시에만 동작하게 deps값을 비워준다.
* 또한 element.current 일때, opacity를 1로 해주고 transition(애니메이션 효과)를 해준다.
* 마지막으로 opacity에 대한 duration의 기본값과, delay에 대한 기본값을 useFadeIn에 적어준다.
```
const useFadeIn = (duration = 1, delay = 0) => {
  if(typeof duration !== "number" || typeof delay !== "number") {
    return;
  }
  const element = useRef();
  useEffect(() => {
    if(element.current){
      const { current } = element;
      current.style.transition = `opacity ${duration}s ease-in-out ${delay}s`;
      current.style.opacity = 1;
    }
  },[])
  return {ref : element, style: {opacity: 0}};
}

const App = () => {
  const faceInH1 = useFadeIn(1,2);
  const fadeInP = useFadeIn(5, 10);
  return (
    <div className="App">
      <h1 {...faceInH1}>Hello</h1>
      <p {...fadeInP}>helhelehelehlehle </p>
    </div>
```
** 2.) useNetwork ** 
* network의 상태가 바뀔때마다 함수를 부르게 한다.
* useNetwork 는 새로운 status를 반환하고, 내부의 useState의 기본값을 navigator.onLine으로 설정해준다.
* 그리고 navigator.onLine은 true와 false를 말할것이다.( navigator.onLine을 기본값으로 설정해줌으로써 online인지 offline인지 알수있게 되는 것 이다)
* useEffect에는 eventlistener을 추가 해주고 (addEventListener : online/offline, 또한 removeEvenetListener) , online/offline 상태이면 handleChange를 실행하게한다.
* 상태가 바뀔때마다 setStatus를 해준다.
* handleNetworkChange 은 on/offline 일때마다 메세지를 출력하기 위한 함수
```
const useNetwork = onChange => {
  const [status, setStatus] = useState(navigator.onLine);
  const handleChange = () => {
    if(typeof onChange === "function"){
      onChange(navigator.onLine)
    }
    setStatus(navigator.onLine);
  };
  useEffect(() => {
    window.addEventListener("online", handleChange);
    window.addEventListener("offline", handleChange);
    () => {
      window.removeEventListener("online", handleChange);
      window.removeEventListener("offline", handleChange);
    };
  }, []);
  return status;
};
const App = () => {
  const handleNetworkChange = (online) => {
    console.log(online?"We just went online" : "We are offline");
  }
 const onLine = useNetwork(handleNetworkChange);
  return (
    <div className="App">
      <h1>{onLine ? "Online" : "Offline"}</h1>
    </div>
 ```
### 2.6 useScroll & useFullscreen
** 1.) useScroll **
* useState의 기본 값을 x:0, y:0 이라고 한다.
* h1 태그 내에 style 중 color를 y > 100 이상일때 red로 하고 아닐땐 blue 색상이게 삼항연산자을 사용해준다. 그리고 app의 height를 1000vh 로 해주어 스크롤을 만들어준다.
> color: y> 100 ? " red" : "blue"
* onScroll 함수는 scroll의 값을 측정해 setState로 값을 지정해준다.
```
const useScroll = () => {
  const[state,setState] = useState({
    x: 0,
    y: 0
  });
  const onScroll = () => {
    setState({ y: window.scrollY, x:window.scrollX});
  }
  useEffect(() => {
    window.addEventListener("scroll", onScroll);
    return () => window.removeEventListener("scroll", onScroll);
  }, [])
  return state;
}
const App = () => {
  const { y} = useScroll();
  return (
    <div className="App" style = {{height: "1000vh" }}>
      <h1 style = {{position:"fixed", color: y> 100 ? " red" : "blue"}}>hi</h1>
    </div>
  );
};
```
** 2.) useFullScreen **
* fullscreen을 만들어 주기위한 버튼을 생성
* useFullscreen 함수를 만들어주고 내부에 두개의 함수 생성
* callback() 함수로 메세지전달
```
const useFullscreen = (callback) => {
  const element = useRef();
  const triggerFull = () => {
    if(element.current) {
      element.current.requestFullscreen();
    if(callback && typeof callback === "function")
      callback(true)
    }
  };
  const exitFull = () => {
    document.exitFullscreen();
    if(callback && typeof callback === "function")
    callback(false)
  }
  return {element, triggerFull, exitFull };
}

const App = () => {
  const onFullS = (isFull) => {
    console.log(isFull ? "We are full" : "We are small")
  }
  const {element, triggerFull, exitFull} = useFullscreen();
  return (
    <div className="App" style = {{height: "1000vh" }}>
      <div ref={element}>
      <img
        src="https://kc-media-cdn-live.azureedge.net/cache/c/2/a/8/2/4/c2a824656c882cf22ca460221a651d379a7d7f1c.jpg" 
        />
      <button onClick={exitFull}>Exit fullscreen</button> 
      </div>
      <button onClick={triggerFull}>Make fullscreen</button>    
    </div>
  );
};
```
### 2.7 useNotification
* 알람이 실행되는 function(ex: 구글 크롬 알람이나 모든 알람)
* Notification API 를 이용
1. Notification.permission은 읽기전용이다.
> default인 경우 모든 알람이 허용되지 않는다. (broweser는 value가 denied인것처럼 행동하게한다.)
```
const useNotification = (title, options) => {
  if(!("Notification" in window)) {
    return;
  }
  useEffect(() => {

  })
  const fireNotif = () => {
    if(Notification.permission !== "granted"){
      Notification.requestPermission().then(permission => {
        if(permission == " granted") {
          new Notification(title, options)
        } else {
          return;
        }
      })
    } else {
      new Notification(title, options)
    }
  };
  return fireNotif;
} 

const App = () => {
  const triggerNotif = useNotification("Can i steal your info", {
    body:"I love you don't you"});
  return (
    <div className="App" style={{ height: "1000vh"}}>
      <button onClick={triggerNotif}>Hello</button>
    </div>      
  );
};
```
