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

<img src = 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAQwAAAC8CAMAAAC672BgAAABDlBMVEX////s7/VxjbboXVr/s1bz9fnkWVjm5uZnhrLw8vfc4ux9lrxtirTmTUnoWlfnT0z75OT4+fvreXb09PTi4uKEm7/+9/fu7u7i5+/pV1N2kbi3xNiqudGis82Xqsjzs7L2x8bxpKPBzN752djwnZvSgInWnKTPprLS1+ONo8Pg3OTMZnHOiZT/rED87+//59D/9ev/r0vqa2j/8eT/y5LysK//5Mv/v3X/3Lj/umn/x4v/0qP0vr33z87N0d7iz7vqyKbn2s3ou4v/wXjuj43pZmP/t2DUs73U1NTGxcW2trmXlpXNzMzmQz/jx6raw8v5zbztd1zyjmf1spvrZUrzoIX3vKbqV0rhemzhv67vKPoNAAAMU0lEQVR4nO2dDWPaNhrHRVoftVtCXGhwEpIQ6Dp3N95CQ2gC2e7W9dJeu7W33fXu+3+Rk+QXybYe+U0EbPzf6lJjHPuXR8+LJGTUQZVcVSgqVapUqVKlSpUqVapUCdm2hkzLsA3DuOtv+mI2rbv+Sb9vGa0uujvpbvpiNi3TtDTd1GrIwC83fTGVtkmmpulEmrbblmFqei0kbTeBYD8hlr5zPEyAhGsfm768h5QcxU7hqMeiINqJxtJqJ2KBfcemr3T9aluiRsFpd4yjIzQL68mep2cveBpl9hxmO5JXUBkcjKeBiFveplLvAFGEg7EXhFFWGq02GFElMMpJo96BswsZjBLSwGaBEMRCDqN0XrTdQhIWMTDKFWE7dbIVB5IkMGqbvgF1Mtv0NysrR+JglMZt1N3JMBIWGMYzX8eiyr4cDYU6TiKo88LRaRfr7IxsT4TNaaM3oUh1f46UlEWN9PrZumWQDkDhAcWPKI7jpJIbBtEdpmDZoJfd4G0oEY2nrmJZOBhgGsU2Dc4sQMPQ/bLdcLqGNc0WlfIFNw03nnoCWPT3jh3tHe8Rx6m9OPa1F+r3KG5AqQcnlwI5hn7yjJXuXQJjj+14EoJR1FzDj6eeoFYSgXG8B8IoaDvhHacjMYuUMIrYTgKO05USGMWLJ2bULOCyJB2MwjmNusAs4IwrHYyCOY1WW9yuoUSqzDDq0Jc1ABZpYRTIg0biKRMMg5XuT9w8g+0oMAyR4/QEwrBPiZ7SLSnU9O4pU+TwooQTUTxlgmDU6IQdGxfvXumucyooDBNwnN7bIAwiXKfqtvSIQsEAHacrGQynZtdt0Wh0AWFIHKenyG15LUGzDCcH0QxLA5tHcWDEmQVRhMXJmavXZ68NYh/4b2/XKUhj22HIHaenyF295iIoaSd877hoqMDRlodWWTzlJIDBcisHBku2CgojmVmgaDqeEcZabyafYuIpr/DtlQ5GEsfpKRxbSwYjQTzlpAbGtgaThI7TlxIY2+k/EztOXyEPmg3GOm4lr4Qde3GficAQ5Rl70jxje3r92P2LO/biFLqv0xe+ntIM9Cl9SXecARno9rSSVr1OeUAde3EKz+ALTwimrwy6hbJxpfeTV4RHmngaFHCHAUkL+fyx5Pwl02Hus2k//fy3rC1XMqErGYzcV4+eH/n6fJD3ZO3r6XT6+O+8dcwnM4Rm+L9YxXzBhNbuhqSEV5BkPN9/5KmZE0bnp+FjoukvOLR6PBaL2/l8frtI8Hm5aXRPsE7J5qS/rmlMymDgQuSxqym59U69Tjzp4eFkNpnNJklOIWPBlfBPhBPcVGSfqmBgUxhPPRrXzj6XR2LJJjLFJl05rt1XVhjBXIIWIgsfxtR/o+MG3GTKAUNJjpHZMvg6zPEQcx/GY/7AVgoemWGoKdGyNxOfhleITIaeYbwJHZqYBxxR5DAUJeI5fIZLgxUib1zTGArcJeaRJB0D3YYcRroLB/V8n+gR2TTSOlBCI1CfXhMa0+FiLAweiXhANKQwUl43qHe/Xl5e/voIby4fjdJ+uB0uRBYYxxtMYubjmNxeX9/6bMx4HgANGYy0ly3XwWFqDlT1e/Ct2Zgm94vhlNoKeyOWh5iGBIb7udEq0z2ENMLN43CQ/nMknkr69giOsetTh+PAW/IEREjDesIUgMF850ABjRF1FelpOL9gWcE+m731Yu378Hu6hIcwpgCj73xMzU9j5NWqKWl4jlPafeEF21CAmd9c38xl9pGkgqUKniArjQtXL99dXNHzXFy883alwyKhMWE56YKj8ZYGnbdIkqDGr5EQbCKustE4/OwV7vtHTbLjqoFfOWqcpzuXpP0zy0CHYyxK5K2DiNKAE7L4b1yIVhQZ9NJdO9Vhw8+1Hh2RHedH7N8pYSA4vbzxTOMX59+T8XjmlzC+VwV4yHEAi6tkoaEUBkxjIshJbz0YfNYuTsjgxgLXIhloqIUB01i8n5LOL76jy7eW6U3gUHECIuj9teQrEKX3G4phoI6Yxhyh8e3tPLCPWcZt+HAxD9PU3Giq61qtHdtHkpqGahhiGnPBPjTzvOpQ2DHakQ27JKt+07YU5TBQh/udzubUNwpZ+MVtpNBnpxInIJ3EwxCrHt784zsmS3q4ehiMxuz9cDod3kIsMA38/nQIsqAnC/OQGkxEq94IvWr6/f6NH6RHczD2FcHwaLjNYBrJwJnmi9tFbBcxl5C1UnamIvTyCr1id9SUw0D0SKdj55L8e8nBWKb7wb5alMZ7vo9cqDH0RuSEhIeZqhuVSQSD/r6As41yDxsFRWj4/tHrI48qwYiSr3b7Ph2K1qeO07MggvHhw/2H9kdRxwNBMcg/pBi4kjrXLTwEDkpsGK6jSNhj6Kquffj4kbwQwbhH/7z/9EnQ73AwYltlatXnggGDgA4TGgbvKBL0kPkf+9S+p8cGYfSWK8nv/cB9b6SWhtn2+8hvxEckMoyoo0jBw1HYMg57y2WwHj/suTpf9qjDGPSult6+VD8LkvmzV4+IHegkgWFA95080zhfCpvJwXK5ZH6y97npCAffd2THJX7l7so/Ck/l0JgO52gsuu9Yw4hkFHw6rrcTTZEaXR2gVw02v4APrYPlsue0hlWTRdCXZIfCUXhfv70dTm8oiCiOiTy/iGQUomk6sUvFji7w5vdXTOEO9NXVFc5SHwSGybLfSXgIRWYYEUeRpYRHxC4SXOTgZRwMHFuwj8kfbrngNRl7p5t9+TKRGEbEUWTq3MEaJc2gY2AMcMF3vrpIeDKJ+FA+o9Yw+4rdUuNfwPFR15it2w+lYBEHY7UaDFa9DGMoEQUSm/EYzb7RH3T0VXCsoPTI2CGM0rB4GJ9BFUzzxl/dn3P0R+g4YemRcagApWLxgDCCNGZ+lfxn4KDEXZ+65cvgv7oX8qRpWDwkjACNL/7PbTAXCuRQQm9hcMOLx+LhRZSSBVp9bnj67MCgr5t0h2IYPA0G45sLA+yjyDrwjNKyILPvfHE7BtwOhWI0Jt9cFvvUg0r6KHJMSUjLApBqm/DEaPzbrRQaX+DSgyjHZBVFLNYGg6PxtYm9037jD01abOWYxqSKxfpgcMPS//nz27f//q+T8TvxDzLBbXRAtKJbNWcMiRukj+/WBVk8zNTHy0azqbaED8slkKRvBmax5ZNikwvTSNYrU9jp0inUuk/4rU4JCyfpIrPow0mXyoaybhh06CPZwIe8IOkTdbt3eCteBFPBxa4XhucoktCI//JNzdbw/8X48k1UzFEAUxZ4xVaqzhIrd2tcKfYBfAZVPI0YFLphOAsQgd9Ty28aDwUjMGVBJCiUeIW77S26Y/X9Yl61aTwYjDgagD2ccKW7s2gZt0P1SrHrT7p8SWkkXilWspxd7qT8YMCU91xxakloAG6xxCvFSmiIWZR6pdgWNBdJzeKo27qYCCCIhhoYxWonII2dXCkWorGLK8USmSIaAIsSrxTryhRU9DCMyEqx3I4SwBDRgGDUDFq6n56QLVluRr/rM0WOLlg4cRShAcJwS3fb0tnsnaKvFBtWiIasL0M3yEqxbs0ao2LCCNGQwHAxJKNRUBghGlEG3lJMZIFYep81A3rIS/FhBGlEWHz/VyZiExYLsXsvQBqFhRGgEYHxY3PfU4P0bcHPa+VVwNDqiaMRhXH0F09hGEVdKTZGjEbYOWaEscF7yS+fRuQJrDsIw6cRjq3ZYGzPso/Z5A3SK4FR3GDiyqWhBEaR/acjZ8pCeAXMTDA2fSsKRGmEnIb+I/cFCQdGzPNaa8V3GVT16OOd9d+/ZyLdFhZ5uEv3lD7qRfy81jK0EiJCI9xOwpU6faSFbAnMUrQSIkIDuMOA1rxS7Jao00rwnYq1rxS7LcI05BxiF1QujWEgMiwtNQ3rNZP4YS+bvgGl6nRkMGIf9lKOUOKrU5fCkCddpcgxeLXa2WFs+trVS0IjBkbJGgkVTEMOo0yRhAmkIYVROofhqvVDehhlZQHahgRGeVmQefdiGFCeUWYWCJkiGtYLPwENPuylnL6TyRS1FO7hLzyLMsbUkCTZ1864CyYz0bfhd8AsHMWvk1B2bxGQHMdOoSAywaGznWkgAZnRfq3Y9YfKLVNzHu+saeZug6jEFMwkajaydUu+7muJZfctu2937+5s/JdloDNkd7ubvqhNCftLXbfsGvljkK+12jWMpBLajeS7UqVKlSpVqlSpUqVKEmV+2nv51Pk/B045QGt+8KoAAAAASUVORK5CYII='>

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

- Component를 만드는 방법은 두 가지가 있는데, 각각 class와 함수를 이용하는 방법임



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
      const name = 'react';				// const : 
      return (
      	<div>
              hello {name}!				<!-- { 변수명 } 을 통해 값을 가져옴 -->
        </div>
      );
  }
}

export default App;
```

