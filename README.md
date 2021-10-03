# TỰ HỌC SQL

[I. SQL SELECT](#i-sql-select)

[II. SQL WHERE, AND, OR, NOT](#ii-sql-where-and-or-not)

[III. SQL ORDER BY](#iii-sql-order-by)

[IV. UPDATE](#iv-update)

[V. DELETE](#v-delete)

[VI. SELECT TOP](#vi-select-top)

[VII. SELECT MIN, MAX](#vii-select-min-max)

[VIII. COUNT(), AVG(), SUM() Functions](#viii-count-avg-sum-functions)

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

## VI. SELECT TOP

`SELECT TOP` dùng để specify số lượng records sẽ lấy ra.

Khôn phải tất cả databases đều support `SELECT TOP`

**SQL server / MS Access Syntax:**

```SQL
    SELECT TOP number|percent column_name(s)
    FROM table_name
    WHERE condition;
```

**MySQL Syntax:**

```SQL
    SELECT column_name(s)
    FROM table_name
    WHERE condition
    LIMIT number;
```

**Oracle 12 Syntax:**

```SQL
    SELECT column_name(s)
    FROM table_name
    ORDER BY column_name(s)
    FETCH FIRST number ROWS ONLY;
```

**Older Oracle Syntax (with ORDER BY):**

```SQL
    SELECT *
    FROM (SELECT column_name(s) FROM table_name ORDER   BY column_name(s))
    WHERE ROWNUM <= number;
```

**Ví dụ:**

```SQL
    SELECT TOP 3 * FROM Customers;
```

Lấy ra 3 record đầu tiên từ Customers

Với MySQL

```SQL
    SELECT * FROM Customers;
    LIMIT 3;
```

`SELECT TOP` bằng percent với SQL Server/MS Access

```SQL
    SELECT TOP 50 PERCENT * FROM Customers;
```

Chọn ra 1/2 số bản ghi đầu tiên của table

Với Oracle

```SQL
    SELECT * FROM Customers
    FETCH FIRST 50 PERCENT ROWS ONLY;
```

Ví dụ kết hợp với `WHERE`

```SQL
    SELECT TOP 3 * FROM Customers
    WHERE Country = 'Viet Nam'
```

Chọn ra 3 bản ghi đầu tiên có trường `Country = Viet Nam`

## VII. SELECT MIN, MAX

`MIN(), MAX()` dùng để lấy ra giá trị nhỏ nhất trong trường được chọn.

**Syntax:**

```SQL
    SELECT MIN|MAX(column_name)
    FROM table_name
    WHERE condition;
```

**Ví dụ:**

```SQL
    SELECT MAX(Price) AS LargestPrice
    FROM Products;
```

Lấy ra giá trị lớn nhất của trường `Price` và sau đó gán cho `LargestPrice` rồi hiển thị ra.

## VIII. COUNT(), AVG(), SUM() Functions

- `COUNT()` trả về số record match với điều kiện.
- `SUM()` trả về tổng giá trị của record ứng với trường

- `AVG()` trả về trung bình cộng giá trị của record ứng với trường.

**Syntax:**

```SQL
    SELECT COUNT|SUM|AVG(column_name)
    FROM table_name
    WHERE condition;
```

Ví dụ:

```SQL
    SELECT COUNT(ProductID)
    FROM Products;
```

Đếm số record có giá trị `ProductID` khác `NULL`

```SQL
    SELECT AVG|SUM(Price)
    FROM Products;
```

Tính giá trị trung bình hoặc tổng trong trường `Price`, bỏ qua các giá trị NULL.

## IX. LIKE

Sử dụng `LIKE` kết hợp với `WHERE` để lọc dữ liệu.

`LIKE` thường đi với các operator:

- `%` để diễn tả thành phần của dữ liệu
- `_` để biểu diễn cho 1 kí tự, do đó `__` là 2 kí tự...

**Chú ý: MS Access sử dụng `*` thay cho `%` và `?` thay cho `_`**

**`LIKE` Syntax:**

```SQL
    SELECT column1, column2, ...
    FROM table_name
    WHERE columnn LIKE pattern;
```

Cách sử dụng LIKE với các operator:

LIKE Operator | Description
--------------|------------
WHERE CustomerName LIKE 'a%' | Tìm record có giá trị trong trường bắt đầu bằng kí tự `a`
WHERE CustomerName LIKE '%a' | Tìm record có giá trị trong trường kết thúc bằng kí tự `a`
WHERE CustomerName LIKE '%or%' | Tìm các record mà trường có giá trị chứa `or`
WHERE CustomerName LIKE '_r%' | Tìm các record mà trường có giá trị chứa `r` ở vị trí thứ 2
WHERE CustomerName LIKE 'a_%' | Tìm các record mà trường có giá trị bắt đầu bằng `a` và có ít nhất 2 kí tự
WHERE CustomerName LIKE 'a__%' | Tìm các record mà trường có giá trị bắt đầu bằng `a` và có ít nhất 3 kí tự
WHERE ContactName LIKE 'a%o' | Tìm các record có giá trị trường bắt đầu bằng `a` và kết thúc bằng `o`

## X. Wildcards

Là các operator đi cùng với `LIKE`

**Wildcards characters trong MS Access:**
Symbol | Description | Example
-------|-------------|--------
`*` | Represents zero or more characters | bl* finds bl, black, blue, and blob
`?` | Represents a single character | h?t finds hot, hat, and hit
`[]` | Represents any single character within the brackets | h[oa]t finds hot and hat, but not hit
`!` | Represents any character not in the brackets | h[!oa]t finds hit, but not hot and hat
`-` | Represents any single character within the specified range | c[a-b]t finds cat and cbt
`#` | Represents any single numeric character | 2#5 finds 205, 215, 225, 235, 245, 255, 265, 275, 285, and 295

**Wildcards characters trong SQL Server:**

Symbol | Description | Example
-------|-------------|--------
`%` | Represents zero or more characters | bl% finds bl, black, blue, and blob
`_` | Represents a single character | h_t finds hot, hat, and hit
`[]` | Represents any single character within the brackets | h[oa]t finds hot and hat, but not hit
`^` | Represents any character not in the brackets | h[^oa]t finds hit, but not hot and hat
`-`| Represents any single character within the specified range | c[a-b]t finds cat and cbt

## XI. IN

`IN` cho phép ta chọn nhiều giá trị trong `WHERE`, nói cách khác nó như là short hand của `OR`

**Syntax:**

```SQl
    SELECT column_name(s)
    FROM table_name
    WHERE column_name IN (value1, value2, ...);
```

hoặc

```SQL
    SELECT column_name(s)
    FROM table_name
    WHERE column_name IN (SELECT STATEMENT);
```

**Ví dụ:**

```SQL
    SELECT * FROM Customers
    WHERE Country IN|NOT IN ('Germany', 'France', 'UK');
```

Chọn ra các record có trường `Country` có giá trị( hoặc ngoài các giá trị) `Germany hoặc France, hoặc UK`.

```SQL
    SELECT * FROM Customers
    WHERE Country IN (SELECT Country FROM Suppliers);
```

Chọn ra các record từ `Customers` table có trường `Country` có các giá trị mà trường `Country` trong `Suppliers` table có.

## XII. BETWEEN

`BETWEEN` + `AND` để chọn ra các record có giá trị trường nằm trong một khoảng nào đó, bao gồm kiểu `numbers`, `text`, `date`

**Syntax:**

```SQL
    SELECT column_name(s)
    FROM table_name
    WHERE column_name BETWEEN value1 AND value2;
```

**Ví dụ:**

```SQL
    SELECT * FROM Products
    WHERE Price NOT BETWEEN 10 AND 20;
```

```SQL
    SELECT * FROM Products
    WHERE ProductName BETWEEN 'Carnarvon Tigers' AND 'Mozzarella di Giovanni'
    ORDER BY ProductName;
```

Sẽ chọn ra các record có `ProductName` nằm trong khoảng từ `Carnarvon Tigers` đến `Mozzarella di Giovanni` theo thứ tự Alphabet rồi sau đó hiển thị ra theo thứ tự Alphabet luôn

```SQL
    SELECT * FROM Products
    WHERE ProductName NOT BETWEEN 'Carnarvon Tigers' AND 'Mozzarella di Giovanni'
    ORDER BY ProductName;
```

Như ví dụ trên nhưng là chọn ngoài khoảng.

**Ví dụ với Date:**

```SQL
    SELECT * FROM Orders
    WHERE OrderDate BETWEEN #07/01/1996# AND #07/31/1996#;
```

hoặc

```SQL
    SELECT * FROM Orders
    WHERE OrderDate BETWEEN '1996-07-01' AND '1996-07-31';
```