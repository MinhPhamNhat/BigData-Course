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
MapReduce được thiết kế bởi Google như 1 mô hình lập trình xử lý tập dữ liệu lớn song song, thuật toán được phân tán trên 1 cụm. Mặc dù, MapReduce ban đầu là công nghệ độc quyền của Google, nó đã trở thành thuật ngữ tổng quát hóa trong thời gian gần đây.

MapReduce gồm các thủ tục: 1 Map() và 1 Reduce(). Thủ tục Map() lọc (filter) và phân loại (sort) trên dữ liệu trong khi thủ tục Reduce() thực hiện tổng hợp dữ liệu. Mô hình này dựa tre7m các khái niệm biến đổi của bản đồ và reduce các chức năng trong lập trình hướng chức năng. Thư viện thủ tục Map() và Reduce() được viết bằng nhiều ngôn ngữ. Cài đặt miễn phí, phổ biến nhất của MapReduce là Apache Hadoop.

<img src="https://expressmagazine.net/sites/default/files/imagesArticle/mapreduce_work_structure.png">
