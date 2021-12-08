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

column constraint

```SQL
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL UNIQUE,
    FirstName varchar(255),
    Age int
);
```

table constraint

```SQL
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    UNIQUE (ID)
);
```

Ở trên là hai cách đặt constraint, trực tiếp vào column hoặc dùng table constraint.

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

Kiểu đặt `UNIQUE` như trên được gọi là column constrains.

Để thêm `UNIQUE` sau khi bảng được tạo:

**Ví dụ:**

```SQL
    ALTER TABLE Persons
    ADD ID int UNIQUE;
```

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

```SQL
    ALTER TABLE Persons
    DROP INDEX UC_Person;
```