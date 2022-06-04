[x만큼 간격이 있는 n개의 숫자](https://programmers.co.kr/learn/courses/30/lessons/12954?language=javascript)

### 입출력

```jsx
x	n	answer
2	5	[2,4,6,8,10]
4	3	[4,8,12]
-4	2	[-4, -8]
```

### 풀이

```jsx
function solution(x, n) {
    var answer = [];
    // console.log(Array(n).fill(x).map((element, index) => element*(index+1)))
    for (let i=1; i<=n; i++) {
        answer.push(x*i)
    }
    return answer;
}
```

리턴할 answer배열을 만든다. for loop를 1부터 n까지 1식 증가한다. i와 x를 곱한 배열을 answer에 push한다.
