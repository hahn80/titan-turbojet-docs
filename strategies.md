# Strategies to handle data anslysis tasks

Để tiếp cận vấn đề xử lý dữ liệu đa chiều và lưu lượng lớn, chúng tôi chia ra hai kiến trúc khác nhau: Phần mềm, thuật toán; và tối ưu giải thuật.

## Software structure:

Về mặt phần mềm, chúng tôi sử dụng song song Rust và C++ để tối ưu các quá trình tính toán, khi xử lý dữ liệu thô, chúng tôi sử dụng công nghệ lưu dạng bảng để đọc và lưu tức thời thay thế hoàn toàn cho các cơ sở dữ liệu truyền thống. Có thể tóm lược những yếu tố chính như sau:

- Dùng **Rust** kết hợp với **C++** trong các tính toán trọng yếu: Mục đích là tiệm cận tốc độ xử lý tối đa của phần cứng hiện có.
- Chạy đa luồng (parallel) và tránh việc sao chép hoặc biến đổi dữ liệu nhiều lần. Dữ liệu đầu vào có thể bất ký, nhưng dữ liệu trao đổi nội bộ phải thống nhất định dạng bảng tiêu chuẩn của APACHE có thể đọc và lưu đệm (CACHE) nhanh chóng trong memory.
- Hiểu được thuật toán cần dữ liệu hoàn chỉnh thế nào cho hợp lý và viết các pipeline xử lý dữ liệu tương ứng với tối ưu được tính cho từng node; tránh sử dụng các thư viện có sẵn để tránh việc mỗi thư viện yêu cầu xử lý khác nhau. Việc này tốn nhiều công sức để xây dựng từ đầu, nhưng về sau chúng tôi sẽ không phụ thuộc vào bất cứ ai, hay thư viện nào.
- Hầu hết các thành viên của đội phát triển là ThS, TS về ngành Toán và data science, chúng tôi tối ưu tất cả các quá trình tính toán thông qua các hàm vector. Mục đích là thực hiện các OPERATIONS cho hàng triệu record cùng một lúc và không dùng loop.
- Nếu dữ liệu tính toán quá lớn so với memory, chúng tôi BATCHING và xử lý đa luồng (parallel) cho đến khi tiếp cận đến phần cuối của dữ liệu.

## Algorithms and Optimizations

Song song với việc tự kiến trúc lại framework cho toàn bộ công ty như đã nêu ở trên, trong quá trình xây dựng các model, huấn luyện nó và đưa vào sử dụng, chúng tôi tập trung rất nhiều vào việc giải quyết bài toán tiết kiệm thời gian cho tiền xử lý dữ liệu (preprocessing). Một số điểm đáng chú ý:

- Các thuật toán phần lớn được phát triển dựa trên các bài báo được xuất bản gần nhất, được đánh giá độ tối ưu và hội tụ nhanh. Vì được phát triển trên nền tảng từ bản giấy đến lập trình nên chúng tôi kiểm soát được quá trình tính toán. Biết rõ chỗ nào cần nhiều thời gian, chỗ nào không cần. Từ đó sẽ chi phối cho việc phân đoạn nào sẽ đưa Rust hay C++ vào, và phần nào cần Python.
- Điểm mạnh của Titan nằm ở chỗ nghiên cứu và ứng dụng (viết code trực tiếp) để tìm đặc trưng nòng cốt của dữ liệu (Feature Engineering và Feature Selection). Điều này giúp chúng tôi giải quyết được 2 vấn đề: (a) Lựa chọn kiểu model thích hợp; lúc nào thì cần machine learning (train và predect real time) và lúc nào sẽ cần đến deep learning. Hiện tại với dữ liệu 10 triệu records, chúng tôi sẽ dùng machine learning, nếu vượt quá số này thì chúng tôi mới dùng deep learning.
- Trong ranh giới giữa machine learning và deep learning, chúng tôi còn phát triển một bước trung gian bằng cách sử dụng tối ưu bậc hai (Quadratic Optimizations) kết hợp với ý tưởng deep learning để train machine learning model. Với cách tiếp cận này, chúng tôi vừa giảm được thời gian huấn luyện model và tiết kiệm chi phí phần cứng.
- Việc sử dụng deep learning sẽ được lập kế hoạch train theo thời gian sau khi đã có dữ liệu để kiểm nghiệm và nâng cấp hệ thống sau mỗi tuần hoặc tháng.
- Trong một số tác vụ bắt buộc phải dùng đến deep learning, chúng tôi sử dụng Federated Learning (Mô hình huấn luyện hợp tác) được đảm nhiệm bởi TS Tuấn Anh. Mục đích chính của mô hình này là phân tán việc training cho các local (địa phương) và chỉ nhận về các tham số mô hình (weight parameters) để cập nhật cho mô hình mẹ ở máy chủ. Kết hợp khả năng tiền xử lý dữ liệu đã được tối ưu trên Rust và C++, cùng với thu thập tham số từ các local, chúng tôi đã tránh được việc sao chép, di chuyển, biến đổi dữ liệu trung gian nhằm phục vụ cho model mẹ được hoàn thành nhanh hơn và đưa vào sử dụng hợp lý cho prediction.
