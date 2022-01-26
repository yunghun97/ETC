- [프로시저 생성]
- [AUTO_INCREMENT 초기화](#DB-AUTO_INCREMENT-값-1부터-N까지-변경) 


## 프로시저 생성 및 실행
```mysql
DELIMITER $$
create procedure [`프로시저 이름`](
	  IN PARAM_ID  VARCHAR(20),
    IN PARAM_PASSWORD VARCHAR(30),
    IN PARAM_EMAIL VARCHAR(30),
    IN PARAM_PHONE VARCHAR(20),
    IN PARAM_AUTH VARCHAR(3)
    OUT total INT
)
BEGIN 
	INSERT INTO User(id,password,email,phone,Auth)
    VALUES(PARAM_ID, PARAM_PASSWORD, PARAM_EMAIL, PARAM_PHONE, PARAM_AUTH);
  SELECT count(id)
  INTO total # 매개변수 total에 count값이 복사된다.
  from User
END
```

## DB AUTO_INCREMENT 값 1부터 N까지 변경 
<br>

```mysql
ALTER TABLE [테이블명] AUTO_INCREMENT=1;
SET @COUNT = 0;
UPDATE [테이블명] SET [AUTO_INCREMENT 적용된 컬럼] = @COUNT:=@COUNT+1;

<!--
사용 예시
ALTER TABLE class_board AUTO_INCREMENT=1;
SET @COUNT = 0;
UPDATE class_board SET id = @COUNT:=@COUNT+1;
-->
```
