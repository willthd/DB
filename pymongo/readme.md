# PyMongo

> python에서 MongoDB와 연동할 때 사용한다.

</br>

### MongoDB 접속

```python
from pymongo import MongoClient

# - MongoDB 접속 정보
# url로 입력하면 안되는데, 이유 정확히 모르겠음
# mongodb://username:password@uri
# username: username
# password: password
# uri: 12.234.234.123:port/something
client = MongoClient('mongodb://username:password@12.234.234.123:port/something')

# db 선택
db = client['chatbot']

# collection 선택
collection = mongo_db['chatbot_log']

# collection 이름 확인
db.collection_names()

# collection 내 데이터 확인
for post in collection.find():
  print(post)
```

