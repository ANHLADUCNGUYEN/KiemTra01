# PHAN I: LY THUYET & CAU HOI NGAN (BAI 01)

### Thong tin sinh vien:
- Họ tên: Nguyễn Minh Đức
- Mã sinh viên: 24810310226
- Tiêu đề: Bài kiểm tra 01 - Phần lý thuyết

---

### Cau 1: Trinh bay su khac nhau giua Value Types (Kieu gia tri) va Reference Types (Kieu tham chieu) trong C# ve co che luu tru vung nho (Stack vs Heap).

- Value Types (Kiểu giá trị): Lưu trữ trực tiếp dữ liệu thực tế tại chính vị trí mà biến được khai báo. Thông thường, các biến cục bộ kiểu giá trị được cấp phát trên bộ nhớ Stack. Bộ nhớ này hoạt động theo cơ chế LIFO (Vào sau ra trước), được hệ thống tự động dọn dẹp ngay khi hàm/khối lệnh kết thúc, mang lại tốc độ truy xuất và cấp phát rất nhanh. (Lưu ý: Nếu Value Type là một phần của một Reference Type, nó sẽ nằm trên Heap cùng với đối tượng chứa nó). Ví dụ: int, float, bool, struct, enum.

- Reference Types (Kiểu tham chiếu): Chia việc lưu trữ thành hai phần. Dữ liệu thực tế của đối tượng được cấp phát trên bộ nhớ Heap – vùng nhớ rộng lớn dùng cho dữ liệu động. Trong khi đó, trên Stack chỉ lưu trữ một tham chiếu (địa chỉ/con trỏ) trỏ tới vùng nhớ trên Heap đó. Vùng nhớ trên Heap không tự giải phóng mà do bộ thu gom rác (Garbage Collector - GC) của .NET quản lý và dọn dẹp khi không còn tham chiếu nào trỏ tới. Ví dụ: class, string, array, object, delegate.

### Cau 2: Tinh nang Init-only Properties (init) trong C# 9/10 khac gi so voi thuoc tinh co set thong thuong? Neu truong hop su dung thuc te.

- Sự khác biệt:

    + Bộ gán set cho phép thuộc tính được thay đổi giá trị tự do ở bất kỳ đâu và bất kỳ lúc nào trong suốt vòng đời của đối tượng.

    + Bộ gán init (Init-only properties) chỉ cho phép gán giá trị một lần duy nhất trong giai đoạn khởi tạo đối tượng (thông qua Constructor hoặc Object Initializer {...}). Ngay sau khi đối tượng khởi tạo xong, thuộc tính đó bị khóa lại và trở thành chỉ đọc (read-only).

- Trường hợp sử dụng thực tế: init được dùng để thiết kế các đối tượng bất biến (Immutable Objects). Ứng dụng phổ biến nhất là khi tạo các DTO (Data Transfer Objects), các model dữ liệu gửi nhận qua API, hoặc các đối tượng cấu hình. Việc dữ liệu không thể bị thay đổi sau khi khởi tạo giúp mã nguồn an toàn hơn, tránh các lỗi thay đổi trạng thái ngoài ý muốn và rất an toàn khi xử lý đa luồng (thread-safe).


### Cau 3: Phan biet su khac nhau giua phuong thuc virtual o lop cha va phuong thuc override o lop con khi trien khai tinh Da hinh (Polymorphism).

- virtual (Tại lớp cha/Base class): Đánh dấu một phương thức hoặc thuộc tính là có một hành vi mặc định, nhưng cho phép (không bắt buộc) các lớp kế thừa nó có thể định nghĩa lại (thay đổi) hành vi này.

- override (Tại lớp con/Derived class): Định nghĩa lại và ghi đè hoàn toàn hiện thực của một phương thức virtual (hoặc abstract) đã được khai báo ở lớp cha, tạo ra hành vi đặc thù cho lớp con.

- Cơ chế Đa hình: Khi gọi phương thức qua một biến có kiểu là Lớp cha nhưng biến này đang trỏ tới một đối tượng của Lớp con, .NET runtime sẽ áp dụng cơ chế Liên kết trễ (Late Binding / Dynamic Dispatch) để tự động kiểm tra kiểu thực tế của đối tượng tại thời điểm chạy (runtime) và gọi hàm override tương ứng ở lớp con, thay vì hàm virtual ở lớp cha.

---

### Cau 4: Tai sao mot thanh phan duoc khai bao la static trong Lop (Class) lai khong the truy xuat thong qua mot the hien (Object Instance) duoc tao bang toan tu new?

- Theo lý thuyết thiết kế của lập trình hướng đối tượng trong C#, từ khóa static chỉ định rằng một thành phần (biến, phương thức) thuộc về chính bản thân kiểu dữ liệu (Class), chứ không thuộc về bất kỳ đối tượng (Instance) cụ thể nào được sinh ra từ kiểu dữ liệu đó.

- Toán tử new tạo ra một khối bộ nhớ mới trên Heap chứa dữ liệu trạng thái riêng biệt cho từng đối tượng (instance members), và cung cấp con trỏ ẩn this để đối tượng tự tham chiếu đến trạng thái của nó.

- Trong khi đó, dữ liệu static được khởi tạo duy nhất một lần (lưu trên vùng nhớ High-Frequency Heap), được chia sẻ chung cho mọi đối tượng cùng lớp và không phụ thuộc vào trạng thái this.

- Trình biên dịch C# cố tình chặn việc dùng cú pháp object.StaticMember để bảo vệ tính toàn vẹn về mặt ngữ nghĩa: nó buộc lập trình viên phải gọi qua tên lớp ClassName.StaticMember nhằm thể hiện rõ ràng rằng dữ liệu này có phạm vi toàn cục của lớp, ngăn chặn sự nhầm lẫn về việc trạng thái này có thể bị thay đổi độc lập bởi từng đối tượng riêng lẻ.
