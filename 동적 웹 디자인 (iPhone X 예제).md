# 동적 웹 디자인

- canvas 1대 1 말고 2배로 해서 응집되게

- keyFrame
  - scroll된 값은 js에서 수치로 받아올 수 있는 값

- video-wrapper 안에 video가 있고, 그 위를 cover-canvas로 덮음

  ```css
  .video-wrapper{
      display:flex;
      justify-content:center;
      align-items:center;		// 위 3개가 안에 있는 비디오 가운데정렬
      position : fixed;		// 스크롤 움직여도 위치 고정
      overflow : hiden;		// 위에 튀어나오는 영상 안보이게	
      top:0;
      left:0;
      width:100vw;			// 브라우저 사이즈만큼
      height:100vh;       
  }
  
  #cover-canvas{		// 사이즈는 script에서
  	position:fixed;	// 스크롤이 움직여도 위치 고정
  	top:0;
  	left:0;
  }
  ```

- canvas 동작 원리 알아보기 (그래프, 시각 처리 등)

  ```javascript
  <!DOCTYPE html>
  <html>
  <head>
  <title>iPhone X</title>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta http-equiv="X-UA-Compatible" content="IE=Edge">
  <style>
  html { height: 100%; font-family: sans-serif; -webkit-text-size-adjust: 100%; -ms-text-size-adjust: 100%; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); }
  body { height: 100%; -webkit-font-smoothing: antialiased; font-smoothing: antialiased; -webkit-overflow-scrolling: touch; overflow-scrolling: touch; }
  html, body, div, span, applet, object, iframe, figure, h1, h2, h3, h4, h5, h6, p, blockquote, pre, a, abbr, acronym, address, big, cite, code,
  del, dfn, em, font, img, ins, kbd, q, s, samp, small, strike, strong, sub, sup, tt, var, dl, dt, dd, ol, ul, li, fieldset, form, label, legend,
  table, caption, tbody, tfoot, thead, tr, th, td { margin: 0; padding: 0; border: 0; }
  article, aside, details, figcaption, figure, footer, header, hgroup, main, nav, section, summary { display: block; }
  div, article, section, p, ul, li, span, label { box-sizing: border-box; }
  
  body {
      background: black;
  }
  
  #cover-canvas {
      position: fixed;
      top: 0;
      left: 0;
  }
  
  .video-wrapper {
      display: flex;
      justify-content: center;
      align-items: center;
      overflow: hidden;
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
  }
  
  #video-studiomeal {
      transform: scale(1);
  }
  </style>
  
  </head>
  <body>
  <div class="video-wrapper">
      <video muted="" playsinline="" autoplay="" loop="" id="video-studiomeal" src="https://images.apple.com/media/us/iphone-x/2017/01df5b43-28e4-4848-bf20-490c34a926a7/overview/primary/hero/large_2x.mp4"></video>
  </div>
  <canvas id="cover-canvas"></canvas>
  
  <script>
  'use strict';
  
  (function () {
  
      var elemCanvas,
          elemVideo,
          elemPhone,
          context,
          windowWidth = 0, // 브라우저 폭
          windowHeight = 0, // 브라우저 높이
          canvasWidth = 0, // 캔버스 폭(브라우저 폭에 맞춤)
          canvasHeight = 0, // 캔버스 높이(브라우저 높이에 맞춤)
          scrollY = 0, // 현재 스크롤 위치
          relativeScrollY = 0, // 각 키프레임에서 사용하는 상대적인 스크롤 위치
          prevDurations = 0, // 이전 키프레임까지의 duration
          totalScrollHeight = 0, // 스크롤을 할 수 있는 전체 높이(body의 높이로 세팅)
          currentKeyframe = 0, // 현재 키프레임(0, 1)
          phoneWidth = 1380, // 아이폰 이미지 기본 크기
          phoneHeight = 3000, // 아이폰 이미지 기본 크기
  
          resizeHandler,
          scrollHandler,
          render,
          drawCanvas,
          calcAnimationValue,
          calcFinalValue,
          init,
          pixelDuration = 0, // 키프레임 당 차지하는 스크롤 높이
          keyframes = [
              {
                  animationValues: {
                      videoScale: [1, 2],			// 원래 크기(1)에서 (2)배로 커짐
                      triangleMove: [0, 200],		// X모양을 위한 삼각형이 0~200
                      rectangleMove: [0, 500]		// 위아래 사각형 0~500 만큼 늘어남
                  }
              },
              {
                  animationValues: {
                      videoScale: [2, 0.5],		// 원래 크기(2)에서 (0.5)로 작아짐
                      triangleMove: [200, 1000],	// x가 200에서 1000
                      							// 지속시간은 같은데 차이가 크므로 빠름
                      rectangleMove: [500, 500]	// y가 500에서 500으로(일정)
                  }
              }
          ],
      
          elemBody = document.body,
          elemCanvas = document.getElementById('cover-canvas'),
          context = elemCanvas.getContext('2d');
          elemVideo = document.getElementById('video-studiomeal');
  
      init = function () {
          windowWidth = window.innerWidth;
          windowHeight = window.innerHeight;
  
          resizeHandler();
          render();
  
          window.addEventListener('resize', function () {
              requestAnimationFrame(resizeHandler);
              // 더 부드럽게 애니메이션이 동작하기 위해 준비가 됐을 때 resizeHanler 호출
          });
          window.addEventListener('scroll', function () {
              requestAnimationFrame(scrollHandler);
          });
  
          elemPhone = document.createElement('img');
          elemPhone.src = 'phone.png';
          elemPhone.addEventListener('load', function () {
              drawCanvas();
          });
      };
  
      resizeHandler = function () {			// 윈도우의 resize 이벤트가 일어날 때
          var i;
          windowWidth = window.innerWidth;
          windowHeight = window.innerHeight;
          totalScrollHeight = 0;					// 초기화
          pixelDuration = 0.5 * windowHeight;		// keyframe (움직이는 부분)이 0.5
  
          for (i = 0; i < keyframes.length; i++) {
              totalScrollHeight += pixelDuration;		// ?
          }
          totalScrollHeight += windowHeight;
  
          elemBody.style.height = totalScrollHeight + 'px';
          elemCanvas.width = canvasWidth = windowWidth * 2;	// 윈도우 폭의 2배(고해상도)
          elemCanvas.height = canvasHeight = windowHeight * 2;	// css로 원래 크기 처리
          elemCanvas.style.width = windowWidth + 'px';
          elemCanvas.style.height = windowHeight + 'px';
      };
  
      scrollHandler = function () {
          scrollY = window.pageYOffset;	// 현재 스크롤된 위치
  
          if(scrollY < 0 || scrollY > (totalScrollHeight - windowHeight)) {
              return;
          }// 전체 스크롤 범위를 벗어나면 함수 종료
  
          // pixelDuration : 각 keyframe마다 갖는 지속시간(스크롤 범위) = 윈도우 절반
          // prevDurations : 지금 스크롤된 값을 duration으로 나타냄
          if (scrollY > pixelDuration + prevDurations) {
              prevDurations += pixelDuration;
              currentKeyframe++;
          } else if (scrollY < prevDurations) {
              currentKeyframe--;
              prevDurations -= pixelDuration;
          }
  
          relativeScrollY = scrollY - prevDurations;
  
          render();		// 실제로 그림을 그리는 함수
      };
  
      render = function () {	// 그림 함수
          var videoScale, triangleMove, rectangleMove;
  
          if (keyframes[currentKeyframe]) {	// 계산하는 역할
              // currentKeyframe : 몇 번째 keyframe인지
              videoScale = calcAnimationValue(keyframes[currentKeyframe].animationValues.videoScale);
              // 맨 아래 색깔있는 비디오 크기
              triangleMove = calcAnimationValue(keyframes[currentKeyframe].animationValues.triangleMove);
              // X모양에 관련된 크기 -> drawCanvas()함수
              rectangleMove = calcAnimationValue(keyframes[currentKeyframe].animationValues.rectangleMove);
          } else {
              return;
          }
  
          elemVideo.style.transform = 'scale(' + videoScale + ')';
          // elemVideo : videoElement, scale
          
          context.clearRect(0, 0, canvasWidth, canvasHeight);	
          // 캔버스가 바뀔 때 지우는 작업을 안하고 덮어쓰므로 삭제해야 함
          
          if (elemPhone) {
              drawCanvas(videoScale, triangleMove, rectangleMove);
          }
      };
  
      calcAnimationValue = function (values) {	// 얻어온 수치들은 
          // relativeScrollY : 현재 얼마나 스크롤됐냐 (상대적인 scroll Y값)
          // pixelDuration : 총 스크롤 높이(윈도우 높이의 절반 - keyframe의 크기(?))
          // values : keyframe에 설정된 속성값들, 애니메이션이 어디부터 어디까지 움직이는지 수치
          // triangleMove, rectangleMove 의 값 배열인데, values[1]은 애니메이션 끝날 때의 값, 			values[0]은 애니메이션 시작 값 (200 -> 1000 까지 움직인다면, 800만큼 움직이는 것)
          // 마지막에 values[0]을 더해주어 초기값보다 작아지지 않게 함
          return (relativeScrollY / pixelDuration) * (values[1] - values[0]) + values[0];
      };
  
      drawCanvas = function (videoScale, triangleMove, rectangleMove) {	// 그려주는 함수
          var videoScale = videoScale || 1,
              triangleMove = triangleMove || 0,
              rectangleMove = rectangleMove || 0;	// 기본값 셋팅
  
          context.save();
          context.translate( (canvasWidth - phoneWidth * videoScale) * 0.5, (canvasHeight - phoneHeight * videoScale) * 0.5 );	// 그리는 기준점을 이동하는 함수
          context.drawImage(elemPhone, 0, 0, phoneWidth * videoScale, phoneHeight * videoScale);			// elemPhone : 폰 이미지, translate로 좌표를 가운데로 이동한 것
          		// 0,0 좌표에 videoScale : 비디오와 같은 크기로 폰 이미지를 움직이게 함
          context.restore();
  
          // context.fillStyle = 'black';
          context.fillStyle = 'red';
  
          // 위 삼각형
          context.beginPath();
          context.moveTo(canvasWidth * 0.5 - 1500, -triangleMove - 1700);
          context.lineTo(canvasWidth * 0.5, canvasHeight * 0.5 - 150 - triangleMove);
          context.lineTo(canvasWidth * 0.5 + 1500, -triangleMove - 1700);
          context.lineTo(canvasWidth * 0.5 - 1500, -triangleMove - 1700);
          context.fill();
          context.closePath();
          
          context.fillStyle = 'orange';
  
          // 아래 삼각형
          context.beginPath();
          context.moveTo(canvasWidth * 0.5 - 1500, canvasHeight + triangleMove + 1700);
          context.lineTo(canvasWidth * 0.5, canvasHeight * 0.5 + 150 + triangleMove);
          context.lineTo(canvasWidth * 0.5 + 1500, canvasHeight + triangleMove + 1700);
          context.lineTo(canvasWidth * 0.5 - 1500, canvasHeight + triangleMove + 1700);
          context.fill();
          context.closePath();
          
          context.fillStyle = 'yellow';
  
          // 왼쪽 삼각형
          context.beginPath();
          context.moveTo(canvasWidth * 0.5 - 1700 - triangleMove, -1700);
          context.lineTo(canvasWidth * 0.5 - 130 - triangleMove, canvasHeight * 0.5);
          context.lineTo(canvasWidth * 0.5 - 1700 - triangleMove, canvasHeight + 1700);
          context.lineTo(canvasWidth * 0.5 - 1700 - triangleMove, -1700);
          context.fill();
          context.closePath();
          
          context.fillStyle = 'green';
  
          // 오른쪽 삼각형
          context.beginPath();
          context.moveTo(canvasWidth * 0.5 + 1700 + triangleMove, -1700);
          context.lineTo(canvasWidth * 0.5 + 130 + triangleMove, canvasHeight * 0.5);
          context.lineTo(canvasWidth * 0.5 + 1700 + triangleMove, canvasHeight + 1700);
          context.lineTo(canvasWidth * 0.5 + 1700 + triangleMove, -1700);
          context.fill();
          context.closePath();
          
          context.fillStyle = 'blue';
  
          // 박스 상, 하
          //context.fillRect(0, canvasHeight * 0.5 - 2600 - rectangleMove, canvasWidth, 2000);
          //context.fillRect(0, canvasHeight * 0.5 + 600 + rectangleMove, canvasWidth, 2000);
      };
  
      init();
   
  })();
  </script>
  </body>
  </html>
  ```

  
