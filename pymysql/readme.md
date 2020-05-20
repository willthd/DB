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



