# Mini guideline — bài làm cá nhân

> Giữ lại các quyết định sau khi self-QC. Các ca mơ hồ bên dưới cần đối chiếu lại với ảnh thật trước khi nộp.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe ô tô con chở người, thân xe thấp, có khoang hành khách và cốp riêng: sedan, hatchback, SUV, taxi con. | Taxi, xe cá nhân trong ảnh giao lộ (ví dụ `drive_003`, `drive_004`). Nếu thân xe liền khối cao và vuông như xe chở hàng/xe nhiều chỗ thì không chắc là `car`, xem lớp `van`. |
| `truck` | Xe tải chở hàng, có đầu cabin tách biệt với thùng hàng hoặc thùng hàng lớn phía sau (thùng kín, thùng lửng, xe ben, đầu kéo). | Xe tải thùng đỏ trong `drive_008`. Khác `van`: `truck` có cabin và thùng tách rời rõ rệt. |
| `bus` | Xe buýt/xe khách cỡ lớn, thân dài, nhiều cửa sổ hai bên, chở nhiều hành khách (kể cả xe buýt khớp nối). | Xe buýt trong `drive_002`, `drive_008`, `drive_009`. Khác `van`: `bus` dài và lớn hơn nhiều, nhiều hàng cửa sổ. |
| `van` | Xe thân liền khối, cao và vuông hơn `car`, không có cabin tách như `truck`: minivan, xe 7–16 chỗ cỡ nhỏ, xe van chở hàng/giao hàng. | Xe nhỏ thân cao màu xanh trong `drive_006`. Nếu không chắc là `van`, `car` hay `bus` thì không tự chọn, ghi case ID rồi hỏi giảng viên. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: mỗi xe hợp lệ có **một box riêng**, ôm sát phần thân xe nhìn thấy được, không lấy thêm mặt đường, bóng đổ, trời hay xe bên cạnh. Năm xe là năm box, kể cả cùng lớp.
- Vật thể bị che khuất: vẫn gán box quanh **phần nhìn thấy được** nếu nhận diện được lớp. Hai xe chồng nhau thì vẽ hai box riêng trên phần nhìn thấy của từng xe.
- Vật thể bị cắt bởi rìa ảnh: box chạm đúng rìa ảnh, không đoán phần nằm ngoài khung.
- Vật thể rất nhỏ/mơ hồ: nếu chỉ còn mảnh rất nhỏ hoặc không thể xác định chắc lớp thì **không tự đoán**; ghi vào bảng bên dưới kèm case ID và hỏi giảng viên.
- Xe đang đỗ: vẫn gán theo loại xe.
- Không gán: người đi bộ, xe đạp, mô tô/xe máy, biển báo, đèn giao thông, phản chiếu trong kính/gương, hình xe trên bảng quảng cáo, và mọi vật thể ngoài bốn lớp của schema.
- Định dạng nhãn: mỗi dòng đúng 5 số `<class_id> <x_center> <y_center> <width> <height>`, chuẩn hóa trong `[0, 1]`. Ảnh không có xe thuộc schema vẫn phải có file `.txt` rỗng.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_006` | Xe nhỏ thân cao màu xanh lá, khó phân biệt giữa `van` (minivan) và `car`. | **Chưa tự chọn.** Ghi case ID `VAN-01` và hỏi giảng viên; trong lúc chờ không gán lớp. | Guideline yêu cầu van mơ hồ với `car` hoặc `bus` thì không tự chọn, ghi case ID và hỏi. |
| `drive_008` | Xe buýt khớp nối dài (hai thân nối với nhau) đi cạnh xe tải thùng đỏ. | Xe buýt khớp nối là **một xe, một box** lớp `bus`; xe tải thùng đỏ là một box lớp `truck`. Cần xác nhận lại với giảng viên nếu chưa chắc. | Mỗi xe hợp lệ là một box riêng; xe buýt khớp nối vẫn là một phương tiện, thuộc lớp `bus`. |
| `drive_009` | Nhiều xe buýt đứng sát và chồng lên nhau trong cảnh đông, cùng xe tải nhỏ màu xanh lá phía trước. | Vẽ box riêng cho từng xe trên phần nhìn thấy; xe bị che nhưng còn nhận ra là xe buýt thì vẫn gán `bus`. Xe tải nhỏ màu xanh lá gán `truck` nếu thấy rõ cabin tách thùng, nếu không rõ thì ghi case ID `VAN-02` và hỏi. | Hai xe chồng nhau thì vẽ hai box riêng; vật thể bị che vẫn gán nếu nhận diện được lớp; ca mơ hồ thì không đoán. |
| `drive_008` | Có xe máy/người đi xe máy ở giữa giao lộ. | Không gán box. | Mô tô/xe máy không thuộc bốn lớp của schema. |
| `drive_004`, `drive_010` | Ảnh ban đêm, nhiều xe ở xa chỉ còn là mảng sáng nhỏ, thấy đèn xe nhưng không rõ thân xe. | Chỉ gán những xe còn nhận ra được lớp và hình dạng; các mảng sáng quá nhỏ không xác định được lớp thì bỏ qua và ghi chú. | Không tự đoán khi không thể xác định chắc lớp; box phải ôm sát thân xe nhìn thấy được. |
