# DataFrame là gì
DataFrame là một kiểu dữ liệu collection phân tán, được tổ chức thành các cột được đặt tên. Về mặt khái niệm, nó tương đương với các bảng quan hệ (relational tables) đi kèm với các kỹ thuật tối ưu tính toán.
DataFrame có thể được xây dựng từ nhiều nguồn dữ liệu khác nhau như Hive table, các file dữ liệu có cấu trúc hay bán cấu trúc (csv, json), các hệ cơ sở dữ liệu phổ biến (MySQL, MongoDB, Cassandra), hoặc RDDs hiện hành. API này được thiết kế cho các ứng dụng Big Data và Data Science hiện đại. Kiểu dữ liệu này được lấy cảm hứng từ DataFrame trong Lập trình R và Pandas trong Python hứa hẹn mang lại hiệu suất tính toán cao hơn.

<br>
<img width="400" alt="6" src="https://user-images.githubusercontent.com/73160254/116809664-5a4f6300-ab69-11eb-84a7-6e9007b9109d.png">

# Tính năng của DataFrame

Một số tính năng đặc trưng của DataFrame như:
-	Tối ưu hóa đầu vào: DataFrames sử dụng các công cụ tối ưu hóa đầu vào như Catalyst Optimizer cho phép xử lý dữ liệu hiệu quả.  Ta có thể sử dụng cùng một công cụ cho tất cả các API Python, Java, Scala và R DataFrame.
-	Xử lý lớn: DataFrames có thể tích hợp với nhiều công cụ BigData khác và cho phép xử lý megabyte đến petabyte dữ liệu cùng một lúc.
-	Tính linh hoạt: DataFrames, giống như RDD, có thể hỗ trợ nhiều định dạng dữ liệu khác nhau, chẳng hạn như CSV, Cassandra, v.v.
-	Quản lý bộ nhớ tùy chỉnh: Trong RDD, dữ liệu được lưu trữ trong bộ nhớ RAM, trong khi DataFrames lưu trữ dữ liệu off-heap (bên ngoài không gian chính của Java Heap, nhưng vẫn bên trong RAM), do đó làm giảm các collection quá tải dư thừa.
-	Xử lý dữ liệu có cấu trúc: DataFrames cung cấp một cái nhìn sơ lược về dữ liệu.  Ở đây, dữ liệu có một số ý nghĩa đối với nó khi nó được lưu trữ
# SQL Context
SQLContext là một lớp và được sử dụng để khởi tạo các chức năng của Spark SQL.  Đối tượng SparkContext là bắt buộc để có thể khởi tạo đối tượng SQLContext.  Lệnh sau được sử dụng để khởi tạo SparkContext.

```
import sys
from pyspark import SparkContext, SparkConf
from pyspark.sql import SQLContext

# Config Spark context
conf = SparkConf().setMaster("local").setAppName("example app")
sc = SparkContext.getOrCreate(conf=conf)

# Config SQL context
sqlContext = SQLContext(sc)
```

# Tương tác với Spark DataFrame

## Đọc file csv
Để đọc file, pyspark.shell cung cấp phương thức read.csv() cho phép đọc file csv vào dataframe.

```
from pyspark.shell import spark
from pyspark.sql.types import *

# Tạo DataFrame từ file CSV
df_data = spark.read.csv('drive/My Drive/Colab Notebooks/click_data_sample.csv')
print(df_data.head(5))
```
Output:
```
[Row(_c0='click.at', _c1='user.id', _c2='campaign.id'),
 Row(_c0='2015-04-27 20:40:40', _c1='144012', _c2='Campaign077'),
 Row(_c0='2015-04-27 00:27:55', _c1='24485', _c2='Campaign063'),
 Row(_c0='2015-04-27 00:28:13', _c1='24485', _c2='Campaign063'),
 Row(_c0='2015-04-27 00:33:42', _c1='24485', _c2='Campaign038')]
```

## Đổi tên cột
Ta có thể dễ dàng thay đổi tên cột bằng withColumnRenamed. Tuy nhiên, về cơ bản thì DataFrame là bất biến (immutable) nên khi thay đổi thì 1 DataFrame mới sẽ được tạo ra.
```
new_df = df_data.withColumnRenamed("_c0", "access_time")\
                .withColumnRenamed("_c1", "userID")\
                .withColumnRenamed("_c2", "campaignID")
print(new_df.printSchema())
```
Output:
```
root
 |-- access_time: string (nullable = true)
 |-- userID: string (nullable = true)
 |-- campaignID: string (nullable = true)

None
```

## Query bằng SQL
Bằng cách sử dụng registerTempTable, ta sẽ có một table được tham chiếu đến Dataframe đó, ta có thể sử dụng tên table này để viết query SQL. Nếu ta sử dụng sqlContext.sql('query SQL') thì giá trị trả về cũng là Dataframe.
Có 1 lưu ý là: Ta cũng có thể viết subquery nhưng subquery cần được gán Alias, nếu không sẽ bị (Syntax error).
Ta thử tìm các dòng có cột campaignID có giá trị là Campaign047 
 ```
#SQL query

new_df.registerTempTable("whole_log_table")

# Query
print (sqlContext.sql(" SELECT * FROM whole_log_table where campaignID == 'Campaign047' ").count())
```
Output:
```
18081
```

Ta in thử 5 dòng đầu trong đó
```
print(sqlContext.sql(" SELECT * FROM whole_log_table where campaignID == 'Campaign047' ").show(5))
```
Output:
```
+-------------------+------+-----------+
|        access_time|userID| campaignID|
+-------------------+------+-----------+
|2015-04-27 05:26:14| 14151|Campaign047|
|2015-04-27 05:26:32| 14151|Campaign047|
|2015-04-27 05:26:34| 14151|Campaign047|
|2015-04-27 05:27:47| 14151|Campaign047|
|2015-04-27 05:28:16| 14151|Campaign047|
+-------------------+------+-----------+
only showing top 5 rows
```

Ta cũng có thể query linh động hơn
```
#Thêm biến số vào trong câu SQL
for count in range(1, 3):
    print("Campaign00" + str(count))
    print(sqlContext.sql("SELECT count(*) as access_num FROM whole_log_table where campaignID == 'Campaign00" + str(count) + "'").show())
```
Output:
```
Campaign001
+----------+
|access_num|
+----------+
|      2407|
+----------+

None
Campaign002
+----------+
|access_num|
+----------+
|      1674|
+----------+
none
```
Đối với trường hợp subquery
```
#Trường hợp Sub Query：
print (sqlContext.sql("SELECT count(*) as first_count FROM (SELECT userID, min(access_time) as first_access_date FROM whole_log_table GROUP BY userID) subquery_alias WHERE first_access_date < '2015-04-28'").show(5))
```
Output:
```
+-----------+
|first_count|
+-----------+
|      20480|
+-----------+

None
```

## Tìm kiếm sử dụng filter, select
Đối với DataFrame , tìm kiếm kèm điều kiện rất đơn giản. Giống với câu query ở trên nhưng filter, select dễ dàng hơn rất nhiều. Vậy filter và select khác nhau thế nào ?
Cùng là để tìm kiếm nhưng filter trả về những row thoả mãn điều kiện, trong đó select  lấy dữ liệu theo column.

_Ví dụ Filer_
```
#Ví dụ filter
print(new_df.filter(new_df["access_time"] > "2015-05-01").show(3))
```
Output:
```
+-------------------+-------+-----------+
|        access_time| userID| campaignID|
+-------------------+-------+-----------+
|           click.at|user.id|campaign.id|
|2015-05-01 22:11:57| 114157|Campaign002|
|2015-05-01 23:36:25|  93708|Campaign055|
+-------------------+-------+-----------+
only showing top 3 rows

None
```

_Ví dụ với select_
```
#Ví dụ select
print(whole_log_df.select("access_time", "userID").show(3))
```
Output:
```
+-------------------+-------+
|        access_time| userID|
+-------------------+-------+
|           click.at|user.id|
|2015-04-27 20:40:40| 144012|
|2015-04-27 00:27:55|  24485|
+-------------------+-------+
only showing top 3 rows

None
```
# Tài liệu tham khảo
<a href="https://blog.vietnamlab.vn/xu-ly-du-lieu-voi-spark-dataframe/">[1] Xử lý dữ liệu với Spark DataFrame </a>
<a href="https://www.tutorialspoint.com/spark_sql/spark_sql_dataframes.htm">[2] Spark SQL - DataFrame</a>
