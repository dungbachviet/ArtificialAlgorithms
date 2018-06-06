
# MỘT SỐ CHÚ Ý TRONG TÌM KIẾM CỤC BỘ

## I. MÔ HÌNH TỔNG QUÁT CỦA BÀI TOÁN TÌM KIẾM CỤC BỘ

### 1. Pha 1 : Tìm lời giải xuất phát (đã bị khử hay triệt tiêu mọi vi phạm ràng buộc)
  - Giải thuật sử dụng : Tham lam 2 bước, Tabu Search,...
  
### 2. Pha 2 : Cải thiện chất lượng của lời giải
  - Thường sử dụng các giải thuật : Tabu Search, Leo Đồi Cải Tiến, Luyện Kim, Tăng Giảm Trần Tuyến Tính, Các giải thuật tiến hóa và bày đàn (GA, PSO, Bầy ong, Bầy kiến) (Các giải thuật này đã trình bày cụ thể trong báo cáo)
  - Với các giải thuật Tìm kiếm cục bộ : 
    - Luôn đi từ một lời giải xuất phát (đã được triệt tiêu vi phạm ràng buộc từ pha 1, nếu bài toán không có pha 1 thì đó sẽ là lời giải xuất phát ngẫu nhiên)
    - Cần thiết kế một chiến lược Sinh hàng xóm hiệu quả với bài toán (đây là yếu tố cốt lõi và quan trọng nhất trong các giải thuật Local Search)
	- Chiến lược xem xét hàng xóm của cá thể hiện tại: 
	  
	  - Chiến lược 1 : Duyệt hết toàn bộ các hàng xóm của cá thể hiện tại : Chẳng hạn khi ta đã đề xuất 5 kiểu sinh hàng xóm, thì với mỗi kiểu sinh hàng xóm này sẽ xem xét toàn bộ các hàng xóm thuộc kiểu đó
	    - Nhược điểm : Tốn kém thời gian và chi phí xem xét
		- Ưu điểm : Nhưng chất lượng lời giải được cải thiện đáng kể sau mỗi lần lặp (do đặc tính "ăn chắc")
	 
	 - Chiến lược 2 : Mỗi kiểu sinh hàng xóm thì chỉ chọn ra 1 hàng xóm : Nếu đã đề xuất 5 kiểu sinh hàng xóm thì sẽ có 5 hàng xóm để xem xét. Sau đó chọn ra hàng xóm tốt nhất trong số 5 hàng xóm rồi đem nó đi so sánh với cá thể hiện tại (theo từng các chiến lược hoạt động cụ thể của các giải thuật Leo đồi, Luyện Kim, Tăng-Giảm Trần,...)
	    - Nhược điểm : Phải mất số vòng lặp lớn thì mới thấy được sự tăng rõ rệt về Fitness (do mỗi vòng lặp xét ít hàng xóm)
		- Ưu điểm : Thời gian xét duyệt trong mỗi lòng vặp rất nhanh (vì chỉ xét K hàng xóm ứng với K kiểu (chiến lược) sinh hàng xóm)
		
	  - Chiến lược 3 : Dung hòa 2 chiến lược 1 và 2 : Nghĩa là với từng kiểu sinh hàng xóm (Ki - kiểu sinh i) thì hãy cân nhắc số hàng xóm được sinh ra từ kiểu sinh đó (Ni - số hàng xóm được sinh ra từ kiểu sinh i). Nếu có m kiểu sinh hàng xóm thì số lượng hàng xóm cần xét trong mỗi vòng lặp là : N1 + N2 + ... + Nm (nên thiết lập tổng này phù hợp và đủ lớn để vừa bao quát được không tìm kiếm vừa cải thiện nhanh chất lượng lời giải)
  
  - Với giải thuật Tiến hóa GA : 
    - Nếu bài toán có yêu cầu ràng buộc thì cần có một chiến lược lai ghép, đột biến sao cho không vi phạm ràng buộc
	- Quan tâm tới khá nhiều khá nhiều tham số (Số lượng quần thể, Số cha mẹ tốt nhất, Tỷ lệ đột biến, Tỷ lệ lai ghép, Sức ép chọn lọc, Tỷ lệ 1 trong random template, số vòng lặp, ...)
	- Quan tâm và đề xuất các chiến lược lai ghép và đột biến phù hợp với bài toán nhất (từng bài toán sẽ có những chiến lược phù hợp cho riêng nó)

### 3. Pha 3 : Cải thiện xa hơn nữa chất lượng của lời giải
  - Thường sử dụng Leo đồi (do các lời giải sau khi đi qua pha 2 đã khá tốt và đang nằm ở gần đỉnh đồi nên lúc này chỉ cần sử dụng Leo đồi để đẩy cá thể đó leo lên tới được đỉnh đồi).
  
  
  