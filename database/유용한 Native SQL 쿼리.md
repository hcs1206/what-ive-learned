# 실무에서 사용하는 SQL 쿼리
## PARTITION BY
- Oracle에서 분석함수를 사용할 때 PARTITION BY를 사용하여 그룹으로 묶어서 연산을 할 수 있다. GROUP BY 절을 사용하지 않고, 조회된 각 행에 그룹으로 집계된 값을 표시할 때 OVER 절과 함께 PARTITION BY 절을 사용하면 된다.
### OVER절 실행 순서
![image/SQL_running_sequence.png](image/SQL_running_sequence.png)
### 사용방법
```sql
SUM(SAL)     --> 분석함수 SUM을 사용했고 SAL 칼럼에 대한 행들이 행 그룹
OVER         --> 분석절. 분석함수에 대한 조절을 OVER절 안에 작성.
PARTITION BY --> GROUP BY와 동일하게 그룹지어 결과 출력
ORDER BY     --> PARTITION BY로 정의된 WINDOW 내에서 행들의 정렬순서를 정의
```
### 사용 예1. ROW_NUMBER()
```oracle
SELECT NAME, SEQ, EMAIL 
FROM (SELECT NAME, SEQ, EMAIL, ROW_NUMBER() OVER (PARTITION BY ID ORDER BY CREATE_DT DESC) AS ROWSEQ
      FROM STUDENT
      ) S
WHERE S.ROWSEQ = 1
```
- ROW_NUMBER()와 함께 사용하여 기준을 세운 후 중복을 제거할 수 있다
### 사용 예2. SUM()
```oracle
SELECT SEQ, NAME, SUM(MONEY) OVER (PARTITION BY SEQ, NAME)
FROM BANK_ACCOUNT;
```
