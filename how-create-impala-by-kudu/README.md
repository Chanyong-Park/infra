# 설치
```
$ git clone https://github.com/apache/kudu.git
$ cd kudu/examples/quickstart/impala
$ docker-compose up -d
```

# 임팔라 쉘 접속
```
$ docker exec -it kudu-impala impala-shell
```

# 쉘 안에서 실행할 SQL
```
CREATE TABLE test_node (
  id INT PRIMARY KEY,
  name STRING,
  created_at TIMESTAMP
) STORED AS KUDU;

INSERT INTO test_node VALUES (1, 'Test Data 01', now());
SELECT * FROM test_node;
```
