- Hoạt động khá tốt và mạnh mẽ, đặc biệt trong vấn đề xác định ý định người dùng (intent) và đối tượng được nhắc đến trong câu (entity) dù dữ liệu bạn thu thập và cung cấp cho rasa là vô cùng ít.

- Rasa NLU(Natura Language Understanding) là phần đầu tiên cũng là phần mà Rasa dùng nhiều nhất.Nếu xây dựng chatbot không phức tạp, hoàn toàn có thể xử lý bằng logic if-else thì chỉ cần phần NLU. Hoạt động của NLU:
 + file config.yml -> đây là phần cấu hình cho NLU, nơi lựa chọn  ngôn ngữ, model cần thiết. Lựa chọn ngôn ngữ, pipeline là một quy trình hoàn chỉnh từ lựa chọn Tokenizer, Featurizer, Extractor đến Classiffer

 + file nlu.md: ở đây chúng ta có các câu message người dùng có thể hỏi đã được gán nhãn là intent tương ứng.

- Rasa Core là phần khá cốt lõi tuy nhiên thường bị bỏ qua không dùng đến vì đọc Docs khá lằng nhằng. Là nơi thực hiện quản lí luồng hội thoại. Dựa vào các intent, entity đã được detect ra ở phần NLU, Rasa Core tiến hành lấy các kết quả này làm đầu vào, rồi quyết định message đầu ra. Hoạt động của Core:
 + Cấu hình trong file config.yml, khai báo cáo Policy cần thiết. Một số Policy như: MemoizationPolicy (quyết định message đầu ra dựa vào thông tin của những đoạn hội thoại trước đó), KerasPolicy (sử dụng mạng LSTM để tính xác suất đưa ra lựa chọn cho message tiếp theo), MappingPolicy(quyết định message dựa vào dữ liệu đã mapping) và trong trường hợp, việc tính xác suất đầu ra không thể vượt được ngưỡng mà FallbackPolicy đề ra, message trả ra sẽ là một utter_fallback kiểu như: "Xin lỗi anh chị ạ, em không hiểu được nội dung anh chị nói ạ" -> Tự đặt
 + Khai báo các thông tin cần thiết trong file domain.yml:
    * intent là các thông tin đã nếu trong file nlu, (có thể có cả entity),
    * action là phần liệt kê các hành động, message đầu ra mà chúng ra định nghĩa.
    * respone là phần chúng ta định nghĩa các message dạng text, hoặc hình ảnh, ... (các respone này thường có dạng utter_{})
    * Với các action cần thao tác với database, chúng ta định nghĩa trong file action.py
    * Cuối cùng là session_config, là phần cấu hình cho một session như thời gian để restart lại một session, có mang slot từ session cũ sang session mới hay không, ...
 + Sau khi khai báo trong domain, ta xây dựng các kịch bản cần thiết cho việc trò chuyện của "bot" -> phần này có logic khá giống if-else
 + file stories.md : dự tính trước các luồng hội thoại và xây dựng sẵn 1 kịch bản, giúp con bot xử lý một cách trơn tru hơn và có vẻ thông minh hơn

- Custom action: 
 + file action.py hỗ trợ lưu trữ thông tin trong database: có thể là ngân hàng câu hỏi, thông tin về lĩnh vực bot được hỏi, các thao tác với database,.... -> xử lý riêng theo từng tác vụ.
 + Mỗi action cụ thể, xây dựng riêng một class