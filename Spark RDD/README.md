# RDD là gì
**_RDD_**: Resilient Distributed Dataset là một cáu trúc cơ bản trong Apache Spark. RDD là đại diện cho tập dữ liệu phân tán, tức là RDD tập hợp các dữ liệu được phân tán đã xử lý qua các node. Ta hãy phân tích RDD: 
-	**_Resilient_**: là khả năng chịu lỗi bằng sự hỗ trợ từ các RDD lineage graph và cố thể tính toán lại các RDD partitions  khi node chứa RDD partitions đố bị lỗi. 
-	**_Distributed_**: Thể hiện việc các dữ liệu được phân tán trên các node. 
- **_Dataset_**: Là tạp dữ liêu được sử dụng. 
Do đó, mỗi và mọi tập dữ liệu trông RDD được phân vùng một cách hợp lý trên nhiều máy chủ để chúng có thể được tính toán trên các node khác nhau của cụm. RDD có khả năng chịu lỗi, tức là nó có khả năng tự phục hồi trong trường hợp bị lỗi. 

![1](https://user-images.githubusercontent.com/73160254/116808952-e65f8b80-ab65-11eb-8d9d-d62065b3a04e.jpg)

Có ba cách để tạo RDD trong Spark, chẳng hạn như - Dữ liệu bộ lưu trữ ổn định (Data in stable storage), các RDD khác và song song hóa collection đã tồn tại trong chương trình trình điều khiển.  Người ta cũng có thể vận hành Spark RDD song song với một API cấp thấp cung cấp các chuyển đổi (transformings) và hành động 
(actions) 

![2](https://user-images.githubusercontent.com/73160254/116808993-2aeb2700-ab66-11eb-9f29-1cf0afe93ea9.jpg)

Đặc điểm quan trọng của 1 RDD là số partitions. Một RDD bao gồm nhiều partition nhỏ, mỗi partition này đại diện cho 1 phần dữ liệu phân tán. Khái niệm partition là logical, tức là 1 node xử lý có thể chứa nhiều hơn 1 RDD partition. Theo mặc định, dữ liệu các partitions sẽ lưu trên memory. Thử tưởng tượng ta cần xử lý 1TB dữ liệu, nếu lưu hết trên mem tính ra thì cung khá tốn kém. Tất nhiên nếu ta có 1TB ram để xử lý thì tốt quá nhưng điều đó không cần thiết. Với việc chia nhỏ dữ liệu thành các partition và cơ chế lazy evaluation của Spark ta có thể chỉ cần vài chục GB ram và 1 chương trình được thiết kế tốt để xử lý 1TB dữ liệu, chỉ là sẽ chậm hơn có nhiều RAM thôi 

# Tại sao chúng ta cần phải sử dụng RDD ? 
Các nhu cầu chính khi sử dụng RDD đó là : 
-	Các thuật toán bị lặp lại (Iterative algorithms) 
-	Công cụ khai tác dữ liệu tương tác (Interactive data mining tools) 
-	DSM (Distributed Shared Memory) có khả năng chịu lỗi trên cột cụm và triển khai các công việc kém. 
-	Trong hệ điện toán phân tán, dữ liệu sẽ được lưu trữ trong hệ thống ổn định như HDFS hay Amazon S3. Việc này khiến cho công việc tính toán chậm hơn vì liên quan nhiều đến các khả năng I/O, sao chép và tuần tự hoá trong các tiến trình.
<br>
Trong hai trường hợp đầu tiên thì RDD lưu dữ liệu trong bộ nhớ và việc này giúp cải thiệu hiệu suất theo cấp độ magnitude.
Thách thức trong việc thiết kế RDD đó là việc tạo ra một API cung cấp khả năng chịu lỗi hiệu quả. Để làm được điều này, RDDs đã cung cấp một dạng bộ nhớ chung [shared memory] hạn chế dựa vào **coarse-grained transformation** thay vì **fine-grained updates** cho các trạng thái chung. 
Spark làm rõ RDD thông qua API tích hợp ngôn ngữ. Khi đó, mỗi tập dữ liệu sẽ được biểu diễn dưới dạng một object và quá trình chuyển đổi RDD có liên quan đến việc sử dụng các phương thức của object này.
Apache Spark đánh giá RDD một cách lười biếng (lazy). RDD chỉ được gọi khi cần thiết, việc này giúp tiết kiệm thời gian và nâng cao hiệu quả.

# Các tính năng của Spark RDD
<div style="text-align:center; width:100%"><img src="https://user-images.githubusercontent.com/73160254/116809225-2ecb7900-ab67-11eb-85db-3844af917b14.jpg" /></div>

## Tính toán trên RAM (In-Memory Computation)
RDDs lưu trữ các kết quả trung gian trên bộ nhớ phân tán (RAM) thay bộ nhớ ổn định [stable storage] (Disk).

## Đánh giá lười biếng (Lazy Evaluations)
Các phép biến đổi (transformations) trong Spark đều lười ở chỗ chúng không tính ngay kết quả mà thay vào đó, chúng chỉ nhớ các phép biến đổi được áp dụng trên các tập dữ liệu. Spark chỉ các phép biến đổi đó khi một hành động(actions) được yêu cầu.

## Khả năng bất biến (Immutablility)
Dữ liệu an toàn để chia sẻ trên các tiến trình.  Nó cũng có thể được tạo hoặc truy xuất bất cứ lúc nào giúp dễ dàng lưu vào bộ nhớ đệm, chia sẻ và nhân rộng.  Do đó, nó là một cách để đạt được sự nhất quán trong tính toán.

## Khả năng chịu lỗi (Fault Tolerance)
Spark RDD có khả năng chịu lỗi nhờ vào sự theo dõi thông tin dòng dữ liệu để tự động xây dựng lại dữ liệu bị mất khi bị lỗi.  Để làm được điều này, mỗi RDD nhớ cách nó được tạo từ các tập dữ liệu khác (bằng các phép biến đổi như map, join hoặc groupBy) để tạo lại chính nó.

## Sự bền bỉ (Persistence)
Người dùng có thể cho biết họ sẽ sử dụng lại những RDD nào và tự thiết lập khả năng lưu trữ cho họ (ví dụ: lưu trữ trong bộ nhớ hoặc trên Đĩa)

## Phân vùng (Partitioning)
Phân vùng là đơn vị cơ bản của tính song song trong Spark RDD.  Mỗi phân vùng là một phân chia dữ liệu hợp lý có thể thay đổi được.  Người ta có thể tạo một phân vùng thông qua một số biến đổi trên các phân vùng hiện có.

## Location-Stickness
RDD có khả năng xác định ưu tiên vị trí để tính toán các partition.  Tùy chọn vị trí đề cập đến thông tin về vị trí của RDD.  DAGScheduler đặt các phân vùng theo cách sao cho tác vụ gần với dữ liệu nhất có thể.

## Coarse-gained Operation
Coarse-gained Operation áp dụng cho tất cả các phần tử trong bộ dữ liệu thông qua map hoặc filter hoặc nhóm theo các phép toán.

#	Các phép toán trong Spark RDD
RDD hỗ trợ 2 phép toán là :
- Tranformations
- Actions
Transformation và Action hoạt động giống như DataFrame lẫn DataSets. Transformation xử lý các thao tác lazily và Action xử lý thao tác cần xử lý tức thời
![4](https://user-images.githubusercontent.com/73160254/116809351-c8932600-ab67-11eb-97db-c51a7ba1cdbc.png)

# Transformations
Spark RDD Tranformations là một hàm nhận vào một RDD và trả về một hay nhiều RDD khác. RDD ban đầu sẽ không bị thay đổi vì RDD mang tính bất biến (Immutability) mà nó sẽ sinh ra các RDD mới bằng các áp dụng các phương thức như _map(), filter(), reduceByKey()_, …
Nhiều phiên bản Tranformations của RDD có thể hoạt động trên các Structured API, Tranformations xử lý lazily, tức là chỉ giúp dựng execution plans, dữ liệu chỉ được truy xuất thực sự khi thực hiện Actions
Transformations gồm các phương thức như:
-	**distinct**: loại bỏ trùng lắp trong RDD
-	**filter**: tương đương với việc sử dụng where trong SQL – tìm các record trong RDD xem những phần tử nào thỏa điều kiện. Có thể cung cấp một hàm phức tạp sử dụng để filter các record cần thiết – Như trong Python, ta có thể sử dụng hàm lambda để truyền vào filter
-	**map**: thực hiện một công việc nào đó trên toàn bộ RDD. Trong Python sử dụng lambda với từng phần tử để truyền vào map
-	**flatMap**: cung cấp một hàm đơn giản hơn hàm map. Yêu cầu output của map phải là một structure có thể lặp và mở rộng được.
-	**sortBy**: mô tả một hàm để trích xuất dữ liệu từ các object của RDD và thực hiện sort được từ đó.
-	**randomSplit**: nhận một mảng trọng số và tạo một random seed, tách các RDD thành một mảng các RDD có số lượng chia theo trọng số.

#	Actions
Actions trả về kết quả cuối cùng qua các tính toán RDD.  Nó kích hoạt thực thi bằng cách sử dụng đồ thị tuyến tính (lineage graph) để load dữ liệu vào RDD gốc, thực hiện tất cả các phép biến đổi trung gian và trả về kết quả cuối cùng cho Driver để xử lý hoặc ghi dữ liệu xuống các công cụ lưu trữ
Actions gồm các phương thức như:
-	**reduce**: thực hiện hàm reduce trên RDD để thu về 1 giá trị duy nhất
-	**count**: đếm số dòng trong RDD
-	**countApprox**: phiên bản đếm xấp xỉ của count, nhưng phải cung cấp timeout vì có thể không nhận được kết quả.
-	**countByValue**: đếm số giá trị của RDD. Phương thức chỉ sử dụng nếu map kết quả nhỏ vì tất cả dữ liệu sẽ được load lên memory của driver để tính toán và ta chỉ nên sử dụng trong tình huống số dòng nhỏ và lượng item khác nhau cũng nhỏ.
-	**first**: lấy giá trị đầu tiên của dataset
-	**max** và min: lấy lần lượcgiá trị lớn nhấy và nhỏ nhất của dataset
-	**take** và các method tương tự: lấy một lượng giá trị từ trong RDD. take sẽ scan qua một partition trước và sử dụng kết quả để dự đoán số lượng partition cần phải lấy thể để thoả số lượng.

#	Giới hạn của Spark RDD
![5](https://user-images.githubusercontent.com/73160254/116809421-258edc00-ab68-11eb-840c-0d1835899ede.jpg)

##	Không có công cụ tối ưu sẵn
Khi làm việc với cấu trúc dữ liệu, RDD không thể phát huy tối đa lợi thế từ bộ tối ưu của Spark như catalyst optimizer and Tungsten execution engine

##	Việc xử lý cấu trúc dữ liệu
Không giống như Dataframe và datasets, RDD không suy ra lược đồ của dữ liệu đã nhập và yêu cầu người dùng phải chỉ định nó.

##	Giới hạn hiệu suất
Là các đối tượng JVM trong bộ nhớ, RDD liên quan đến chi phí Thu gom rác và Tuần tự hóa Java, những thứ này rất tốn kém khi dữ liệu phát triển.

## Giới hạn lưu trữ
RDDs suy giảm khi không có đủ bộ nhớ để lưu trữ chúng.  Người ta cũng có thể lưu trữ partition của RDD đó trên đĩa do không đủ với RAM.  Do đó, nó sẽ cung cấp hiệu suất tương tự như các hệ thống song song dữ liệu.

