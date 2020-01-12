# React



## 0. 리액트는 무엇인가

- 현재의 web은 web application
- button을 눌러 텍스트를 변경하려면, DOM에 접근하여 해당 DOM을 변경해야 함

##### DOM에 직접 접근하여 값 변경

- 사용자와 interaction이 없다면 만들 수는 있음
- 편의성이 떨어짐 ... 생산성, 유지보수성 ↓

```html
<div>
    <h1 id="number">0</h1>
	<button id="increase">+</button>
</div>

<script>
	let num = document.getElementById('id');
    
    btnIncrease.onclick = function(){
        num++;
        elNumber.innerText = number;
    }
</script>
```



#### Angular

- 다양한 기능이 이미 내장되어있음
  - httpClient, 다국어 지원, router 등이 내장되어있거나 라이브러리 존재
- 기존의 Angular 1으로 작성된 코드들은 리모델링 중일 것
- 프레임워크 차원에서는 성숙 / 인지도는 성장 중
- typescript 기반



##### React

- Component 개념에 집중된 <라이브러리>
  - 프레임워크가 X
  - 데이터를 넣으면 지정한 interface를 조립하여 보여줌
- 사용자에게 전달되는 view만 신경쓰고, 나머지는 third party에서 처리



##### Vue

- 입문자가 사용하기 쉽고 편함
- 단순 cdn 불러와 사용하기 (html에서 script tag로 불러오기)
- 공식 router 및 공식 상태 관리 library가 존재
- Angular의 directive / React의 virtual dom 기반 component 등이 존재
- React와 Angular의 장점을 몇 가지 짬뽕한 느낌?



### 리액트의 Virtual DOM

---

```
  지속해서 데이터가 변화하는 대규모 애플리케이션을 구축하기 위해 리액트를 만들었다.
```

- MVC, MVVM, MVW 등을 담당하던 기존 웹 프레임워크 / 라이브러리
  - Model : 데이터단 담당
  - View : 화면 처리 및 담당
  - Controller : 사용자가 발생시키는 이벤트 관리

- 타 기술들은 양방향 바인딩을 통해 Model을 처리했음

  - 특정 이벤트가 발생했을 때 Model의 변화를 일으키고, 어떤 View를 가져올 지 로직을 정해줌

```
  그냥 Mutation을 하지 말고, 데이터가 바뀌면 기존 view를 날려버리고 새로 만들어버리면 어떨까?
  
  다만, 새로운 뷰를 계속 만들면 성능적으로 문제가 큼!
```



#### Virtual DOM

---

<img src = 'data:image/png;base64,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'>

- js로 이루어진 가상의 DOM에 한번 rendering하고, 기존의 DOM과 비교한 후 변화가 있던 곳에만 update하는 방식으로 뷰를 변경

```
virtual DOM 추가설명 : https://velopert.com/3236
영상 : https://www.youtube.com/watch?v=muc2ZF0QIO4&feature=youtu.be
```

```
Model이 View에게,
 username, points, shape+color 정보를 입력 >> View는 빈 DOM에 paint()
 											View는 빈 DOM에 paint하는 것을 빠르게 잘 함

이후 Model이 View에게
 username update 요청 	>> new rule 생성 및 div.username 저장  	>> view 변경
 points update 요청 		>> new rule 생성 및 div.points 저장		>> view 변경
 color update 요청		>> new rule 생성 및 new DOM Node(div.shape) 저장	>> view 변경

 계속해서 변경 요청이 들어오며 너무 많은 rule(update 규칙)을 기억해야 했고,
 가끔식 업데이트 순서가 틀리면 UI Event에 영향을 끼치기도 함
 view가 giveup() 함
 
React의 생성 후,

   View (Virtual DOM) : Model이 변경 사항을 줄 때마다 DOM에 새롭게 적용
   React (Real DOM)	: View가 전달한 DOM 정보를 기억하고, 새 정보가 오면 바뀐 부분만 빠르게 수정
	
즉,
 Model이 View에게 완성된 데이터 세트를 보내주면  
 View는 데이터를 어떻게 DOM에 반영시키고 그려야 할지 알고 있었기 때문에 빠르게 작업 완료
 이후 React에 전해주면, 이것을 스캔해서 빠르고 정확한 메모리장치를 통해 기존에 진짜 DOM에 뭐가 그려져 있었는지 정확히 기억함
 Model이 View에게 변경된 새로운 정보를 주면, 빈 DOM에 새롭게 paint() 하고 React로 전달
 React는 기존 DOM에 기억된 정보와 새로운 정보를 비교하여 바뀐 부분만 빠르게 수정
```



## 1. 리액트 프로젝트 시작하기

### 본격적인 리액트 코드 작성하기

---

#### Webpack

- 코드들을 의존하는 순서대로 잘 합쳐 하나 또는 여러개의 결과물로 만들어냄
  - js에서 png를 import한다면, bundling(build) 작업마다 처리 작업을 도움
  - js에서 png를 불러올 때, 사용자 경로가 문자열로 들어오는데, 

- js 파일을 여러 개 만들었을 때 하나의 파일로 합쳐주기도 함
  - 원한다면 규칙에 따라 분리도 가능
- ES6라는 새 js문법을 사용할 때, babel 사용할 것

#### Babel

- JavaScript 변환 도구 



#### React Basic

##### App.js

```react
import React, { Component } from 'react';

class App extends Component {
  render() {
    return (
      <div>
        <h1>안녕하세요 리액트</h1>
      </div>
    );
  }
}

export default App;
```

- ##### Component를 만드는 방법은 두 가지가 있는데, 각각 class와 함수를 이용하는 방법임

  - ##### 모든 Component에는 render() 함수가 있어야 함



## 2. JSX

### JSX 기본 문법 알아보기

---

- HTML과 비슷하지만, 지켜야 할 규칙이 몇 가지 있음

```
화살표 함수
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/%EC%95%A0%EB%A1%9C%EC%9A%B0_%ED%8E%91%EC%85%98
```

- 태그는 꼭 닫혀야 함

```html
  <input type="text"> 	// html에는 이런 경우가 있지만 jsx에서는 불가능!
```

- 두 개 이상의 엘리먼트는 무조건 하나의 엘리먼트로 감싸져있어야 함
  - Fragment를 이용해 불필요한 div를 사용하지 않도록 하기
    - 불필요한 div는 css 등에도 영향을 미치기 때문

```react
import React, { Component, Fragment } from 'react';		// Fragment로 불필요한 div제거 가능

class App extends Component {
  render() {
    return (
     <div> or <Fragment>
       <div></div>
       <div></div>				<!-- 두 개의 div가 나열만 되어있으면 오류! -->
     </div> or </Fragment>		// 감싸주는 div 또는 Fragment가 무조건 있어야 함!
    );
  }
}

export default App;
```

- ##### JSX 안에 자바스크립트 값 사용하기

```react
import React, { Component, Fragment } from 'react';		// Fragment로 불필요한 div제거 가능

class App extends Component {
  render() {
      const name = 'react';				// const : 한번 선언 후 고정값
      return (
      	<div>
              hello {name}!				<!-- { 변수명 } 을 통해 값을 가져옴 -->
        </div>
      );
  }
}

export default App;
```

- #### let과 const의 scope : 블록 단위

  - let : 유동적인 값 (이전의 var)
  - const : 한번 선언 후 고정적인 값



- ##### 조건부 렌더링

```react
import React, { Component, Fragment } from 'react';		// Fragment로 불필요한 div제거 가능

class App extends Component {
  render() {
      const name = 'velopert';				// const : 한번 선언 후 고정값
      const value = 3;
      return (
        <Fragment>
     	 	<div>
      			{
                	  1+1 === 2 ?
                    	'맞다'
                	  : '틀리다'
            	 }
      		</div>
            <div>
            	{
                    name === 'velopert!' && <div>벨로퍼트다!</div>	
                        				// &&를 이용한 조건부 렌더링
                } 
            </div>
            <div>
            	{	// 함수 형식을 통해 다중 조건문 사용 가능
                    // 함수는 (function(){...})로 괄호로 감싸고, 마지막에 ()로 실행
                    // 화살표함수 (() => {...})() 를 이용하여 쉽게 사용 가능
                    (function(){
                        if(value === 1) return <div>1이다!</div>
                        if(value === 2) return <div>2이다!</div>
                        if(value === 3) return <div>3이다!</div>
                        return <div>없다</div>
                    })()
                }  
              
            </div>
        </Fragment>
      );
  }
}

export default App;
```

#### 화살표함수

- ES6 의 문법 중 하나인듯

```react
(function(){
	if(value === 1) return <div>1이다!</div>
    if(value === 2) return <div>2이다!</div>
    return <div>없다</div>
})()

위의 함수는

(() => {
    if(value === 1) return <div>1이다!</div>
    if(value === 2) return <div>2이다!</div>
    return <div>없다</div>
})()

화살표함수로 줄이기 가능
```



## JSX를 이용하여 CSS (style, class) 적용하기

- ##### background-color, font-size 와 같이 사용하던 css를 camelcasing 하여 사용함

- ##### class는 className="class명" 형식으로 적용해야 함 (class=이 아님)

```react
import React, { Component } from 'react'
import './App.css'

class App extends Component{
    render(){
        const styles = {
            backgroundColor : 'black',
            padding : '16px',
            color : 'white',
            fontSize : 5+10+'px'
        };
        return (
            <div style={styles}>
                안녕하세요!
            </div>
            ---------------------------------
            <div className="App">
                안녕하세요!
            </div>        
        );
    }
}

export default App;
```



#### 주석 작성하기

- { /*  ... */ } 와 같이 중괄호로 감싸주어야 함
- js 태그 안에서 //를 통해 주석을 넣을 수 있음 (단, 주석 뒤는 줄바꿈 해주어야 함)

```react
render(){
    // 하이! (js는 주석O)
    return(
        <div>
            {/* 이건 주석이야*/}
            {/*
            	멀티라인 주석은
            	이렇게!
            */}
        	<h1 something="dfd" // 여기에 주석 넣을 수 있지
             >
                리액트
            </h1>
        </div>
    );
}
```



## 3. props 와 state

- ##### 리액트에서 데이터를 다룰 때 사용

<img src="data:image/png;base64,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">

# props ★★

- 부모 component에서 자식 component로 값을 전달할 때 사용
- 자식 입장에서는 읽기 전용

```react
 <Child value = "values" />      // 여기서 value가 props
								 // 부모가 자식에게 넘겨주는 값
```

##### MyName.js

- props는 this.props.name 으로 받으며, name은 변수
- props값이 없을 때, defultProps를 설정 가능
  - 방법 1 static (코드 상단, 최신)
    - static defaultProps={ name : '기본이름' }
  - 방법 2 class (코드 하단)
    - MyName.defaultProps = { name: '기본이름' };

```react
import React, { Component } from 'react';

class MyName extends Component{
    
    static defaultProps={
        name: '기본이름'		// props가 없을 때는 기본 props값 적용법
    }
    render(){
        return (
        	<div>
                안녕하세요! 제 이름은 <b>this.props.name</b>입니다.		
                {/* props를 넣을 자리 */} 
            </div>
        );
    }
}

MyName.defaultProps = {
    name: '기본이름2'			// props가 없을 때 기본 props값 적용법 2
};

export default MyName;
```

##### App.js

```react
import React, { Component } from 'react';

class App extends Component{
    render(){
        return (
        	<Myname name="리액트" />;		// MyName.js를 불러와 props를 적용
        )
    }
}

export default MyName;
```

##### App.js 결과

```html
안녕하세요! 제 이름은 리액트입니다.
```



### 함수형 Component 작성과 Props

- props로 받아와서 rendering하면 됨!

```react
 import React from 'react';		// 함수형 컴포넌트는 import { component } 안해도 됨

 const MyName = ({ name }) => {		// 비구조화 할당 문법
    return <div>안녕하세요! 제 이름은 {name} 입니다.</div>
 };

 MyName.defaultProps = {
    name: 'velopert'
 };

 export default MyName;
```

##### 비구조화 할당 문법

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment

- ##### 개발자도구로 실행

```react
 const object = { a:1, b:2 };
 const a = object.a; const b = object.b;
 const { a,b } = object;
 
 a					// 1
 b					// 2
 
 function sayHello({ name, age }){
 	console.log(name+ '의 나이는'+age);
 }

sayHello({ name : 'react', age:'몰라' })			// react의 나이는 몰라
```



# State

- 자식 입장에서 읽기 전용인 props 와 다르게 state는 component 내에서 setState() 함수를 통해 내용을 변경 할 수 있다.

##### Counter.js

```react
import React, { Component } from 'react';

class Counter extends Component {
    
    state = {				// state는 문자나 숫자면 안 됨, 항상 객체 형식
        number : 0        
    };
	
	constructor(props){    	// 화살표 함수를 사용하지 않을 때 this를 명시하기 위해 사용
        					// 화살표 함수를 사용한다면 생략해도 됨
		super(props);   
        this.handleIncrease = this.handleIncrease.bind(this);
        this.handleDecrease = this.handleDecrease.bind(this);
    }

	handleIncrease = () => {	// 화살표 함수를 쓰지 않으면 this가 가리키는 게 뭔지 모름
    	// this.state.number = this.state.number + 1;    이렇게 하면 절대 안됨!
        
        this.setState({
            number: this.state.number + 1
        });
    };
    
    handleDecrease = () => {
        this.setState({
            number: this.state.number -1
        });
    };
	
    render(){
    	return (
        	<div>
      	    	<h1>Counter</h1>
        		<div> 값 : {this.state.number} </div>  	{/* state값 받아오기 */} 
            	<button onClick={this.handleIncrease}>+</button>
            	<button onClick={this.handleDecrease}>-</button>
        	</div>
    	);
	}
}
```

##### App.js

```react
import React, { Component } from 'react';
import Counter from './Counter';

class App extends Component{
    render(){
        return (
        	return <Counter />;
        )
    }
}

export default App;
```



## 4. LifeCycle API



## LifeCycle API 소개 및 사용법

```
https://codesandbox.io/s/xl313zyrkw
```

- 컴포넌트가 브라우저 상에서 <b>나타날 때, 업데이트 될 때, 사라질 때</b> 중간중간 과정에서 작업할 때 사용

<img src = "https://jistol.github.io/assets/img/frontend/react-lifecycle-methods/1.jpeg">

### lifecycle API 종류

---

- ### Mounting : 컴포넌트가 브라우저 상에 나타나는 것

  ##### App.js

  ```react
  import React, {Component} from 'react';
  
  class App extends Component {
      /*
      state={
          a:1
      }
      */
      constructor(props){					// constructor
          super(props);
          console.log('constructor');
          this.state = {
              a: '1',
          }
      }
      componentDidMount(){				// componentDidMount()
          console.log('componentDidMount');
          console.log(this.myDiv.getBoundingClientRect().height);	// 아래 ref값
      }
      render(){
          return(
          	<div ref={ref => this.myDiv = ref}>
              	<h1>안녕하세요 리액트</h1>
              </div>
          );
      }    
  }
  
  export default app;
  ```

  - constructor (생성자 함수)

    -  컴포넌트가 처음 브라우저에 나타날 때 가장 먼저 실행되는 함수
    - state 초기설정, 컴포넌트 전처리 작업 실행    

  - getDerivedStateFromProps

    ```react
    static getDerivedStateFromProps (nextProps, prevState){
    	// setState를 하는 것이 아니라 특정 props가 바뀔 때 설정하고 싶은 state 값 리턴
    	if (nextProps.value !== prevState.value){
            return { value: nextProps.value };
        }
        return null; // null을 리턴하면 따로 업데이트 할 것은 없다라는 의미   
    }
    ```

    - props로 받은 값을 state로 바로 동기화할 때 이용
    - Mounting, Updating(New Props) 에서 사용

  - <b>render</b>

    - 어떤 dom을 만들게 될지, 내부 tag에 어떤 값을 전달하게 될지를 나타냄
    - Mounting, Updating 모두에서 사용

  - componentDidMount

    - 실제로 브라우저 상에 나타난 후 요청할 것들 / 특정 event listening 기능
    
    - 외부 라이브러리 (DC, charttest) 등의 차트 라이브러리 등을 사용할 때 특정 dom에 차트를 그려주세요!
    
    - Network / ajax 요청할 때 처리
    
    - component가 나타난 후 몇 초 뒤 / component가 나타난 후 스크롤 작업 등
    
      

- ### Updating : Props / State 가 바뀌었을 때

  ##### MyComponent.js

  ```react
  import React, {Component} from 'react';
  
  class MyComponent extends Component {
      state={
          value:0
      };
  
  	static getDerivedStateFromProps(nextProps, prevState){	// 이 함수는 static
          						// 받아올 props, 현재 state
          if(prevState.value !== nextProps.value){	// 받을 값과 현재 값이 다를 때
              return{
                  value : nextProps.value				// value값 변경
              }
          }
          return null;
      }
  
      shouldComponentUpdate(nextProps, nextState){   // shouldComponentUpdate 함수
          if(nextProps.value === 10) return false;   // 특정 조건에 따라 rendering
          return true;
      }
  
  	componentDidUpdate(prevProps, prevState){		// componentDidUpdate
          if(this.props.value != prevProps.value){	// Props와 State 비교
              console.log('value값이 바뀌었다!', this.props.value);
          }
      }
  
  	componentWillUnmount(){
          console.log('Good Bye');
      }
  
      render() {
          return (
              value : nextProps.value;
          )
      }
  }
  
  export default MyComponent;
  ```

  ##### App.js

  ```react
  import React, {Component} from 'react';
  import MyComponent from './MyComponent';
  
  class App extends Component {
      state={
          counter: 1,
      }
  
      constructor(props){					// constructor
          super(props);
          console.log('constructor');
      }
      componentDidMount(){				// componentDidMount()
          console.log('componentDidMount');
      }
      handleClick = () =>{
          this.setState({
              counter: this.state.counter+1
          });
      }
      render(){
          return(
          	<div>
                  { this.state.counter < 10 && <MyComponent value={this.state.counter} />
                  <button onClick={this.handleClick}>Click Me</button>
              </div>
          );
      }    
  }
  
  export default app;
  ```

  

  - New Props / setState() / forceUpdate() 로 구분

  - getDerivedStateFromProps

  - <b>shouldComponentUpdate</b>
    - virtual dom에도 rendering을 할지 말지 결정해주는 함수
    
      - ##### 특정 조건에만 update / rendering 해주는 함수
    
  - component가 update되는 성능을 최적화시키고 싶을 때 사용
    
    - 원래 부모 component가 re-rendering되면, 자식 component도 전부 rendering 하여 차이점을 virtual dom에 반영함 >> 이러한 virtual dom에 반영하는 시간조차 최적화 하기 위해 souldComponentUpdate를 사용함
    
    - true / false 값을 반환하여, New props 혹은 setState()로 값이 변경된 경우 shouldComponentUpdate 함수의 로직에 따라 true 를 반환하면 rendering process를 진행하고, false값을 반환하면 실제 rendering이 되지 않기 때문에 화면에도 반영되지 않게 됨
    
    ```react
    shouldComponentUpdate(nextProps, nextState){
      // return false 하면 업데이트를 안함
        // return this.props.checked !== nextProps.checked
    	return true;    
    }
    ```
    
  - render

  - getSnapshotBeforeUpdate
    
    ```react
  getSnapshotBeforeUpdate(prevProps, prevState){
        // DOM 업데이트가 일어나기 직전의 시점
        // 새 데이터가 상단에 추가되어도 스크롤바 유지
        // scrollHeight 는 전 후를 비교하여 스크롤 위치를 설정하기 위함
        // scrollTop은 크롬에 구현되어 있는데, 이미 구현되었다면 처리하지 않도록 하기위함
        if(prevState.array !== this.state.array){
            const {
                scrollTop, scrollHeight
            } = this.list;
            
            // 여기서 반환하는 값은 componentDidMount 에서 snapshot 값으로 받아올 수 있음
            return {
                scrollTop, scrollHeight
            };
        }
    }
    
    componentDidUpdate(prevProps, prevState, snapshot) {
        	// event가 발생하더라도 scroll이 움직이지 않도록
        if(snapshot) {
            const { scrollTop }=this.list;
            if(scrollTop !== snapshot.scrollTop) return;	// 기능이 구현되었다면 처리하지 않음
            const diff = this.list.scrollHeight - snapshot.scrollHeight;
            this.list.scrollTop += diff;
        }
    }
    ```
    
    - component가 update되어(rendering 후) 브라우저에 반영되기 바로 직전 호출되는 함수
      - 실행순서 : render() >> getSnapshotBeforeUpdate() >> DOM변화 >> componentDidUpdate
    - DOM 변화가 일어나기 직전의 DOM 상태를 가져오고, 여기서 리턴하는 값은 componentDidUpdate에서 3번째 파라미터로 받아올 수 있게 됨
    - 돔의 크기, 스크롤의 위치 등을 사전에 가져오고 싶을 때 사용
    
  - componentDidUpdate
    - 작업을 마치고 component가 업데이트 되었을 때 호출
    - state가 바꼈다 >> 이전의 상태와 지금의 상태가 페이지가 바꼈다 >> 그럼 어떤 작업을 하겠다! 이런 것을 정의함
    
  - componentDidCatch

    ```react
    componentDidCatch(error, info){
        this.setState({
            error:true
        });
    }
    ```

    - render함수에서 에러가 발생한다면, 리액트 앱이 크래쉬 되어버림
    - state.error를 true로 설정하게 함수쪽에서 에러를 띄워주면 됨

    

- ### Upmounting : 컴포넌트가 브라우저에서 사라질 때

  - componentWillUnmount
    - componentDidMount에서 이용한 event listener 를 지움 



## 5. 인풋 상태 관리

```
객체 안에서 사용되는 [e.target.name]: 문법은 속성 계산명 이라는 문법입니다.
참고링크: https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer
코드: https://codesandbox.io/s/84j07kxq98
```

- Reactjs code snippets

  - js 파일에서 rcc 입력 >> class 형태로 만들어진 component 양식 생성

    ​					rcs 입력 >> 함수 형태로 만들어진 component 양식 생성

##### PhoneForm.js

```react
import React, { Component } from 'react';

class PhoneForm extends Component {
    state = {
        name:'',
        phone:'',
    }

    handleChange = (e)=>{
        this.setState({
            //name: e.target.value
            [e.target.name] : e.target.value
        });
    }

    handleSubmit = (e) => {
        e.preventDefault();     // 주소창에 값이 들어가는 것을 방지
        /*
        this.props.onCreate({
            name : this.state.name,
            phone : this.state.phone,
        });
        */
        this.props.onCreate(this.state);

        // 두 onCreate 함수는 같은 역할
    }

    render() {
        return (
            <form onSubmit = {this.handleSubmit}> 
                <input
                    name="name"
                    placeholder="이름"
                    onChange={this.handleChange}
                    value={this.state.name}
                />
                <input 
                    name="phone"
                    placeholder="전화번호" 
                    onChange={this.handleChange}
                    value={this.state.phone}
                />
                <button type="submit">등록</button>
                <div>
                    {this.state.name}
                    {this.state.phone}
                </div>
            </form>
        );
    } 
}

export default PhoneForm;
```



## 6. 배열 데이터 렌더링 및 관리

- ##### 자식 컴포넌트가 부모에게 값 전달하기

### 배열에 데이터 삽입하기

---

- react는 불변성을 꼭 유지해야 함!
  - this.setState() 꼭 사용하기
  - 내부의 배열이나 객체 수정 시, 기존 배열이나 객체를 수정하지 않고 그것을 기반으로 새로운 객체나 배열을 만들어서 값을 주입해야 함 (concat 함수 등)

##### App.js

```react
import React, { Component } from 'react';
import PhoneForm from './components/PhoneForm';

class App extends Component {

  id=0;   // 데이터 추가 시 마다 id값 줄 것
          // id값은 rendering되는 값이 아니기 때문에 굳이 state에 넣을 필요 없음

  state = {
    information : [],
  }

  handleCreate = (data) => {
    const { information  }= this.state;   // 비구조 할당 문법 사용
    this.setState({
      //information : this.state.information.concat(data)
      information : information.concat({  // 비구조 할당 문법으로 편의성,가독성 증가
      // name : data.name,
      // phone : data.phone,    // 방법 1
      
      /*
       information : information.concat(
         Object.assign({}, data, {id: this.id++}
        ))
          // Object.assign함수를 통해 빈 배열({})에 data와 id값을 넣음
          // 방법 2
      */
      ...data,           // 방법 3
      id: this.id++     // id 추가
      })
    });
  }

  render() {
    return (
      <div>
        <PhoneForm onCreate = {this.handleCreate}/>
        {JSON.stringify(this.state.information)}
      </div>
    );
  }
}

export default App;

```

### 배열 렌더링하기

---

```javascript
const numbers = [1,2,3,4,5];
const squared = numbers.map(n => n*n);		// numbers를 모두 제곱시켜라
```

##### 	  위와 같은 map함수를 react에서 구현

##### PhoneInfo.js

```react
import React, { Component } from 'react';

class PhoneInfo extends Component {
    render() {
        const { name, phone, id } = this.props.info;    // 비구조 할당
        const style={
            border: '1px solid black',
            padding: '8px',
            margin : '8px',
        };

        return (
            <div style={style}>
                <div><b>{name}</b></div>
                <div>{phone}</div>
                <div>{id}</div>
            </div>
        );
    }
}

export default PhoneInfo;
```

##### PhoneListInfo.js

```react
import React, { Component } from 'react';
import PhoneInfo from './PhoneInfo'

class PhoneInfoList extends Component {
    static defaultProps = {             // data에 props값이 들어오지 않을 때 static으로 default값 정의
        data: []
    }
    render() {
        const {data} = this.props;      // data에 값을 전달받아 배열로 만들어줌
        
        if(!data) return null;          // data에 값이 안들어간다면 null return
                                    // 위처럼 static defaultProps를 넣어준다면 ㄱㅊ

        const list = data.map(          // data는 map
            info => (<PhoneInfo info={info} key={info.id}/>)
            // data 안의 info 값을 넣어주고, 
            // key는 여러 값을 rendering할 때 고유값을 넣어 최적화
            // info들의 배열을 가지고 PhoneInfo component로 변환해 준 것
        );
        return (
            <div>
                {list}      {/* rendering*/}
            </div>
        );
    }
}

export default PhoneInfoList;
```

- ##### key가 없다면?

  - 배열 내 값이 생성/삭제될 때 모든 원소가 바뀌며 맞춰감 >> 비효율적

  ```javascript
  ['a','b','c','d']	['a','b','z','c','d']	
  <div>a</div>		<div>a</div>
  <div>b</div>		<div>b</div>
  <div>c</div>		<div>z</div>	// c가 z로 바뀜
  <div>d</div>		<div>c</div>	// d가 c로 바뀜
      				<div>d</div>	// 새로 d 생성		// 3개의 원소가 바뀜
  ```

- key가 있다면?

  - 생성/삭제되는 값 하나만 바뀜 >> 효율적
  
  ```javascript
  ['a','b','c','d']			['a','b','z','c','d']	
  <div key={0}>a</div>		<div key={0}>a</div>
  <div key={1}>b</div>		<div key={1}>b</div>
  <div key={2}>c</div>		<div key={5}>z</div>	// key가 5인 z 하나만 추가
  <div key={3}>d</div>		<div key={2}>c</div>
      						<div key={3}>d</div>	// 1개의 원소가 바뀜
  ```

##### App.js

```react
import React, { Component } from 'react';
import PhoneForm from './components/PhoneForm';
import PhoneInfoList from './components/PhoneInfoList';

class App extends Component {

  id=0;   // 데이터 추가 시 마다 id값 줄 것
          // id값은 rendering되는 값이 아니기 때문에 굳이 state에 넣을 필요 없음

  state = {
    information : [],
  }

  handleCreate = (data) => {
    const { information  }= this.state;   // 비구조 할당 문법 사용
    this.setState({
      //information : this.state.information.concat(data)
      information : information.concat({  // 비구조 할당 문법으로 편의성,가독성 증가
      // name : data.name,
      // phone : data.phone,    // 방법 1
      
      /*
       information : information.concat(
         Object.assign({}, data, {id: this.id++}
        ))
          // Object.assign함수를 통해 빈 배열({})에 data와 id값을 넣음
          // 방법 2
      */
      ...data,           // 방법 3
      id: this.id++     // id 추가
      })
    });
  }

  render() {
    return (
      <div>
        <PhoneForm onCreate = {this.handleCreate}/>
        {JSON.stringify(this.state.information)}
        <PhoneInfoList />
      </div>
    );
  }
}

export default App;
```



### 배열에서 데이터 수정/제거하기

---

- ##### 불변성을 유지하며 데이터를 수정/제거해야 함

  - ##### 제거 : js 내장함수 .slice 또는 .filter 를 이용

  - ##### 수정 : js 내장함수 .slice 또는 .map 이용

  ```javascript
  const numbers = [1,2,3,4,5];
  
  # slice 함수
  numbers.slice(0,2);				// [1,2]  0번째 칸 포함 2번째 칸 전까지
  numbers.slice(1,3);				// [2,3]
  numbers.slice(3,6);				// [4,5]  범위를 넘은 index(6)가 있다면 무시
  numbers.slice(0,2).concat(number.slice(3,6))
  								// [1,2,4,5] 원본 배열은 건드리지 않고 3만 제거
  
  [								// spread 문법 사용 시
      ...numbers.slice(0,2),
      10,
      ...numbers.slice(3,5)
  ]								// [1,2,10,4,5]
  
  # filter 함수
  numbers.filter(n => n > 3);		// [4,5]	n이 3보다 큰 경우에만 가져옴
  numbers.filter(n=> n!== 3);		// [1,2,4,5]	3을 제외한 값만 가져와줘
  								// 원본 배열은 건드리지 않고 배열 수정 가능
  
  # map 함수
  numbers.map(n => {
     if(n === 3){
         return 9;				// n이 3이면 9로 변경
     }
      return n;					// 아니면 그대로 return
  });								// [1,2,9,4,5]
  ```

  
