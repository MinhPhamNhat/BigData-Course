# Spark Properties

## Spark Properties

Spark properties kiểm soát hầu hết các cài đặt ứng dụng và được cấu hình riêng
cho từng ứng dụng. Các thuộc tính này có thể được đặt trực tiếp trên SparkConf và
truyền tới SparkContext. SparkConf cho phép cấu hình một số thuộc tính phổ biến
(ví dụ: URL và tên ứng dụng), cũng như các cặp khóa-giá trị tùy ý thông qua phương
thức set (). Ví dụ: chúng ta có thể khởi tạo một ứng dụng có hai luồng như sau:

val conf = new SparkConf()

.setMaster("local[2]")

.setAppName("Spark Practice")

val sc = new SparkContext(conf)

# Apache Spark RDD

**1. RDD là gì?**

**_RDD:_** _Resilient Distributed Dataset_ là mô ̣t cáu trúc cơ bản trong Apache Spark.
RDD là đại diện chô tập dữ liệu phân tán, tứ c là RDD ta ̣p hợ p các dữ liê ̣u đượ c phân
tán đã xử lý qua các node. Ta hãy phân tích RDD:

- **Resilient:** là khả năng chịu lỗi bàng sự hỗ trợ từ các RDD lineage graph và cố
    thể tính toán lại các RDD partitions khi node chứ a RDD partitions đố bị lỗi.


- **Distributed:** Thể hiê ̣n viê ̣c các dữ liê ̣u được phân tán trên các node.
- **Dataset:** Là ta ̣p dữ liê ̣u đượ c sử dụng.

Dô đó, mỗi và mọi tập dữ liệu trông RDD được phân vùng một cách hợp lý trên
nhiều máy chủ để chúng có thể được tính tôán trên các node khác nhau của cụm.
RDD có khả năng chịu lỗi, tức là nó có khả năng tự phục hồi trông trường hợp bị lỗi.

Có ba cách để tạô RDD trông Spark, chẳng hạn như - Dữ liệu bộ lưu trữ ổn định
( _Data in stable storage)_ , các RDD khác và song song hóa collection đã tồn tại trông
chương trình trình điều khiển. Người ta cũng có thể vận hành Spark RDD sông sông
với một API cấp thấp cung cấp các chuyển đổi (transformings) và hành động
(actions)


```
Figure 1 Data in stable storage
```
Đặc điểm quan trọng của 1 RDD là số **partitions**. Một RDD bao gồm nhiều
partition nhỏ, mỗi partition này đại diện cho 1 phần dữ liệu phân tán. Khái niệm
partition là logical, tức là 1 node xử lý có thể chứa nhiều hơn 1 RDD partition. Theo
mặc định, dữ liệu các partitions sẽ lưu trên memory. Thử tưởng tượng ta cần xử lý
1TB dữ liệu, nếu lưu hết trên mem tính ra thì cung khá tốn kém. Tất nhiên nếu ta có
1TB ram để xử lý thì tốt quá nhưng điều đó không cần thiết. Với việc chia nhỏ dữ
liệu thành các partition và cơ chế lazy evaluation của Spark ta có thể chỉ cần vài chục
GB ram và 1 chương trình được thiết kế tốt để xử lý 1TB dữ liệu, chỉ là sẽ chậm hơn
có nhiều RAM thôi

**2. Tại sao chúng ta cần phải sử dụng RDD?**

```
Các nhu càu chính khi sử dụng RDD đố là :
```
- Các thua ̣t toán bị la ̣p lại (Iterative algorithms)
- Công cụ khai tác dữ liê ̣u tương tác (Interactive data mining tools)
- DSM (Distributed Shared Memory) cố khả năng chịu lỗi trên mô ̣t cụm và
    triển khải các công viê ̣c kếm.
- Trong hê ̣ điê ̣n toán phân tán, dữ liê ̣u sễ đượ c lưu trữ trong hê ̣ thống ổn
    định như HDFS hay Amazôn S3. Viê ̣c này khiến cho công viê ̣c tính toán
    cha ̣m hơn vì liên quan nhiều đến các khả năng I/O, sao chếp và tuàn tự hoá
    trong các tiến trình.

Trông hai trườ ng hợ p đàu tiên thì RDD lưu dữ liê ̣u trong bô ̣ nhớ và viê ̣c này
giúp cải thiê ̣u hiê ̣u suát theo cáp đô ̣ magnitude.


Thách thứ c trong viê ̣c thiết kế RDD đố là viê ̣c tạo ra mô ̣t API cung cáp khả
năng chịu lỗi hiê ̣u quả. Để làm được điều này, RDDs đã cung cáp mô ̣t dạng bô ̣ nhớ
chung [shared memory] hạn chế dự a vào **coarse-grained transformation** thay vì **fine-
grained** updates cho các trạng thái chung.

Spark làm rỗ RDD thông qua API tích hợ p ngôn ngữ. Khi đố, mỗi ta ̣p dữ liê ̣u sễ
đượ c biểu diễn dướ i dạng mô ̣t object và quá trình chuyển đổi RDD cố liên quan đến
viê ̣c sử dụng các phương thứ c của object này.

Apachê Spark đánh giá RDD mô ̣t cách lười biếng (lazy). RDD chỉ đượ c gội khi càn
thiết, viê ̣c này giúp tiết kiê ̣m thời gian và nâng cao hiê ̣u quả.

**3. Các tính năng của Spark RDD**
    RDD cố các tính năng như là:

```
Figure 2 C á c t í nh n ăng củ a RDD
```
```
3.1. Tính toán trên RAM (In-Memory Computation)
```
RDDs lưu trữ các kết quả trung gian trên bô ̣ nhớ phân tán (RAM) thay bô ̣ nhớ ổn
định [stable storage] (Disk).

```
3.2. Đánh giá lười biếng (Lazy Evaluations)
```

Các phếp biến đổi (transfôrmatiôns) trông Spark đều lườ i ở chỗ chúng không
tính ngay kết quả mà thay vàô đố, chúng chỉ nhớ các phếp biến đổi đượ c áp dụng
trên các ta ̣p dữ liê ̣u. Spark chỉ các phếp biến đổi đố khi mô ̣t hành đô ̣ng(actions)
đượ c yêu càu.

```
3.3. Khả năng bất biến (Immutablility)
```
Dữ liệu an tôàn để chia sẻ trên các tiến trình. Nó cũng có thể được tạo hoặc truy
xuất bất cứ lúc nào giúp dễ dàng lưu vàô bộ nhớ đệm, chia sẻ và nhân rộng. Do đó,
nó là một cách để đạt được sự nhất quán trong tính toán.

```
3.4. Khả năng chịu lỗi (Fault Tolerance)
```
Spark RDD có khả năng chịu lỗi nhờ vào sự theo dõi thông tin dòng dữ liệu để tự
động xây dựng lại dữ liệu bị mất khi bị lỗi. Để làm đượ c điều này, mỗi RDD nhớ cách
nó được tạo từ các tập dữ liệu khác (bằng các phép biến đổi như map, join hoặc
groupBy) để tạo lại chính nó.

```
3.5. Sự bền bỉ ( Persistence)
```
Người dùng có thể cho biết họ sẽ sử dụng lại những RDD nào và tự thiết la ̣p khả
năng lưu trữ cho họ (ví dụ: lưu trữ trong bộ nhớ hoặc trên Đĩa)

```
3.6. Phân vùng (Partitioning)
```
Phân vùng là đơn vị cơ bản của tính song song trong Spark RDD. Mỗi phân vùng
là một phân chia dữ liệu hợp lý có thể thay đổi được. Người ta có thể tạo một phân
vùng thông qua một số biến đổi trên các phân vùng hiện có.

```
3.7. Location-Stickness
```
RDD có khả năng xác định ưu tiên vị trí để tính toán các partition. Tùy chọn vị trí
đề cập đến thông tin về vị trí của RDD. DAGSchêdulêr đặt các phân vùng theo cách
sao cho tác vụ gần với dữ liệu nhất có thể.

```
3.8. Coarse-gained Operation
```
Coarse-gained Operation áp dụng cho tất cả các phần tử trong bộ dữ liệu thông
qua map hoặc filter hoặc nhóm theo các phếp toán.


**4. Các phép toán trong Spark RDD**

```
RDD hỗ trợ 2 phếp toán là :
```
- Tranformations
- Actions

Transformation và Action hoạt động giống như DataFramê lẫn DataSets.
Transformation xử lý các thao tác lazily và Action xử lý thao tác cần xử lý tức thời

```
Figure 3 C á ch th ứ c RDD chuy ể n ho á data
```
```
4.1. Transformations
```
Spark RDD Tranformations là mô ̣t hàm nha ̣n vào mô ̣t RDD và trả về mô ̣t hay
nhiều RDD khác. RDD ban đàu sễ không bị thay đổi vì RDD mang tính bát biến
(Immutability) mà nố sễ sinh ra các RDD mớ i bàng các áp dụng các phương thứ c
như Map(), filtêr(), rêducêByKêy(), ...

Nhiều phiên bản Tranformations của RDD có thể hôạt động trên các Structured
API, Tranformations xử lý lazily, tức là chỉ giúp dựng êxêcutiôn plans, dữ liệu chỉ
được truy xuất thực sự khi thực hiện Actions

```
Transformations gồm các phương thứ c như:
```
- **distinct** : lôại bỏ trùng lắp trông RDD


- **filter** : tương đương với việc sử dụng whêrê trông SQL – tìm các record trong
    RDD xêm những phần tử nàô thỏa điều kiện. Có thể cung cấp một hàm phức
    tạp sử dụng để filtêr các rêcôrd cần thiết – Như trông Pythôn, ta có thể sử
    dụng hàm lambda để truyền vàô filter
- **map** : thực hiện một công việc nàô đó trên tôàn bộ RDD. Trông Pythôn sử
    dụng lambda với từng phần tử để truyền vàô map
- **flatMap** : cung cấp một hàm đơn giản hơn hàm map. Yêu cầu ôutput của map
    phải là một structurê có thể lặp và mở rộng được.
- **sortBy** : mô tả một hàm để trích xuất dữ liệu từ các ôbjêct của RDD và thực
    hiện sôrt được từ đó.
- **randomSplit** : nhận một mảng trọng số và tạô một randôm sêêd, tách các RDD
    thành một mảng các RDD có số lượng chia thêô trọng số.

```
4.2. Actions
```
Actions trả về kết quả cuối cùng qua các tính toán RDD. Nó kích hoạt thực thi
bằng cách sử dụng đồ thị tuyến tính (lineage graph) để load dữ liệu vào RDD gốc,
thực hiện tất cả các phép biến đổi trung gian và trả về kết quả cuối cùng cho Driver
để xử lý hoặc ghi dữ liê ̣u xuống các công cụ lưu trữ

```
Actions gồm các phương thứ c như:
```
- **reduce** : thực hiện hàm rêducê trên RDD để thu về 1 giá trị duy nhất
- **count** : đếm số dòng trong RDD
- **countApprox:** phiên bản đếm xáp xỉ của count, nhưng phải cung cáp
    timeout vì cố thể không nha ̣n đượ c kết quả.
- **countByValue** : đếm số giá trị của RDD. Phương thứ c chỉ sử dụng nếu map
    kết quả nhổ vì tát cả dữ liê ̣u sễ đượ c load lên memory của driver để tính
    toán và ta chỉ nên sử dụng trong tình huống số dồng nhổ và lượ ng item
    khác nhau cũng nhổ.
- **first:** láy giá trị đàu tiên của dataset
- **max và min:** láy làn lượ cgiá trị lớ n nháy và nhổ nhát của dataset
- **take và các method tương tự:** láy mô ̣t lượng giá trị từ trong RDD. take sễ
    scan qua mô ̣t partition trướ c và sử dụng kết quả để dự đoán số lượ ng
    partition càn phải láy thể để thoả số lượng.
**5. Giới hạn của Spark RDD**


```
Figure 4 C á c h ạ n ch ế c ủ a RDD
```
```
5.1. Không có công cụ tối ưu sẵn
```
Khi làm viê ̣c vớ i cáu trúc dữ liê ̣u, RDD không thể phát huy tối đa lợ i thế từ bô ̣ tối
ưu của Spark như **catalyst optimizer** and **Tungsten execution engine**

```
5.2. Việc xử lý cấu trúc dữ liệu
```
Không giống như Dataframê và datasets, RDD không suy ra lược đồ của dữ liệu
đã nhập và yêu cầu người dùng phải chỉ định nó.

```
5.3. Giới hạn hiệu suất
```
Là các đối tượng JVM trông bộ nhớ, RDD liên quan đến chi phí Thu gôm rác và
Tuần tự hóa Java, những thứ này rất tốn kém khi dữ liệu phát triển.

```
5.4. Giới hạn lưu trữ
```
RDDs suy giảm khi không có đủ bộ nhớ để lưu trữ chúng. Người ta cũng có thể
lưu trữ partition của RDD đó trên đĩa do không đủ với RAM. Dô đó, nó sẽ cung cấp
hiệu suất tương tự như các hệ thống sông sông dữ liệu.

# Apache Spark SQL - DataFrame


**1. DataFrame là gì**

DataFrame là một kiểu dữ liệu côllêctiôn phân tán, được tổ chức thành các cột
được đặt tên. Về mặt khái niệm, nó tương đương với các bảng quan hệ (relational
tablês) đi kèm với các kỹ thuật tối ưu tính toán.

DataFrame có thể được xây dựng từ nhiều nguồn dữ liệu khác nhau như Hive
table, các file dữ liệu có cấu trúc hay bán cấu trúc (csv, json), các hệ cơ sở dữ liệu
phổ biến (MySQL, MongoDB, Cassandra), hoặc RDDs hiện hành. API này được
thiết kế cho các ứng dụng Big Data và Data Science hiện đại. Kiểu dữ liệu này
được lấy cảm hứng từ DataFrame trong Lập trình R và Pandas trong Python hứa
hẹn mang lại hiệu suất tính toán cao hơn.

```
Figure 5 T ố c độ th ự c thi c ủ a c á c công c ụ kh á c nhau
```
**2. Tính năng của DataFrame**

```
Mô ̣t số tính năng đa ̣c trưng của DataFrame như:
```
- Tối ưu hóa đầu vào: DataFrames sử dụng các công cụ tối ưu hóa đầu
    vào như **Catalyst Optimizer** cho phếp xử lý dữ liệu hiệu quả. Ta có thể
    sử dụng cùng một công cụ cho tất cả các API Python, Java, Scala và R
    DataFrame.


- Xử lý lớ n: DataFrames có thể tích hợp với nhiều công cụ BigData khác
    và cho phép xử lý mêgabytê đến petabyte dữ liệu cùng một lúc.
- Tính linh hoạt: DataFrames, giống như RDD, có thể hỗ trợ nhiều định
    dạng dữ liệu khác nhau, chẳng hạn như CSV, Cassandra, v.v.
- Quản lý bộ nhớ tùy chỉnh: Trong RDD, dữ liệu được lưu trữ trong bộ
    nhớ RAM, trông khi DataFramês lưu trữ dữ liệu off-heap (bên ngoài
    không gian chính của Java Heap, nhưng vẫn bên trong RAM), dô đó làm
    giảm các collection quá tải dư thừ a.
- Xử lý dữ liệu có cấu trúc: DataFrames cung cấp một cái nhìn sơ lượ c về
    dữ liệu. Ở đây, dữ liệu có một số ý nghĩa đối với nó khi nó được lưu trữ
**3. SQL Context**

SQLContext là một lớp và được sử dụng để khởi tạo các chức năng của Spark SQL.
Đối tượng SparkContext là bắt buộc để cố thể khởi tạô đối tượng SQLContext. Lệnh
sau được sử dụng để khởi tạo SparkContext thông qua spark-shell.

**4. Tương tác với Spark DataFrame**

```
Config context
```
import sys
from pyspark import SparkContext, SparkConf
from pyspark.sql import SQLContext

# Config Spark context
conf = SparkConf().setMaster("local").setAppName("word counting")
sc = SparkContext.getOrCreate(conf=conf)

# Config SQL context
sqlContext = SQLContext(sc)

```
Đọc file csv
```
Để độc file, pyspark.shell cung cáp phương thứ c read.csv() cho phếp độc file csv
vào dataframe.

from pyspark.shell import spark
from pyspark.sql.types import *

# Tạo DataFrame từ file CSV
df_data = spark.read.csv('drive/My Drive/Colab Notebooks/click_data_sample
.csv')


print(df_data.head(5))

Output:

[Row(_c0='click.at', _c1='user.id', _c2='campaign.id'),
Row(_c0='2015- 04 - 27 20:40:40', _c1='144012', _c2='Campaign077'),
Row(_c0='2015- 04 - 27 00:27:55', _c1='24485', _c2='Campaign063'),
Row(_c0='2015- 04 - 27 00:28:13', _c1='24485', _c2='Campaign063'),
Row(_c0='2015- 04 - 27 00:33:42', _c1='24485', _c2='Campaign038')]

```
Đổi tên cột
```
Ta có thể dễ dàng thay đổi tên cột bằng withColumnRenamed. Tuy nhiên, về cơ bản
thì DataFrame là bát biến (immutable) nên khi thay đổi thì 1 DataFrame mới sễ
đượ c tạo ra.

new_df = df_data.withColumnRenamed("_c0", "access_time")\
.withColumnRenamed("_c1", "userID")\
.withColumnRenamed("_c2", "campaignID")
print(new_df.printSchema())
Output:

root
|-- access_time: string (nullable = true)
|-- userID: string (nullable = true)
|-- campaignID: string (nullable = true)

None

```
Query bằng SQL
```
Bằng cách sử dụng registerTempTable, ta sẽ có một tablê được tham chiếu đến
Dataframê đó, ta có thể sử dụng tên tablê này để viết quêry SQL. Nếu ta sử dụng
sqlContext.sql('query SQL') thì giá trị trả về cũng là Dataframê.

Có 1 lưu ý là: Ta cũng có thể viết subquêry nhưng subquêry cần được gán Alias, nếu
không sễ bị (Syntax error).

Ta thử tìm các dồng cố cô ̣t campaignID cố giá trị là Campaign


#SQL query

new_df.registerTempTable("whole_log_table")

# Query
print (sqlContext.sql(" SELECT * FROM whole_log_table where campaignID ==
'Campaign047' ").count())
Output:

18081

Ta in thử 5 dồng đàu trong đố

print(sqlContext.sql(" SELECT * FROM whole_log_table where campaignID == '
Campaign047' ").show( 5 ))
Output:

+-------------------+------+-----------+
| access_time|userID| campaignID|
+-------------------+------+-----------+
|2015- 04 - 27 05:26:14| 14151|Campaign047|
|2015- 04 - 27 05:26:32| 14151|Campaign047|
|2015- 04 - 27 05:26:34| 14151|Campaign047|
|2015- 04 - 2 7 05:27:47| 14151|Campaign047|
|2015- 04 - 27 05:28:16| 14151|Campaign047|
+-------------------+------+-----------+
only showing top 5 rows

Ta cũng cố thể query linh đô ̣ng hơn

#Thêm biến số vào trong câu SQL
for count in range( 1 , 3 ):
print("Campaign00" + str(count))
print(sqlContext.sql("SELECT count(*) as access_num FROM whole_log_tab
le where campaignID == 'Campaign00" + str(count) + "'").show())
Output:

Campaign
+----------+
|access_num|
+----------+
| 2407|
+----------+


None
Campaign00 2
+----------+
|access_num|
+----------+
| 1674|
+----------+

none

Đối với trường hợ p subquery

#Trường hợp Sub Query：
print (sqlContext.sql("SELECT count(*) as first_count FROM (SELECT userID,
min(access_time) as first_access_date FROM whole_log_table GROUP BY userI
D) subquery_alias WHERE first_access_date < '2015- 04 - 28'").show( 5 ))
Output:

+-----------+
|first_count|
+-----------+
| 20480|
+-----------+

None

**Tìm kiếm sử dụng filter, select**
Đối với DataFramê , tìm kiếm kèm điều kiện rất đơn giản. Giống với câu quêry
ở trên nhưng filter, select dễ dàng hơn rất nhiều. Vậy filter và select khác nhau
thế nàô?

Cùng là để tìm kiếm nhưng filter trả về những rôw thôả mãn điều kiện, trông
đó select lấy dữ liệu thêô côlumn.

Ví dụ Filer

#Ví dụ filter
print(new_df.filter(new_df["access_time"] > "2015- 05 - 01").show( 3 ))
Output:

+-------------------+-------+-----------+
| access_time| userID| campaignID|
+-------------------+-------+-----------+
| click.at|user.id|campaign.id|
|2015- 05 - 01 22:11:57| 114157|Campaign002|
|2015- 05 - 01 23:36:25| 93708|Campaign055|
+-------------------+-------+-----------+


only showing top 3 rows

None

Ví dụ vớ i select

#Ví dụ select
print(whole_log_df.select("access_time", "userID").show( 3 ))
Output:

+-------------------+-------+
| access_time| userID|
+-------------------+-------+
| click.at|user.id|
|2015- 04 - 27 20:40:40| 144012|
|2015- 04 - 27 00:27:55| 24485|
+-------------------+-------+
only showing top 3 rows

None

## Tài liệu tham khảo

```
Spark Properties:
```
[1] Spark Configuration

```
Spark RDD:
```
[ 2 ] Apache Spark Fundamentals - Phần 2: Spark Core và RDD

[ 3 ] Spark RDD – Introduction, Features & Operations of RDD

[ 4 ] Apache Spark RDD

_Spark DataFrame:_

[ 5 ] Xử lý dữ liê ̣u vớ i Spark DataFrame

[ 6 ] Spark SQL - DataFrame


