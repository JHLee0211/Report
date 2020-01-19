### 기본적인 Machine Learning(ML) 용어와 개념

---

 - Explicit programming(개발자가 하나하나 지정해주는 프로그램)의 한계 : 이메일 spam filter, 자동 운전 등은 너무 많은 규칙들 필요
 - Machine Learning : 개발자가 지정하지 않고 직접 컴퓨터가 학습하여 배우는 프로그램 (1959, Arthur Samuel)
 ####  Supervised Learning 

-  정해져있는 data(labeled data / training set) 를 가지고 학습

  - ex) 고양이 사진 모음, 개 사진 모음, 컵 사진 모음 등을 주입하여 학습시킴
  - Image labeling(이미지 러닝) / Email spam filter(스팸메일 러닝) / predicting exam score(이전 시험성적과 공부시간 정보를 통한 예측 러닝)
  - Training data set : X 정보에 대한 결과값 Y 가 있는 Training data set를 ML에 넣으면 학습하여 test를 넣었을 때 그에 맞는 Y값 산출
  -  알파고 또한 이전 기보들(Training data set)의 정보를 이용하여 이세돌이 바둑돌을 놓았을 때 그에 맞는 값 Y를 산출하여 대국함
  
  ##### Supervised Learning의 타입
  
  | 이름                       | 설명 / 예시                                                  |
  | -------------------------- | ------------------------------------------------------------ |
  | Regression                 | - 시간 소비에 따른 시험 성적(0~100점)과 같이 예측 '범위'가 넓은 경우<br />(10시간-90점) / (9시간-80점) / (3시간-50점) / (2시간-30점) 의 data set이 있을 경우, 7시간 공부하면 대략 73점 정도 나올 것을 예측 |
  | Binary Classification      | - 시간 소비에 따른 시험 합격/불합격 예측(두 가지 경우)<br />(10시간-합격) / (9시간-합격) / (7시간-불합격) / (3시간-불합격) 의 경우, 5시간 공부하면 불합격일 것 |
  | multi-label classification | - 시간 소비에 따른 학점(A,B,C,D,F)를 예측하는 경우<br />- (10시간-A) / (9시간-B) / (3시간-D) / (2시간-F) 일 경우 9시간 반은 B+일 것 |
  
 #### Unsupervised Learning 

-  자동적으로 유사한 데이터를 그룹화함
  
- 구글 뉴스 그룹핑, word clustering 등
  
    
  

### Linear Regression의 Hypothesis와 cost

---

####  Regression 

 - 시간 소비에 따른 시험 성적(0~100점)과 같이 예측 범위가 넓은 경우

  ex) 	
| 시간   | 점수 |
| ---- | ---- |
| 10시간 | 90점 |
| 9시간  | 80점 |
| 3시간  | 50점 |
| 2시간  | 30점 |

  - 위의 data set이 있을 경우, 7시간 공부하면 대략 73점 정도 나올 것을 예측

    

 #### Linear Hypothesis 

 - Regression에서  X값에 따라 Y값을 예측할 수 있는 함수의 형태가 될 때, 일차 함수의 꼴이 될 경우 Linear Hypothesis 가능
		```
		H(x) = Wx + b
	```
	



 #### Cost function(Loss function)

 -  실제 데이터 값과 Linear Hypothesis를 이용하여 나온 값(or 그래프)의 차이가 작을 수록 좋은 그래프이며,
				그 차이(거리)를 측정하는 것이 Cost(Loss) function
	
- (H(x)-y)^2 에서 제곱의 이유

       -  차이가 음수이던 양수이던 거리를 나타내주고, 차이가 작을 때보다 차이가 클 때 penality가 높아짐
     -  위 식은 한 점에서만 계산한 것이고, 함수 선에 대해 결과 값들 차이의 평균을 내면 cost이다.

     ``` 
     	cost = ( (H(x1)-y(1))^2 + (H(x2)-y(2))^2 + (H(x3)-y(3))^2 ) / 3 
     ```

     즉, hypothesis : H(x) = Wx + b 일 때 cost function은, 	

     		cost(W,b) = 1/m * (i=1~m) ∑ (H(xi)-y(i))^2

     이며, W와 b의 식으로 나타낼 수 있게 된다.

     Linear Regression의 학습은 이 cost를 가장 작게 하도록 학습하는 것이 목표이다. 

     > > Minimize cost(W,b)



### Linear Regression의 cost 최소화 알고리즘의 원리
---

```
	Hypothesis : H(x) = Wx + b
```



- Cost Function : cost(W,b) = 1/m * (i=1~m) ∑ (H(xi) - y(i))^2
  - 이 때 cost를 최소화하는 W와 b의 값을 가진 데이터를 구하는 것이 Linear Regression의 목적

		Simplified Hypothesis : H(x) = Wx
		cost(W) = 1/m * (i=1~m) ∑ (Wx(i) - y(i))^2
		Wx = H(x) 라고 생각!!


#### algorithm (경사를 따른 감소 알고리즘)

  - Minimize cost function, 많은 minimization problem에 이용

  - cost(W,b)에서 가장 minimize된 값을 찾으며, cost(w1,w2,...) 의 general function에도 적용가능

      - x축이 W, y축이 cost인 그래프
      
    - y값이 작아지는 경사를 따라 x값을 움직이며 기울기가 0인 지점을 찾기 >> 이 지점이 cost 최소값
    
	- 어느 위치에서나 시작 가능
    
    - 경사도를 구하는 법 : 미분
    
    ##### Cost(W)

    ```
      Cost(W) = 1/(2*m) * (i=1~m) ∑ (Wx(i) - y(i))^2
      	>> 미분을 편하게 하기 위해 맨 앞에 2를 나눠줌
    ```
    ##### W 
    
    ```
      W := W - α*(W에 대한 cost(W)의 미분값)
        >> cost(W)값을 미분하여 기울기값을 얻고, 기울기의 음/양에 따라 움직이는 방향을 알기 위함
        
         = W - α* 1/m * (i=1~m) ∑ (Wx(i) - y(i))*x(i)
        		>> α:learning rate(상수)
        		
    - 위 계산을 여러 번 실행시키며 W값이 바뀌면 결국 W값은 최소의 cost를 갖는 W로 이동
    ```
    ##### Cost(W)
    ```
      Gradient descent algorithm : W := W - α* 1/m * (i=1~m) ∑ (Wx(i) - y(i))*x(i)
    ```



#### Convex Function 

  - ##### 3차원 그래프에서, 밥그릇 모양으로 가운데가 볼록하게 들어간 함수
    
  - W,b에 대한 함수로 cost function을 만들 때 3차 함수가 만들어질 경우, Gradient descent algorithm이 통하지 않을 것
    
- 
  따라서, 항상 cost(W,b)함수가 Convex function 형태인지 확인해야 함!

  

### Linear Regression 의 cost 최소화의 TensorFlow 구현 

---


* 코드 (C:\Users\cjstn\AppData\Local\Programs\Python\Python36 안의 hello/tensorPlot/tensorPrac1,2,3)

* tensorPlot.py

  ```python
  import tensorflow as tf
  import matplotlib.pyplot as plt
  
  X = [1,2,3]
  Y = [1,2,3]
  
  W = tf.placeholder(tf.float32)	 # W값을 임의대로 바꾸어 가면서 보겠음
  #Our hypothesis for linear model X*W
  hypothesis = X*W
  
  # cost/Loss function
  
  cost = tf.reduce_mean(tf.square(hypothesis - Y)) # hypothesis와 Y값의 차를 제곱하며 평균냄(cost 구하는 식)
  
  # Launch the graph in a session
  
  sess = tf.Session()
  
  # Initializes global variables in the graph.
  
  sess.run(tf.global_variables_initializer())	 # session을 열고 variable을 초기화
  
  # variables for plotting cost function
  
  W_val = []
  cost_val = []			# W, cost 값을 저장할 _val list 생성
  
  for i in range(-30, 50):						# range -30 ~ 50
      feed_W = i * 0.1							# W를 -3~5 정도로 움직이겠다
      curr_cost, curr_W = sess.run([cost,W], feed_dict={W: feed_W})	# W값을 위의 값으로 feed_W 하고, cost와 W의 값을 curr_ 에 넣어 저장
      W_val.append(curr_W)						
      cost_val.append(curr_cost)		# curr에 넣은 W와 cost를 _val list에 넣음
  
  # Show the cost function
  
  plt.plot(W_val, cost_val)		# 가로가 W, 세로가 cost(W) 인 그래프 생성
  plt.show()		# W값이 1일 때 cost(W)가 최소인 것을 알 수 있는 2차원 그래프 생성
  
  
  ```

  

#### Gradient descent

- Cost(W) = 1/m * (i=1→m) ∑ (W*x(i) - y(i))^2
  - 미분을 편하게 하기 위해 맨앞에 2를 나눠줌

	Gradient descent algorithm : W := W - α* 1/m * (i=1~m) ∑ (Wx(i) - y(i))*x(i)
- tensorPrac1.py (수동으로 cost를 minimize 하는 법)

```python
# Minimize: Gradient Descent using derivative: W-= Learning_rate * derivative

  learning_rate = 0.1
  gradient = tf.reduce_mean((W * X - Y) * X)		# gradient = 1/m * (i=1~m) ∑ (Wx(i) - y(i))*x(i)
  descent = W - learning_rate * gradient		# descent = W - α* gradient
  update = W.assian(descent)				# update : W = descenct


  sess = tf.Session()
  sess.run(tf.global_variables_initializer())		# 이것 또한 그래프이기 때문에 session을 염

  for step in range(21):
	sess.run(update, feed_dict={X: x_data, Y: y_data})				# 위의 update를 켜고, X와 Y에 값을 feed함
	print(step, sess.run(cost, feed_dict={X: x_data, Y: y_data}), sess.run(W))  	# cost와 sess.run(W)의 W값을 확인함
```



### multi-variable linear regression

---

#### 복습

```
 Hypothesis : H(x) = Wx + b (W=weight, b=bias) >> 두 개의 값 학습
 
 Cost function
 	cost(=loss) 를 구하는 법
 	Cost(W,b) = 1/(m) * (i=1→m) ∑ (H(x(i)) - y(i))^2
 
 Gradient descent algorithm : 경사면을 따라 내려가면서 최적한 값을 찾는 것
```

​	위 수식들은 하나의 variable을 가지고 결과를 도출하지만, multi-variable 은 어떻게 할까?

#####   Hypothesis

​	H(x) = Wx + b
​	H(x1, x2, x3) = w1x1 + w2x2 + w3x3 + b

#####   Cost function 

​	H(x1, x2, x3) = w1x1 + w2x2 + w3x3 + b
​	cost(W,b) = 1/(m) * (i=1→m) ∑ (H(x1(i),x2(i),x3(i)) - y(i))^2

##### 	ㄴ 하지만, 위의 방법은 수가 많아질 수록 식이 복잡해지고 불편함.... >>> 따라서 Matrix 이용!



#### Matrix multiplication

- ##### 함수의 곱을 이용하여 Hypothesis 표현

  ```
  			   ( w1 )
  ( x1 x2 x3 ) * ( w2 ) = ( x1w1 + x2w2 + x3w3 )
  			   ( w3 )
  ```

- ##### H(X) = XW

  ```
  variable이 한 개일 때는 H(x) = Wx + b이지만, 
  matrix 사용 시에는 H(X) = XW 로 X를 먼저 두고 계산해주면 
  tensorflow에서 별도의 처리 없이 바로 계산 가능
  ```

  

#### Multi-variable linear regression 구현하기


* 코드 (C:\Users\cjstn\AppData\Local\Programs\Python\Python36 안의 tensorPrac4.py)
* 코드 (C:\Users\cjstn\AppData\Local\Programs\Python\Python36 안의 tensorPrac5.py)

##### Loading data from file 구현하기

https://www.youtube.com/watch?v=o2q4QNnoShY


 ▶ Slicing

	nums = range(5)		# range is a built-in function that creates a list of integers
	print nums		# Prints "[0, 1, 2, 3, 4]"
	print nums[2:4]		# Get a slice from index 2 to 4 (exclusive); prints "[2, 3]"
	print nums[2:]		# Get a slice from index 2 to the end; prints "[2, 3, 4]"
	print nums[:2]		# Get a slice from the start to index 2 (exclusive); prints "[0,1]"
	print nums[:]		# Get a slice from the whole list; prints ["0, 1, 2, 3, 4]"
	print nums[:-1]		# Slice indices can be negative; prints ["0, 1, 2, 3]"		# -1 = end, prints all exclusive end
	print num[-1]		# Prints only end of the array "[4]"
	nums[2:4] = [8,9]	# Assign a new sublist to a slice
	print nums		# prints "[0, 1, 8, 9, 4]"


 ▶ 2차원 Slicing

	- Array또한 indexed, sliced, iterated 가능(list와 같이)
	- list와 같이, 콜론(:) syntax를 통해 슬라이싱 가능
	- 콜론(:)은 dots(...)로 대체 가능


	arr = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])
	# arr ([[ 1,  2,  3,  4],
	#	[ 5,  6,  7,  8],
	#	[ 9, 10, 11, 12]])
	
	arr[:, 1]			# , 기준으로 앞은 row, 뒤는 col
	# arr ([ 2, 6, 10])
	
	arr[-1]
	# arr ([ 9, 10, 11, 12])
	
	arr[-1, :]
	# arr ([ 9, 10, 11, 12])
	
	arr[-1, ...]
	# arr ([ 9, 10, 11, 12])
	
	arr[0:2, :]
	# arr ([[1, 2, 3, 4],
	#	[5, 6, 7, 8]])


 ▶ 파일 불러오기


 * data-01-test-score.csv

```	
  #EXAM1,EXAM2,EXAM3,FINAL
  73,80,75,152
  93,88,93,185
  89,91,90,180
  96,98,100,196
  73,66,70,142
  53,46,55,101
```


 * 코드
```python
import numpy as np
import tensorflow as tf

xy = np.loadtxt('data-01-test-score.csv', delimiter=',', dtype = np.float32)

x_data = xy[:, 0:-1]		# row는 전부, col은 0에서 (end를 제외한) 전부
y_data = xy[:, [-1]]		# row는 전부, col은 end만


# Make sure the shape and data are OK (불러온 데이터 확인)

print(x_data.shape, x_data, len(x_data))
print(y_data.shape, y_data)
```



### Logistic Classification

---

- ##### Regression은 숫자 예측이었다면, Classification 은 Binary 중 결정하는 것

  - Pass / Fail, True / False, Up / Down, Show / Hide 등을 예측

  ```
   g(x) = 1/(1+e^(-x))
  ```
  
  ​		>> x가 작아질수록 g(x)는 0에 가까워지고, x가 커질수록 g(x)는 1에 가까워짐

#####   sigmoid	
      H(X) = 1 / (1+e^(-W^T * X))
      cost(W) = 1/m ∑ c(H(x),y)
      c(H(x),y) = -log(H(x))    : y = 1
    		    = -log(1-H(x))  : y = 0
    
      c(H(x),y) = -ylog(H(x)) - (1-y)log(1-H(x))

