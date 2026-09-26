# Revision log

Guideline v1 = bản nháp đầu; v2 = sau cập nhật taxonomy; v3 = sau khi đồng bộ guideline với problem statement và downstream contract; v4 = sau khi đồng bộ các giá trị attribute với contract. Mỗi lần tăng `Version` trong
`02_guideline.md`, thêm một hoặc nhiều dòng vào bảng: đổi gì và vì sao, kèm bằng chứng (sample_id, dòng
calibration report, câu hỏi trong clarification log, feedback của peer).

Cột Version ghi dạng `v1`, `v2`, `v3` — `make status` tìm dòng bảng có `v2` và dòng có `v3`.

| Version | Đổi gì | Vì sao | Bằng chứng |
|---|---|---|---|
| v1 | Khởi tạo bản nháp guideline ban đầu với đầy đủ 10 mục bắt buộc cho bài toán Traffic Light State & Ego-Relevance | Thiết lập scope, ontology, và các quy tắc gán nhãn chuyển giao được trước khi calibration | Thảo luận thiết kế nhóm và downstream contract tại `01_problem_statement.md` |
| v2 | Bổ sung trạng thái `wait on` và `unknow`; đồng bộ quy tắc UNKNOWN và trường hợp lóa sáng với `unknow` | Cập nhật taxonomy trạng thái theo yêu cầu và tránh dùng giá trị `off_or_unk` không có trong danh sách nhãn | Yêu cầu cập nhật guideline; chưa có sample_id calibration kèm theo |
| v3 | Đồng bộ state thành `red` / `yellow` / `green` / `off_or_unk`; cập nhật scope 12 × 12 px, dữ liệu 20 ảnh S2TLD và đầu ra LABEL / IGNORE / UNKNOWN / ESCALATE | Làm guideline khớp downstream contract, phạm vi dữ liệu và quy tắc an toàn cho Behavior Planning/AEB | `01_problem_statement.md`; nguồn S2TLD Kaggle ghi trong guideline; chưa có sample_id calibration kèm theo |
| v4 | Đồng bộ `state` thành `undefined` / `red` / `yellow` / `green` / `unknown` / `off`; phân biệt giá trị thiếu trong dữ liệu với trạng thái không xác định qua ảnh | Khớp taxonomy mới và giữ riêng quyết định đèn tắt (`off`) với màu/trạng thái không rõ (`unknown`) | Yêu cầu cập nhật problem statement; chưa có sample_id calibration kèm theo |
