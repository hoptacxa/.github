Trong một hệ thống kiến trúc **microservices**, các dịch vụ không chỉ hoạt động độc lập mà còn cần phối hợp để đảm bảo các giao dịch phức tạp diễn ra một cách an toàn và nhất quán. Đây là nơi mà **Saga Service** trở thành yếu tố quan trọng.

**Saga Service** quản lý các giao dịch phân tán (distributed transactions) trong hệ thống. Thay vì sử dụng cơ chế khóa đồng bộ (như trong hệ thống monolithic), Saga chia giao dịch lớn thành các bước nhỏ hơn (saga steps) để các microservices thực hiện tuần tự hoặc song song. Nếu có lỗi, Saga sẽ kích hoạt các thao tác hoàn tác (compensation actions).

### Các Saga Service lớn trong hệ thống HTX bao gồm:

* * * * *

### **1\. Booking Saga Service** 📅

-   Quản lý các giao dịch liên quan đến việc đặt xe:
    -   Gửi yêu cầu từ khách hàng đến hệ thống điều phối.
    -   Kiểm tra tài xế khả dụng và xác nhận chuyến đi.
    -   Hoàn tác: Hủy đặt xe nếu không tìm được tài xế trong thời gian quy định.\
        **Ví dụ:** Khi bạn đặt một chuyến đi, nhưng tài xế không nhận, hệ thống sẽ tự động hoàn lại trạng thái ban đầu.

* * * * *

### **2\. Payment Saga Service** 💳

-   Điều phối các bước xử lý thanh toán:
    -   Xác minh tài khoản thanh toán của người dùng.
    -   Tạm giữ số tiền trong tài khoản hoặc ví điện tử.
    -   Hoàn tất giao dịch khi chuyến đi kết thúc.
    -   Hoàn tác: Nếu chuyến đi bị hủy, hệ thống sẽ giải phóng số tiền tạm giữ.

* * * * *

### **3\. Trip Lifecycle Saga Service** 🚕

-   Theo dõi toàn bộ chu trình của một chuyến đi:
    -   Bắt đầu từ việc ghép nối tài xế và khách hàng.
    -   Cập nhật trạng thái chuyến đi (đã bắt đầu, đang trên đường, hoàn thành).
    -   Kích hoạt các sự kiện khác như gửi thông báo hoặc cập nhật hồ sơ chuyến đi.\
        **Hoàn tác:** Nếu chuyến đi bị hủy giữa chừng, Saga sẽ khởi tạo quy trình xử lý tiền cước hoặc tìm tài xế khác.

* * * * *

### **4\. Refund & Compensation Saga Service** 💸

-   Quản lý hoàn tiền hoặc bồi thường:
    -   Xử lý các yêu cầu hoàn tiền từ khách hàng.
    -   Thực hiện các bước để bồi thường (nếu có lỗi từ phía tài xế hoặc hệ thống).
    -   Hoàn tác: Hủy yêu cầu hoàn tiền nếu lỗi không hợp lệ.

* * * * *

### **5\. Loyalty & Rewards Saga Service** 🎁

-   Điều phối các bước liên quan đến chương trình khách hàng thân thiết:
    -   Cập nhật điểm thưởng sau mỗi chuyến đi.
    -   Áp dụng ưu đãi khi khách hàng đủ điều kiện.
    -   Hoàn tác: Nếu chuyến đi bị hủy, Saga sẽ rút lại điểm thưởng đã cộng.

* * * * *

### **Tại sao Saga Service quan trọng?**

-   **Đảm bảo tính nhất quán:** Dù hệ thống phân tán, mọi giao dịch vẫn được thực hiện theo thứ tự logic.
-   **Khả năng chịu lỗi:** Nếu một bước thất bại, hệ thống có thể hoàn tác mà không làm hỏng toàn bộ quy trình.
-   **Tăng tính linh hoạt:** Cho phép hệ thống xử lý nhiều giao dịch đồng thời mà không làm chậm hệ thống.

