

### &#128526;
[문제 링크](https://programmers.co.kr/learn/courses/30/lessons/42841)
[참고 자료](https://geehye.github.io/programmers-brute-force-search-04)

<br>
<br>
<br>

## 설명
* Level: 2
* Brute Force (완전탐색)
* 숫자야구 규칙 아래, 주어진 baseball 배열과 같은 조건들을 모두 만족하는 숫자들의 경우의 수를 구하라.


<br>
<br>
<br>

## 접근법
(실수) 처음에는 123부터 987까지 모든 수를 넣은 배열에서, baseball배열의 조건들을 분석한 뒤, 하나씩 제거하려고 했다. 근데 생각해보니 baseball 배열안의 아이템이 몇개나 되는지도 불확실하고 + 모든 조건들을 고려하지도 못할것같아서 포기....

결국 구글링을 통해 찾은 [접근법](https://geehye.github.io/programmers-brute-force-search-04)을 통해 구현했다.<br>



1) 일단 특정 기준과 임의의 숫자가 주어졌을 때, 스트라이크 / 볼을 판별할 수 있는 함수를 만든다.
```JAVA
// 스트라이크 판별
static int getStrike(String check, String num) {
  int cnt = 0;
  for(int i=0;i<3;i++) {
    if(check.charAt(i)==num.charAt(i)) { // 자릿수마다 동일한 숫자인지 체크. (Max = 3)
      cnt++;
    }
  }
  return cnt;
}
```
```JAVA
// 볼 판별
static int getBall(String check, String num) {
	int cnt = 0;
	for(int i=0;i<3;i++) {
		// 숫자들은 포함하지만, 같은 자리는 아닐 때만 cnt++
		if(check.contains(String.valueOf(num.charAt(i))) && check.charAt(i)!=num.charAt(i)) {
			cnt++;
		}
	}
	return cnt;
}
```

2) 먼저 후보군이 될 수 있는 숫자들을 모두 생성해놓은 HashMap에 집어넣는다. (굳이 HashMap이 아니라 List도 괜찮을듯?????)
```JAVA
HashMap<String,Boolean> map = new HashMap<String,Boolean>();
    for(int i=1;i<10;i++) {
      for(int j=1;j<10;j++) {
        if(i==j) continue;
        for(int k=1;k<10;k++) {
          if(i==k || j==k) continue;
          String sub = String.valueOf(i).concat(String.valueOf(j)).concat(String.valueOf(k));
          map.put(sub, false);
        }
      }
    }
```


3) forEach문을 통해 keySet에서 key를 하나씩 추출한다.
```JAVA
for(String num:map.keySet())
```


4) 뽑은 key(임의의 숫자)값이 baseball배열의 아이템 수만큼 반복문을 돌며 해당 아이템과 조건이 일치할 때마다 cnt++을 해준다. 
```JAVA
for(int i=0;i<baseball.length;i++) {
  String check = String.valueOf(baseball[i][0]);
  int strike = baseball[i][1];
  int ball = baseball[i][2];
  if(strike==getStrike(check,num) && ball==getBall(check,num)) {
    cnt++;
  }
}
```


5) 즉, 결과적으로 부합하는 조건의 개수(cnt)가 **baseball의 모든 아이템과 조건이 부합할때만!!!**,(==baseball.length) answer++
```JAVA
if(cnt==baseball.length) answer++;
```


<br>
<br>
<br>

## 유용한 함수 혹은 API
* Char 변수를 String 변수로 바꾸는 법: String.valueOf(char a)
```JAVA
String.valueOf(num.charAt(i))
```

<br>
<br>
<br>

## 숙지해야할 점
1) 완전탐색(Brute Force)문제에서는, 조건을 분석하거나 if문으로 세세하게 모든 경우의 수를 판별하기보다는 그냥 간단하게 생각하는 연습을 해야겠다......



<br>
<br>
<br>
