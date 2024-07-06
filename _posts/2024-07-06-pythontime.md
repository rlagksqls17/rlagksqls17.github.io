---
layout: post
title: "[Python] 프로파일링하기"
categories: project
tags: test
last_modified_at: 2024-07-06T08:00:00~18:00
---  


## 핵심요약 
프로파일링은 주어진 코드의 전체적인 수행 능력을 확인하는 것이다. 코드를 돌릴 때 필요한 서버의 자원을 예측하여, 발생할 문제들을 미리 막을 수 있다. 효율적으로 프로파일링을 하는 여러 방법들이 있다.   

첫째, 데코레이터를 활용하면 반복해서 모든 함수의 수행시간을 확인할 필요가 없다.  

둘째, timeit 모듈을 활용해서 주어진 파일 외부에서도 함수의 수행시간을 확인 가능하다.  

---

## 개요  

미샤 고렐릭, 이안 오스발트가 지은 "고성능 파이썬"이라는 책을, 저번 [GIL 관련 글](https://rlagksqls17.github.io/proejct/2024/05/30/pythonGIL.html)에 이어서 학습하여 쓴 글이다. 본인은 보통 코드를 작성하고 난 뒤에 걸리는 시간 및 메모리 확인, 그리고 출력값은 다음의 명령어를 써서 확인하는 편이다.  


```python
print()
```
=> 기본적인 출력값 확인

```python
$free -h  
```  
=> 현재 사용하는 메모리 용량 확인


```python 
$top
```
=> 현재 사용하는 프로세스 정보 확인  


이외 프로그램이 걸리는 시간은 사실 본인이 프로그램을 돌린 시간과 끝나는 시간 (30분 마다 프로세스 확인)을 체크하여 계산한다. 이 책을 읽고 나서 이런 프로파일링 과정을 더 효율적으로 수행할 수 있는 방법들을 알았고, 그 방법들에 대해 여기다 기록해두려고 한다.  


---  

## 1. 데코레이터  

데코레이터를 활용해 현재 함수의 걸리는 시간을 확인할 수 있다. 다음 코드를 한번 봐보자.  
책에서는 동작하는데 시간이 많이 걸리는 복잡한 알고리즘 코드를 예시로 들었기 때문에, 해당 알고리즘이 무엇인지 설명은 가독성 위해 생략하였다.  
줄리아 집합 그래프를 그리는 알고리즘 코드라고 한다. 

---

```python 
import time


def timefn(fn):   
    def measure_time(*args, **kwargs): 
        t1 = time.time()
        result = fn(*args, **kwargs) 
        t2 = time.time()
        print("@timefn: {} took {} seconds".format(fn.__name__, t2 - t1))
        return result
    return measure_time
```

timefn()은 데코레이터 함수이다.   

calc_pure_python 및 calculate_z_serial_purepython 함수가 인자(즉 fn)로 들어가게 된다.
**앞으로 시간을 재고자 하는 함수를 fn이라고 부르겠다.**  

measure_time()은 timefn() 데코레이터 함수에 의해 실행된다. fn의 수행 시간을 계산해주는 기능을 한다. 이 함수 내 코드, 즉 result = fn(*args, **kwargs)에서, [args 및 kwargs](https://brunch.co.kr/@princox/180)는, fn의 인자를 말한다.  

그리고 나서 print()를 통해서 fn의 수행시간을 출력하게 된다.  

---

이제 fn의 코드를 살펴보자. '@'가 있어 익숙하지 않은 분들은 익숙하지 않을 것이다. 코드를 먼저 본 후에, 아래 단락의 설명을 봐보자.

```python
@timefn
def calc_pure_python(desired_width, max_iterations):
    """
    상세 알고리즘 생략
    """
    return output


if __name__ == "__main__":
    calc_pure_python(desired_width = 1000, max_iterations = 300)
```


'@'는 데코레이터다. 이전에 우리는 timefn 함수를 정의하였고, 따라서 주어진 fn함수를 인자로 해서, timefn() 데코레이터 함수에 넘겨서 실행하도록 한다. 즉 @timefn이 붙은 calc_pure_python 함수는, 그 calc_pure_python 함수의 인자까지 모두 포함해서 timefn에 그대로 넘겨준다.  


**왜 굳이 이렇게 짜야할까? 왜냐면 또다른 함수의 수행시간을 살펴봐야 할때도 그냥 그대로 @timefn 데코레이터를 붙이기만 하면 되기 때문이다. 즉 반복되는 코드를 줄일 수 있다.**  

결과는 다음처럼 나온다.  

```python
timefn: calculate_z_serial_purepython took 3.9253692626953125 seconds
```

---  

## 2. timeit  

timeit은 주어진 코드 파일 외부에서도 입력하여 쉽게 수행시간을 확인 가능한 모듈이다.  

사용방법은 간단하다.  

```python
python -m timeit -n 5 -r 5 \
-s "import {file_name}" "{file_name}.{target_function_for_timecheck}"  
```  
<br>
주어진 파일 외부에서 time it 함수를 사용할 때, "import {file_name}" 명령어를 넣어줌으로써, 주어진 파일, 즉 모듈을 import 해야한다는 것을 잊지 말아야 한다. 그리고 나서 그 모듈 내의 메소드의 시간을 확인하면 된다 ("{file_name}.target_function_for_timecheck").  

-n 5, -r 5는 해당 모듈을 5번 테스트한 평균값을 다시 5번 구해, 제일 빠른 값을 출력해준다.  

---  

