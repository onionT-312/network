Tính kết quả giữa a và b dựa vào q
Sever: 112.137.129.129:27004


Mỗi gói tin gồm tối thiểu 2 trường

- type: int 4 bytes, little endian, là loại gói tin
- len: int 4 bytes, little endian, là độ dài data đi kèm đằng sau

Mỗi gói tin có thể kèm theo data có độ dài len

Type:
- 0: PKT_HELLO
	- là gói tin đầu tiên trao đổi, bắt buộc phải có
	- data theo sau là string chứa mã sinh viên (bắt buộc)
	- độ dài của mã sinh viên chứa trong trường len.

- 1: PKT_CALC
	- Server sẽ gửi yêu cầu tính toán qua gói tin này
	- data đằng sau sẽ gồm
		- 4 bytes đầu: int, little endian, là số a
		- 4 bytes sau: int, little endian, là số b	 	
		- 4 bytes sau: int, big endian, là q

		q = 1 tính a+b
		q = 2 tính a-b
		q = 3 tính a*b
		q = 4 tính a/b (kết quả làm tròn 2 chữ số thập phân)

- 2: PKT_RESULT:
	- Client gửi kết quả bằng gói tin này sau khi nhận PKT_CALC
	
	- data đằng sau gồm 4 bytes đầu kiểu int, big endian, biểu thị kiểu dữ liệu đằng sau: 
		- 1: int
		- 2: float
		- 3: không hợp lệ (TH b = 0)

	- data đằng sau gồm 4 bytes sau, biểu thị kết quả tính toán
		- nếu là int, big endian
		- nếu là float, big endian	

- 3: PKT_BYE
	- Server gửi gói tin này nếu client làm sai quá 20% câu hỏi

- 4: PKT_FLAG
	- Server gửi gói tin này sau khi client đã trả lời đúng 80% câu hỏi
	- Trường len có giá trị bằng độ dài flag
	- data theo sau là flag có độ dài len
	- Kết nối chấm dứt
	- Sinh viên nộp flag được trả về từ server lên máy chủ và điểm sẽ được công nhận
