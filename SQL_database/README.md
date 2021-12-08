# SQL database

## 1. Tạo database

**Syntax:**

```SQL
    CREATE DATABASE ten_database;
```

**Ví dụ:**

```SQL
    CREATE DATABASE testdb;
```

=> Tạo ra database tên là `testdb`.

Sau khi tạo xong có thể dùng lệnh:

```SQL
    SHOW DATABASES;
```

để kiểm tra database đã tạo

## 2. Drop database

Để xóa database ta có

**Syntax:**

```SQL
    DROP DATABASE ten_database;
```

## 3. Backup database

Dùng để backup database

**Syntax:**

```SQL
    BACKUP DATABASE databasename
    TO DISK = 'filepath';
```

Dùng `DIFFERENTIAL` statement để backup các phần mới được thay đổi mà chưa được backup. Giống như commit, push request của github.

**Syntax:**

```SQL
    BACKUP DATABASE databasename
    TO DISK = 'filepath'
    WITH DIFFERENTIAL;
```

**Ví dụ:**

```SQL
    BACKUP DATABASE testDB
    TO DISK = 'D:\backups\testDB.bak';
```

back up ra file `testDB.bak` ở ổ D.

```SQL
    BACKUP DATABASE testDB
    TO DISK = 'D:\backups\testDB.bak'
    WITH DIFFERENTIAL;
```

Nên kiểm tra kĩ và backup thường xuyên để database luôn ở dạng tối ưu mong muốn.

## 4.Create table

**Syntax:**

```SQL
    CREATE TABLE table_name (
        column1 datatype,
        column2 datatype,
        column3 datatype,
        ....
    );
```

**Ví dụ:**

```SQL
    CREATE TABLE Persons (
        PersonID int,
        LastName varchar(255),
        FirstName varchar(255),
        Address varchar(255),
        City varchar(255)
    );
```

cột `PersonID` có kiểu dữ liệu là số nguyên. Các cột còn lại có kiểu dữ liệu là chuỗi kí tự với độ dài tối đa là 255 kí tự.

Câu lệnh trên sẽ tạo ra bảng trống gồm các trường như vậy.

**Ví dụ: Tạo bảng mới từ bảng có sẵn:**

**Syntax:**

```SQL
    CREATE TABLE new_table_name AS
        SELECT column1, column2,...
        FROM existing_table_name
        WHERE ....;
```

**Ví dụ:**

```SQL
    CREATE TABLE TestTable AS
    SELECT customername, contactname
    FROM customers;
```

Câu lệnh trên sẽ tạo bảng mới tên `TestTable` với hai trường `CustomerName` và `ContactName` lấy từ bảng `Customers` có sẵn.

## 5.DROP table

Dùng `DROP TABLE` để xóa bảng.

**Syntax:**

```SQL
    DROP TABLE ten_bang;
```

**Ví dụ:**

```SQL
    DROP TABLE testtable;
```

câu lệnh trên sẽ xóa bảng `testtable`.

Ta có thể dùng lệnh `TRUNCATE TABLE` để xóa hết dữ liệu trong bảng mà không xóa bảng.

**Syntax:**

```SQL
    TRUNCATE TABLE ten_bang;
```

So sanh câu lệnh này với

```SQL
    DELETE FROM ten_bang;
```

Xem chi tiết tại: [https://www.sqltutorial.org/sql-truncate-table/](https://www.sqltutorial.org/sql-truncate-table/)

## 6.ALTER TABLE

`ALTER TABLE` Dùng để thêm, sửa, xóa cột trong bảng.

**Syntax:**

```SQL
    ALTER TABLE table_name
    ADD column_name datatype;
```

**Ví dụ:**

```SQL
    ALTER TABLE Customers
    ADD Email varchar(255);
```

Ví dụ trên đã thêm một cột `Email` có kiểu dữ liệu `varchar` tối đa 255 kí tự vào bảng `Customers`.

Để xóa cột:

```SQL
    ALTER TABLE table_name
    DROP COLUMN column_name;
```

**Ví dụ:**

```SQL
    ALTER TABLE Customers
    DROP COLUMN Email;
```

Thay đổi kiểu dữ liệu của cột:

Đối với MySQL

```SQL
    ALTER TABLE table_name
    MODIFY COLUMN column_name datatype;
```

## 7. Constraints

Dùng để định nghĩa rules cho kiểu dữ liệu.

`constraint` có thể được định nghĩa ngay khi tạo bảng trong `CREATE TABLE` hoặc sau khi tạo bảng bằng cách dùng `ALTER`

**Syntax:**

```SQL
CREATE TABLE table_name (
    column1 datatype constraint,
    column2 datatype constraint,
    column3 datatype constraint,
    ....
);
```

Khi đã đặt luật cho kiểu dữ liệu thì dữ liệu trong bảng hoặc trong cột phải tuân theo luật đó. Nếu không theo luật thì sẽ gặp lỗi.

Ví dụ muốn đặt luật cho một trường nào đó như trường `ID` các giá trị trong trường phải là duy nhất, không được trùng nhau.

Ta có thể đặt luật theo kiểu column constraints hoặc table constrains. Một số `constraints` được dùng:

- `NOT NULL` - đảm bảo cột không có giá trị nào là NULL
- `UNIQUE` - đẩm bảo các giá trị trong cột là duy nhất, không được trùng nhau.
- `PRIMARY KEY` - Kết hợp của `NOT NULL` và `UNIQUE`. đảm bảo dữ liệu phải xác định và duy nhất.
- `FOREIGN KEY` - ngăn chặn hành động có thể hủy liên kết giữa các bảng.
- `CHECK` - đảm bảo giá trị trong cột thỏa mãn điều kiện nào đó
- `DEFAULT` - Đặt một giá trị mặc định nếu như không có giá trị nào được set.
- `CREATE INDEX` - Dùng để tạo và lấy dữ liệu từ bảng một cách nhanh chóng.

Tiếp theo ta sẽ tìm hiểu kĩ hơn về từng loại `constraints` này

## 8. NOT NULL

`NOT NULL` để đảm bảo dữ liệu phải xác định, không được trống. Do đó ta không thể thêm 1 record mới, chỉnh sửa mà không gán giá trị.

**Ví dụ:**

```SQL
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255) NOT NULL,
    Age int
);
```

Câu lệnh trên đã đặt cho các trường `ID`, `LastName` và `FirstName` bắt buộc phải xác định, không được có giá trị `NUL`. Đây là cách thêm `constraint` vào khi tạo bảng.

Thêm `constraint` vào dữ liệu sau khi tạo bảng bằng `ALTER`:

```SQL
    ALTER TABLE Persons
    MODIFY Age int NOT NULL;
```

Các kiểu trên được gọi là column constraints.

## 9. UNIQUE

**Ví dụ:**

Với MySQL

```SQL
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    UNIQUE (ID)
);
```

Dùng `UNIQUE` với nhiều cột:

```SQL
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CONSTRAINT UC_Person UNIQUE (ID,LastName)
);
```

Câu lệnh trên đặt tên cho `UNIQUE` constraint và định nghĩa `UNIQUE` cho 2 cột `ID` và `LastName`.

Kiểu đặt `UNIQUE` như trên được gọi là table constrains.

Để thêm `UNIQUE` sau khi bảng được tạo:

**Ví dụ:**

Đối với MySQL

```SQL
    ALTER TABLE Persons
    ADD UNIQUE (ID);
```

Hoặc

```SQL
    ALTER TABLE Persons
    ADD CONSTRAINT UC_Person UNIQUE (ID,LastName);
```

Để loại bỏ `UNIQUE`

Đối với MySQL:

```SQL
    ALTER TABLE Persons
    DROP INDEX UC_Person;
```

## 10. Primary Key

`PRIMARY KEY` là kết hợp của `NOT NULL` và `UNIQUE`

`PRIMARY KEY` khác với `UNIQUE` ở các điểm sau:

  # | PRIMARY KEY constraint | UNIQUE constraint
---|------------------------|------------------
Số constraint cho phép | Chỉ 1 |Nhiều
NULL values | Không cho phép | cho phép

Khi thực hiện một bảng. Trong bảng có thể có 1 hoặc 1 nhóm các cột mang tính xác định duy nhất cho mỗi record trong bảng, các giá trị trong cột này sẽ phải xác định và duy nhất. Ta gọi đó là `PRIMARY KEY` của bảng.

Một bảng chỉ có thể có 1 `PRIMARY KEY` được tạo bởi 1 hoặc nhiều cột.

**Ví dụ:**

Dưới đây là ví dụ bảng với `PRIMARY KEY` là `course_id` vì ứng với mỗi giá trị `course_id` xác định và duy nhất thì ta xác định được một record trong bảng.

![primary_key_1](/img/primary_key_1.png)

Ví dụ với `PRIMARY KEY` là nhóm các cột:

![primary_key_2](/img/primary_key_2.png)

Ta thấy nhóm 2 cột `employee_id` và `course_id` là `PRIMARY KEY` của bảng. Trong đó các giá trị tại cột `course_id` có thể lặp lại nhưng tổng quan cả nhóm thì vẫn phải mang tính chất xác định và duy nhất.

Ta có thể tạo `PRIMARY KEY` khi tạo bảng hoặc thêm vào bảng bằng `ALTER` đối với bảng có sẵn.

**Ví dụ:**

Tạo `PRIMARY KEY` khi tạo bảng:

Với MySQL

```SQL
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    PRIMARY KEY (ID)
);
```

`PRIMARY KEY` cho nhóm các cột:

```SQL
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CONSTRAINT PK_Person PRIMARY KEY (ID,LastName)
);
```

Ở trên là ví dụ `PRIMARY KEY` với nhóm cột gồm `ID` và `LastName` được thêm bởi table constraint với tên là `PK_Person`.

Với cách thêm `PRIMARY KEY` bằng `ALTER` cho table có sẵn. Ta có ví dụ:

```SQL
    ALTER TABLE Persons
    ADD PRIMARY KEY (ID);
```

```SQL
    ALTER TABLE Persons
    ADD CONSTRAINT PK_Person PRIMARY KEY (ID,LastName);
```

Chú ý nếu thêm `PRIMARY KEY` bằng `ALTER` thì khi tạo bảng ta phải đặt `UNIQUE` cho cột muốn đặt `PRIMARY KEY`.

Để xóa `PRIMARY KEY`:

Đối với MySQL

```SQL  
    ALTER TABLE Persons
    DROP PRIMARY KEY;
```

## 11.Foreign key

`Foreign key` là một hoặc một nhóm các cột đảm bảo sự liên kết giữa hai bảng.

Bảng có `foreign key` được gọi là bảng con. Bảng với `primary key` liên kết với bảng con gọi là bảng cha/mẹ.

Ví dụ với 2 bảng sau:

![foreign_key_1](/img/foreign_key.png)

![foreign_key_2](/img/foreign_key_2.png)

Ta thấy, hai bảng có mối liên kết với nhau tại trường `PersonID`. Từ hai bảng ta có thể rút ra là `PersonID` là `primary key` đối với bảng `Persons` và là `foreign key` đối với bảng `Orders`.

`Foreign key` đảm bảo rằng sẽ không có trường hợp dữ liệu nào được add vào `foreign key` column trong bảng con mà không tồn tại trong `primary key` bảng cha.

**Ví dụ khi thêm foreign key khi tạo bảng với MySQL:**

```SQL
CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);
```

Để tạo `FOREIGN KEY` với nhóm các cột và đặt tên cho nó, ta có ví dụ:

```SQL
CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID)
    REFERENCES Persons(PersonID)
);
```

Đối với cách thêm `FOREIGN KEY` bằng `ALTER` ta có ví dụ sau:

Đối với MySQL

```SQL
    ALTER TABLE Orders
    ADD FOREIGN KEY (PersonID) REFERENCES Persons(PersonID);
```

Thêm tên cho `FOREIGN KEY` cho cột hoặc nhiều cột:

```SQL
    ALTER TABLE Orders
    ADD CONSTRAINT FK_PersonOrder
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID);
```

Để xóa `FOREIGN KEY`:

```SQL
    ALTER TABLE Orders
    DROP FOREIGN KEY FK_PersonOrder;
```

## 12. Check

Ta dùng `CHECK` để kiểm tra điều kiện của dữ liệu đầu vào. Chỉ những dữ liệu thỏa mãn điều kiện mới được đưa vào bảng.

**Ví dụ với MySQL:**

```SQL
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CHECK (Age>=18)
);
```

Câu lệnh trên kiểm tra dữ liệu người dùng. Chỉ có những người có số tuổi >= 18 mới được thêm vào bảng.

Để đặt tên cho `CHECK constraints` và đặt `CHECK` cho nhiều cột khác nhau ta có ví dụ:

```SQL
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    City varchar(255),
    CONSTRAINT CHK_Person CHECK (Age>=18 AND City='Sandnes')
);
```

Câu lệnh trên đã đặt luật cho các record với các dữ liệu phải có `Age` >=18 và có `City` là Sandnes mới được thêm vào bảng.

Để thêm `CHECK` vào bảng có sẵn. Ta có ví dụ:

```SQL
    ALTER TABLE Persons
    ADD CHECK (Age>=18);
```

```SQL
    ALTER TABLE Persons
    ADD CONSTRAINT CHK_PersonAge CHECK (Age>=18 AND City='Sandnes');
```

Để loại bỏ `CHECK`:

```SQL
    ALTER TABLE Persons
    DROP CHECK CHK_PersonAge;
```

## 13. Default

Dùng để set value mặc định cho trường khi không có giá trị nào được set.

**Ví dụ với MySQL:**

```SQL
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    City varchar(255) DEFAULT 'Sandnes'
);
```

Với ví dụ trên, nếu không có giá trị nào được set cho trường `City` của data thì nó auto = `Sadnes`.

Thêm bẳng `ALTER`

```SQL
    ALTER TABLE Persons
    ALTER City SET DEFAULT 'Sandnes';
```

Để xóa:

```SQL
    ALTER TABLE Persons
    ALTER City DROP DEFAULT;
```

**Lưu ý: Tất cả các ví dụ trên đều được thực hiện với cú pháp của MySQL.**

## 13. INDEX

Sử dụng `CREATE INDEX` để cho việc lấy dữ liệu nhanh chóng hơn. Người dùng không thể thấy index này. Tuy nhiên việc có index sẽ làm update table lâu hơn do phải update cả index.

**Syntax:**

```SQL
CREATE INDEX index_name
ON table_name (column1, column2, ...);
```

Index có thể có các giá trị lặp nhau, nếu muốn các index là giá trị duy nhất. Ta có cú pháp:

```SQL
CREATE UNIQUE INDEX index_name
ON table_name (column1, column2, ...);
```

Để xóa index:

Đối với MySQL

```SQL
ALTER TABLE table_name
DROP INDEX index_name;
```

## 14. Auto Increment

Thường được dùng cho `primary key` để được tự động thêm vào khi insert record mới và tự động thay đổi.

**Ví dụ với MySQL:**

```SQL
CREATE TABLE Persons (
    Personid int NOT NULL AUTO_INCREMENT,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    PRIMARY KEY (Personid)
);
```

Ví dụ trên sẽ tạo cho trường `Personid` được generate tự động và tự động tăng lên khi insert record mới.

Mặc định `AUTO_INCREMENT` sẽ bắt đầu từ giá trị 1, tăng thêm 1 với mỗi record mới. Để `AUTO_INCREMENT` bắt đầu với giá trị khác thì ta có ví dụ:

```SQL
ALTER TABLE Persons AUTO_INCREMENT=100;
```

Như vậy khi insert record mới thì ta không cần quan tâm đến `Personid` nữa vì nó sẽ tự động được tạo.

## 15. Làm việc với kiểu dữ liệu DATE

Đối với MySQL, dữ liệu `DATE` có các format:

- `DATE` - format YYYY-MM-DD
- `DATETIME` - format: YYYY-MM-DD HH:MI:SS
- `TIMESTAMP` - format: YYYY-MM-DD HH:MI:SS
- `YEAR` - format YYYY or YY

**Ví dụ:**

![date_1](/img/date_1.png)

Với câu lệnh

```SQL
SELECT * FROM Orders WHERE OrderDate='2008-11-11'
```

Kết quả:

![date_2](/img/date_2.png)

Ta có thể dễ dàng thực hiện các query với date format không có time portion. Ta nên để date format mà không có time portion trừ khi ta bắt buộc phải có.