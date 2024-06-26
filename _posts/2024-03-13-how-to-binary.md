---
title:  "2진수에 대하여"
excerpt: "2진수에 대하여"

category: "디미고"
tags: ["2진수", "디미고"]
 
date: 2024-03-13 00:00:00 +0900

math: true
---

# 2진수에 대하여
학교에서 컴퓨터 일반 수업의 내용을 친구들이 잘 이해하지 못하는 것 같아 이 글을 씁니다. 그래서 2진수의 모든 내용보다는 친구들이 "이건 왜 이럴까?"라고 할 만한 부분을 썼습니다. 특히 보수에 대해 잘 모르는 것 같아 보수를 중심으로 썼습니다. 이상하거나 모르겠는 내용이 있으면 편하게 댓글 남겨주세요. 밑으로 쭉 내리면 댓글 남길 수 있습니다.

## 2진수와 10진수
2진수와 10진수 모두 위치 기수법을 사용하는 수의 **표현**하는 방법입니다. 2진수, 10진수 모두 그저 수를 표현하는 방법이라는 것을 명심해야 합니다.

## 10진수 to 2진수
보통 수를 0이 될 때까지 2로 나누어 가며 나온 나머지가 수를 2진수로 바꾼 값이라고 배웁니다.

$$
\begin{aligned}
2{\underline{\smash{\big)}\,13\phantom{)}}}& \\
2{\underline{\smash{\big)}\,6\phantom{)}}}& \cdots 1 \\
2{\underline{\smash{\big)}\,3\phantom{)}}}& \cdots 0 \\
2{\underline{\smash{\big)}\,1\phantom{)}}}& \cdots 1 \\
0& \cdots 1 \\
\end{aligned}
$$


$$13_{(10)} = 1101_{(2)}$$

위와 같은 방법으로 13을 2진수로 표현할 수 있습니다. 하지만 왜 이런 방식으로 하면 10진수가 2진수로 바뀔까요?

수를 2로 나누는 것은 비트의 자릿수를 하나 내려주는 것과 같습니다. 왜냐하면

$$1010_{(2)} = 2^3 + 2^1$$

$$1010_{(2)} \div 2 = (2^3 + 2^1) \div 2 = 2^2 + 2^0 = 101_{(2)}$$

이를 일반화하면 (편의상 LSB를 0번째 비트라고 하겠습니다.)

$$a \div 2 = (\sum_{k=0}^{n-1} C_k * 2^k) \div 2 = \sum_{k=1}^{n-1} C_k * 2^{k-1} (단 n은 a의 비트 개수, C_i는 a의 i번째 비트의 값)$$

이기 때문입니다.

10진수를 2진수로 바꾸는 과정에서 10진수를 2진수로 표현해 봅시다.

$$
\begin{aligned}
2{\underline{\smash{\big)}\,1101_{(2)}\phantom{)}}}& \\
2{\underline{\smash{\big)}\,110_{(2)}\phantom{)}}}& \cdots 1 \\
2{\underline{\smash{\big)}\,11_{(2)}\phantom{)}}}& \cdots 0 \\
2{\underline{\smash{\big)}\,1_{(2)}\phantom{)}}}& \cdots 1 \\
0_{(2)}& \cdots 1 \\
\end{aligned}
$$

느낌이 오시나요? 자릿수를 하나 내려주며 비트를 하나씩 뽑아주고 있습니다. 이러한 이유 2로 나누며 나머지로 2진수로 변환하는 방법이 가능한 것입니다. 소수점을 2진수로 바꾸는 방법도 이와 비슷한 방법으로 이해할 수 있으니 직접 해보세요.

## 왜 보수로 바꾸고 더하면 빼기가 되는가?
### 1의 보수란
1의 보수는 수 a와 b가 있을 때 $$a + b = \overbrace{111\cdots 1_{(2)}}^{\rm N}$$ 일때 b를 a의 1의 보수, a를 b의 1의 보수라고 합니다. 물론 $$0 \leq a, b$$입니다.

### 2의 보수
우리 디미고생은 a의 2의 보수를 (a의 1의 보수) + 1이라고 배웠습니다. 물론 이는 맞지만 아래와 같이 다른 방식으로 이해해 볼 수 있습니다. 수 a의 2의 보수는 $$2^N - a$$입니다. 즉 $a + (a의 2의 보수) = 2^N$입니다. 왜 $$2^N - a = \overbrace{111\cdots 1_{(2)}}^{\rm N} - a + 1$$일까요? $$\overbrace{111\cdots 1_{(2)}}^{\rm N} + 1 = 2^N$$이기 때문입니다. 1의 보수 또한 2의 보수 -1입니다.

### 보수로 음수를 N개의 비트에 표현하기
음수를 표현하기 위해 보수를 쓰는 것이니 절댓값 a의 보수를 취해서 사용합니다. 오버플로우 같은 일이 일어나지 않는 이상 부호 비트는 항상 1인 것을 알 수 있습니다.

$$a < 0$$

$$a를 1의 보수로 표현했을 때 = \vert a \vert의 1의 보수$$

$$a를 2의 보수로 표현했을 때 = \vert a \vert의 2의 보수$$

### 2의 보수를 사용해 뺄셈 하기
$$a - b = a + (-b) \simeq a + (b의 2의 보수)$$

$$a + (b의 2의 보수) = a + (2^N - b) = a - b + 2^N$$
입니다. 

만약 $$a - b$$가 양수라면 $2^N$은 버려지게 되는데 왜냐하면 $2^N$는 $N+1$번째 비트가 켜져 있는 값이기 때문입니다. $a - b + 2^N$에서 $2^N$이 버려져서 결과는 $a - b$입니다.

만약 음수라면 $$a - b + 2^N = 2^N - (b - a) = 2^N - \vert a - b\vert$$ 가 되므로 $(a - b)$을 2의 보수로 나타낸 값이 됩니다.

### 1의 보수를 사용해 뺄셈 하기
$$a - b = a + (-b) \simeq a + (b의 1의 보수)$$

$$\overbrace{111\cdots 1_{(2)}}^{\rm N} = 2^N - 1$$

$$a + (b의 1의 보수) = a + (\overbrace{111\cdots 1_{(2)}}^{\rm N} - b) = a + (2^N - 1 - b)$$

$$a + (2^N - 1 - b) = a - b + (2^N - 1)$$

만약 $a - b$가 양수라면 

$$a - b + (2^N - 1) = (2^N - 1) - (b + a)$$
이기 때문에 $a - b$에 1의 보수를 취한 값이 됩니다. 1의 보수인 것을 없애주기 위해 1을 더합니다. 이것이 MSB에서 carry가 발생할 때 1을 더해주는 이유입니다.

$$(2^N - 1) - (b + a) + 1 = 2^N + (a - b)$$

$2^N$은 버려져 결과는 $a - b$가 됩니다.

만약 $a - b$가 음수라면

$$a - b + (2^N - 1) = (2^N - 1) - (b + a)$$

$$(2^N - 1) - (b + a)$$가 $a - b$를 1의 보수로 표현한 것임으로 옳은 값이 나옵니다.