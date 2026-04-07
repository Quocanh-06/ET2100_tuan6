# BÀI TẬP TUẦN 6: QUẢN LÝ DANH SÁCH FILE BẰNG LINKED LIST

## 1. Phân tích sơ bộ giải thuật
* **Cấu trúc dữ liệu:** Sử dụng **Danh sách liên kết đơn (Singly Linked List)**. Mỗi nút (Node) đại diện cho một file trong thư mục. Việc dùng Linked List giúp thao tác chèn file vào giữa danh sách (để giữ đúng thứ tự thời gian) đạt hiệu quả cao hơn so với mảng tĩnh.
* **Tiêu chí sắp xếp:** Danh sách được duy trì theo trật tự **thời gian (Timestamp)** tăng dần. File cũ đứng trước, file mới đứng sau.
* **Giải thuật Copy-Paste (Chèn):** Khi thêm file, ta không chèn vào cuối mà phải duyệt danh sách để tìm vị trí thích hợp sao cho tính chất sắp xếp theo thời gian được bảo toàn.
* **Giải thuật Backup:** Đây là bài toán tối ưu hóa số lượng file. Để lưu được nhiều file nhất vào USB 32GB, giải thuật sẽ tìm và **loại bỏ dần các file có kích thước nhỏ nhất** cho đến khi tổng dung lượng thỏa mãn điều kiện.

---

## 2. Mô tả thuộc tính và các hàm thực thi

### A. Thuộc tính (Attributes)
* **Cấu trúc File:**
    * `name`: Tên file (Kiểu chuỗi).
    * `size`: Kích thước file (Kiểu số nguyên lớn - Byte).
    * `time`: Thời gian tạo/chỉnh sửa (Kiểu số nguyên).
* **Cấu trúc Node:**
    * `data`: Lưu trữ thông tin File.
    * `next`: Con trỏ trỏ đến Node tiếp theo trong danh sách.

### B. Các hàm cần thực thi (Functions)
1.  `Insert_By_Time`: Tìm vị trí và chèn file mới vào danh sách theo đúng thứ tự thời gian.
2.  `Calculate_Total_Size`: Duyệt qua toàn bộ danh sách để tính tổng dung lượng các file.
3.  `Find_And_Remove_Smallest`: Tìm Node có kích thước file nhỏ nhất và thực hiện thao tác xóa Node đó khỏi danh sách liên kết.
4.  `Perform_Backup`: Vòng lặp kiểm tra tổng dung lượng, nếu lớn hơn 32GB thì gọi hàm xóa file nhỏ nhất cho đến khi đạt yêu cầu.

---

## 3. Phân tích giải thuật chi tiết (Mã giả)

### Thao tác: Chèn file theo trật tự thời gian

```text
Hàm Insert_By_Time(List, NewFile):
    newNode = Tạo_Node_Mới(NewFile)
    
    // Trường hợp danh sách rỗng hoặc file mới có thời gian nhỏ nhất
    Nếu List.head == NULL HOẶC NewFile.time < List.head.data.time:
        newNode.next = List.head
        List.head = newNode
        Kết thúc
    
    // Duyệt tìm vị trí để chèn sau node 'curr'
    curr = List.head
    Trong khi (curr.next != NULL VÀ curr.next.data.time < NewFile.time):
        curr = curr.next
    
    newNode.next = curr.next
    curr.next = newNodeHàm Backup_To_USB(List):
    DUNG_LUONG_USB = 32 * 1024 * 1024 * 1024  // 32GB quy đổi ra Byte
    
    Trong khi Calculate_Total_Size(List) > DUNG_LUONG_USB:
        Nếu List.head == NULL: Thoát vòng lặp
        
        minNode = List.head
        minPrev = NULL
        curr = List.head
        prev = NULL
        
        // Bước 1: Tìm Node có size nhỏ nhất trong toàn danh sách
        Trong khi curr != NULL:
            Nếu curr.data.size < minNode.data.size:
                minNode = curr
                minPrev = prev
            prev = curr
            curr = curr.next
            
        // Bước 2: Thực hiện xóa 'minNode'
        Nếu minPrev == NULL: // minNode nằm ở đầu danh sách
            List.head = List.head.next
        Nếu không: // 'Bắc cầu' qua node cần xóa
            minPrev.next = minNode.next
            
        Giải phóng bộ nhớ(minNode)