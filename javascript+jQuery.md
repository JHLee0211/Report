# Javascript



## Javascript 란?

 - 웹 브라우저 제어 
   
- 사용자가 어떠한 동작을 할 때 어떠한 동작이 일어나도록 할 때 사용
  
 - 탈 웹 브라우저
    - 이제는 웹브라우저를 제어하기 위한 언어로만 쓰이지 않음
    - 웹 서버 동작 도구 (node.js - 서버 사이드 스크립트)

 - 환경
    - JavaScript 가 ~('Hello world'); 명령을 내릴 때,
        - 웹 브라우저 :  alert
        - node. js :    write
        - SpreadSheet : msgBox
    
    위와 같이, 여러 환경에 사용되며 각 환경에 따라 명령이 달라질 수 있음

 - js 실행
    - script 태그 내부에 javascript 코드 작성
    - 줄바꿈 또는 ; 은 명령이 끝났음을 의미



## 숫자와 문자

###  	수의 함수와 연산

		Math.pow(3,2);		// 9
		Math.round(10.6);	// 11 (반올림)
		Math.ceil(10.2);	// 11 (올림)
		Math.floor(10.2);	// 10 (내림)
		Math.random();		// 소수값 난수 (< 1)
		100*Math.random();	// 소수값 난수 (< 100)



### 	문자 출력과 연산

	 	\  : \뒤의 문자는 기능적인 측면이 아닌, 단순 문자로 취급
	 	\n : 문자열 개행
	 	
	 	typeof 1 		// "number"
	 	typeof "1"		// "string"
	 	"1" + "1"		// "11"
	 	
	 	"code".indexOf("o")		// 1 (문자의 index 검색)



### 문자 관련 함수

```
	"jongho".toUpperCase();		// "JONGHO"
	let a = ["lee", "jong", "ho"];
	alert(a.length);			// 3
```



## 연산자

- 연산자 : 어떠한 작업을 지시하기 위한 기호

- 등호

  ```
    alert(1 == '1')			// true  (내용만 비교)
    alert(1 === '1');			// false (데이터 타입까지 비교)
    alert(null == undefined);	// true	(undefined : 값이 없음, 프로그래머가 의도하지 않은 상황)
    alert(null === undefined);	// false
    alert(true == 1);			// true	(js에서 true는 1로 간주)
    alert(true == '1');		// true
    alert(true === 1);		// false
    alert(true === '1');		// false
   	
    alert(0 === -0);			// true (0과 -0은 같다)
    alert(NaN === NaN);		// false (NaN은 계산 불가능이므로 false)
  ```

  

- 부정과 부등호

  ```
   alert(1!=2);		// true
   alert(1!=1);		// false
   마찬가지로, !== 도 있다.
  ```



- 비교 연산자

  ```
  alert(10>1);	// true
  alert(10>10);	// false
  alert(20<=20);	// true;
  ```

- 조건문 / 논리연산자 / 반복문생략



### 출력 방식

---

####   alert

- 경고창 출력

	####   prompt

- alert와 비슷하게 경고창이 떠 입력받을 수 있음,  입력창에 입력값을 넣으면 값이 넘어감

  ex) alert(prompt("당신의 나이는?")*2);		// 20 입력

  ​	   >> alert(20*2);										// 경고창에 40 출력

  

  ex2) id=prompt("아이디를 입력해주세요.')

  ​		if(id=='leejh' && pw='1111') { alert('정보가 일치합니다. '+id+'님 환영합니다.') }

  ​		else { alert('정보가일치하지 않습니다.') }

####   document.write

- 웹 페이지에 직접 표시



### Chrome Debugger

---

```
 chrome 개발자도구 - Sources 에서 line에 breakpoint 설정
 설정 후 실행하면 breakpoint에서 멈추는데,
  - |> 클릭 시, 이후 전체 실행
  - ↓  클릭 시, 반복문이나 함수 내의 코드로 들어감 or 다음 명령 실행
```



### 함수(function)

---

- 함수 : 하나의 로직을 재실행 할 수 있도록 하는 것
  - 재사용성 + 유지보수 용이 + 가독성

```
 function 함수명 ( [param...[,param]] ){	// 함수 정의
 	코드
 	return 반환값				// 출력
 }
 
 함수명(argument);				// 함수 호출
```

- 함수 정의

   function numbering ( ) { } 		// 이 정의방법과

   numbering = function () { }		// 이 정의방법은 같음

  ​	ㄴ 둘 모두 numbering(); 으로 호출 가능	

- 익명 함수

   ( function ( ){ } ) ( );					// 함수 정의 후 괄호로 묶고, (); 로 바로 선언 가능

  ​													 // 이름 없이 실행할 때 사용

  

### 배열

---

- let member=[ 'lee', 'jong', 'ho'];

  - member[0], member[1] 등으로 호출 가능

  

- 함수와 배열

```
  function (get_members()) {
  	return [ 'lee', 'jong', 'ho' ];			// 배열 자체를 리턴
  }
  
  let members=get_members();		// 배열 객체를 함수 호출로 생성
  document.write(members[1]);		// 바로 접근 가능
```

  

- 배열 데이터 수정하기

  ##### 데이터 추가
	
  ```
  let li = ['a','b','c'];
  
  li.push('e');					// push 함수로 배열에 인자 하나 추가
  li = li.concat( ['f','g'] );	// concat 함수로 배열(여러 인자) 추가
  li.splice(3,1,'d');				// 3번째 index에 1개 원소('e')를 삭제하고 'd'를 넣어라
  										ㄴ index는 0부터 시작
  										ㄴ splice의 return값은 삭제된 원소 ['e']
  li.unshift('z');				// unshift 함수로 배열 맨 앞에 추가
  
  추가 후 : document.write(li);	  // ["z","a","b","c","d","f","g"]
  ```
  ##### 데이터 삭제
	
  ```
  let del = ['a','b','c'];

  del.shift();					// 맨 앞 원소 삭제, return 값 : ["a"]
  del.pop();						// 맨 뒤 원소 삭제, return 값 : ["c"]

  삭제 후 : document.write(del);	  // ["b"]
  ```
    ##### 데이터 정렬
  ```
  let li = ['c','d','b','a'];

  li.sort();						// ['a','b','c','d']
  li.reverse();					// ['d','c','b','a']

  function sortNumber(a,b){ return a-b; }		// sort정렬 구현
  li.sort(sortNumber);						// 실행
  ```
  
  

### 객체 (Dictionary)

---

- 객체 (dictionary)
  
  - 연관배열(associative array), 맵(map), 딕셔너리(Dictionary) 등의 데이터 타입
  - 객체 지향 패러다임과 연관
  
  ```
   ex) let grades = {'lee' : 10, 'jong' : 5, 'ho' : 15};
   	 grades['jong']				// 5
   	 grades.jong				// 5
   	 grades['jo'+'ng']			// 5
   	
   	위 예제에서 문자는 key, 값은 value 가 됨
  ```
  
  ```
  배열과 차이점 - 대괄호가 아닌 중괄호로 표기
  			- 인덱스를 숫자가 아닌 문자를 사용하고 싶을 때 객체 사용
  ```

- 객체와 반복문 (for-in, for-each, for-of)

  - 배열은 index가 순서를 가지고 있지만, 객체는 key-value 형태임 - 특별한 for문 필요
  
- for-in 반복문 : key값을 가져오기
  
    ```html
    let grades = {'lee' : 10, 'jong' : 5, 'ho' : 15};
    <ul>
        for(key in grades){		// 변수 in 객체
            document.write("<li>key : "+key+" value : "+grades[key]+"</li>");
      }
    </ul>
    ```
    
  - foreach 반복문
  
    ```
    
    ```
  
    
  
  - for-of 반복문
  
    
  
- 객체 지향 프로그래밍

  ```html
  var grades={
  	'list' : {'egoing' : 10, 'k8805' : 8, 'sorialgi' : 80},
  	'show' : function(){
  		alert('Hello World');
  	}
  	'show_this' : function(){
  		alert(this);				// this : 함수가 소속된 객체(grades)를 가리킴
  		console.log(this.list);		// this가 가리키는 grades 안의 list 배열
  		
  		for(let name in this.list){
  			console.log(name, this.list[name]);		// ,를 통해 한번에 여러 값 출력
  		}
  	}
  }
  grades['show']();				// js에서는 함수도 값이므로 호출 가능
  								// == grades.show();
  ```

  

### 모듈

---

- 프로그램을 부분부분으로 나누어 코드의 재활용성을 높이고, 유지보수를 쉽게 할 수 있는 다양한 기법

  - 모듈화를 통해 모듈로 나눈다

  

- 장점

  - 자주 사용되는 코드를 별도의 파일로 만들어 필요할 때마다 재활용 가능
  - 코드를 개선하면 이를 사용하는 모든 애플리케이션 동작이 개선
  - 코드 수정 시 필요한 로직을 빠르게 검색 가능
  - 필요한 로직만 로드하여 메모리 낭비 감소
  - 한 번 다운된 모듈은 웹브라우저에 저장 >> 동일 로직 로드 시 네트워크 트래픽 절약 (브라우저만 해당)

- 순수한 js에는 모듈 개념이 분명하게 존재하지 않음 

  - 호스트 환경에 따라 모듈화 방법이 다름 / 여기서는 웹 브라우저에 대해 알아봄

    

- 모듈 불러오기

```html
	<script src = "greeting.js"></script> 
	// 이 부분은 별도의 파일로 모듈화된 greeting.js 파일을 읽어와 불러오는 효과를 가짐
```



- 라이브러리 : 자주 사용되는 로직을 재사용하기 편리하도록 잘 정리한 일련한 코드의 집합 (모듈과 비슷)
  - ex)  jQuery



### UI와 API

---

- API 란?
  -  Application Programming Interface
  - 프로그램이 동작하는 환경을 제어하기 위해 환경에서 제공되는 조작 장치
  - 프로그래밍 언어를 통해 조작 가능
    - 주소창에 javascript:alert("Hello World!"); 입력 시 Hello World! 라는 경고창이 뜸
    - 브라우저로 하여금 alert를 통해 경고창이 뜨도록 명령하는 Interface

- UI 란?
  - User Interface
  - 사람과 컴퓨터 사이의 대면 접점 / 중계자 역할



- 일반 사용자는 UI를 통해, 개발자는 코드를 통해 API를 이용하여 조작함



### 정규표현식

---

- 문자열에서 특정한 문자를 찾아내는 도구



- 컴파일 : 문자를 판단하거나, 패턴을 찾는 것

  - 패턴 찾는 법 2가지

    ```html
    1. let pattern = /a/;				// 슬래시 사이의 a가 찾고자 하는 패턴
    
    2. let pattern = new RegExp('a');	// 새 정규표현식 객체에 찾을 패턴을 넣기 
    ```

  

- 실행 : 어떠한 작업을 구체적으로 하는 것







# DOM과 jQuery



## DOM 

- DOM은 문서 객체 모델(Document Object Model) 의 약자로 js가 웹 페이지 콘텐츠에 접근할 때 사용

- DOM tree  : 웹 브라우저는 HTML 문서를 읽을 때 엘리먼트를 마치 나무와 같은 구조로 해석

  ```
  						<html>
  			 		 	 ↙ ↘
  					<head>   <body>
  					   ↓	  ↙ ↘ 
  					<title>  <h1> <p>
  ```

- 아이디를 사용한 엘리먼트 식별하기

  ```html
  // id 속성 넣기
  <h1 id="main-heading"> 안녕하세요! </h1>
  
  // document.getElementById() 함수를 이용해 id 값 받아오기
  <script>
  	let headingElement = document.getElementById("main-heading");
      console.log(headingElement.innerHTML);		// 변수.innerHTML 을 통해 내부 문자 가져옴
  </script>
  ```



## jQuery

- jQuery는 $ 함수를 이용하여 HTML 엘리먼트 선택

  - $ 함수는 선택자 문자열(selector string) 인수를  하나 전달

    

##### jQuery를 이용한 제목 텍스트 바꾸기

```html
<script src="https://code.jquery.com/jquery-3.4.1.min.js"> </script>

<body>
    <h1 id="main-heading"> 안녕하세요! </h1>

	<script>
		let newHeadingText = prompt("새로운 제목을 입력하세요.");
        $("#main-heading").text(newHeadingText);
	</script>
</body>
```



##### jQuery를 이용해 엘리먼트 새로 만들기

```html
<script>
    for(let i=0; i<3; i++){
        let hobby = prompt("취미를 입력해주세요!");
        $("body").append("<p>"+ hobby +"</p>")
    }
</script>
```



##### jQuery를 사용한 애니메이션

```html
<script>
    $("h1").text("페이드아웃").fadeOut(3000).fadeIn(2000);
    // chaining : 한 줄에 메서드를 여러 번 호출하는 것
    $("h2").text("슬라이드업").slideUp(3000).slideDown(2000);
</script>
```



## Interactive Programming

##### setTimeout을 이용한 코드 지연

- setTimeout(함수, 밀리초);
  - 함수 : 밀리초가 지난 후 호출할 함수

```html
<script>
    let timeUp = function(){
        alert("시간 끝!");
    };

	setTimeout(timeUp, 3000);
    clearTimeout(timeUp);		// 타임아웃 취소하기
</script>
```



##### setInterval을 사용해 코드를 여러 번 호출하기

- setInterval(함수, 밀리초);
  - 함수 : 시간 간격 밀리초마다 호출할 함수

```html
<script>
    let counter = 1;
    let printMessage = function(){
        console.log("기록을 시작한 지 "+ counter
                   +" 초 지났습니다.");
        counter++;
    };
    
    let intervalId = setInterval(printMessage, 1000);
    
    clearInterval(intervalId);		// interval 취소하기
</script>
```



## 사용자 행동에 반응하기 (Event Handler)

##### 클릭에 반응하기

```html
<script>
	let clickHandler = function(event){
        console.log("Click! "+ event.pageX + " " + event.pageY);
        // 대개 x와 y는 화면의 왼쪽 상단을 (0,0)으로 설정하고 시작한다.
    };
    
    $("h1").click(clickHandler);
</script>
```



##### 마우스 이동 이벤트

```html
<script src="https://code.jquery.com/jquery-3.4.1.min.js"> </script>
<script>
	$("html").mousemove(function(event){		// mousemove(핸들러) 함수
        $("#heading").offset({
            left: event.pageX,
            top : event.pageY
        });
    });
</script>
```



##### 클릭한 위치에서 타겟 위치 거리 계산하기

```html
<script src="https://code.jquery.com/jquery-3.4.1.min.js"> </script>
<script>
    let width = 400;
    let height = 400;
    
    let target = {
        x:getRandomNumber(width);
        y:getRandomNumber(height);		// 시작 시마다 타겟 위치 랜덤값
    }
    
	let getDistance = function(event, target){
        var diffX = event.offsetX - target.X;	// offsetX,Y : 클릭 위치 좌표 얻기
        var diffY = event.offsetY - target.Y;
        return Math.sqrt((diffX * diffX) + (diffY * diffY));
    };
</script>
```

