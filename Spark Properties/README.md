
# Spark Properties
Spark properties kiểm soát hầu hết các cài đặt ứng dụng và được cấu hình riêng 
cho từng ứng dụng. Các thuộc tính này có thể được đặt trực tiếp trên SparkConf và
truyền tới SparkContext. SparkConf cho phép cấu hình một số thuộc tính phổ biến 
(ví dụ: URL và tên ứng dụng), cũng như các cặp khóa-giá trị tùy ý thông qua phương 
thức set (). Ví dụ: chúng ta có thể khởi tạo một ứng dụng có hai luồng như sau:
```
val conf = new SparkConf()
 .setMaster("local[2]")
 .setAppName("Spark Practice")
val sc = new SparkContext(conf)
```
