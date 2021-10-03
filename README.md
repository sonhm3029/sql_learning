# TỰ HỌC SQL

## I. SQL SELECT

`SELECT` dùng để select data từ database.

Kết quả trả về dưới dạng bảng, gọi là result-set

**Syntax:**

```SQL
    SELECT column1, column2...
    FROM table_name;
```

Trong đó, `column1, column2` là các trường (**field**) được select từ `table_name`. Nếu muốn lấy tất cả các trường sử dụng:

```SQL
    SELECT * FROM table_name;
```

Nếu mong muốn các giá trị trong trường được select ra không có giá trị nào bị lặp lại ta dùng:

```SQL
    SELECT DISTINCT _tentruong FROM _ten_table
```

Và

```SQL
    SELECT COUNT (DISTINCT _tentruong) FROM _ten_table
```

Để xuất ra số giá trị khác nhau trong trường được chọn

**Note: SELECT DISTINCT không dùng được với MS Access database, để dùng ta có ví dụ:**

```SQL
    SELECT Count(*) AS DistinctCountries
    FROM (SELECT DISTINCT Country FROM Customers);

```

## II. SQL WHERE, AND, OR, NOT

`WHERE` được dùng như điều kiện lọc các record được chọn ra.

**Syntax:**

```SQL
    SELECT column1, column2, ...
    WHERE condition;
    FROM table_name
```

Khi thực hiện lệnh, chỉ có các record thỏa mãn `condition` mới được select ra.

Sử dụng kết hợp `AND, OR, NOT` với `WHERE` với:

- `AND` đồng thời xảy ra (logic and)
- `OR` ít nhất một xảy ra(logic OR)
- `NOT` phủ định

**Ví dụ:**

```SQL
    SELECT * FROM Customers
    WHERE Country='Germany' AND CustomerId >=2;
```

Chọn ra các record có đồng thời `Country` là `Germany` và có `CustomerId` lớn hơn hoặc bằng 2.

Chú ý các trường với giá trị `numeric` không nên có ngoặc đơn `''`.

Operator | Description
---------|------------
`=` | Equal
`>` | Greater than |
`<` | Less than |
`>=` | Greater than or equal |
`<=` | Less than or equal |
`<>` | Not equal. Note: In some versions of SQL this operator may be written as != |
`BETWEEN` | Between a certain range |
`LIKE` | Search for a pattern |
`IN` | To specify multiple possible values for a column

## III. SQL ORDER BY

Dùng `ORDER BY` để in ra record theo thứ tự:

- `ASC`: Sắp xếp tăng dần (default)
- `DESC`: Sắp xếp giảm dần.

**Syntax:**

```SQL
    SELECT * FROM Customers
    ORDER BY field_1, field_2 ASC|DESC;
```

nếu không thêm `ASC` hoặc `DESC` sẽ default là `ASC`, khi để `field_1`, `field_2` tức là ta sẽ sắp xếp theo thứ tự với `field_1` trước, đối với 2 record có cùng `field_1` sẽ thực hiện sắp xếp theo `field_2`.

**Ví dụ:**

Sắp xếp theo nhiều trường.

```SQL
    SELECT * FROM Customers
    ORDER BY Country, CustomerName;
```

Sắp xếp theo thứ tự _tăng dần_ của trường Country, nếu 2 record có cùng trường Country thì sắp xếp theo thứ tự _giảm dần_ của trường **CustumerName**

```SQL
    SELECT * FROM Customers
    ORDER BY Country ASC, CustomerName DESC;
```

## III. INSERT INTO Statement

`INSERT INTO` dùng để thêm dữ liệu vào database

Có 2 kiểu để thêm:

```SQL
    INSERT INTO table_name (column1, column2, column3, ...)
    VALUES (value1, value2, value3, ...);
```

Thêm record với giá trị tương ứng với trường mong muốn. Cách thêm này ta có thể thêm  1 record với các trường mà ta muốn có giá trị. Trường nào k được thêm sẽ có giá trị null.

**Ví dụ:**

```SQL
    INSERT INTO Customers (CustomerName, City, Country)
    VALUES ('Cardinal', 'Stavanger', 'Norway');
```

Kết quả:

![anh-ket-qua](img/kq-1.png)

Ta để ý thấy tuy ta không thêm giá trị cho trường CustomerID nhưng nó vẫn tự generate và tăng theo đúng quy tắc.

Hoặc có thể dùng

```SQL
    INSERT INTO table_name
    VALUES (value1, value2, value3, ...);
```

Để thêm record với tất cả các trường trong record đều có giá trị. Các giá trị của trường tương ứng theo thứ tự từ trái qua phải.

**Chú ý: Các trường không được thêm value khi hiển thị sẽ nhận giá trị NULL. Để kiếm tra giá trị của một trường trong record có phải NULL hay không. Ta sử dụng `IS NULL` hoặc `IS NOT NULL`**

**Syntax:**

```SQL
    SELECT column_names
    FROM table_name
    WHERE column_name IS NOT NULL;
```

hoặc

```SQL
    SELECT column_names
    FROM table_name
    WHERE column_name IS NULL;
```

## IV. UPDATE

Dùng để chỉnh sửa record đã có trong table

**Syntax:**

```SQL
    UPDATE table_name
    SET column1 = value1, column2 = value2, ...
    WHERE condition;
```

Dùng `WHERE` để chọn xem record nào cần update, nếu như k có `WHERE` tất cả các record trong table đều được update

**Ví dụ:**

```SQL
    UPDATE Customers
    SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'
    WHERE CustomerID = 1;
```

Như vậy sẽ update trường `ContactName` và `City` của record có `CustomerID = 1`.

Có thể Update nhiều record một lúc, ví dụ ta để `WHERE City = 'Paris'` thì sẽ update tất cả các record có trường `City = 'Paris'`.

## V. DELETE

Dùng để xóa record khỏi table

**Syntax:**

```SQL
    DELETE FROM table_name WHERE condition;
```

Nếu như bỏ qua `WHERE` sẽ xóa tất cả record khỏi table mà không xóa table