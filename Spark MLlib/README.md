# Machine Learning với PySpark

Apache Spark đã và đang là một nền tảng rất mạnh mẽ cho các dự án Big-Data do khả năng xử lý luồng thời gian thực, xử lý tương tác, xử lý đồ thị, xử lý trong bộ nhớ cũng như xử lý hàng loạt với tốc độ rất nhanh, dễ sử dụng và giao diện chuẩn.
Do vậy, khai thác Machine Learning đối với Apache Spark là rất tiềm năng và PySpark cung cấp cho ta khả năng thực hiện việc đó một các dễ dàng qua các thư viện Sql và Ml.

Để trực quan, ta sẽ sử dụng dataset Banking Marketing và thực hiện một model Machine Learning để dự đoán trong tập dữ liệu này.

Tập dữ liệu được tải trên <a href="https://www.kaggle.com/rouseguy/bankbalanced/data">Kaggle</a> và sử dụng Logistic Regression trong học máy để dự đoán. 

<img src="https://miro.medium.com/max/1250/1*J_4joYwf_HHMbBt8s1Kuqw.jpeg">

# Xử lý và chuẩn bị cho Model

Tập dữ liệu liên quan đến các chiến dịch tiếp thị trực tiếp (gọi điện thoại) của một tổ chức ngân hàng Bồ Đào Nha. Mục tiêu phân loại là để dự đoán liệu khách hàng có đăng ký (Có / Không) đối với một khoản tiền gửi có kỳ hạn hay không

Ta sử dụng PySpark SQL để đọc dữ liệu và tạo một DataFrame tiện cho việc xử lý dữ liệu.
```
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName('ML').getOrCreate()
df = spark.read.csv('bank.csv', header = True, inferSchema = True)
df.printSchema()
```
Output:
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
Tập dữ liệu gồm đầu ra là cột <i>deposit</i> và đầu vào là các cột còn lại. Có khá nhiều cột kiểu dữ liệu String. Ta cần đưa về numeric để xử lý.

```
df.show()
+---+-----------+--------+---------+-------+-------+-------+----+-------+---+-----+--------+--------+-----+--------+--------+-------+
|age|        job| marital|education|default|balance|housing|loan|contact|day|month|duration|campaign|pdays|previous|poutcome|deposit|
+---+-----------+--------+---------+-------+-------+-------+----+-------+---+-----+--------+--------+-----+--------+--------+-------+
| 59|     admin.| married|secondary|     no|   2343|    yes|  no|unknown|  5|  may|    1042|       1|   -1|       0| unknown|    yes|
| 56|     admin.| married|secondary|     no|     45|     no|  no|unknown|  5|  may|    1467|       1|   -1|       0| unknown|    yes|
| 41| technician| married|secondary|     no|   1270|    yes|  no|unknown|  5|  may|    1389|       1|   -1|       0| unknown|    yes|
| 55|   services| married|secondary|     no|   2476|    yes|  no|unknown|  5|  may|     579|       1|   -1|       0| unknown|    yes|
| 54|     admin.| married| tertiary|     no|    184|     no|  no|unknown|  5|  may|     673|       2|   -1|       0| unknown|    yes|
| 42| management|  single| tertiary|     no|      0|    yes| yes|unknown|  5|  may|     562|       2|   -1|       0| unknown|    yes|
| 56| management| married| tertiary|     no|    830|    yes| yes|unknown|  6|  may|    1201|       1|   -1|       0| unknown|    yes|
| 60|    retired|divorced|secondary|     no|    545|    yes|  no|unknown|  6|  may|    1030|       1|   -1|       0| unknown|    yes|
| 37| technician| married|secondary|     no|      1|    yes|  no|unknown|  6|  may|     608|       1|   -1|       0| unknown|    yes|
| 28|   services|  single|secondary|     no|   5090|    yes|  no|unknown|  6|  may|    1297|       3|   -1|       0| unknown|    yes|
| 38|     admin.|  single|secondary|     no|    100|    yes|  no|unknown|  7|  may|     786|       1|   -1|       0| unknown|    yes|
| 30|blue-collar| married|secondary|     no|    309|    yes|  no|unknown|  7|  may|    1574|       2|   -1|       0| unknown|    yes|
| 29| management| married| tertiary|     no|    199|    yes| yes|unknown|  7|  may|    1689|       4|   -1|       0| unknown|    yes|
| 46|blue-collar|  single| tertiary|     no|    460|    yes|  no|unknown|  7|  may|    1102|       2|   -1|       0| unknown|    yes|
| 31| technician|  single| tertiary|     no|    703|    yes|  no|unknown|  8|  may|     943|       2|   -1|       0| unknown|    yes|
| 35| management|divorced| tertiary|     no|   3837|    yes|  no|unknown|  8|  may|    1084|       1|   -1|       0| unknown|    yes|
| 32|blue-collar|  single|  primary|     no|    611|    yes|  no|unknown|  8|  may|     541|       3|   -1|       0| unknown|    yes|
| 49|   services| married|secondary|     no|     -8|    yes|  no|unknown|  8|  may|    1119|       1|   -1|       0| unknown|    yes|
| 41|     admin.| married|secondary|     no|     55|    yes|  no|unknown|  8|  may|    1120|       2|   -1|       0| unknown|    yes|
| 49|     admin.|divorced|secondary|     no|    168|    yes| yes|unknown|  8|  may|     513|       1|   -1|       0| unknown|    yes|
+---+-----------+--------+---------+-------+-------+-------+----+-------+---+-----+--------+--------+-----+--------+--------+-------+
only showing top 20 rows
```
```
string_features = ['job', 'marital', 'education', 'default', 'housing', 'loan', 'contact','month', 'poutcome', 'deposit']
```
PySpark ML cung cấp cho ta một số thư viện để chuyển các kiểu dữ liệu String sang Numeric như StringIndexer, OneHotEncoder, ... Ở đây, ta sử dụng StringIndexer để transform dữ liệu.
```
# Convert String col to Numeric
from pyspark.ml.feature import StringIndexer
for i in string_features:
  indexer = StringIndexer()
  indexer.setInputCol(i).setOutputCol(i+"_indexer")
  df = indexer.fit(df).transform(df)
df = df.drop(*string_features)
```
```
df.printSchema()
root
 |-- age: integer (nullable = true)
 |-- balance: integer (nullable = true)
 |-- day: integer (nullable = true)
 |-- duration: integer (nullable = true)
 |-- campaign: integer (nullable = true)
 |-- pdays: integer (nullable = true)
 |-- previous: integer (nullable = true)
 |-- job_indexer: double (nullable = false)
 |-- marital_indexer: double (nullable = false)
 |-- education_indexer: double (nullable = false)
 |-- default_indexer: double (nullable = false)
 |-- housing_indexer: double (nullable = false)
 |-- loan_indexer: double (nullable = false)
 |-- contact_indexer: double (nullable = false)
 |-- month_indexer: double (nullable = false)
 |-- poutcome_indexer: double (nullable = false)
 |-- deposit_indexer: double (nullable = false)
```
Tập dữ liệu bây giờ không còn kiễu dữ liệu String nữa. Dữ liệu đã sẵn sàng để có thể train model. Tuy nhiên, PySpark cũng cấp VectorAssembler cho phép ta kết hợp một danh sách các cột nhất định thành một cột vectơ duy nhất. Việc này rất hữu ích để kết hợp các tính năng thô và các tính năng được tạo bởi các cột đặc trưng khác nhau thành một vectơ đặc trưng duy nhất, để train các mô hình ML như hồi quy logistic và cây quyết định. VectorAssembler chấp nhận các kiểu cột đầu vào sau: tất cả các kiểu số, kiểu boolean và kiểu vectơ. Trong mỗi hàng, giá trị của các cột đầu vào sẽ được nối thành một vectơ theo thứ tự được chỉ định. 
```
from pyspark.ml.feature import VectorAssembler

# Vector feature
assembler = VectorAssembler(inputCols=feature_names, outputCol="features")
transformed_data = assembler.transform(df)
```
Sau khi VectorAssembler, các cột dữ liệu được kết hợp thành cột features sẽ là
```
transformed_data.select("features").show()
+--------------------+
|            features|
+--------------------+
|(16,[0,1,2,3,4,5,...|
|(16,[0,1,2,3,4,5,...|
|(16,[0,1,2,3,4,5,...|
|(16,[0,1,2,3,4,5,...|
|(16,[0,1,2,3,4,5,...|
|[42.0,0.0,5.0,562...|
|[56.0,830.0,6.0,1...|
|[60.0,545.0,6.0,1...|
|(16,[0,1,2,3,4,5,...|
|[28.0,5090.0,6.0,...|
|[38.0,100.0,7.0,7...|
|(16,[0,1,2,3,4,5,...|
|[29.0,199.0,7.0,1...|
|[46.0,460.0,7.0,1...|
|[31.0,703.0,8.0,9...|
|[35.0,3837.0,8.0,...|
|[32.0,611.0,8.0,5...|
|(16,[0,1,2,3,4,5,...|
|(16,[0,1,2,3,4,5,...|
|[49.0,168.0,8.0,5...|
+--------------------+
only showing top 20 rows
```
Bây giờ dữ liệu đã sẵn sàng để train Model. Ta sẽ sử dụng Logistic Regression đối với bài toán này.

# Logistic Regression Model

```
# Train test split
(training_data, test_data) = transformed_data.randomSplit([0.8,0.2])
```
Đầu tiên ta phải tách tập train và test. PySpark DataFrame cho phép ta tách DataFrame theo tỉ lệ ta mong muốn
```
from pyspark.ml.classification import LogisticRegression
model = LogisticRegression(featuresCol = 'features',labelCol='deposit_indexer', maxIter=30)
```
Sao đó ta tạo model. Cột cần được train là features và cột target là deposit_indexer
```
# Fit model
fit_model = model.fit(training_data)

# Prediction
y_pred = fit_model.transform(test_data)
```
Train model và đưa tập test vào để dự đoán
```
from pyspark.ml.evaluation import MulticlassClassificationEvaluator

# Evaluate accuracy
multi_evaluator = MulticlassClassificationEvaluator(labelCol = 'deposit_indexer', metricName = 'accuracy')
print('Logistic Regression Accuracy:', multi_evaluator.evaluate(y_pred))

Logistic Regression Accuracy: 0.782549109182275
```
Ta xác định chất lượng model. Model khá tốt, nếu ta thực hiện tiền xử lý tốt hơn có lẽ model sẽ được cải thiện.
```
y_pred.select("age","balance","deposit","rawPrediction", "probability", "prediction").show(10)

+---+-------+-------+--------------------+--------------------+----------+
|age|balance|deposit|       rawPrediction|         probability|prediction|
+---+-------+-------+--------------------+--------------------+----------+
| 18|      5|     no|[1.24031836691664...|[0.77561942577722...|       0.0|
| 18|    108|    yes|[-0.8424151044749...|[0.30102637901402...|       1.0|
| 18|    108|    yes|[0.93865241633341...|[0.71882737144269...|       0.0|
| 18|    348|    yes|[-0.1202583591761...|[0.46997159090632...|       1.0|
| 19|     55|     no|[-0.8383004794600...|[0.30189284335200...|       1.0|
| 19|     60|     no|[0.53454555853161...|[0.63054266587903...|       0.0|
| 19|    103|    yes|[1.12953722001224...|[0.75575348457604...|       0.0|
| 19|    329|    yes|[-0.5368318763241...|[0.36892487585105...|       1.0|
| 19|    372|    yes|[0.15061621411956...|[0.53758303202370...|       0.0|
| 19|    394|    yes|[-0.6405631974937...|[0.34511923892910...|       1.0|
+---+-------+-------+--------------------+--------------------+----------+
only showing top 10 rows
