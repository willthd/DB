# MySQL

> MySQL DB에서 사용하는 SQL 구문

</br>

### Describe

```mysql
desc sample21;
```

</br>

### 검색

```mysql
-- 모든 열
select * from sample21;

-- 특정 열 a,b
select a, b from sample21;

-- 특정 열 a,b와 행 no가 2 또는 5인 행(and, or, not 모두 가능. 괄호 주의해서 사용)
select a, b from sample21 where no=2 or no=5;

-- 특정 열 a,b와 no가 2가 아닌 행
select a, b from sample21 where no<>2;

-- null검색
select * from sample21 where birthday is null;
select * from sample21 where birthday is not null;

-- like(pandas에서 isin과 비슷한 개념), 찾고자 하는 문자 앞뒤로 %붙임
select * from sample25 where text like '%SQL%';
-- 이스케이프 방법은 문자 앞에 \붙임. ex) text에서 %들어간 행
select * from sample25 where text like '%\%%';
-- 문자열 상수 '의 이스케이프
select * from sample25 where text like 'It''s';
```

</br>

### 정렬과 연산

```mysql
-- age열의 값을 내림차순으로 정렬, default는 오름차순(asc)
select * from sample31 order by age desc;

-- 복수열 정렬
select * from sample32 order by a asc, b desc;

-- MySQL에서 null은 가장 작은 값으로 취급

-- 행수 제한(pandas.display.options.max_rows == 5와 동일)
select * from sample33 order by a asc limit 3;

-- offset 지정(3이후 부터 3개)
select * from sample33 limit 3 offset 3;

-- 열 별칭 붙이기. 여기선 amount, 한글로 붙일 때는 ''안에 넣음
select *, price * quantity as amount from sample34;

-- 아래 구문은 불가. where구 -> select구 순서로 처리되기 때문에 select구에서 지정한 별칭은 where구에서 사용 불가
select *, price * quantity as amount from sample34 where amount >= 2000;
-- 아래는 가능
select *, price * quantity as amount from sample34 where price * quantity >= 2000;

-- order by 구에서는 select구에서 지정한 별칭 사용 가능. where -> select -> order by
select *, price * quantity as amount from sample 34 order by amount desc;

-- null값의 연산은 모두 null. 따로 에러 나지 않음
select *, price * null as amount from sample34;

-- 함수도 연산자와 동일하게 활용
-- round, 소수점 1째 자리까지 표현. 즉 둘째 자리에서 반올림. -1은 1의 자리에서 반올림
select amount, round(amount, 1) from sample341;

-- 문자열 연산(concat)
select concat(quantity, unit) from sample35;

-- 문자열 연산(substring), 1째 자리부터 4개 추출. '2014' 반환
select substring('20140125001', 1, 4);

-- 문자열 연산(trim). 문자열 앞뒤로 여분의 스페이스 제거해 반환. 'abc'
select trim('  abc   ');

-- 문자열 연산(char_length). 5 
select char_length('asvsd');

-- 날짜 연산(시스템 날짜)
select current_timestamp;

-- 날짜 연산(문자열 데이터 날짜형 데이터로 변환). 형식 동일해야 반환됨. 다를 경우 null 반환
select str_to_date('20140105', '%Y%m%d');
select str_to_date('2014-01-05', '%Y-%m-%d');

-- 날짜 뺄셈
select datediff('2014-02-28', '2014-01-01');

-- case 조건문. else를 생략하면 조건문에 해당하지 않는 것들은 모두 null이 되기 때문에 지정하는 편이 좋음. end 가장 마지막에 있는 것 주의
select a as code, case when a=1 then 'man' when a=2 then 'woman' else 'unknown' end as 'sex' from sample37;

-- coalesce. a가 null인 경우 0으로 대체
select coalesce(a, 0) as 'new_col' from sample37;
```

</br>

### 데이터의 추가, 삭제, 갱신

```mysql
-- 행 추가하기
-- 추가하기 전에 desc을 통해 각 컬럼의 타입을 확인해야 함
insert into sample41 values(1, 'ABC', '2014-01-25');
-- 열을 지정해 행 추가하기. 별도 값 지정하지 않은 열에는 null이 저장(not null 제약 있는 경우에는 null 저장 불가). 지정하지 않은 열에는 desc로 확인했을 때 default값이 들어감
insert into sample41(a, no) values('XYZ', 2);

-- 삭제하기
-- where구에 지정하지 않으면 모든 행 삭제
delete from sample41;
delete from sample41 where no=3;

-- 갱신하기
-- where구에 지정하지 않으면 테이블의 모든 행 갱신
update sample41 set a=1
-- 복수열에 대해 갱신. 이 때 순서는 적혀 있는 순서대로 갱신
update sample41 set a='KKK', b='2014-04-05' where no=2;
-- 갱신할 값을 열이 포함된 식으로도 표기할 수 있으며, 해당 열이 갱신 대상이 되는 열이라 해도 상관 없음
update sample41 set no=no+1;

```

