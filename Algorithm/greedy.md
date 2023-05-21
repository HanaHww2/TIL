# 그리디 알고리즘(Greedy Algorithm)


>📢 글로벌 최적을 찾기 위해 **각 단계에서 로컬 최적의 선택**을 하는 휴리스틱 문제 해결 알고리즘이다.

그리디 알고리즘이란 바로 눈 앞의 이익만을 좇는 알고리즘을 말한다.

대부분의 경우에는 뛰어난 결과를 도출하지 못하지만, 드물게 최적해를 보장하는 경우도 있다. 그리디 알고리즘은 **최적화 문제를 대상**으로 한다.

**탐욕 선택 속성Greedy Choice Property**을 갖고 있는 **최적 부분 구조Optimal Substructure** 문제들에 잘 적용한다.

- 탐욕 선택 속성 :  **앞의 선택 이후 선택에 영향을 주지 않는 것**을 말한다. 그리디 알고리즘은 선택을 다시 고려하지 않는다.
- 최적 부분 구조 : **문제의 최적 해결 방법이 부분 문제에 대한 최적 해결 방법으로 구성**되는 경우를 말한다.
- 위 2가지 조건을 만족하면 최적해를 찾을 수 있다.
- 그렇지 않더라도 대부분의 경우 계산 속도가 빠르므로, 정답을 근사하게 찾는 용도로 활용할 수 있다.

### 그리디 알고리즘 예

1. 최단 경로 문제를 풀이하는 다익스트라 알고리즘, 최적해를 찾을 수 있다.
2. 압축 알고리즘인 허프만 코딩Huffman Coding 알고리즘은 허프만 트리를 빌드할 때, 그리디 알고리즘을 사용하며, 마찬가지로 최적해가 보장된다.
3. 머신러닝 분야에서 의사결정 트리 알고리즘으로 유명한 ID3 알고리즘, 최선의 답을 구하므로 최적에 가까운 답을 찾을 수 있으나, 반드시 최적해를 찾는 것은 아니다. 정답을 근사하게 찾는 용도로 활용된다.

### 다이나믹 프로그래밍과 비교

두 알고리즘 모두 최적 부분 구조 문제를 다룬다. 그러나 서로 풀 수 있는 문제의 성격이 다르고 알고리즘 접근 방식 또한 다른다.

- 다이나믹 프로그래밍은 하위 문제에 대한 최적의 솔루션을 찾은 다음, 이 결과들을 결합한 정보에 입각해 전역 최적 솔루션Globally Optimum Solution에 대한 선택을 한다.
- 그리디 알고리즘은 각 단계마다 로컬 최적해Locally Optimum Solution를 찾는 문제로 접근해 문제를 더 작게 줄여나가는 형태로, 서로 반대 방향으로 접근하는 구조를 띤다.

## 배낭 문제Knapsack Problem

조합 최적화Combinatorial Optimization 분야의 매우 유명한 문제다.

배낭에 담을 수 있는 무게의 최댓값이 정해져 있을 때, 짐의 가치와 무게가 알려져 있는 짐들을 배낭에 넣어서 최대 가치 합을 갖도록 짐을 고르는 문제이다.

짐을 쪼갤 수 있는 경우인 분할 가능 배낭 문제Fractional Knapsack Problem(그리디 알고리즘으로 해결)와 짐을 쪼갤 수 없는 경우인 0-1 배낭 문제(다이나믹 프로그래밍으로 해결)로 나뉜다.

### 분할 가능 배낭 문제

무게당 단가가 가장 높은 짐부터 차례대로 채워나간다.

```python
cargo = [(4, 12), (2, 1), (10, 4), (1, 1), (2, 2), ]

def fractional_knapsack(cargo):
    capacity = 15
    pack = []

    for c in cargo:
        pack.append([c[0]/c[1], c[0], c[1]])
    pack.sort(reverse=True)

    total_value = 0
    for p in pack:
        if capacity >= p[2]:
            capacity -= p[2]
            total_value += p[1]
        else:
            fraction = capacity / p[2]
            total_value += p[1] * fraction
            break
    return total_value

r = fractional_knapsack(cargo)
print(r) # 17.3333...
```

## 동전 바꾸기 문제 Coin-Change Problem

동전을 거슬러 줄 때, 가장 작은 동전 갯수로 거슬러 주는 방법을 구하는 문제로, 동전의 액면이 10원, 50원, 100원처럼 증가하면서 이전 액면의 배수 이상이 되면 그리디 알고리즘으로 풀 수 있다. 그러나, 그 외의 경우에는 0-1 배낭 문제와 마찬가지로 다이나믹 프로그래밍으로 풀어야 한다.

## 가장 큰 합

그리디 알고리즘의 실패 사례 중 하나다. 트리의 노드를 계속 더해가다가 마지막에 가장 큰 합이 되는 경로를 찾는 문제다. 

정렬을 활용하지 않는 이상, 그리디 알고리즘을 바로 적용할 수 없다.

# 리트코드

- 78) 주식을 사고팔기 가장 좋은 시점2
    - [Best Time to Buy and Sell Stock II - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

- 79) 키에 따른 대기열 재구성
    - [Queue Reconstruction by Height - LeetCode](https://leetcode.com/problems/queue-reconstruction-by-height/submissions/)

- 80) 태스크 스케줄러
    - [Task Scheduler - LeetCode](https://leetcode.com/problems/task-scheduler/)

- 81) 주유소
    - [Gas Station - LeetCode](https://leetcode.com/problems/gas-station/)

- 82) 쿠키 부여
    - [Assign Cookies - LeetCode](https://leetcode.com/problems/assign-cookies/)