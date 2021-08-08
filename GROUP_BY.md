> ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다. 

### 고양이와 개는 몇 마리 있을까

> 동물 보호소에 들어온 동물 중 고양이와 개가 각각 몇 마리인지 조회하는 SQL문을 작성해주세요. 이때 고양이를 개보다 먼저 조회해주세요.

```sql
SELECT ANIMAL_TYPE, COUNT(DISTINCT(ANIMAL_ID)) AS 'count'
FROM ANIMAL_INS
GROUP BY ANIMAL_TYPE
```

### 동명 동물 수 찾기

> 동물 보호소에 들어온 동물 이름 중 두 번 이상 쓰인 이름과 해당 이름이 쓰인 횟수를 조회하는 SQL문을 작성해주세요. 이때 결과는 이름이 없는 동물은 집계에서 제외하며, 결과는 이름 순으로 조회해주세요.

```sql
SELECT NAME, COUNT(NAME) AS 'COUNT'
FROM ANIMAL_INS
GROUP BY NAME
HAVING COUNT(NAME)>=2
ORDER BY NAME

# 헷갈렸다..
```

> ANIMAL_OUTS 테이블은 동물 보호소에서 입양 보낸 동물의 정보를 담은 테이블입니다. ANIMAL_OUTS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, NAME, SEX_UPON_OUTCOME는 각각 동물의 아이디, 생물 종, 입양일, 이름, 성별 및 중성화 여부를 나타냅니다.

### 입양 시각 구하기(1)
```sql
SELECT EXTRACT(HOUR FROM DATETIME) AS 'HOUR', COUNT(ANIMAL_ID) AS 'COUNT'
FROM ANIMAL_OUTS
GROUP BY HOUR
HAVING HOUR BETWEEN 9 AND 19
ORDER BY HOUR
# between으로 안하고 <= 으로 비교했다가 틀릴 뻔했다... 
```

### 입양 시각 구하기(2) 
- level 4 문제.. 풀지 못했다 ㅠㅠ 그래서 다른 분의 풀이로 대체한다. 지금 바로 해결하기는 어려울 것 같다. 내공이 좀 더 쌓이고 나서 도전해 볼 것....
- https://velog.io/@jinseock95/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4MySQL3.GROUP-BY-%EC%9E%85%EC%96%91-%EC%8B%9C%EA%B0%81-%EA%B5%AC%ED%95%98%EA%B8%B02

```sql
WITH RECURSIVE HOUR AS(
SELECT 0 AS h # 3번 정리(비반복문)
UNION ALL # 2번 정리(UNION 사용)
SELECT h+1 FROM HOUR WHERE h<23); # 4,5번 정리( HOUR 참조, where 정지조건)

SELECT h AS HOUR, COALESCE(COUNT(ANIMAL_ID),0) AS COUNT
FROM HOUR LEFT JOIN ANIMAL_OUTS ANI ON HOUR.h = HOUR(ANI.DATETIME)
GROUP BY HOUR.h;
```
