---
title: "MySQL Database 생성 및 권한 부여"
output:
  html_document:
    keep_md: true
date: '2022-06-19'
---

## 개요

- MySQL 관리자 계정인 root로 DB 관리시스템에 접속 후 DB를 생성한다.

## 사전준비

- MySQL 설치 및 환경변수를 설정한다.
    - 참조 : [https://dschloe.github.io/settings/mysql_installation_windows11/](https://dschloe.github.io/settings/mysql_installation_windows11/)

## DB 생성

- 콘솔창에서 MySQL 명령을 실행한다.

```sql
C:\Users\j2hoo>mysql -uroot -p
Enter password: ****
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 19
Server version: 8.0.28 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

- DB 생성 명령을 실행한다. DB명은 `homestead` 로 지정했다.

```sql
mysql> CREATE DATABASE homestead;
Query OK, 1 row affected (0.01 sec)
```

## DB사용자 생성

- 해당 DB에 접근할 수 있는 계정을 생성한다.
    - {username}과 {password}에 각 개인이 원하는대로 지정한다.

```sql
mysql> CREATE USER '{username}'@'localhost' IDENTIFIED BY '{password}';
mysql> CREATE USER '{username}'@'%' IDENTIFIED BY '{password}';
```

- 필자는 아래와 같이 했다.

```sql
mysql> CREATE USER 'homestead'@'localhost' IDENTIFIED BY 'secret';
Query OK, 0 rows affected (0.02 sec)
```

- 생성한 계정에 권한을 부여한다.
- 첫번째 명령어는 해당 DB에 모든 권한을 부여한다는 뜻이다.
- 두번째 명령어는 DBMS에 적용하라는 의미를 말하며, 반드시 실행해야 한다.

```sql
mysql> GRANT ALL PRIVILEGES ON homestead.* TO 'homestead'@'localhost';
Query OK, 0 rows affected (0.00 sec)
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)
```

## 접속

- 이제 homestead DB에 접속한다.
- 기존 cmd 창은 root 계정이기 때문에 exit를 통해 선 종료 한다.

```sql
mysql> exit 
Bye
```

- 직접 해당 DB에 접근하는 것은 다음과 같다.

```sql
mysql -h127.0.0.1 -u{username} -p {database} # 예시
mysql -h127.0.0.1 -uhomestead -p homestead # 적용
```

- 실제로 적용하면 아래와 같이 실행될 것이다.

```sql
C:\Users\j2hoo>mysql -h127.0.0.1 -uhomestead -p homestead
Enter password: ******
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 21
Server version: 8.0.28 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

- 현재 DB의 종류를 조회해본다.

```sql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| homestead          |
| information_schema |
+--------------------+
2 rows in set (0.00 sec)
```

## MySQL Workbench 사용

- 이제 DB를 사용했으니, Workbench를 통해 접속해본다.
- 윈도우 돋보기에서 MySQL을 조회 후, Workbench 8.0 CE를 실행한다.

/images/my_sql
![Untitled](/images/my_sql/Untitled.png)

- 메뉴바에서 Database - Connect to Database를 선택한다. (Ctrl + U)

![Untitled](/images/my_sql/Untitled%201.png)

- DB 생성 시 사용했던 Username과 Password를 추가적으로 입력한다.

![Untitled](/images/my_sql/Untitled%202.png)

- `show databases;` 명령어를 입력해 2개의 Database 이름이 존재하는지 확인한다.

![Untitled](/images/my_sql/Untitled%203.png)