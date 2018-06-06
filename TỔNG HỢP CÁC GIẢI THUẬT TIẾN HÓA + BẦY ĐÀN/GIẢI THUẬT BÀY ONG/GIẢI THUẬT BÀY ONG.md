
1. GIẢI THUẬT BÀY ONG
- Thuật toán :
  - Bước 1. Khởi tạo quần thể ban đầu với n cá thể, mỗi cá thể đặc trưng cho một lời giải (hay một vùng đất). Các cá thể này được chọn ngẫu nhiên hoặc bằng một thuật toán heuristic nào đó.
  - Bước 2. Đánh giá độ thích nghi cho mỗi cá thể của quần thể (bằng một hàm mục tiêu được xây dựng trước)
  - Bước 3. Thực hiện vòng lặp While với điều kiện dừng (số vòng lặp tối đa, thời gian tối đa cho phép,...)
    - Trong n cá thể ban đầu, chọn ngẫu nhiên p cá thể (p < n) và trong số p cá thể này tiếp tục chọn ra e cá thể có độ thích nghi cao nhất (có fitness cao nhất). Lúc này, quần thể được chia thành 3 phần : phần chứa e cá thể (vùng-e), phần chứa p-e cá thể (vùng-pe), phần còn lại chứa n-p cá thể (vùng-np).
      - Với mỗi cá thể trong vùng-e, ta sẽ tiến hành tìm ra NEP hàng xóm của nó (theo chiến lược sinh hàng xóm). Sau đó so sánh cá thể ban đầu với NEP hàng xóm này để chọn ra hàng xóm tốt nhất. Do vậy, ta sẽ chọn được e cá thể mới tốt nhất.
	  - Với mỗi cá thể trong vùng-pe, ta sẽ tiến hành tìm ra NSP hàng xóm của nó (theo chiến lược sinh hàng xóm).  Sau đó so sánh cá thể ban đầu với NSP hàng xóm này để chọn ra hàng xóm tốt nhất. Do vậy, ta sẽ chọn được (p-e) cá thể mới tốt nhất.
	  - Với mỗi cá thể trong vùng-np sẽ bị thay thế bởi một cá thể hoàn toàn ngẫu nhiên khác (sinh ngẫu nhiên toàn bộ)

- Nhận xét : 
  - Thao tác chọn ngẫu nhiên p cá thể mà không phải chọn p cá thể tốt nhất từ n cá thể: Nhằm tạo ra một sự ngẫu nhiên trong sự chọn lọc, tránh hiện tượng "hướng tốt" (đi vào đỉnh cục bộ tìm kiếm)
  - Trong p cá thể ngẫu nhiên chọn được, lúc này mới chọn ra e cá thể tốt nhất trong số đó ==> Tạo một sự định hướng tốt trong trong quần thể ngẫu nhiên gồm p cá thể này
  - (n-p) cá thể còn lại sau mỗi vòng lặp đều bị bỏ đi "không thương tiếc" và bị thay thế bởi (n-p) cá thể khác ==> Với mong muốn đi khai phá những vùng đất mới (có thể giàu tiềm năng hơn). Chú ý : trong số n-p cá thể bị bỏ đi này, có thể chứa cá thể có độ fitness cao nhất trong quần thể ban đầu. Nhưng tại sao vẫn bị bỏ đi? Bởi vì : khả năng có cá thể tốt vẫn nằm trong nhóm n-p cá thể thì còn tùy thuộc vào xác suất ==> Do vậy, nó vẫn đảm bảo tính công bằng. Hơn nữa có thể, số lượng n-p này chỉ được thiết lập đủ nhỏ (không quá lớn, vừa tránh mất mát những cá thể tốt hay "chảy máu chất xám nhiều", vừa sẽ thu nạp được một vài cá thể mới có tiềm năng sẽ tốt ở những vùng đất mới)