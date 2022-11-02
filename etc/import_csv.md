# mysql csv import

## 구조가 같은 경우
````
CREATE TABLE test (
    id INT NOT NULL AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    created_at DATE NOT NULL,
    PRIMARY KEY (id)
);
````
````
/* csv file format */
id,name,created_at
1,"test1",2022-01-01
2,"test2",2022-01-01
````
````
LOAD DATA INFILE '/test.csv' 
INTO TABLE emp
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
````

## 구조가 다른 경우
````
/* csv file format */
id,name,created_at,updated_at
1,"test1",2022/01/01
2,"test2",2022/01/01
````
````
LOAD DATA INFILE '/test.csv'
INTO TABLE emp
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(@id,@name,@created_at,@updated_at)
SET id = @id,
    name = @name,
    created_at = STR_TO_DATE(@created_at, '%m/%d/%Y');
````
