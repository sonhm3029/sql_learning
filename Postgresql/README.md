# POSTGRESQL NOTE

# Data types:

- Text: Kiểu chữ
- Numeric: Kiếu số
- Temporal: Kiểu thời gian
- Boolean: True vs False


## I. Kiểu số:

Kiểu | Mô tả
-----|------
SMALLINT| sô nguyên có dấu (8 bit) -32,768 --> 32.767
INTEGER| Số nguyên có dấu (32 bit) -2,147,483,648 --> 2,147,483,647
SERIAL| Kiểu số nguyên được `PostgreSql` tự động tạo và điền theo giá trị tăng dần vào cột `SERIAL`. Tăng không giới hạn. (tương đương với `AUTO_INCREMENT` trong MySql)
FLOAT(n)|Sô thực dấu phẩy động có độ chính xác ít nhất là n. Lưu trữ tối đa bằng 8 byte(32 bit)
REAL/FLOAT8| số thực dấu phẩy động lưu trữ bằng 16 bit.
NUMERIC hay NUMERIC(p,s)| là số thực có `p` chữ số với số `s` sau dấu thập phân. Tương đương với kiểu `DECIMAL`


## II. Kiểu chữ:

Kiểu | Mô tả
-----|------
TEXT| Dùng cho kiểu chuỗi không biết giới hạn độ dài
VARCHAR(N)| Kiểu chuỗi dùng cho cả kiểu chuỗi giới hạn kích thước khi thêm `VARCHAR(N)` với `N` là kích thước. Và chuỗi không giới hạn kích thước `VARCHAR`, nếu khai báo thế này thì tương ứng với kiểu `TEXT`. Với kiểu này, ta có thể lưu dữ liệu chuỗi có độ dài ngắn hơn hoặc bằng giới hạn kích thước `N`. Nếu kích thước chuỗi vượt quá `N` thì sẽ báo lỗi.
CHAR(N)| Kiểu dữ liệu dùng cho chuỗi giới hạn độ dài. Nếu chỉ khai báo `CHAR` thì dữ liệu nhận được mặc định có độ dài là 1. Nếu như chuỗi khai báo có kích thước nhỏ hơn `N` thì dữ liệu nhận được sẽ tự động được thêm các khoảng trắng đằng sau sao cho dữ liệu có kích thước là `N`. Nếu kích thước vượt quá `N` thì sẽ báo lỗi.

## III. Kiểu BOOLEAN

`BOOL` hoặc `BOOLEAN` có giá trị `true` hoặc `false`

## IV. Kiểu Temporal

Kiểu | Mô tả
-----|------
DATE| Ngày tháng năm
TIME| Giá trị thời gian
TIMESTAMP| Ngày tháng năm + thời gian
TIMESTAMPZ| Dấu thời gian và múi giờ
INTERVAL| Khoảng thời gian

## V. Kiểu ENUM

Giống với các kiểu ENUM trong các ngôn ngữ lập trình:

```PostgreSql
CREATE TYPE week AS ENUM ('Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun');
```

Sau khi khai báo như thế này thì có thể dùng tương tự các loại dữ liệu khác

## VI. ARRAY

```PostgreSql
CREATE TABLE monthly_savings (
   name text,
   saving_per_quarter integer[],
   scheme text[][]
);
```

hoặc 

```PostgreSql
CREATE TABLE monthly_savings (
   name text,
   saving_per_quarter integer ARRAY[4],
   scheme text[][]
);
```

### Thêm vào mảng:

```PostgreSql
INSERT INTO monthly_savings 
VALUES (‘Manisha’, 
‘{20000, 14600, 23500, 13250}’, 
‘{{“FD”, “MF”}, {“FD”, “Property”}}’); 
```

# Database


## I. Tạo database

**Syntax:**

```PostgreSql
CREATE DATABASE dbname;
```

## II. Chọn database để làm việc

Trên psql shell:

```cmd

```

# PSQL COMMAND LINE

## Các tham số cơ bản:

- `--h — --host=HOSTNAME` | database server host or socket directory (default: “local socket”)
- `--p — --port=PORT` | database server port (default: “5432”)
- `--U — --username=USERNAME` | database username (default: “your_username”)
- `--w — --no-password` | never prompt for password
- `--W — --password` | force password prompt (should happen automatically)


## Câu lệnh để kết nối đến database thuộc quyền user nào đó:

```psql
psql -d tenDb -U tenUser
```

Mặc định với lệnh

```cmd
psql -U postgres
```

Sẽ truy cập đến database `postgres` thuộc quyền truy cập của superuser `postgres` (Do hệ thống tạo từ đâu)

=> Kết quả nhận được là 

```cmd
postgres=#
```

Kết quả hiện thị trên command line với `postgres` là tên database đang truy cập và dấu `#` thể hiện user đang sử dụng là superuser.

Nếu không phải superuser sẽ hiển thị như sau

```cmd
postgres=>
```

## Các command cơ bản

Các command của `psql` thường đi với `\`. Ví dụ 

```shell
postgres=# \conninfo
You are connected to database "postgres" as user "your_username" via socket in "/tmp" at port "5432".
```

Một số lệnh khác:

- `\q` thoát khỏi psql 
- `\c tenDB` kết nối với database nào đó
- `\dt` xem tất cả các bảng hiện có trong database
- `\du` xem tất cả các user hiện có
- `\list` hoặc `\l` xem tất cả các database hiện có
- `\d tenTable` hoặc `\d+ tenTable` để xem thông tin chi tiết bảng nào đó.
## Lệnh setup user, db...

### Tạo user mới

```shell
postgres=# CREATE ROLE me WITH LOGIN PASSWORD '123456789';
```

Tạo `user` tên là `me` với password để login là `123456789`

Để user này có thể thực hiện các quyền (tạo table, tạo database, truy vấn....)

```shell
ALTER ROLE me CREATEDB;
```

hoặc có thể thêm các quyền cho user trong `pgAdmin`.

Như vậy khi run `\du` sẽ có kêt quả:

```cmd
me          | Create DB                           | {}
postgres    | Superuser, Create role, Create DB   | {}
```
