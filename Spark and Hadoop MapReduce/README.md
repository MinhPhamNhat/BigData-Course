# Spark và Hadoop MapReduce

Apache Hadoop và Apache Spark đều là các Big Data Framework - chúng cung cấp một số công cụ phổ biến nhất được sử dụng để thực hiện các tác vụ phổ biến liên quan đến Big Data. Vậy các nền tảng này khác nhau ở đâu? Hãy cùng so sánh với một số thông số cơ bản:

<img src='https://www.scnsoft.com/blog-pictures/business-intelligence/spark-vs-hadoop.png'>

Các mục:
* Apache Spark
* Apache Hadoop
* MapReduce
* Kiến trúc
* Hiệu suất
* Bảo mật

## Apache Spark
Apache Spark là một cụm framework điện toán nhanh, được thiết kế để tính toán nhanh trong xử lý dữ liệu quy mô lớn. Apache Spark là một công cụ xử lý phân tán (distributed processing engine) nhưng nó không đi kèm với trình quản lý tài nguyên cụm (inbuilt cluster resource manager) và hệ thống lưu trữ phân tán sẵn có (distributed storage system) mà phải cắm vào một trình quản lý cụm và hệ thống lưu trữ.

Apache Spark bao gồm Spark Core và Bộ thư viện. Spark core thực thi và quản lý công việc bằng cách cung cấp trải nghiệm liền mạch cho người dùng. Người dùng phải gửi công việc tới Spark core và Spark core đảm nhiệm việc xử lý, thực thi và trả lời lại cho người dùng qua API Spark Core bằng các ngôn ngữ lập trình khác nhau như Scala, Python, Java và R.

Apache Spark là công cụ xử lý dữ liệu cho các chế độ hàng loạt và phát trực tuyến có các truy vấn SQL, Xử lý đồ thị (Graph Processing) và Machine Learning.

## Apache Hadoop
Hadoop là một Apache framework mã nguồn mở cho phép phát triển các ứng dụng phân tán (distributed processing) để lưu trữ và quản lý các tập dữ liệu lớn. Hadoop hiện thực mô hình MapReduce, mô hình mà ứng dụng sẽ được chia nhỏ ra thành nhiều phân đoạn khác nhau được chạy song song trên nhiều node khác nhau. Hadoop được viết bằng Java tuy nhiên vẫn hỗ trợ C++, Python, Perl bằng cơ chế streaming.

<img src="https://topdev.vn/blog/wp-content/uploads/2019/06/hadoop_architecture.jpg">

## MapReduce
MapReduce bắt nguồn từ Google. Các bạn có thể tham khảo mô hình đơn giản của MapReduce qua bài báo “MapReduce: Simplified Data Processing on Large Clusters”.

MapReduce được chia thành hàm là Map và Reduce. Những hàm này được định nghĩa bởi người dùng và là hai giai đoạn liên tiếp trong quá trình xử lý dữ liệu.

+ Map nhận input là tập các cặp khóa/giá trị và output là tập các cặp khóa/giá trị trung gian và ghi xuống đĩa cứng và thông báo cho Reduce nhận dữ liệu đọc.

+ Reduce sẽ nhận khóa trung gian I và tập các giá trị ứng với khóa đó, ghép nối chúng lại để tạo thành một tập khóa nhỏ hơn. Các cặp khóa/giá trị trung gian sẽ  được đưa vào cho hàm reduce thông qua một con trỏ vị trí (iterator). Điều này cho phép ta có thể quản lý một lượng lớn danh sách các giá trị để phù hợp với bộ nhớ.

Thực chất giữa bước map và reduce còn có một bước phụ mà bước này thực hiện song song với bước reduce đó là shuffle. Tức là sau khi map thực hiện xong toàn bộ công việc của mình,  output của map được đặt rải rác trên các cluster khác nhau nên shuffle sẽ làm nhiệm vụ thu thập các cặp khóa-giá trị trung gian do map sinh ra mà có cùng khóa để chuyển qua cho reduce thực hiện tiếp công việc của mình.

<img src="https://expressmagazine.net/sites/default/files/imagesArticle/mapreduce_work_structure.png">

## Kiến trúc
### Hadoop
Để bắt đầu, tất cả các tệp được truyền vào HDFS được chia thành các khối. Mỗi khối được sao chép một số lần xác định trên toàn cụm dựa trên kích thước khối và hệ số sao chép được định cấu hình. Thông tin đó được truyền đến NameNode, theo dõi mọi thứ trên toàn cụm. NameNode gán các tệp cho một số nút dữ liệu mà sau đó chúng được ghi. Tính sẵn sàng cao đã được  triển khai vào năm 2012 , cho phép NameNode chuyển đổi dự phòng sang Node dự phòng để theo dõi tất cả các tệp trên một cụm.

Thuật toán MapReduce nằm trên HDFS và bao gồm một JobTracker. Khi một ứng dụng được viết bằng một trong các ngôn ngữ, Hadoop chấp nhận Trình theo dõi công việc, chọn nó và phân bổ công việc (có thể bao gồm mọi thứ từ đếm từ và làm sạch tệp nhật ký, để chạy truy vấn HiveQL trên đầu dữ liệu được lưu trữ trong kho Hive ) để TaskTrackers lắng nghe trên các nút khác.

YARN phân bổ các tài nguyên mà JobTracker tạo ra và giám sát chúng, di chuyển các quy trình xung quanh để có hiệu quả cao hơn. Tất cả các kết quả từ giai đoạn MapReduce sau đó được tổng hợp và ghi lại vào đĩa trong HDFS.

### Spark
Spark hoạt động theo cách tương tự như Hadoop, ngoại trừ việc tính toán được thực hiện trong bộ nhớ và được lưu trữ ở đó cho đến khi người dùng chủ động duy trì chúng. Ban đầu, Spark đọc từ một tệp trên HDFS, S3 hoặc một filestore khác, thành một cơ chế được thiết lập có tên là SparkContext. Tại SparkContext, Spark tạo ra một cấu trúc gọi là RDD hoặc   Resilient Distributed.

Khi RDD và các hành động liên quan đang được tạo, Spark cũng tạo ra một DAG, hoặc đồ thị theo chu kỳ có hướng, để trực quan hóa thứ tự các hoạt động và mối quan hệ giữa các hoạt động trong DAG. Mỗi DAG có các giai đoạn và các bước; theo cách này, nó tương tự như một kế hoạch giải thích trong SQL.  

Ta có thể thực hiện các phép biến đổi, các bước trung gian, hành động hoặc các bước cuối cùng trên RDD. Kết quả của một chuyển đổi đã cho đi vào DAG nhưng không tồn tại trên đĩa, nhưng kết quả của một hành động vẫn tồn tại tất cả dữ liệu trong bộ nhớ vào đĩa.

Một bản tóm tắt mới trong Spark là DataFrames, được phát triển trong Spark 2.0 như một giao diện đồng hành với RDD. Hai cái này cực kỳ giống nhau, nhưng DataFrames sắp xếp dữ liệu thành các cột được đặt tên, tương tự như các gói gấu trúc hoặc R của Python. Điều này làm cho chúng thân thiện với người dùng hơn RDD, vốn không có bộ tham chiếu tiêu đề cấp cột tương tự. SparkQuery cũng cho phép người dùng truy vấn DataFrames giống như các bảng SQL trong các kho dữ liệu quan hệ.  

## Hiệu suất
Về tốc độ xử lý thì Spark nhanh hơn Hadoop. Spark được cho là nhanh hơn Hadoop gấp 100 lần khi chạy trên RAM, và gấp 10 lần khi chạy trên ổ cứng. Hơn nữa, người ta cho rằng Spark sắp xếp (sort) 100TB dữ liệu nhanh gấp 3 lần Hadoop trong khi sử dụng ít hơn 10 lần số lượng hệ thống máy tính.

<img src="https://images.viblo.asia/de17071c-f13c-41c9-80ad-b39401d16cc2.jpg">

Sở dĩ Spark nhanh là vì nó xử lý mọi thứ ở RAM. Nhờ xử lý ở bộ nhớ nên Spark cung cấp các phân tích dữ liệu thời gian thực cho các chiến dịch quảng cáo, machine learning (học máy), hay các trang web mạng xã hội.

Tuy nhiên, khi Spark làm việc cùng các dịch vụ chia sẻ khác chạy trên YARN thì hiệu năng có thể giảm xuống. Điều đó có thể dẫn đến rò rỉ bộ nhớ trên RAM. Hadoop thì khác, nó dễ dàng xử lý vấn đề này. Nếu người dùng có khuynh hướng xử lý hàng loạt (batch process) thì Hadoop lại hiệu quả hơn Spark.

Tóm lại ở yếu tố hiệu năng, Spark và Hadoop có cách xử lý dữ liệu khác nhau. Việc lựa chọn framework nào phụ thuộc yêu cầu cụ thể từng dự án.

## Bảo mật
Bảo mật của Spark đang được phát triển, hiện tại nó chỉ hỗ trợ xác thực mật khẩu (password authentication). Ngay cả trang web chính thức của Apache Spark cũng tuyên bố rằng, "Có rất nhiều loại mối quan tâm bảo mật khác nhau. Spark không nhất thiết phải bảo vệ chống lại tất cả mọi thứ".

Mặt khác, Hadoop trang bị toàn bộ các mức độ bảo mật như Hadoop Authentication, Hadoop Authorization, Hadoop Auditing, and Hadoop Encryption. Tất cả các tính năng này liên kết với các dự án Hadoop bảo mật như Knox Gateway và Sentry.

Vậy là ở mặt bảo mật thì Spark kém bảo mật hơn Hadoop. Nếu có thể tích hợp Spark với Hadoop thì Spark có thể "mượn" các tính năng bảo mật của Hadoop.

# Reference
[1] <a href="https://viblo.asia/p/hadoop-va-spark-big-data-framework-nao-tot-nhat-cho-ban-4dbZNqRqKYM">Hadoop và Spark Big data framework nào tốt nhất cho bạn</a>
</br>
[2] <a href="https://cloudfun.vn/threads/phan-biet-apache-hadoop-va-apache-spark.94/">Phân biệt Apache Hadoop và Apache Spark</a>
</br>
[3] <a hrè="https://helpex.vn/article/hadoop-so-voi-spark-so-sanh-truc-tiep-5c6b1c03ae03f628d053bf1a">Hadoop so với Spark: So sánh trực tiếp</a>
