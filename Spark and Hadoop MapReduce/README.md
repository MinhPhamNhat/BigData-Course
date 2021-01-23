# Spark và Hadoop MapReduce

Apache Hadoop và Apache Spark đều là các Big Data Framework - chúng cung cấp một số công cụ phổ biến nhất được sử dụng để thực hiện các tác vụ phổ biến liên quan đến Big Data. Vậy các nền tảng này khác nhau ở đâu? Hãy cùng so sánh với một số thông số cơ bản:

<img src='https://www.scnsoft.com/blog-pictures/business-intelligence/spark-vs-hadoop.png'>

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
