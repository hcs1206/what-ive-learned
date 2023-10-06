# JOIN 조건에 DUAL 테이블을 사용한다면?

- 해당 문서는 실무에서 발생한 잘못된 JOIN 동작에 대한 원인 분석 및 회고입니다.

## 개요

- 사용자의 로그인기록을 추적하고 이상거래탐지 시스템으로 데이터를 전송하는 배치 프로그램에서 이슈
- 사용자가 접속한 IP를 십진수로 변환하고 '국가별IP대역' 테이블과 LEFT OUTER JOIN하여 국가 코드를 가져오는 쿼리에서 문제가 발생함

## 무엇이 문제인가?

- 작성한 쿼리중 일부 발췌

```oracle
SELECT B.국가코드

.....

FROM 로그인기록 A
 LEFT JOIN 국가별IP대역 B ON 
        (SELECT
            REGXP_SUBSTR(A.로그인IP, '[^.]+', 1, 1) * 256 * 256 * 256
            + REGXP_SUBSTR(A.로그인IP, '[^.]+', 1, 2) * 256 * 256
            + REGXP_SUBSTR(A.로그인IP, '[^.]+', 1, 3) * 256
            + REGXP_SUBSTR(A.로그인IP, '[^.]+', 1, 4)
            FROM DUAL
            ) BETWEEN B.시작IP AND B.끝IP
 .....
```

- 언뜻보면 문제가 없어 보이는 쿼리였지만 실제로는 서브쿼리에서 DUAL을 통해 JOIN하여 A 테이블의 첫 번째 열만 가져오는 문제 발생
- 기대한 데이터는 각 IP별로 올바른 국가코드가 나와야했지만 실제로는 첫 번째 행의 국가코드가 모든 행에 복사되어 출력하게 됨

| 사용자명 | 접속IP            | 국가명     |
|------|-----------------|---------|
| 철수   | 102.168.123.125 | 한국      |
| 영희   | 28.102.179.89   | 한국      |
| 민수   | 59.105.15.3     | 한국      |

- 위의 데이터에서 영희와 민수는 다른 국가가 표시되어야 하지만 잘못된 조인 조건으로 인해 철수의 국가인 한국이 표시 

## 다른 예제 쿼리

- LEFT JOIN에서만 이런 문제가 발생하는지 확인하기 위해 여러 케이스를 시도해 봄

### LEFT JOIN과 DUAL

```oracle
SELECT A.NAME, A.MEMBER_ID, (SELECT A.MEMBER_ID FROM DUAL), B.MEMBER_ID
FROM MEMBER A
LEFT JOIN MEMBER_DETAIL B ON (SELECT A.MEMBER_ID FROM DUAL) = B.MEMBER_ID
```

| 이름 | 회원아이디 | DUAL사용_회원아이디 | 조인테이블_회원아이디 |
|----|-------|--------------|-------------|
| 철수 | CS    | CS           | CS          |
| 영희 | YH    | YH           | CS          |
| 민수 | MS    | MS           | CS          |

- 이미 알고 있던 내용으로 잘못 조인된 모습을 확인할 수 있음

### WHERE절과 DUAL

```oracle
SELECT A.NAME, A.MEMBER_ID, (SELECT A.MEMBER_ID FROM DUAL), B.MEMBER_ID
FROM MEMBER A
LEFT JOIN MEMBER_DETAIL B ON (SELECT A.MEMBER_ID FROM DUAL) = B.MEMBER_ID
WHERE (SELECT A.MEMBER_ID FROM DUAL) = 'MS'
```

| 이름 | 회원아이디 | DUAL사용_회원아이디 | 조인테이블_회원아이디 |
|----|-------|--------------|-------------|
| 민수 | MS    | MS           | null        |

```oracle
SELECT A.NAME, A.MEMBER_ID, (SELECT A.MEMBER_ID FROM DUAL), B.MEMBER_ID
FROM MEMBER A
LEFT JOIN MEMBER_DETAIL B ON (SELECT A.MEMBER_ID FROM DUAL) = B.MEMBER_ID
WHERE A.MEMBER_ID = 'MS'
```

| 이름 | 회원아이디 | DUAL사용_회원아이디 | 조인테이블_회원아이디 |
|----|-------|--------------|-------------|
| 민수 | MS    | MS           | MS          |

- 이 결과 차이에 대해서는 해석이 잘 되지 않았다
- WHERE 절에서 DUAL로 넣으면 join이 안되는데 일반 컬럼으로 넣으면 또 조인이 된다?

### INNER JOIN과 DUAL

```oracle
SELECT A.NAME, A.MEMBER_ID, (SELECT A.MEMBER_ID FROM DUAL), B.MEMBER_ID
FROM MEMBER A
INNER JOIN MEMBER_DETAIL B ON (SELECT A.MEMBER_ID FROM DUAL) = B.MEMBER_ID
```

| 이름 | 회원아이디 | DUAL사용_회원아이디 | 조인테이블_회원아이디 |
|----|-------|--------------|-------------|
| 철수 | CS    | CS           | CS          |
| 영희 | YH    | YH           | YH          |
| 민수 | MS    | MS           | MS          |

- 재밌게도 INNER JOIN에서는 또 정상 조회가 된다.
- 아마 INNER JOIN과 OUTER JOIN 동작 방식의 차이에서 오는 것이라 생각함