
# THIẾT KẾ MÃ GIẢ CHI TIẾT - LẬP TRÌNH FOG COMPUTING

## 1. Input (thông tin đầu vào)
- Số lượng quần thể (được duy trì) : NUY
- Số lượng con cháu (offspring) (được duy trì) trong mỗi giai đoạn (chọn cha mẹ, lai ghép, đột biến) : LAMDA
- Số lượng Task : N
- Số lượng Fog Node : M
- Số vòng lặp tối đa : MAX_ITERATIONS
- Thời gian tối đa : MAX_TIME
- Sức ép chọn lọc : PS = 2
- Tỷ lệ lai ghép : CROSSOVER_RATE
- Tỷ lệ số 1 trong Template ngẫu nhiên : RATE_OF_ONE
- Tỷ lệ đột biến : MUTATION_RATE
- Bán kính hàng xóm : RC (ban đầu để lớn, sau nhiều thế hệ cho nó giảm dần)

## 2. Tổng quan quá trình Tiến hóa
### 2.1. Khởi tạo Quần thể gồm NUY cá thể, trong đó
- Mỗi cá thể là một mảng : individual[] gồm N phần tử (tức N ô)
  - trong đó, mỗi ô trong mảng được gán một giá trị trong miền từ : 0 -> M-1
- Ý nghĩa của cá thể (mảng indivial) : Cho biết thông tin về 1 lời giải - tức quá trình phân bổ các task cho các Fog Nodes

### 2.2. Đánh giá độ Fitness (của mọi cá thể) trong quần thể
- Mỗi cá thể có độ fitness : fi, trong đó : fi thuộc miền (0,1)


### 2.3. Kiểm tra điều kiện dừng 
- Quá trình tiến hóa kết thúc khi 1 trong các điều kiện sau diễn ra : 
  - Số vòng lặp > MAX_ITERATIONS
  - Thời gian chạy > MAX_TIME
 
## 3. Toán tử Tái sản xuất (Chọn cha mẹ tốt nhất) - Reproduction Operator

### 3.1. Lựa chọn LAMDA cá thể tốt nhất từ quần thể

### 3.2. Lựa chọn ngẫu nhiên, có trùng lặp LAMDA cá thể từ quần thể

### 3.3. Lựa chọn ngẫu nhiên, không trùng lặp LAMDA cá thể từ quần thể

### 3.4. Toán tử Chọn lọc cân xứng (Proportional Selecion)- sử dụng Sức ép chọn lọc
  - Bước 1 : Thực hiện Scaling Fitness (Sàng lọc, Tinh chế lại Fitness) theo Sức ép chọn lọc PS=2
	- Cách 1 : Kỹ thuật Scaling Tuyến tính :
	  - Tính giá trị scaling tuyến tính a:
	    - Tính Fitness trung bình trên toàn quần thể : f_average
		- Tìm Fitness của cá thể tốt nhất trong quần thể : f_best
		- Tính a = (PS*f_average - f_best)/(PS - 1)
	  - Điều chỉnh lại Fitness của toàn bộ cá thể theo công thức : fi' = max(fi - a, 0) (để đảm bảo fitness luôn dương)
	
	- Cách 2 : Kỹ thuật Scaling Lũy thừa 
	  - Điều chỉnh Fitness theo công thức sau : fi' = exp(fi/T) (công thức The Boltzmann selection (De La Maza and Tidor 1993 [De La Maza and Tidor, 1993]), trong đó : T is usually a decreasing function of the number of generations, thus enabling the selection pressure to grow with time (T giống như nhiệt độ giảm dần theo lịch trình của chiến lược Luyện kim vậy, khi T (nhiệt độ) càng nhỏ thì một sự khác nhau rất nhỏ giữa fi và fj sẽ dẫn đến một sự khác biệt rõ rệt giữa fi' và fj')
	
	- Cách 3 : Kỹ thuật Scaling bằng Xếp hạng
	  - Sắp xếp các cá thể theo thứ hạng r từ 1 ... NUY tương ứng theo chiều giảm của fitness trên mỗi cá thể (cá thể tốt nhất có thứ hạng r=1)
	  - Điều chỉnh lại fir' = (1 - (r/NUY))^(PS-1) (trong đó PS là giá trị của sức ép chọn lọc)
	  - Chỉ phù hợp với bài toán khi độ phức tạp tính toán fitness là rất lớn, mà chỉ có thể ước lượng được
	  - Nhược điểm của phương pháp này đó là : Đánh giá không sát về sự khác biệt về fitness giữa các cá thể

  - Bước 2 : Tính lamda_i cho từng cá thể :
    - Công thức : lamda_i = (LAMDA * fi)/(SUM(fi))
	  - Trong đó, lamda_i là một số thực dương, cho biết : Mức độ được nhân bản (nhiều lần) của cá thể i vào tập cha mẹ tốt nhất (offspring)
	- Do lamda_i là một số thực chỉ mức độ được nhân bản, trong khi đó số lần nhân bản của mỗi cá thể vào tập offspring phải là một số nguyên. Nếu sử dụng phương pháp làm tròn thông thường sẽ không dẫn đến sự hiệu quả. Do vậy, người ta sử dụng phương pháp SUS để làm tròn và tính ra số lần nhân bản của mỗi cá thể như sau : 
	  - Chọn random_offset (chọn ngẫu nhiên trong khoảng (0,1) vì 1 là giá trị lamda_i trung bình). Cá thể đầu tiên được chọn nằm tại vị trí random_offset này
	  - Sau đó, chia (SUM(lamda_i) - random_offset) thành (LAMDA-1) khoảng (để chọn ra LAMDA cá thể, ứng với LAMDA mút tạo thành) ==> mỗi điểm mút tương ứng với 1 cá thể được lựa chọn vào tập cha mẹ tốt nhất (chú ý : SUM(lamda_i) thực chất = LAMDA)

## 3. Toán tử Lai ghép - Crossover Operator
  - Bước 1 : Chọn các cặp cha mẹ từ tập cha mẹ tốt nhất tham gia lai ghép (với tỷ lệ lại ghép : CROSSOVER_RATE)
    - Kỹ thuật chọn ngẫu nhiên (Trong mã nguồn đã cài đặt)
	- Kỹ thuật chọn theo hàng xóm đủ gần : 
	  - Định nghĩa Thế nào là hàng xóm? Khi sự khác biệt về gen trên 2 cá thể (ADN) nằm trong phạm vi bán kính RC (Bán kính sẽ giảm dần theo từng thế hệ) ==> Bàn thêm về bán kính RC nên được thay đổi như thế nào ???
	  - Với một cá thể trong tập cha mẹ tốt nhất, chọn ra 1 cá thể khác trong đó nhưng phải là hàng xóm của nó
	  - ? Vấn đề : Nếu không chọn đủ các cặp hàng xóm (so với tỷ lệ lai ghép cho phép) thì sao? ==> Bàn thêm 
  - Bước 2 : Thực hiện lai ghép từ các cặp cha mẹ (parent1, parent2) đã được chọn :
    - Lai ghép 1 điểm cắt
	
	- Lai ghép 2 hoặc nhiều điểm cắt
	
	- Lai ghép sử dụng Template ngẫu nhiên : Tạo ra ngẫu nhiên một template nhị phân (0,1) : 
	  - Có tỷ lệ số 1 trong template là RATE_OF_ONE
	  - Có kích thước bằng với kích thước của cá thể 
	  - Sau đó, ta tiến hành duyệt từng gene trên template, với gen = 1 ta tiến hành hoán đổi cặp gen tương ứng trên cha và mẹ, và ngược lại khi gen = 0 ta không thực hiện gì cả !
	  
	- Lai ghép BLX-ALPHA (Chọn ALPHA = 0.5 - giá trị tác giả khuyên và đề xuất nên sử dụng)
	  - Từ 2 cá thể cha mẹ x=(x1,x2,..xn) và y=(y1,y2,..yn) ==> Tạo ra 2 cá thể con z=(z1,z2,...zn) theo công thức sau:
	    - Công thức : zi = xi − ALPHA*(yi − xi) + (1 + 2*ALPHA)(yi − xi) · U(0, 1)
		- Trong đó U(0,1) là hàm random với Kỳ vọng = 0, Độ lệch chuẩn = 1
		- Làm tròn zi về giá trị nguyên
		- Phương pháp này không làm thay đổi Kỳ vọng, nhưng thay đổi Mức độ đa dạng (độ lệch chuẩn của tập con cháu, cụ thể : ALPHA càng lớn khiến cho tập con cháu sản sinh có xu hướng phân bố đa dạng hơn, ngược lại khi ALPHA càng nhỏ khiến cho tập con cháu có xu hướng hội tụ về 1 điểm trung tâm)
		
	- Lai phép BLX_ALPHA tuyến tính (chọn ALPHA=0.5)
	  - Không thực hiện lai ghép trên từng thành phần mà tiến hành lai ghép trên toàn vector như sau : 
	    - Công thức : z = x − ALPHA*(y − x) + (1 + 2*ALPHA)(y − x) · U(0, 1)
		
4. Toán tử Đột biến - Mutation Operator
  - Kỹ thuật đột biến ngẫu nhiên : Chọn một Gene bất kỳ, rồi gán cho nó một giá trị bất kỳ thuộc miền giá trị của nó.
  
  - Kỹ thuật đột biến Gaussian với luật 1/5 (với tỷ lệ tham gia đột biến là MUTATION_RATE)
    - Tạo một vector ngẫu nhiên r với N thành phần, trong đó mỗi thành phần được sinh ra ngẫu nhiên theo phân bố N(0,XICH_MA) (điều này cho thấy r là một điểm nằm trong khối siêu lập phương có tâm tại gốc O và có bán kính bằng XICH_MA)
	- Từ vector cần đột biến x ==> Tạo ra vector đã đột biến z như sau : z = x + r (cộng theo từng thành phần trong 2 vector)
	  - ?? Vấn đề : Đẳng hướng hay không đẳng hướng? (các thành viên trong vector ngẫu nhiên là giống nhau hay khác nhau)
	  - ?? Vấn đề z sau đó bị âm ? ==> Giải pháp
	- Tính toán tỷ lệ đột biến thành công : sucessful_mutation_rate
      - Nếu tỷ lệ này > 0.2 ==> Tăng XICH_MA (tăng 1/0.85 lần) (hiện tượng : cá thể đang tập trung nhiều ở chân đồi, cần tăng xích ma tức mở rộng bán kính để mong muốn để chúng lên đỉnh đồi)
	  - Nếu tỷ lệ này < 0.2 ==> Giảm XICH_MA (hiện tượng : cá thể đang tập trung nhiều ở gần đỉnh đồi, cần giảm xích ma hay bán kính thay đổi để mong muốn chúng thay đổi ít hơn để leo lên được đỉnh đổi)
	  - Nểu tỷ lệ không đổi ==> Không thay đổi xích ma (hiện tượng : không có manh mối gì để suy luận ra hướng đi tốt)
  
5. Toán tử Thay thế - Replacement Operator
  - Chọn ra NUY Cá thể tốt nhất từ (NUY + LAMDA) cá thể thu được
  
  
6. Kết thúc giải thuật Di truyền. Chọn ra cá thể tốt nhất trong quần thể. Có thể tiếp tục đưa các cá thể tốt nhất này qua các giải thuật khác (như Tìm kiếm cục bộ, ...)