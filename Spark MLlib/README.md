# Machine Learning với PySpark

Apache Spark đã vầ đang là một nền tảng rất mạnh mẽ cho các dự án Big-Data do khả năng xử lý luồng thời gian thực, xử lý tương tác, xử lý đồ thị, xử lý trong bộ nhớ cũng như xử lý hàng loạt với tốc độ rất nhanh, dễ sử dụng và giao diện chuẩn.
Do vậy, khai thác Machine Learning đối với Apache Spark là rất tiềm năng và PySpark cung cấp cho ta khả năng thực hiện việc đó một các dễ dàng qua các thư viện Sql và Ml.

Để trực quan, ta sẽ sử dụng dataset Banking Marketing và thực hiện một model Machine Learning để dự đoán trong tập dữ liệu này.

Tập dữ liệu được tải trên <a href="https://www.kaggle.com/rouseguy/bankbalanced/data">Kaggle</a> và sử dụng Logistic Regression trong học máy để dự đoán. 

## Khai phá dữ liệu

Tập dữ liệu liên quan đến các chiến dịch tiếp thị trực tiếp (gọi điện thoại) của một tổ chức ngân hàng Bồ Đào Nha. Mục tiêu phân loại là để dự đoán liệu khách hàng có đăng ký (Có / Không) đối với một khoản tiền gửi có kỳ hạn hay không

Ta sử dụng PySpark SQL để đọc dữ liệu và tạo một DataFrame tiện cho việc xử lý dữ liệu.
```
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName('ML').getOrCreate()
df = spark.read.csv('bank.csv', header = True, inferSchema = True)
df.printSchema()
```

```
root
 |-- age: integer (nullable = true)
 |-- job: string (nullable = true)
 |-- marital: string (nullable = true)
 |-- education: string (nullable = true)
 |-- default: string (nullable = true)
 |-- balance: integer (nullable = true)
 |-- housing: string (nullable = true)
 |-- loan: string (nullable = true)
 |-- contact: string (nullable = true)
 |-- day: integer (nullable = true)
 |-- month: string (nullable = true)
 |-- duration: integer (nullable = true)
 |-- campaign: integer (nullable = true)
 |-- pdays: integer (nullable = true)
 |-- previous: integer (nullable = true)
 |-- poutcome: string (nullable = true)
 |-- deposit: string (nullable = true)
```
