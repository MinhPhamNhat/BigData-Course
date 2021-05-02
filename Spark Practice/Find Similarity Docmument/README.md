# Tổng quan – Finding similar documents (Tìm văn bản tương tự)
<p>Vấn đề cần giải  quyết: Lọc các bản tin tương tự từ tập hợp các bản tin trên các web tin tức, sau đó khi đưa một bản tin vào thì sẽ hiển thị các bản tin tương tự.</p>
Các bước chính khi thực hiện thuật toán LSH:
1.	Shingling
2.	Min hashing
3.	Locality-sensitive hashing

![6](https://user-images.githubusercontent.com/73160254/116810145-2de91600-ab6c-11eb-998c-951d2c4eb3ca.png)

# Shingling

Mỗi tài liệu sẽ được chuyển đổi thành một tập hợp các ký tự có độ dài k (còn được gọi là k-shingles hoặc k-gram). Và mỗi tài liệu sẽ được thể hiện dưới dạng k-singles

![6](https://user-images.githubusercontent.com/73160254/116810152-3f322280-ab6c-11eb-8204-1469904e309a.png)

Ví dụ: Giả sử tài liệu D là chuỗi {abcde}, và nếu chọn k = 2. Khi đó 2-shingles cho D là {ab, bc, cd, de}. Còn k = 3 thì 3-shingles cho D là {abc, bcd, cde}.
-	Các tài liệu tương tự có nhiều khả năng có nhiều shingles hơn
-	Việc sắp xếp lại các đoạn văn trong một tài liệu thay đổi từ ngữ thì không ảnh hưởng nhiều đến shingles
-	Giá trị k thường được sử dụng trong thực tế là từ 8 -> 10. Lưu ý: một giá trị nhỏ sẽ dẫn đến nhiều singles thường xuyên xuất hiện rất nhiều trong hầu hết các tài liệu (ảnh hưởng tới việc phân biệt tài liệu).

##	Jaccard similarity

Sau khi có một tập các tài liệu dưới dạng shingles, chúng ta sẽ cần có một thước đo độ tương đồng giữa các tài liệu cho nên ta sẽ chọn Jaccard.

![6](https://user-images.githubusercontent.com/73160254/116810166-5c66f100-ab6c-11eb-91e4-106fadc09e57.png)

- Sim(D1, D2): biểu thị sự giống nhau về Jaccard của D1 và D2.

Ví dụ: Trong hình bên dưới, có 2 tập S và T. Trong đó có 3 phần tử là giao điểm giữa 2 tập và có tổng cộng 8 phần tử trong cả 2 tập. Do đó: Sim(S, T) = 3/8.

![6](https://user-images.githubusercontent.com/73160254/116810182-756fa200-ab6c-11eb-9598-c8bcf65f6d82.png)

Tương tự: nếu ta có 2 tập A: {abcde} và B: {bcade}, với 2-shingles thì: 

<br>
A: {ab, bc, cd, de} và B: {bc, ca, ad, de}.

<br>
Vì có giao điểm là {bc, de} và 6 phần tử trong cả 2 tập nên Sim(A, B) = 2/6.

# Min-hashing

Nếu sử dụng với các dữ liệu lớn cần rất nhiều thời gian và không gian rất lớn. Do đó chúng ta cần sử dụng hàm băm.
<br>
Tìm một hàm băm h (·) sao cho:
- nếu Sim(C1, C2) là cao, thì với xác suất cao. h(C1) = h(C2)
- nếu sim(C1, C2) thấp, thì với xác suất cao. h(C1) ≠ h(C2)
<br>

> Hàm băm phụ thuộc vào chỉ số tương tự.
> Hàm băm thích hợp với Jaccard similarity là Min-hashing. 

Ví dụ: 

![6](https://user-images.githubusercontent.com/73160254/116810213-b962a700-ab6c-11eb-8426-83da7580d48a.png)

Các bước trong Min-hashing:
-	**Bước 1**: Random hoán vị π (Permutation π) của chỉ số hàng của ma trận tài liệu shingles (Shingles x Documents).
-	**Bước 2**: Hàm băm chỉ số (Signature matrix M)  của hàng đầu tiên (theo thứ tự hoán vị) trong cột C có giá trị 1. <br> Ví dụ: cột màu tím. Dò trong (Input matrix) cột đầu tiên trong những giá trị trong (Permutation π) đối ứng  với C có giá trị 1 thì có thể thấy 2 là giá trị nhỏ nhất cho nên được chọn, làm tương tự với các cột tiếp theo trong (Input matrix) ta sẽ có được hàng đầu tiên như trong hình trên.

Sau đó làm điều này nhiều lần (với hoán vị khác nhau) để hoàn thành giống trong hình.

## Min-hash Property
Sự giống nhau về Signature là phần nhỏ của hàm Min-hash mà chúng được ghép với nhau. Vì vậy, sự giống nhau về Signature của C1 và C3 là 2/3 vì hàng thứ nhất và hàng thứ ba là như nhau

![6](https://user-images.githubusercontent.com/73160254/116810726-9b4a7600-ab6f-11eb-913c-c39e6ae37732.png)

> Độ giống nhau mong đợi của 2 signature thì bằng độ tương tự của Jaccard của các cột. Nếu signature càng dài thì lỗi càng thấp.

![6](https://user-images.githubusercontent.com/73160254/116810745-b321fa00-ab6f-11eb-9228-6c23d10ff5e8.png)

Bên trên là ví dụ, bởi vì độ dài signature chỉ bằng 3 cho nên có sự khác biệt. Nhưng nếu tăng độ dài của signature thì sẽ có sự tương đồng lớn hơn.

<br>
Vì vậy cho thấy sử dụng Min-hashing giúp giải quyết được vấn đề không gian và thời gian bằng cách loại bỏ sự thưa thớt và đồng thời cũng bảo đảm độ tương tự.

# Locality Sensitive Hashing

Tìm tài liệu có độ tương tự Jaccard ít nhất s (đối với một số ngưỡng tương tự, s = 0.8). Và dùng hàm băm để xem liệu x và y có phải là một cặp candidate hay không.
-	Chia Signature matrix M thành b bands (dải), mỗi band có r hàng.
-	Các cặp cột Candidate (ứng cử viên) là những cặp cột được băm vào cùng một nhóm có ít nhất 1 dải.
-	Đối với mỗi dải, băm phần của mỗi cột thành một bảng băm với k nhóm. Nhưng sẽ có vấn đề nếu k quá lớn. Ví dụ: nếu 1 band có 5 hàng và các phần tử trong signature là số nguyên 32 bit thì k trong trường hợp này sẽ là (232)5. Dẫn đến k rất lớn.
-	Vì vậy nếu 2 tài liệu giống nhau thì chúng sẽ xuất hiện như một cặp candidate trong ít nhất một trong các nhóm.

![6](https://user-images.githubusercontent.com/73160254/116810770-d482e600-ab6f-11eb-827c-c45412de6682.png)

## Lựa chọn b & r

Nếu chúng ta lấy b lớn thì tức là số lượng hàm băm nhiều hơn, thì chúng ta phải giảm r vì b*r (số hàng trong signature matrix). Thì sẽ giúp tăng xác suất để tìm thầy một candidate, tương đương với việc lấy một ngưỡng tương tự (s) nhỏ.

<br>
_Giả sử_: signature matrix có 100 hàng. Ta nên chọn b = 20 và r = 5 sẽ hiệu quả hơn là chọn b = 10 và r = 10. Bởi vì nếu b = 20 thì sẽ có cơ hội cao hơn để 2 tài liệu xuất hiện trong cùng một nhóm ít nhất một lần vì chúng có nhiều cơ hội hơn ( 20 > 10) và ít yếu tố của signature được so sánh hơn vì (5 < 10).

<br>
Ví dụ: Ta có:
- 100k tài liệu được lưu trữ dưới dạng chữ ký có độ dài 100.
- Signature matrix: 100 * 100000
- Lấy b = 20 và r = 5

_**Lấy ngưỡng tương tự (s): 80%**_

Ta sẽ có 2 bộ tài liệu A và B có độ giống nhau 80% được băm trong cùng một nhóm cho ít nhất một trong 20 dải thì:
-	P (A và B giống hệt nhau trong cùng một dải cụ thể) = (0,8)5 = 0,328
-	P (A và B không giống nhau trong tất cả các dải) = (1 – 0,328)20 = 0,00035
Ta sẽ tìm thấy 99,965% cặp tài liệu thực sự giống nhau.
<br>
Ngoài ra, ta sẽ có 2 bộ tài liệu C và D có độ giống nhau 30% không được băm trong cùng một nhóm cho bất kỳ dải nào trong số 20 dải.
-	P (C và D giống hệt nhau trong cùng một dải cụ thể) = (0,3)5 = 0,00243
-	P (C và D tương tự nhau ở ít nhất 1 trong 20 dải) = 1 - (1 – 0,00243)20 = 0,0474
Sẽ có khoảng 4,74% cặp tài liệu với sự giống nhau là 30% cuối cùng trở thành cặp candidate.

<br>
<p>Vì vậy, chúng ta có thể thấy rằng chúng ta có một số dương tính giả và một số âm tính giả. Tỷ lệ này sẽ thay đổi theo sự lựa chọn b và r. (Nếu chúng ta chỉ có 10 dải trong 10 hàng, số lượng false positives sẽ giảm xuống, nhưng số lượng false negatives sẽ tăng lên).</p> 


