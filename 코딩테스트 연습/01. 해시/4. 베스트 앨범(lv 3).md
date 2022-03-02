### 문제 설명

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.



### 제한사항

- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.



### 입출력 예

| genres                                          | plays                      | return       |
| ----------------------------------------------- | -------------------------- | ------------ |
| ["classic", "pop", "classic", "classic", "pop"] | [500, 600, 150, 800, 2500] | [4, 1, 3, 0] |



### 입출력 예 설명

classic 장르는 1,450회 재생되었으며, classic 노래는 다음과 같습니다.

- 고유 번호 3: 800회 재생
- 고유 번호 0: 500회 재생
- 고유 번호 2: 150회 재생

pop 장르는 3,100회 재생되었으며, pop 노래는 다음과 같습니다.

- 고유 번호 4: 2,500회 재생
- 고유 번호 1: 600회 재생

따라서 pop 장르의 [4, 1]번 노래를 먼저, classic 장르의 [3, 0]번 노래를 그다음에 수록합니다.



### 풀이

```python
def solution(genres, plays):
    genres_sum = {}
    plays_sum = {}
    for i in range(len(genres)):
        if genres[i] in genres_sum:
            genres_sum[genres[i]] += plays[i]
            plays_sum[genres[i]][i] = plays[i]       
        else:
            genres_sum[genres[i]] = plays[i]
            plays_sum[genres[i]] = {i:plays[i]}
    genres_sum = sorted(genres_sum.items(), key = (lambda x: x[1]), reverse = True)
    answer = []
    for (genre, _) in list(genres_sum):
        x = sorted(plays_sum[genre].items(), key = (lambda x: x[1]),reverse = True)
        count = 1
        for (i, _) in x:
            if count < 3:
                answer.append(i)
                count += 1
            else:
                break
    return answer
```

해시를 꼭 사용해야 하는 문제이다. 

* 장르 별 재생 횟수 합산을 위해 사용 (첫번째 for 문)
* 여기서 장르별 고유번호 별 재생횟수도 함께 저장한다.

파이썬 내장 함수인 sorted를 이용해 dictionary를 원하는 순서대로 정렬한다. 

* 여기서는 장르에 대해 재생 횟수로 정렬 
* 원래 정렬 기준은 key 기준이지만 여기서는 lambda x: x[1]으로 value 기준으로 정렬, reverse 통해 내림차순

plays_sum 의 경우 {장르, {고유 번호, 재생 수}}의 구조이므로 이를 다시 재정렬한다.

그리고 for문을 2개만 가져온다.

* 다시 생각해보니 이미 정렬되어 있어서 가져오는 부분은 굳이 반복문을 사용할 필요가 없을 듯하다.



