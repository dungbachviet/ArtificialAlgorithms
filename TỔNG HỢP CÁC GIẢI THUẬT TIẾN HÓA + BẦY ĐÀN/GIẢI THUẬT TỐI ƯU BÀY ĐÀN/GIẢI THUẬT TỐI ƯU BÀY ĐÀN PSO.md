GIẢI THUẬT TỐI ƯU HÓA BÀY ĐÀN - PSO

1. THUẬT TOÁN

```c++
// Khởi tạo quần thể P gồm N cá thể
P = Particle_Initialization();

// Vòng lặp qua MAX_ITERATION thế hệ
while (chưa đủ MAX_ITERATION)
	// Cập nhật kỷ lục "cục bộ"
	For each particle p in P do
		fp = f(p);
		
		if fp is better than f(pBest)
		pBest = p;
	end
	
	// Cập nhật Kỷ lục "toàn cục"
	gBest = cá thể tốt nhất trong số các kỷ lục pBest[] của N cá thể;
	
	// Cập nhật vận tốc mới v và vị trí mới p của từng cá thể sau mỗi thế hệ
	For each particle p in P do
		v = w*v + c1*rand()*(pBest – p) + c2*rand()*(gBest – p);
		p = p + v;
	end
end

```

2. PHÂN TÍCH :
- Thuật toán PSO mô phỏng quá trình Tìm kiếm thức ăn của Bầy chim
- Mỗi con chim trong bầy vừa : 
  - Bay theo hướng bay hiện tại của nó (theo quán tính của vận tốc hiện tại)
  - Bay theo hướng bay về phía vùng đất chứa nhiều thức ăn nhất mà nó tìm được trước đó, do nó rất cẩn thận không muốn đánh mất hay làm mất dấu nguồn thức ăn mà nó đã tìm được trước đó.
  - Bay theo hướng bay về phía vùng đất mà chứa nhiều thức ăn nhất (mà cả đàn chim đã phát hiện ra)
  
- Mỗi con chim i sẽ phải lưu giữ các thông tin sau : 
  - Vùng đất hiện tại mà nó đang đứng : p[i] (chính là tọa độ nơi nó đang đứng)
  - Vùng đất chứa nguồn thức ăn nhiều nhất mà nó từng đi qua : pBest[i] (kỷ lục cục bộ của chim i) (tọa độ chứa nguồn thức ăn kỷ lục "cục bộ")
  - Vùng đât chứa nguồn thức ăn nhiều nhất (do cả đàn đã chia sẻ với nhau) : gBest (kỷ lục toàn cục) (tọa độ chứa nguồn thức ăn kỷ lục "toàn cục")
  - Vector vận tốc hiện tại của nó : v (là vector vận tốc, nên sẽ cho biết cả độ lớn và hướng của vận tốc này)
  
- Phân tích công thức cập nhật : v = w*v + c1*rand()*(pBest – p) + c2*rand()*(gBest – p); và p = p + v;
	- v : là vận tốc hiện tại của chú chim i ==> Cho thấy sự quán tính leo theo vận tốc hiện tại.
	- w : cho biết mức độ quán tính nhiều hay ít (nghĩa là w thuộc từ 0.8-->1.2
	- c1, c2 : hiểu là gia tốc theo từng hướng bay về phía Kỷ lục cục bộ và Kỷ lục toàn cục. Thường thì c1, c2 sẽ gần với 2. Tức c1+c2 sẽ gần với 4.
	- (pBest-p) : cho biết sự chênh lệch khoảng cách giữa vị trí kỷ lục cục bộ với vị trí hiện tại
	- (gBest-p) : cho biết sự chênh lệch khoảng cách giữa vị trí kỷ lục toàn cục với vị trí hiện tại
	- rand() : sinh ra một số trong miền (0,1) nhằm tạo ra một sự ngẫu nhiên về mức độ ảnh hưởng của các kỷ lục cục bộ và kỷ lục toàn cục
	
- Cân nhắc về tính toán học : 
  - Mối quan hệ giữa : Độ dài x, vận tốc v, gia tốc a
    - Vận tốc = Độ dài đi được trong 1 đơn vị thời gian ==> Nếu coi thời gian là 1 đơn vị thời gian thì delta(x) = v
    - Gia tốc = Sự thay đổi của vận tốc trong một đơn vị thời gian ==> Nếu coi thời gian là 1 đơn vị thời gian thì delta(v) = a
  - Do vậy trong công thức cập nhật trên, ta nên ngầm hiểu rằng : đang tính trên một đơn vị thời gian, lúc này thì sẽ không còn sự khác biệt giữa v và delta(x) nữa, tức v và delta(x) có cùng một đơn vị đo
  - Các giá trị : w, c1*rand(), c2*rand() được hiểu là các trọng số thể hiện sẽ ưu tiên đi theo hướng nào nhiều hơn (hướng hiện tại, hay hướng theo kỷ lục cục bộ hay hướng theo kỷ lục toàn cục)
  - Sau khi cập nhật được v, thì ta chỉ cần cập nhật p theo cách : p = p + v là hoàn toàn hợp lý (vì v = delta(x) sự chênh lệch khoảng cách tới vị trí mới)  

- Phân tích sâu hơn công thức cập nhật : 
  - c1*rand()*(pBest – p) + c2*rand()*(gBest – p) : Là thành phần Intensification thể hiện sự quan tâm tới quá khứ, kinh nghiệm (các giải pháp trước đó)
  - w*v : Là thành phần Diversification thể hiện việc đi tìm một giải pháp mới (tại một vùng đất mới giàu tiềm năng)
  ===> 2 Yếu tố này giống như câu nói : "Vừa đi vừa ngó lại" hay vừa "vừa đi vừa dòm ngó xung quanh" để tìm ra vùng đất nhiều tiềm năng nhất !!!