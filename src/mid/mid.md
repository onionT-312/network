# MID
Sever: 112.137.129.129:27016
Cho N, M và 1 mảng có N phần tử, tìm phần tử đầu tiên thỏa mãn sao cho tổng các phần tử bên trái bằng tổng các phần tử bên phải nó.

Mỗi gói tin gồm tối thiểu 2 trường.
- type: int 4 bytes, little endian, là loại gói tin.
- len: int 4 bytes, little endian, là độ dài *data* đi kèm đằng sau, tính bằng byte.

Mỗi gói tin có thể kèm theo *data* có độ dài *len*.

## Types:
- 0: PKT_HELLO
  - là gói tin đầu tiên trao đổi, bắt buộc phải có.
  - Trường *len* là độ dài của mã sinh viên.
  - Trường *data* theo sau là string có độ dài *len* chứa mã sinh viên.
- 1: PKT_CALC
  - Server sẽ gửi yêu cầu tính toán qua gói tin này.
  - Trường *len* có giá trị bằng 4 * (N + 2).
  - Trường *data* đằng sau gồm:
    - 4 bytes đầu: biểu thị M.
    - 4 bytes tiếp theo: biểu thị checksum (có thể đúng hoặc sai).
    - 4*N bytes sau: biểu thị các số trong dãy từ a<sub>1</sub> - a<sub>n</sub>
- 2: PKT_RESULT:
  - Client gửi kết quả bằng gói tin này sau khi nhận PKT_CALC.
  - Trường *len* có giá trị bằng 4 hoặc 8 tùy theo nội dung phía sau.
  - Trường *data* đằng sau gồm:
    - 4 bytes đầu: int, little endian, kết quả kiểm tra checksum
      - bằng 1 nếu checksum đúng
        - checksum đúng = tích các phần tử trong dãy mod M.
      - bằng 0 nếu checksum sai. Nếu checksum sai, không cần gửi kết quả.
    - 4 bytes sau: int, little endian, là vị trí của phần tử thỏa mãn đề bài.
- 3: PKT_BYE
  - Server từ chối kết quả, kết nối chấm dứt.
- 4: PKT_FLAG
  - Server gửi gói tin này sau khi client trả lời hết toàn bộ câu hỏi.
  - Trường *len* có giá trị bằng độ dài *flag*.
  - Trường *data* theo sau là *flag* có độ dài *len*.
  - Kết nối chấm dứt.
  - Sinh viên nộp *flag* được trả về từ server lên máy chủ và điểm sẽ được công nhận.

