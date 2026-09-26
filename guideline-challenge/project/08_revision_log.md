# Revision log

Guideline v1 = bản nháp đầu; v2 = sau cập nhật taxonomy; v3 = sau khi đồng bộ guideline với problem statement và downstream contract; v4 = sau khi đồng bộ các giá trị attribute với contract. v5 là consistency repair trước freeze: giới hạn dữ liệu trong catalog của repo, bổ sung temporal rule, và làm rõ ví dụ. v5 **chưa** phải revision dựa trên blind test. Mỗi lần tăng `Version` trong `02_guideline.md`, ghi đổi gì, vì sao và bằng chứng thật (sample ID, calibration report, clarification log hoặc peer feedback). Bằng chứng nào chưa có phải ghi rõ là chưa có; không tự tạo.

Cột Version ghi dạng `v1`, `v2`, `v3` — `make status` tìm dòng bảng có `v2` và dòng có `v3`.

| Version | Đổi gì | Vì sao | Bằng chứng |
|---|---|---|---|
| v1 | Khởi tạo bản nháp guideline ban đầu với đầy đủ 10 mục bắt buộc cho bài toán Traffic Light State & Ego-Relevance | Thiết lập scope, ontology, và các quy tắc gán nhãn chuyển giao được trước khi calibration | Thảo luận thiết kế nhóm và downstream contract tại `01_problem_statement.md` |
| v2 | Bổ sung trạng thái `wait on` và `unknow`; đồng bộ quy tắc UNKNOWN và trường hợp lóa sáng với `unknow` | Cập nhật taxonomy trạng thái theo yêu cầu và tránh dùng giá trị `off_or_unk` không có trong danh sách nhãn | Yêu cầu cập nhật guideline; chưa có sample_id calibration kèm theo |
| v3 | Đồng bộ state thành `red` / `yellow` / `green` / `off_or_unk`; cập nhật scope 12 × 12 px, dữ liệu S2TLD và đầu ra LABEL / IGNORE / UNKNOWN / ESCALATE | Làm guideline khớp downstream contract và quy tắc an toàn cho Behavior Planning/AEB | Bản v3 được ghi trong revision history; chưa có sample_id calibration kèm theo |
| v4 | Đồng bộ `state` thành `undefined` / `red` / `yellow` / `green` / `unknown` / `off`; phân biệt giá trị thiếu trong dữ liệu với trạng thái không xác định qua ảnh | Khớp taxonomy mới và giữ riêng quyết định đèn tắt (`off`) với màu/trạng thái không rõ (`unknown`) | Yêu cầu cập nhật problem statement; chưa có sample_id calibration kèm theo |
| v5 | Giới hạn nguồn ảnh vào catalog trong repo; bổ sung quy tắc state theo từng ảnh; thay ví dụ dùng sample ID không có trong catalog bằng ví dụ minh hoạ; đồng bộ UNKNOWN thành `state = unknown` | Gỡ mâu thuẫn giữa guideline, downstream contract và quy định dữ liệu của repo trước khi freeze | `01_problem_statement.md`, `03_cvat_labels.json`, README mục dùng ảnh trong `data/`; chưa có bằng chứng calibration hoặc blind handoff cho v5 |
