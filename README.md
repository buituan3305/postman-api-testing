# postman-api-testing
BÁO CÁO THỰC HÀNH KIỂM THỬ API BẰNG POSTMAN
Đề tài: Kiểm thử API quản lý bài viết trên JSONPlaceholder
Họ và tên: Bùi Anh Tuấn
Mã sinh viên: 23010590

1. Giới thiệu và mục tiêu
API (Application Programming Interface) là giao diện cho phép các hệ thống phần mềm trao đổi dữ liệu và sử dụng chức năng của nhau. Với API qua HTTP, ứng dụng gửi yêu cầu tới một địa chỉ xác định; máy chủ xử lý và trả về phản hồi gồm mã trạng thái, các header và phần nội dung nếu có.
Kiểm thử API giúp đánh giá chức năng thông qua dữ liệu đầu vào và phản hồi, không phụ thuộc vào giao diện người dùng. Bài thực hành lựa chọn API quản lý bài viết của JSONPlaceholder để thực hiện các thao tác đọc, thêm, sửa và xóa bằng Postman.
Các mục tiêu cụ thể:
- Tạo và tổ chức request trong một Collection.
- Cấu hình HTTP method, URL, header và request body.
- Sử dụng biến để tái sử dụng địa chỉ API.
- Phân tích mã trạng thái và dữ liệu JSON trả về.
- Viết script kiểm thử tự động với pm.test và pm.expect.
- Kiểm thử tình huống hợp lệ và tình huống tài nguyên không tồn tại.
- Chạy tập kiểm thử bằng Collection Runner và ghi nhận bằng chứng.
- Xuất Collection và trình bày báo cáo trong README.md trên GitHub.
2. Công cụ và phạm vi thực hành
2.1. Công cụ
Công cụ/thành phần	Vai trò
Postman	Tạo, gửi request, viết script và chạy kiểm thử
JSONPlaceholder	Cung cấp API mẫu và dữ liệu giả lập
JavaScript trong Postman	Viết các phép kiểm tra phản hồi
GitHub	Lưu Collection, báo cáo và ảnh minh họa
Markdown	Định dạng nội dung báo cáo trong README


2.2. Môi trường thực thi
Thuộc tính	Giá trị
Hệ điều hành thực tế
Phiên bản Postman
Base URL	https://jsonplaceholder.typicode.com
Tài nguyên kiểm thử	/posts
Định dạng dữ liệu	JSON
Xác thực	No Auth; API mẫu không yêu cầu API key
Collection	JSONPlaceholder - Kiem thu API bai viet
Số lần lặp trong một lượt chạy	1


2.3. Phạm vi và giới hạn
Bài thực hành gồm 6 tình huống: lấy danh sách bài viết, xem chi tiết, thêm bài viết, cập nhật bài viết, xóa bài viết và truy vấn bài viết không tồn tại. Các phép kiểm tra tập trung vào mã trạng thái, cấu trúc dữ liệu và nội dung phản hồi.
JSONPlaceholder là API giả lập. Các thao tác POST, PUT và DELETE trả về phản hồi mô phỏng nhưng không lưu thay đổi thật trên máy chủ. Vì vậy:
- Không dùng ID nhận từ POST để khẳng định có thể đọc lại bản ghi vừa tạo.
- PUT và DELETE sử dụng bài viết có sẵn với ID 1.
- GET lại bài viết sau PUT hoặc DELETE không chứng minh thay đổi đã được lưu.
- Các request trong bài được kiểm tra độc lập; đây không phải kiểm thử vòng đời CRUD có lưu dữ liệu bền vững.
Bài thực hành chưa đánh giá xác thực, phân quyền, bảo mật, tải lớn hoặc tính toàn vẹn của một cơ sở dữ liệu thật. API giả lập cũng không được dùng làm căn cứ để kết luận một hệ thống thực tế xử lý đầy đủ dữ liệu đầu vào không hợp lệ.
3. Cơ sở lý thuyết
3.1. Các phương thức HTTP
Phương thức	Ý nghĩa trong bài thực hành	Endpoint
GET	Đọc danh sách hoặc một bài viết	/posts, /posts/1
POST	Gửi dữ liệu để mô phỏng tạo bài viết	/posts
PUT	Gửi dữ liệu để mô phỏng thay thế nội dung bài viết	/posts/1
DELETE	Mô phỏng xóa bài viết	/posts/1


3.2. Các mã trạng thái sử dụng
Mã	Tên	Ý nghĩa trong tình huống kiểm thử
200	OK	Yêu cầu được xử lý thành công
201	Created	Phản hồi tạo tài nguyên thành công
404	Not Found	Không tìm thấy tài nguyên được yêu cầu


Một tình huống có thể đạt khi API trả về 404 nếu đó chính là phản hồi mong đợi. Ngược lại, 200 không đủ để kết luận toàn bộ kiểm thử đạt: dữ liệu trả về còn phải thỏa các phép kiểm tra đã thiết kế. Mã phản hồi khi xóa phụ thuộc API; bài này kiểm tra 200 theo hành vi mong đợi của endpoint mẫu, không áp dụng mặc định cho mọi API.
3.3. Cấu trúc dữ liệu bài viết
Trường	Kiểu dữ liệu trong dữ liệu mẫu	Ý nghĩa
userId	Number	Mã người tạo bài viết
id	Number	Mã bài viết
title	String	Tiêu đề bài viết
body	String	Nội dung bài viết


3.4. Kiểm thử tự động trong Postman
Script được đặt trong Scripts → Post-response của từng request và chạy sau khi nhận phản hồi. Kết quả hiển thị tại Test Results.
Thành phần	Chức năng
pm.test(name, callback)	Khai báo một kiểm thử có tên
pm.response.to.have.status(code)	Đối chiếu mã trạng thái
pm.response.json()	Đọc response dưới dạng dữ liệu JavaScript
pm.expect(value)	Tạo phép khẳng định về dữ liệu
.to.eql(value)	So sánh giá trị mong đợi
.to.have.all.keys(...)	Kiểm tra đúng tập khóa của object


Trong báo cáo này, test case là một tình huống gắn với một request; test script là một khối pm.test. Một khối pm.test có thể chứa nhiều phép khẳng định.
4. Thiết lập Collection
1. Tạo Collection tên JSONPlaceholder - Kiem thu API bai viet.
2. Trong tab Variables, khai báo base_url bằng https://jsonplaceholder.typicode.com, không thêm dấu / ở cuối.
3. Sử dụng URL dạng {{base_url}}/posts trong các request.
4. Chọn No Auth cho các request.
5. Với POST và PUT, chọn Body → raw → JSON và bảo đảm header Content-Type: application/json.
6. Với GET và DELETE, không gửi request body.
7. Đặt script tại từng request và lưu trước khi chạy Collection.
Việc sử dụng biến Collection giúp thay đổi địa chỉ gốc tại một vị trí. Các request vẫn dùng chung cấu hình và dễ được nhập lại trên máy khác.
5. Thiết kế các tình huống kiểm thử
Tiền điều kiện chung: Postman truy cập được Internet, API mẫu hoạt động và biến base_url được khai báo đúng. Bài viết ID 1 là bản ghi mẫu dùng cho các tình huống xem, sửa và xóa.
Mã	Tình huống	Method	Endpoint	Kết quả mong đợi	Số pm.test
TC01	Lấy danh sách bài viết	GET	/posts	200; mảng không rỗng; mỗi phần tử đủ trường	3
TC02	Xem chi tiết bài viết	GET	/posts/1	200; ID bằng 1; tiêu đề là chuỗi không rỗng	3
TC03	Thêm bài viết	POST	/posts	201; ID là số dương; dữ liệu phản hồi khớp dữ liệu gửi	3
TC04	Cập nhật bài viết	PUT	/posts/1	200; ID bằng 1; dữ liệu phản hồi khớp nội dung cập nhật	3
TC05	Xóa bài viết	DELETE	/posts/1	200; response là object rỗng {}	2
TC06	Xem bài viết không tồn tại	GET	/posts/999999	404; response là object rỗng {}	2
Tổng	6 tình huống				16


5.1. TC01 — Lấy danh sách bài viết
Mục đích: Kiểm tra khả năng trả về danh sách bài viết và cấu trúc các phần tử.
Request: GET {{base_url}}/posts. Không có body.
Thực hiện: Tạo request TC01 - Lay danh sach bai viet, cấu hình method và URL, thêm script sau, lưu rồi chọn Send.
pm.test("Status code la 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Du lieu tra ve la mang khong rong", function () {
    const data = pm.response.json();
    pm.expect(data).to.be.an("array").that.is.not.empty;
});

pm.test("Moi bai viet co du cac truong", function () {
    const data = pm.response.json();
    data.forEach(function (post) {
        pm.expect(post).to.have.all.keys("userId", "id", "title", "body");
    });
});
Tiêu chí đạt: Cả 3 test script đều PASS. Phép kiểm tra này xác nhận danh sách không rỗng và tập trường; chưa kiểm tra kiểu dữ liệu của mọi trường trong mọi bài viết.
Kết quả thực tế: Chưa ghi nhận.
Minh chứng: Bổ sung ảnh tc01-get-posts.png, thể hiện URL, mã trạng thái và Test Results.
<!-- Sau khi tải ảnh lên cùng thư mục README, bỏ dấu chú thích dòng bên dưới. -->
<!-- ![TC01 - Kết quả lấy danh sách bài viết](tc01-get-posts.png) -->

5.2. TC02 — Xem chi tiết bài viết
Mục đích: Kiểm tra API trả về đúng bài viết được yêu cầu và tiêu đề có nội dung.
Request: GET {{base_url}}/posts/1. Không có body.
Thực hiện: Tạo request TC02 - Xem chi tiet bai viet, cấu hình request và script, lưu rồi chọn Send.
pm.test("Status code la 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Tra ve dung bai viet ID 1", function () {
    const data = pm.response.json();
    pm.expect(data.id).to.eql(1);
});

pm.test("Tieu de la chuoi khong rong", function () {
    const data = pm.response.json();
    pm.expect(data.title).to.be.a("string").that.is.not.empty;
});
Tiêu chí đạt: Cả 3 test script đều PASS.
Kết quả thực tế: Chưa ghi nhận.
Minh chứng: Bổ sung ảnh tc02-get-post.png.
<!-- ![TC02 - Kết quả xem chi tiết bài viết](tc02-get-post.png) -->

5.3. TC03 — Thêm bài viết
Mục đích: Kiểm tra phản hồi mô phỏng tạo mới, ID được cấp và nội dung trả về.
Request: POST {{base_url}}/posts.
Header: Content-Type: application/json.
Body (raw, JSON):
{
  "title": "Bai thuc hanh Postman",
  "body": "Kiem thu chuc nang them bai viet",
  "userId": 1
}
Thực hiện: Tạo request TC03 - Them bai viet, nhập body và script, lưu rồi chọn Send.
pm.test("Status code la 201", function () {
    pm.response.to.have.status(201);
});

pm.test("Bai viet duoc cap ID", function () {
    const data = pm.response.json();
    pm.expect(data.id).to.be.a("number").above(0);
});

pm.test("Phan hoi dung du lieu da gui", function () {
    const data = pm.response.json();
    pm.expect(data.title).to.eql("Bai thuc hanh Postman");
    pm.expect(data.body).to.eql("Kiem thu chuc nang them bai viet");
    pm.expect(data.userId).to.eql(1);
});
Tiêu chí đạt: Cả 3 test script đều PASS. Kết quả này chỉ xác nhận phản hồi mô phỏng, không xác nhận dữ liệu đã được lưu bền vững.
Kết quả thực tế: Chưa ghi nhận.
Minh chứng: Bổ sung ảnh tc03-post.png; có thể chụp thêm một ảnh body và một ảnh Test Results nếu không thể hiện đủ trong cùng khung hình.
<!-- ![TC03 - Kết quả thêm bài viết](tc03-post.png) -->

5.4. TC04 — Cập nhật bài viết
Mục đích: Kiểm tra phản hồi mô phỏng cập nhật bài viết ID 1.
Request: PUT {{base_url}}/posts/1.
Header: Content-Type: application/json.
Body (raw, JSON):
{
  "id": 1,
  "title": "Tieu de da cap nhat",
  "body": "Noi dung da cap nhat bang Postman",
  "userId": 1
}
Thực hiện: Tạo request TC04 - Cap nhat bai viet, nhập body và script, lưu rồi chọn Send.
pm.test("Status code la 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Dung ID bai viet can cap nhat", function () {
    const data = pm.response.json();
    pm.expect(data.id).to.eql(1);
});

pm.test("Phan hoi dung noi dung cap nhat", function () {
    const data = pm.response.json();
    pm.expect(data.title).to.eql("Tieu de da cap nhat");
    pm.expect(data.body).to.eql("Noi dung da cap nhat bang Postman");
    pm.expect(data.userId).to.eql(1);
});
Tiêu chí đạt: Cả 3 test script đều PASS.
Kết quả thực tế: Chưa ghi nhận.
Minh chứng: Bổ sung ảnh tc04-put.png.
<!-- ![TC04 - Kết quả cập nhật bài viết](tc04-put.png) -->

5.5. TC05 — Xóa bài viết
Mục đích: Kiểm tra mã trạng thái và nội dung phản hồi của thao tác xóa mô phỏng.
Request: DELETE {{base_url}}/posts/1. Chọn Body là none.
Thực hiện: Tạo request TC05 - Xoa bai viet, thêm script, lưu rồi chọn Send.
pm.test("Status code la 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Phan hoi la object rong", function () {
    pm.expect(pm.response.json()).to.deep.equal({});
});
Tiêu chí đạt: Cả 2 test script đều PASS. Không đặt điều kiện GET lại /posts/1 phải trả về 404 vì máy chủ không xóa dữ liệu thật.
Kết quả thực tế: Chưa ghi nhận.
Minh chứng: Bổ sung ảnh tc05-delete.png.
<!-- ![TC05 - Kết quả xóa bài viết](tc05-delete.png) -->

5.6. TC06 — Truy vấn bài viết không tồn tại
Mục đích: Kiểm tra phản hồi khi yêu cầu tài nguyên không có trong tập dữ liệu mẫu.
Request: GET {{base_url}}/posts/999999. Không có body.
Thực hiện: Tạo request TC06 - Bai viet khong ton tai, thêm script, lưu rồi chọn Send.
pm.test("Status code la 404", function () {
    pm.response.to.have.status(404);
});

pm.test("Khong tra ve du lieu bai viet", function () {
    pm.expect(pm.response.json()).to.deep.equal({});
});
Tiêu chí đạt: Cả 2 test script đều PASS. Đây là kiểm thử tình huống không hợp lệ về tài nguyên; mã 404 là kết quả mong đợi.
Kết quả thực tế: Chưa ghi nhận.
Minh chứng: Bổ sung ảnh tc06-not-found.png.
<!-- ![TC06 - Kết quả truy vấn bài viết không tồn tại](tc06-not-found.png) -->

6. Chạy toàn bộ tập kiểm thử
Quy trình chạy bằng Collection Runner:
1. Lưu cả 6 request cùng script tương ứng.
2. Mở Collection và chọn Run collection hoặc Run.
3. Chọn 6 request theo thứ tự TC01 đến TC06.
4. Đặt Iterations = 1; nếu có cấu hình Delay, có thể đặt 500 ms.
5. Chạy Collection và xem kết quả từng request.
6. Ghi số request, số test PASS/FAIL, lỗi thực thi và thời điểm chạy.
7. Chụp ảnh kết quả tổng hợp vào file collection-run.png.
Các script trong báo cáo chỉ được đặt tại cấp request. Nếu thêm script tại cấp Collection hoặc Folder, tổng số test trong Runner có thể khác 16.
6.1. Bảng kết quả thực tế
Mã	Status mong đợi	Status thực tế	Số test dự kiến	PASS thực tế	FAIL thực tế	Đánh giá
TC01	200	Chưa ghi nhận	3	—	—	Chưa xác nhận
TC02	200	Chưa ghi nhận	3	—	—	Chưa xác nhận
TC03	201	Chưa ghi nhận	3	—	—	Chưa xác nhận
TC04	200	Chưa ghi nhận	3	—	—	Chưa xác nhận
TC05	200	Chưa ghi nhận	2	—	—	Chưa xác nhận
TC06	404	Chưa ghi nhận	2	—	—	Chưa xác nhận
Tổng			16	Chưa ghi nhận	Chưa ghi nhận	Chưa xác nhận


Dấu “—” biểu thị chưa có số liệu, không có nghĩa là 0. Chỉ ghi “Đạt” cho một test case khi tất cả script của request đó PASS và không có lỗi gửi request hoặc lỗi chạy script chưa xử lý. Nếu request không nhận được phản hồi, ghi rõ lỗi thực thi thay vì gán một HTTP status.
6.2. Kết quả tổng hợp
- Thời điểm chạy: [Điền thời điểm thực tế].
- Số request thực tế đã chạy: [Điền số lượng].
- Số test PASS: [Điền số lượng].
- Số test FAIL: [Điền số lượng].
- Lỗi thực thi, nếu có: [Ghi lỗi hoặc “Không có” theo kết quả thực tế].
Nếu cả 6 request chạy đầy đủ với đúng mã kiểm thử trên, kết quả mục tiêu là 16 PASS, 0 FAIL. Đây là mục tiêu kiểm thử, chưa phải kết luận của lần thực thi.
Minh chứng tổng hợp: Bổ sung ảnh collection-run.png.
<!-- ![Kết quả chạy toàn bộ Collection](collection-run.png) -->

6.3. Ghi nhận và xử lý sai lệch
Khi có test FAIL, cần đối chiếu request, script và response để xác định nguyên nhân. Không đổi điều kiện mong đợi chỉ nhằm làm test chuyển sang PASS. Nếu sửa cấu hình hoặc script, lưu thay đổi và chạy lại để có bằng chứng mới.
Hiện tượng có thể gặp	Hướng kiểm tra
Không phân giải được {{base_url}}	Kiểm tra biến trong Collection, chính tả và nơi lưu request
Không gửi được request	Kiểm tra kết nối mạng, URL và thông báo lỗi của Postman
POST/PUT không trả về nội dung dự kiến	Kiểm tra raw JSON, Content-Type và dữ liệu đầu vào
Không xuất hiện Test Results	Kiểm tra vị trí Post-response, lưu request và gửi lại
Test báo lỗi đọc JSON	Kiểm tra body thực tế; có thể là phản hồi HTML hoặc lỗi mạng trung gian
Sửa/xóa xong nhưng GET vẫn trả dữ liệu cũ	Đối chiếu giới hạn mô phỏng của JSONPlaceholder


Bảng trên là hướng xử lý tham khảo, không phải danh sách lỗi đã xảy ra trong lần thực hành.
7. Tổ chức sản phẩm và hướng dẫn chạy lại
7.1. Các file cần có trong repository
Đặt các file dưới đây tại thư mục gốc để đường dẫn ảnh trong báo cáo hoạt động khi được bật:
File	Nội dung
README.md	Báo cáo thực hành
JSONPlaceholder.postman_collection.json	Collection được xuất từ Postman, gồm request, biến và script
tc01-get-posts.png	Minh chứng TC01
tc02-get-post.png	Minh chứng TC02
tc03-post.png	Minh chứng TC03
tc04-put.png	Minh chứng TC04
tc05-delete.png	Minh chứng TC05
tc06-not-found.png	Minh chứng TC06
collection-run.png	Minh chứng chạy tổng hợp


Các file Collection và ảnh trong bảng là những thành phần cần bổ sung từ Postman; bảng không khẳng định chúng đã được tải lên repository.
7.2. Xuất và chạy lại Collection
Xuất: Trong menu của Collection, chọn Export hoặc More → Export collection, sau đó xuất JSON. Nếu được chọn phiên bản định dạng, sử dụng Collection v2.1. Đặt tên file JSONPlaceholder.postman_collection.json và tải lên repository.
Chạy lại:
1. Tải file Collection JSON từ repository.
2. Mở Postman → Import → chọn file.
3. Kiểm tra biến base_url có giá trị https://jsonplaceholder.typicode.com; điền lại nếu bị trống.
4. Mở Collection, kiểm tra có đủ 6 request và script Post-response.
5. Chạy từng request hoặc chạy Collection với 1 iteration.
6. Đối chiếu phản hồi với bảng tiêu chí tại mục 5.
7.3. Bổ sung hình ảnh vào README
Sau khi tải ảnh thực tế lên GitHub, bỏ cặp ký hiệu <!-- và --> bao quanh dòng chèn ảnh ở mục tương ứng. Ví dụ:
![Kết quả lấy danh sách bài viết](tc01-get-posts.png)
Tên file phải trùng chính xác, kể cả chữ hoa/chữ thường. Ảnh cần cho thấy request, status và kết quả kiểm thử; có thể dùng nhiều ảnh cho một tình huống nếu cần. Không sử dụng ảnh minh họa tự tạo thay cho bằng chứng chạy thực tế.
8. Nhận xét và kết luận
Thiết kế kiểm thử bao phủ bốn nhóm thao tác cơ bản trên tài nguyên bài viết: đọc, tạo, cập nhật và xóa; đồng thời có tình huống truy vấn tài nguyên không tồn tại. Tổng cộng 6 test case sử dụng 16 khối pm.test để đối chiếu mã trạng thái và dữ liệu phản hồi.
Việc tổ chức request trong Collection, khai báo biến địa chỉ gốc và viết script giúp quy trình kiểm thử có thể lặp lại. So với chỉ quan sát response, các phép khẳng định cung cấp tiêu chí đạt rõ ràng và giúp phát hiện sai lệch khi chạy lại.
Tuy nhiên, phạm vi này chỉ phản ánh một số hành vi của API mẫu. Nó chưa chứng minh độ an toàn, hiệu năng hoặc tính đúng đắn toàn diện của hệ thống. Đặc biệt, do dữ liệu thêm/sửa/xóa không được lưu thật, không thể dùng kết quả bài thực hành để kết luận về tính bền vững dữ liệu.
Kết luận thực thi hiện tại: Chưa đủ bằng chứng để xác nhận số lượng PASS/FAIL và mức độ đạt của các test case. Sau khi chạy Postman, cần cập nhật mục 6 và ảnh minh chứng trước khi đưa ra kết luận cuối cùng.
Hướng mở rộng phù hợp là kiểm thử lọc dữ liệu qua query parameter, kiểm tra kiểu dữ liệu/schema đầy đủ, và thực hành trên API có cơ sở dữ liệu thật để kiểm tra chuỗi tạo → đọc → cập nhật → xóa. Với API có xác thực, có thể bổ sung các tình huống thiếu token, token không hợp lệ và truy cập vượt quyền.
9. Tài liệu tham khảo
1. JSONPlaceholder — API mẫu và hướng dẫn sử dụng.
2. Postman — Viết script kiểm thử phản hồi.
3. Postman — Ví dụ script kiểm thử.
4. Postman — Sử dụng Collection Runner.
5. Postman — Xuất dữ liệu.
6. GitHub — Tạo repository.
