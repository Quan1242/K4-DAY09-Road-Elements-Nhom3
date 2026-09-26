# Annotation guideline — Traffic Light State & Ego-Relevance

**Version:** v2

## 1. Objective + scope

- **Mục tiêu:** Nhận diện và gán nhãn trạng thái tín hiệu (`state`) cùng độ liên quan đến làn đường xe chủ (`relevance: ego_lane` vs `other_lane`) của các cụm đèn giao thông tại các nút giao cắt đô thị và đại lộ. Dữ liệu phục vụ huấn luyện mô hình nhận thức và lập kế hoạch di chuyển tự động (Motion Planning) của xe tự lái cấp độ L3/L4.
- **Trong scope (bắt buộc gán nhãn):**
  - Mọi cụm đầu đèn tín hiệu giao thông (traffic light heads) có thể nhìn thấy, quay mặt về phía xe chủ hoặc xéo góc < 60 độ, điều khiển luồng phương tiện cơ giới.
  - Áp dụng cho cả cột đèn đứng bên đường, đèn gắn trên cột cao, hoặc cụm đèn treo trên xà ngang (gantry) vươn ra giữa đường.
  - Kích thước chiều cao phần vỏ nhìn thấy của đầu đèn ≥ 10 pixel.
- **Ngoài scope (bỏ qua - Ignore):**
  - Đèn tín hiệu dành riêng cho người đi bộ (biểu tượng hình người đứng/đi bộ, đếm giây người đi bộ qua đường).
  - Đèn tín hiệu quay mặt về hướng ngược lại (hướng đối diện) hoặc quay vuông góc hoàn toàn (điều khiển xe cắt ngang).
  - Đèn xe khác (đèn hậu, đèn pha), đèn chiếu sáng công cộng, biển quảng cáo, hình ảnh phản chiếu trên nắp capo, kính xe hoặc mặt đường.
  - Cụm đèn quá xa hoặc quá mờ (chiều cao < 10 pixel).

## 2. Annotation unit

- **Loại đơn vị:** Bounding Box 2D dạng chữ nhật (`rectangle`) cho từng cụm đầu đèn độc lập (Instance-level).
- **Quy tắc tách instance:** Mỗi đầu đèn vật lý có hộp vỏ (housing) riêng biệt là một instance riêng. Cột đèn có 3 đầu đèn đặt cạnh nhau (ví dụ: 1 đầu rẽ trái, 2 đầu đi thẳng) phải được gán thành 3 bounding box riêng biệt, không gộp chung vào 1 box lớn.
- **Dạng dữ liệu:** Gán theo từng ảnh tĩnh (`Shape`) đối với dữ liệu BDD100K; gán theo chuỗi đối tượng liên tục (`Track`) đối với video chuỗi frame LISA.

## 3. Geometry rule

- **Quy chuẩn Bounding Box:**
  - Dạng hình học: `rectangle` (2D axis-aligned bounding box).
  - Độ bao phủ: Tight box ôm khít toàn bộ phần vỏ ngoài nhìn thấy (visible housing/enclosure) của cụm đèn, bao gồm cả nắp che nắng (visors/cowls) của các bóng đèn nếu có.
  - Không kéo box bao gồm cả cột đèn (pole) hoặc thanh xà đỡ ngang (gantry arm).
  - Dung sai cho phép (tolerance): Độ lệch mép box không vượt quá 2 pixel ở mỗi cạnh so với biên vật lý của vỏ đèn. Không được cắt lẹm vào bóng đèn phát sáng và không để chừa khoảng trống nền trời/cây cối quá 2 pixel.

## 4. Taxonomy

Cấu trúc taxonomy gồm 1 Class duy nhất và 3 thuộc tính (Attributes) bắt buộc:

### Class: `traffic_light` (Shape: rectangle)

### Attribute 1: `state` (Trạng thái phát sáng của đèn)
- `red`: Đèn đang bật bóng đỏ (yêu cầu dừng).
- `yellow`: Đèn đang bật bóng vàng (chuẩn bị dừng / chuyển pha).
- `green`: Đèn đang bật bóng xanh (được phép di chuyển).
- `off`: Cả cụm đèn không sáng bóng nào (đèn tắt do hỏng hoặc ngắt điện).
- `wait on`: đèn chờ.
- `unknow` : không xác định được
- *Default:* `off`.

### Attribute 2: `relevance` (Độ liên quan đối với làn xe chủ)

- `ego_lane`: Đèn điều khiển làn đang đi (xe đi thẳng, đi rẽ,...).
- `other_lane`: Đèn của làn khác (đèn rẽ trái/phải khi xe đi thẳng, đèn làn xe buýt/làn phụ, hoặc đèn chiều ngược lại).
- `ambiguous`: Không rõ làn (mất vạch kẻ đường, xe đang đè vạch chuyển làn, góc chụp quá xéo hoặc nhiều đèn san sát không rõ tương quan). Tuyệt đối không đoán mò.
- *Default:* `ego_lane`.

### Attribute 3: `shape` (Hình dạng tín hiệu)

- `circle`: Bóng đèn tròn đặc thông thường (kể cả khi đèn tắt nhưng mặt kính tròn trơn).
- `arrow`: Tín hiệu hình mũi tên chỉ hướng (rẽ trái/phải, đi thẳng; kể cả khi đèn tắt nhưng thấy rõ khuôn mũi tên).
- `other`: Dạng khác (đèn đếm giây, chữ X, hoặc bị lóa/che khuất không rõ hình dạng tròn hay mũi tên).
- *Default:* `circle`.

## 5. Inclusion / exclusion

### Bắt buộc gán nhãn (Inclusion):
- Mọi đầu đèn phương tiện giao thông phía trước hướng di chuyển của xe.
- Đèn đang sáng hoặc đèn đang tắt nhưng nhìn rõ cụm vỏ (chiều cao ≥ 10px).
- Đèn bị che khuất một phần bởi cành cây, biển báo, xe tải nhưng vẫn nhận diện được tối thiểu 50% diện tích vỏ hoặc nhìn rõ bóng đèn phát sáng.

### Bỏ qua hoàn toàn (Exclusion):
- Đèn người đi bộ (kể cả khi treo chung trên cùng một cột với đèn giao thông cơ giới).
- Đèn quay lưng lại phía xe chủ (chỉ thấy mặt sau màu xám/đen của vỏ đèn).
- Đèn có chiều cao < 10px.
- Vùng phản chiếu ánh đèn trên mặt đường ướt, nắp capo, kính chắn gió.

## 6. Visibility / occlusion

- **Bị che khuất một phần (Occlusion):**
  - Nếu đầu đèn bị che khuất < 50% diện tích (ví dụ bị cành cây mảnh, dây điện che ngang): Vẽ box ôm sát phần vỏ nhìn thấy (visible part).
  - Nếu bị che > 50% nhưng bóng đèn đang phát sáng rõ rệt: Vẫn vẽ box bao quanh phần nhìn thấy của bóng đèn và vỏ đèn.
  - Nếu bị che > 50% và đèn tắt (không thể nhận diện rõ cấu trúc cụm đèn): Bỏ qua (Ignore).
- **Bị cắt ở mép ảnh (Truncation):** Nếu đầu đèn bị viền ảnh cắt ngang, vẽ box ôm sát phần nằm bên trong khung hình ảnh.
- **Điều kiện ban đêm và lóa đèn (Low Visibility / Glare):**
  - Ban đêm đèn phát sáng tạo vầng hào quang (halo/glare) tỏa rộng: Bounding box phải căn theo kích thước thực tế của vỏ đèn (hoặc ước lượng kích thước bóng đèn thực tế), **không** được vẽ bao trọn toàn bộ quầng sáng lóa tỏa ra bầu trời.
  - Nếu ánh sáng chói làm mờ hoàn toàn màu sắc: Đặt `state = unknow`.

## 7. Ambiguity / escalation

Quy định chuẩn hóa 4 mức quyết định để đảm bảo thể hiện minh bạch trong export CVAT:
1. **LABEL:** Đủ bằng chứng hình ảnh -> Vẽ box `traffic_light` và chọn các giá trị tương ứng (`state`, `relevance`, `shape`).
2. **IGNORE:** Không đủ điều kiện kích thước (< 10px) hoặc nằm ngoài scope -> Không tạo bounding box.
3. **UNKNOWN:** Khi nhìn rõ cụm đèn nhưng không chắc chắn màu đèn hoặc hình dạng bóng đèn -> Chọn `state = unknow` hoặc `shape = other`.
4. **ESCALATE / AMBIGUOUS:**
   - Nếu không xác định được đèn thuộc làn xe mình hay làn khác: Đặt `relevance = ambiguous`.
   - Nếu toàn bộ khung ảnh có vấn đề nghiêm trọng (camera hỏng, mờ mịt hoàn toàn không thể quan sát tín hiệu giao thông): Gán thẻ tag cấp ảnh `image_escalate`.

## 8. Temporal rule

- **Ảnh tĩnh (BDD100K):** Áp dụng theo dạng Shape đơn lẻ cho từng ảnh độc lập.
- **Chuỗi Video Frame (LISA):**
  - Duy trì cùng một Track ID cho cùng một đầu đèn xuyên suốt các frame liên tiếp khi đầu đèn còn xuất hiện trong khung hình.
  - Thuộc tính `state` là `mutable`: Nếu đèn chuyển từ xanh sang vàng hoặc đỏ giữa các frame, annotator cập nhật giá trị `state` tại keyframe tương ứng; hệ thống giữ nguyên Track ID.
  - Thuộc tính `relevance` và `shape` là không đổi (`immutable`) đối với một đầu đèn dọc theo clip.

## 9. Examples

Bảng ví dụ tham chiếu từ các ảnh trong tập `example` và `calibration`:

| sample_id | Thấy gì | Expected output | Rule áp dụng |
|---|---|---|---|


## 10. Common mistakes

1. **Gộp chung nhiều đầu đèn vào một box:** Vẽ 1 box khổng lồ ôm cả cụm 3 đầu đèn. *Khắc phục: Mỗi đầu đèn vật lý có hướng điều khiển riêng phải là một bounding box riêng biệt.*
2. **Nhầm lẫn đèn làn rẽ thành đèn làn xe chủ:** Nhìn thấy đèn xanh bật ở làn rẽ trái liền gán `relevance = ego_lane`. *Khắc phục: Quan sát kỹ vạch kẻ đường, mũi tên trên mặt đường và hình dạng bóng đèn (shape=arrow) để xác định đúng làn.*
3. **Vẽ box phình to theo quầng sáng lóa ban đêm:** Kéo box rộng gấp 3 lần kích thước thật của đèn vì ánh hào quang ban đêm. *Khắc phục: Căn chỉnh viền box theo mép vỏ đèn hoặc đường kính bóng đèn thật.*
4. **Vẽ cả đèn người đi bộ:** Gán nhãn luôn cả đèn hình người qua đường bên cạnh. *Khắc phục: Đèn người đi bộ thuộc Exclusion scope (ngoài phạm vi), tuyệt đối không vẽ.*
