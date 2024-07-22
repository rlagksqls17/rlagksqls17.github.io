---
layout: post
title: "[Python] 프로파일링하기 (기술 모음)"
categories: project
tags: test
last_modified_at: 2024-07-22T08:00:00~18:00
---  


## 핵심요약 
프로파일링은 주어진 코드의 전체적인 수행 능력을 확인하는 것이다. 코드를 돌릴 때 필요한 서버의 자원을 예측하여, 발생할 문제들을 미리 막을 수 있다. 효율적으로 프로파일링을 하는 여러 방법들이 있다.   

첫째, 데코레이터를 활용하면 굳이 반복해서 모든 함수의 수행시간을 확인할 필요가 없다. 

둘째, timeit 모듈을 활용해서 주어진 파일 외부에서도 개별 함수의 수행시간을 확인 가능하다.  

셋째, cProfile 프로파일러를 활용하여 함수별 수행시간 확인 가능하다.  

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

## 3. cProfile  

cProfile은 파이썬 표준 라이브러리가 제공하는 세 가지 프로파일러 중 하나로, 이를 사용할 경우 코드 수행 시간이 길어질 수 있지만 그만큼 더 많은 정보를 제공한다.  

아래는 윈도우 Visual studio code에서 수행했을 때의 결과인데, 결과가 좀 복잡해보인다.  

```python
line 1: PS C:\Users\USER\OneDrive\바탕 화면\code> python3 -m cProfile -s cumulative .\high_perform_julia_set.py
line 2: Length of x: 1000
line 3: Total elements: 1000000
line 4: 33219980
line 5: @timefn: calculate_z_serial_purepython took 7.9557788372039795 seconds
line 6: @timefn: calc_pure_python took 8.56427788734436 seconds
line 7: Function name: calc_pure_python
line 8: 36222041 function calls (36222040 primitive calls) in 8.561 seconds

   Ordered by: cumulative time

  ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    8.561    8.561 {built-in method builtins.exec}
        1    0.000    0.000    8.561    8.561 high_perform_julia_set.py:1(<module>)
      2/1    0.016    0.008    8.559    8.559 high_perform_julia_set.py:9(measure_time)
        1    0.397    0.397    8.544    8.544 high_perform_julia_set.py:18(calc_pure_python)
        1    4.927    4.927    7.951    7.951 high_perform_julia_set.py:47(calculate_z_serial_purepython)
 34219980    3.021    0.000    3.021    0.000 {built-in method builtins.abs}
  2002000    0.192    0.000    0.192    0.000 {method 'append' of 'list' objects}
                    ... 생략 ...
       12    0.000    0.000    0.000    0.000 {built-in method builtins.setattr}
```


순차적으로 하나하나 기록해보면, 먼저 line 1에서 -s cumulative 옵션은 각 함수에서 소비한 누적 시간순으로 정렬되어 어떤 함수가 더 느린지 쉽게 확인할 수 있다 (책에서 인용).  

line 2 ~ 7번까지는 모두 해당 코드의 결과값이다 (여기서 timefn은 이 글의 첫번째 섹션에서 데코레이터를 활용해 코드를 작성했을 때 포함된 결과값이기 때문에, cProfile의 시간 측정 결과는 아니다.).   

line 8번부터가 진짜 cProfile의 결과다. 파이썬 내 함수가 총 36222041번 호출되었고, 8.561초가 걸렸음을 확인할 수 있다.  

그 아래에 이상한 수치들과 함께 결과들이 나열되어 있는데, 이는 해당 코드 내 각 주요 함수들의 수행시간이다. cumtime 컬럼이 실제 컴퓨터에서 수행된 누적 시간 (맨 아래에서부터 각각의 함수가 실행된 시간을 누적한다.), 그리고 tottime 컬럼이 해당 함수가 실제로 수행되는데 걸린 시간이다 (실제 함수 수행시간 + cProfile이 측정하는데 걸린 시간이니 주의하자). 

ncalls 컬럼의 경우, 전체 코드가 한번 실행될 때 그 함수 혹은 메소드가 호출된 횟수를 뜻한다. 작성된 코드에 따라 ncalls가 달라지니 참조하자. 예를 들어, 이 코드에서는 list.append() 메소드가 2002000번 호출되었고, 걸린 시간은 총 0.192 초이다.  

---

