# Chapter 02. Optimization Problems




  필요역량

- Experience writing object-oriented programs in python
- 계산 복잡도라는 개념에 익숙해야 한다.
- 간단한 알고리즘에 대해서도 익숙해야 한다.

### 주요 주제
##### Coomputation Model
- 이제까지 일어났던 무언가를 이해하거나 매일 보는 현상들을 설명하는 모델
- 아직 일어나지 않은 미래를 예측할 수 있게 해주는 모델
- 예시) 날씨 변화 모델 : 천년 동안 날씨가 어떻게 변했는지에 대한 모델, 미래에 어떻게 될 지 예측하는 모델

###### 1. Optimization Model
- 목적함수를 가지고 시작한다. 이 목적 함수는 최대화해야 하거나 최소화해야 한다. 
- 이 목적 함수의 위에 제한 조건들을 올려놓는 경우가 많다. 없는 경우도 있지만, 있다면 반드시 따라야 한다.
- 정리하면 목적 함수란 최대화해야 하거나 최소화해야 하는 것이고 제한 조건들은 몇몇 해를 제거하는 것이다. 이 둘 사이에는 비대칭성이 있으므로 둘을 다르게 다뤄야 한다.
- 최적화 알고리즘을 사용하는 걸 피하면서는 살아가기 힘들다.
- 구체적인 예시) 냅색(배낭) 문제
- 도둑의 관점) 많은 물건을 훔치고 싶은데 한정된 양만을 담을 수 있는 냅색이기 때문에 최적화 문제를 풀어야 한다. 가장 값비싼 물건을 훔쳐야 하는 최적화 문제를 말이다. 이 경우 목적함수는 매매할 때 값이 최대한 많이 나가도록 하는 것이고, 제한 조건은 냅색 안에 다 들어가야 한다는 것이다. 이 문제는 두 가지 종류가 있다.
> 1. 0/1 냅색 문제 : 물건을 가져가거나 못가져가는 것을 의미(금괴를 가져가거나 못 가져가거나), 복잡하다. 왜냐하면 한 번 결정을 하면 이 결정이 다음 결정에 영향을 주기 때문이다. greedy algorithm이 최적의 해를 보장해주지 못한다. 벡터와 벡터 간의 연산으로 쉽게 표현 가능하다.
> 2. 연속 냅색 문제 : 분수 냅색 문제라고 불리우며 일부분만을 가져갈 수 있는 경우(금괴를 금모래를 변환), 풀기 쉽다. greedy algorithm으로도 풀 수 있다.

- 0/1 냅색 문제를 푸는 방법
  - 가장 명백한 해답은 무차별 대입 알고리즘 : 모든 가능한 조합을 나열하고, 사용 가능한 집합의 모든 부분집합을 만든다. 이는 멱집합이라고 불리운다. 임의의 집합의 멱집합은 공집합을 포함하고 전체 집합을 포함하며 그 사이의 모든 집합을 포함한다. 즉 크기 1, 크기 2 등의 부분집합을 모두 포함합니다. 모든 가능한 집합을 만들면 모든 집합의 목적함수의 값을 알 수 있다. 값이 가장 큰 집합을 택하는 것이다. 물론 하나가 아닐 수 있기 때문에 아무거나 선택하면 된다. 정리하면 모든 경우의 수를 계산하고 승자를 선택하는 것이다.
  - 이 방법은 실용적이지 않다. 길이 100인 벡터가 있을 때의 멱집합이 있다고 가정하면, 매우 긴 시간이 필요하다. n이 1000에 가까운 실생활의 문제에서 무차별 대입 알고리즘은 비효율적이라고 할 수 있다. 그렇다면 정답을 주는 더 좋은 알고리즘이 있는가? 안타깝게도 냅색문제에서는 존재하지 않는다. 게다가 많은 최적화 문제는 본질적으로 지수적이다. 
  - 지수적이라함은 최악의 경우에도 문건들 갯수의 지수승보다 빠르게 정확한 해를 제공하는 알고리즘은 존재하지 않는다는 것이다. 지수적으로 어려운 문제이다.
  - 그러나 꽤 좋은 알고리즘이 있다. 다음을 살펴보자


**greedy algorithm**
- 냅색이 꽉 차지 않는 한 가장 좋은 물건을 냅색에 넣는다. 가득차면 그 때 끝내면 된다. 가장 좋은 것의 의미는 무엇일까? 가장 값이 많이 나가는 물건? 가장 저렴한 물건? 아니면 단위 당 값의 비율이 가장 높은 걸 의미할까? 
- 음식으로 생각해 보자.
```Python
class Food(object):
    def __init__(self, n, v, w):
        self.name = n
        self.value = v
        self.calories = w
    def getValue(self):
        return self.value
    def getCost(self):
        return self.calories
    def density(self):
        return self.getValue()/self.getCost()
    def __str__(self):
        return self.name + ': <' + str(self.value)\
                 + ', ' + str(self.calories) + '>'
```



```python
def buildMenu(names, values, calories):
    """names, values, calories lists of same length.
       name a list of strings
       values and calories lists of numbers
       returns list of Foods"""
    menu = []
    for i in range(len(values)):
        menu.append(Food(names[i], values[i],
                          calories[i]))
    return menu
```

이름, 가치, 그리고 칼로리의 배열을 입력을 받아 메뉴를 만드는 함수이다. 메뉴는 튜블로 구성되는데  이 때 음식은 이름과 가치와 칼로리를 알려준다. 

```python
def greedy(items, maxCost, keyFunction):
    """Assumes items a list, maxCost >= 0,
         keyFunction maps elements of items to numbers"""
    itemsCopy = sorted(items, key = keyFunction,
                       reverse = True)
    result = []
    totalValue, totalCost = 0.0, 0.0
    for i in range(len(itemsCopy)):
        if (totalCost+itemsCopy[i].getCost()) <= maxCost:
            result.append(itemsCopy[i])
            totalCost += itemsCopy[i].getCost()
            totalValue += itemsCopy[i].getValue()
    return (result, totalValue)
```

keyFunction 때문에 유연한 탐욕 알고리즘이라고 부를 수 있다. 물건의 원소를 숫자에 대응시켜주는 일을 한다. 이는 물건을 정렬하는 데 사용된다. 즉 물건들을 가장 좋은 것부터 나쁜 순서로 정렬할 때 이 함수는 가장 좋은 것의 의미를 어떻게 정의했는지 알려주는 데 사용한다. keyFunction은 가치를 반환할 수도 있고, 가중치를 반환할수도 있고, density의 함수를 반환할 수도 있다. 그러나 중요한 것은 가장 좋은 것의 정의와 독립적인 greedy algorithm을 만들기 위한 것이다.

어디서 시간이 많이 소요될까? 첫번째로 sorted부분이 보인다. python은 팀 정렬을 사용하는 데 가장 나쁜 경우의  시간복잡도가 병합 정렬과 같은 퀵 정렬의 일종이기 때문에 nlogn이 된다.(여기서 n은 물건의 개수)

두번째로 for loop가 있다. 각 물건마다 한 번씩 돌고 전체 물건을 다 돈 후 루프를 끝내는 루프를 총 n번 돈다.

그렇다면 최종 시간복잡도는 n+nlogn=nlogn이 된다.

O(nlogn)은 매우 효율적이다. 일백만 정도로 큰 숫자에 대해서 사용해도 문제없을 정도이다. 1,000,000*log(1,000,000)은 그리 큰 숫자가 아니기 때문이다.  매우 효율적이다.

```python
def testGreedy(items, constraint, keyFunction):
    taken, val = greedy(items, constraint, keyFunction)
    print('Total value of items taken =', val)
    for item in taken:
        print('   ', item)
```

Using greedy : 물건, 제한 조건과 keyFunction을 입력 받아서 greedy 함수를 호출한다. 이 경우 제한조건은 무게일 것이다. 그 다음 현재 가진 것을 출력한다. 

```python
def testGreedys(foods, maxUnits):
    print('Use greedy by value to allocate', maxUnits,
          'calories')
    testGreedy(foods, maxUnits, Food.getValue)
    print('\nUse greedy by cost to allocate', maxUnits,
          'calories')
    testGreedy(foods, maxUnits,
               lambda x: 1/Food.getCost(x))
    print('\nUse greedy by density to allocate', maxUnits,
          'calories')
    testGreedy(foods, maxUnits, Food.density)
```

여기서 lambda를 사용했다.칼로리가 적은 순으로 정렬하길 원하기 때문에 간단한 작업을 하기 위해 인라인 함수로 작성했다.



**lambda**

익명함수이다. 사용하는 방법은 키워드인 lambda를 먼저 쓰고 식별자 배열을 쓰고 :를 쓴 뒤 원하는 식을 표현한다. 람다가 하는 일은 이 파라미터들로 표현된 식을 계산하고 그 표현식의 결과를 반환하는 함수를 만드는 것이다. def를 쓰는 대신에 인라인 함수로 정의한 것이다.  람다 표현식은 한 줄에 쓸 수 없다면 따로 def를 쓰고 함수의 정의를 하는걸 추천한다. 디버깅이 쉽기 때문이다. 그러나 한 줄에 쓸 수 있다면 람다를 사용하라!




다시 돌아가 위의 lambda의 x의 자료형은 무엇이 되어야 하는가
- 정답은 getCost 함수의 인자의 자료형을 보아야하고 이것은 Food 이며 Food 자료형이어야 한다.



예시의 해를 살펴보면 탐욕 알고리즘의 중요한 점을 볼 수 있다. 알고리즘을 사용했지만 다른 해를 얻었다는 점이다. 탐욕 알고리즘이 갖은 문제는 각 점에서의 지역 최적 해를 선택해 지역 최적 해의 수열을 만들지만 이 수열이 항상 전역 최적 해를 가져다주는 것은 아니다. 이는 보통 언덕 오르기라는 예로 설명할 수 있다. 지역 최적해에 도달할 경우 더 이상 진행하지 않고 멈추기 때문에 전역 최댓값에 도달할 수 없다.

그렇다면 어떤 조건이 최적의 조건인가? 미리 알 수 있는 방법은 없다. 이런 정의가 더 잘 될 때가 있고 저런 정의가 더 잘 될 때도 있다. 심지어는 어떤 정의도 잘 안되서 최적의 해를 구할 수 없는 경우도 있다. 탐욕 알고리즘으로는 좋은 해를 구할 수는 있어도 최적 해를 구할 수는 없다는 것이다.

더 좋은 최적해를 구하는 방법이 있을까?




###### 2. Statistical Model


###### 3. Simulation Model
















