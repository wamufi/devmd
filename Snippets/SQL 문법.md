---
title: SQL
date: 2024-07-18
time: 02:10:53
tags:
  - database
  - sql
---
### GROUP BY
**특정 열(column)을 기준으로 그룹을 만들어 그룹별로 데이터를 집계하거나 /분석하는 기능 제공**
`SELECT column1, aggregate_function(column2) FROM test_table GROUP BY column1`
column1에서 그룹화한 데이터를 SELECT에서 어떻게 보여줄지 지정

#### 집계 함수 (aggregate function)
**그룹화된 데이터를 집계하는 함수**
전체 집계: GROUP BY 없이 사용할 경우, 전체 데이터 세트에 대한 집계 수행
그룹별 집계: GROUP BY와 함께 사용하면, 지정된 열(column)에 따라 그룹을 형성하고 각 그룹에 대한 집계 수행. 일반적으로 사용됨.

##### SUM()
**그룹 내 값들의 합 계산**
##### COUNT()
**그룹 내 행의 계수 계산**
##### AVG()
**그룹 내 값들의 평균 계산**
##### MIN()
**그룹 내 값들 중 최소값 검색**
##### MAX()
**그룹 내 값들 중 최대값 검색**

### DISTINCT
**쿼리 결과에서 중복된 값을 제거하여 유일한(unique) 값들만 반환**
`SELECT DISTINCT column1, column2 FROM test_table;`
중복된 값이 제거되고 각 값은 한 번만 표시
데이터를 탐색하고 분석할 때 유용하게 사용

### HAVING
**그룹화 된 결과에 조건을 적용하여 그룹을 필터링**
`SELECT column1, aggregate_function(column2) FROM test_table GROUP BY column1 HAVING condition;`
일반적으로 집계 함수와 함께 사용되며, 집계된 값을 기준으로 필터링할 때 유용. 
특정 그룹의 개수가 특정 값 이상인 경우를 찾거나, 평균 값이 특정 범위에 있는 그룹을 선택하는 등의 작업에 사용
> `SELECT department, COUNT(*) AS num_employees FROM employees GROUP BY department HAVING COUNT(*) >= 3;`
> employee 테이블에서 부서별로 직원의 수가 3명 이상인 부서를 찾는 쿼리

### ORDER BY
**그룹화 한 결과를 특정 기준에 따라 정렬**
`SELECT column1, column2, ... FROM test_table ORDER BY column1 \[ASC | DESC], column2 \[ASC | DESC], ...`
결과를 의미 있게 정렬하여 데이터를 분석하고 해석하는 데 유용
ASC: 오름차순 1 -> 100 (기본값)
DESC: 내림차순 100 -> 1
> `SELECT employee_id, last_name, first_name FROM employees ORDER BY last_name;`
> 성을 오름차순으로 정렬하여 직원 목록 검색하는 쿼리

### WHERE
**데이터를 필터링하는 데 사용되는 절**
`SELECT column1, column2, ... FROM test_table WHERE condition;`
특정 조건을 만족하는 행만을 검색하거나 수정할 때 사용. 조건을 지정하고, 만족하는 행만 결과로 반환할 수 있음.
*집계 함수의 결과를 기반으로 한 조건은 WHERE에서 에러.*
> `SELECT employee_id, last_name, first_name, age FROM employees WHERE age >= 30;`
> age가 30 이상인 직원들의 정보 검색

#### LIKE
**특정 패턴을 포함하는지 확인**
`SELECT column1, column2, ... FROM test_table WHERE column1 LIKE 'pattern';`
대소문자를 구분하지 않음. 패턴에 맞는 문자열이 없을 경우, 빈 결과 집합 반환
> `SELECT employee_id, last_name, first_name FROM employees WHERE first_name LIKE 'J%';`
> 이름이 'J'로 시작하는 직원들의 정보 검색

##### 와일드카드
* %: 임의의 문자열을 나타내는 와일드카드. '%A%'는 'A'가 포함되는 모든 문자열
* \_: 임의의 단일 문자를 나타내는 와일드카드. 'D__'는 'D'로 시작하고 3글자인 모든 문자열