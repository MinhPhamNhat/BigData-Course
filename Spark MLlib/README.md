# Machine Learning với PySpark

Apache Spark đã vầ đang là một nền tảng rất mạnh mẽ cho các dự án Big-Data do khả năng xử lý luồng thời gian thực, xử lý tương tác, xử lý đồ thị, xử lý trong bộ nhớ cũng như xử lý hàng loạt với tốc độ rất nhanh, dễ sử dụng và giao diện chuẩn.
Do vậy, khai thác Machine Learning đối với Apache Spark là rất tiềm năng và PySpark cung cấp cho ta khả năng thực hiện việc đó một các dễ dàng qua các thư viện Sql và Ml.

Để trực quan, ta sẽ sử dụng dataset Banking Marketing và thực hiện một model Machine Learning để dự đoán trong tập dữ liệu này.

Tập dữ liệu được tải trên <a href="https://www.kaggle.com/rouseguy/bankbalanced/data">Kaggle</a> và sử dụng Logistic Regression trong học máy để dự đoán. 

## Khai phá dữ liệu

```
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName('ML').getOrCreate()
df = spark.read.csv('bank.csv', header = True, inferSchema = True)
df.printSchema()
```
