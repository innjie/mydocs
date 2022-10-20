쿼리 결과에서 첫 행과 마지막 행을 가져올 수 있다.

-   정렬 `ORDER BY`와 함께 쓰기 좋음

````
title: code example

```SQL
SELECT ENAME, JOB, SAL
	, FIRST_VALUE(SAL) OVER AS SAL_FIRST
	, LAST_VALUE(SAL) OVER AS SAL_LAST
FROM EMP
WHERE JOB IN ('MANAGER', 'ANALYST');

````

```ad-caution
title: 주의사항

- `LAST_VALUE` 함수에서 `ORDER BY`를 사용한 경우 WINDOWING에 다음 조건을 추가해야 한다.

~~~SQL
UNBOUNDED PRECEDING -- PARTITION의 첫번째 행에서 윈도우가 시작한다.
UNBOUNDED FOLLOWING -- PARTITION의 마지막 행에서 윈도우가 시작한다.
~~~

- NULL 값을 표시하지 않을 경우
~~~SQL
SELECT ENAME, JOB, COMM, FIRST_VALUE(COMM IGNORE NULLS) OVER() AS COMM_FIRST
FROM EMP
WHERE JOB IN ('MANAGER', 'SALESMAN')
~~~

```
