# PyMySQL

> python에서 MySQL, MariaDB와 연동할 때 사용한다.

</br>

### MariaDB 접속

```python
import pymysql

# MariaDB 접속 정보
host = "uri" # 15.165.156.132
port = 4406
username  = "username"
password = "password"
maria_db = "dbname"

conn = pymysql.connect(host=host, port=port, user=username, password=password, db=maria_db, charset='utf8')

"""
pymysql은 python에서 mysql 연동을 위한 라이브러리 입니다.
해당 코드는 pymysql을 이용하여 CRUD하는 간단한 예시입니다.
"""
import pymysql

# MySQL Connection 연결
conn = pymysql.connect(host='localhost', user='root', password='', db='test', charset='utf8')

curs = conn.cursor()

# ==== select example ====
sql = "select * from webtoonCommends"
curs.execute(sql)

# 데이타 Fetch
rows = curs.fetchall()
print(rows)

# ==== insert ====
sql = "insert into customer(name,category,region) values (%s, %s, %s)"
curs.execute(sql, ('홍길동', 1, '서울'))
curs.execute(sql, ('이연수', 2, '서울'))
# commit을 통해 DB 상태 변경 후 저장
conn.commit()

# ==== update =====
sql = "update customer set region = '서울특별시' where region = '서울'"
curs.execute(sql)
conn.commit()

# ==== delete =====
sql = "delete from customer where id=%s"
# 다 지울 때
sql = "delete from customer"
curs.execute(sql)
conn.commit()

# ==== select =====
sql = "select * from customer"
curs.execute(sql)
rows = curs.fetchall()
for row in rows:
  print(row)
```

</br>

### try와 with문의 사용

SQL Connection을 열고 프로그램 중간에서 에러가 발생하면, Connection은 그대로 열려 있는 상태로 있을 수 있다. 이렇게 오픈되어 있는 Connection이 증가하면, 나중에 새로운 Connection을 오픈할 수 없게 되는데, 이를 Connection Leak 이라 부른다. 이러한 Connection Leak을 막기 위하여 아래 예제와 같이 try...finally 블력을 사용하여 finally에서 항상 Conneciton을 Close해 주는 것이 좋다.

```python
import pymysql
 
conn = pymysql.connect(host='localhost', user='tester', password='7890',
                       db='testdb', charset='utf8')
 
try:
    # INSERT
    with conn.cursor() as curs:
        sql = "insert into customer(name,category,region) values (%s, %s, %s)"
        curs.execute(sql, ('이광수', 1, '서울'))
 
    conn.commit()
 
    # SELECT
    with conn.cursor() as curs:
        sql = "select * FROM customer"
        curs.execute(sql)
        rows = curs.fetchall()
        for row in rows:
            print(row)

except Exception as e:
  # 에러가 발생하면 쿼리를 롤백한다.
  conn.rollback()
  raise e
 
finally:
  # 최종적으로 connection을 닫는다.
  conn.close()
```

위 예제의 try 블럭을 보면, INSERT와 SELECT 문을 각기 다른 커서에서 사용하고 있다. 첫번째 INSERT 실행 시 with 문으로 커서를 만들어 자동으로 커서 리소스가 해제되도록 하였고, 두번째 SELECT 시에도 with 문으로 해당 커서가 자동 해제되도록 하였다. 이러한 예제에서 보듯이, SQL 객체들을 다룰 때 try...finally 나 with 문을 적절히 사용하여 리소스를 해제해 주는 것이 좋다.

