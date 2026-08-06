# Requirements Document

## Introduction

Hệ thống **SDLC AI Agents** là một nền tảng gồm nhiều AI Agent chuyên biệt, mỗi agent đảm nhận một công đoạn cụ thể trong vòng đời phát triển phần mềm (Software Development Life Cycle). Hệ thống cho phép các agent hoạt động độc lập hoặc phối hợp tuần tự/song song nhằm hỗ trợ đội phát triển phần mềm tăng tốc độ, nâng cao chất lượng và giảm thiểu rủi ro trong toàn bộ quy trình phát triển.

Các công đoạn được hỗ trợ bao gồm: Phân tích yêu cầu, Thiết kế hệ thống, Sinh Task List, Lập trình, Kiểm thử, Triển khai và Vận hành/Giám sát.

Ngoài các agent chuyên biệt, hệ thống còn bao gồm: Orchestrator (điều phối pipeline), Context_Store (chia sẻ Artifact), Knowledge_Store (cơ sở tri thức vector database cho RAG), FAQ_Store (trả lời trực tiếp câu hỏi thường gặp), Sentiment_Agent (phân tích cảm xúc và chuyển hướng hỗ trợ), hệ thống phân quyền đa cấp (Organization/Team/Project), ghi nhật ký prompt AI, đánh giá chất lượng AI (benchmarking), Admin Portal quản trị tập trung, và các cơ chế quản lý tự động (RADIO reporting, Change Request, Q&A, Deliverables/Limitations, Definition of Done, Approve Checklist, Self-Review Loop).

---

## Glossary

- **SDLC_System**: Hệ thống tổng thể quản lý và điều phối toàn bộ các AI Agent trong vòng đời phát triển phần mềm.
- **Orchestrator**: Agent điều phối trung tâm, chịu trách nhiệm khởi tạo, phân công và theo dõi các agent chuyên biệt.
- **Requirements_Agent**: Agent chuyên phân tích và tổng hợp yêu cầu phần mềm từ đầu vào của người dùng.
- **Design_Agent**: Agent chuyên tạo ra tài liệu thiết kế hệ thống (kiến trúc, mô hình dữ liệu, API).
- **Task_Agent**: Agent chuyên phân rã tài liệu thiết kế thành danh sách task triển khai cụ thể, có thứ tự ưu tiên và dependency.
- **UIUX_Agent**: Agent chuyên sinh HTML prototype (wireframe/mockup) từ tài liệu thiết kế, cho phép stakeholder xem và confirm giao diện trực quan trên browser.
- **Coding_Agent**: Agent chuyên sinh mã nguồn dựa trên tài liệu thiết kế đã được phê duyệt.
- **Testing_Agent**: Agent chuyên tạo ca kiểm thử, thực thi kiểm thử và báo cáo kết quả.
- **Deployment_Agent**: Agent chuyên thực hiện quy trình CI/CD và triển khai ứng dụng lên môi trường đích.
- **Operations_Agent**: Agent chuyên giám sát ứng dụng vận hành, cảnh báo sự cố và đề xuất giải pháp.
- **Sentiment_Agent**: Agent chạy song song để phân tích cảm xúc người dùng trong mỗi prompt, phát hiện sự thất vọng/khó chịu và kích hoạt chuyển hướng hỗ trợ chuyên gia.
- **ProjectHealth_Agent**: Agent tự động tổng hợp và báo cáo sức khỏe dự án (tiến độ, chất lượng, rủi ro, chi phí, velocity) từ tất cả nguồn dữ liệu trong hệ thống.
- **Artifact**: Sản phẩm đầu ra của một agent (tài liệu yêu cầu, tài liệu thiết kế, task list, mã nguồn, báo cáo kiểm thử, v.v.).
- **Pipeline**: Chuỗi các agent được thực thi tuần tự hoặc song song để hoàn thành một mục tiêu phát triển.
- **Human_Reviewer**: Thành viên nhóm phát triển thực hiện xem xét và phê duyệt Artifact.
- **Context_Store**: Kho lưu trữ trung tâm chia sẻ ngữ cảnh và Artifact giữa các agent.
- **Knowledge_Store**: Cơ sở dữ liệu vector (vector database) lưu trữ kiến thức nội bộ doanh nghiệp/dự án dưới dạng embeddings, hỗ trợ semantic search để các agent truy vấn thông tin theo ngữ cảnh (RAG pattern).
- **RAG (Retrieval-Augmented Generation)**: Kỹ thuật bổ sung ngữ cảnh từ cơ sở tri thức vào prompt của AI trước khi sinh output, giúp câu trả lời chính xác hơn trong bối cảnh cụ thể.
- **FAQ_Store**: Kho lưu trữ các cặp câu hỏi-câu trả lời đã được duyệt sẵn, hỗ trợ so khớp ngữ nghĩa (semantic matching) để trả lời trực tiếp mà không cần gọi mô hình AI.
- **SKU (Software Knowledge Utilization)**: Hệ thống quản lý tri thức và hỗ trợ chuyên gia bên ngoài, nơi tiếp nhận issue khi AI Agent không đáp ứng được nhu cầu người dùng.
- **Template_Registry**: Kho lưu trữ source code tiêu chuẩn (baseline templates) đã qua kiểm thử, bao gồm project scaffolds, feature modules và architecture patterns.
- **Doc_Template_Registry**: Kho lưu trữ template chuẩn cho tài liệu quy trình (requirements, design, tasks) và cấu hình hệ thống (skills, hooks, steering).
- **Eval_Store**: Kho lưu trữ các bộ test đánh giá chất lượng AI (evaluation test suites), bao gồm input/ground truth/scoring criteria, dùng để benchmark và nghiệm thu chất lượng output của agent.
- **Project_Brain**: Bộ nhớ dự án (second brain) tự động tích lũy kiến thức riêng của từng project trong quá trình làm việc — bao gồm artifacts, decision logs, relationships, terminology — tách biệt với Knowledge_Store (kiến thức chung công ty).
- **RADIO**: Framework báo cáo tiến độ 5 thành phần (Review, Action, Difficulty, Information, Outcome) được AI tự động sinh từ dữ liệu thực thi.
- **DoD (Definition of Done)**: Bộ tiêu chí hoàn thành bắt buộc cho mỗi artifact/phase, được AI tự động kiểm tra trước khi đánh dấu "Done".
- **CR (Change Request)**: Yêu cầu thay đổi từ client/stakeholder, được quản lý theo lifecycle: Pending → Approved → In Progress → Review → Done.
- **5WHY (Five Whys)**: Phương pháp hỏi "Tại sao?" liên tiếp 5 lần để tìm nguyên nhân gốc rễ (root cause) của bug/issue.

---

## Requirements

### Requirement 1: Khởi tạo và Quản lý Agent

**User Story:** Là một kỹ sư phần mềm, tôi muốn khởi tạo và cấu hình từng AI Agent cho từng công đoạn SDLC, để có thể tùy chỉnh hệ thống phù hợp với quy trình của đội nhóm.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp cơ chế đăng ký từng agent thuộc một trong chín vai trò định nghĩa sẵn ban đầu (Requirements_Agent, Design_Agent, UIUX_Agent, Task_Agent, Coding_Agent, Testing_Agent, Deployment_Agent, Operations_Agent, ProjectHealth_Agent) — có thể mở rộng thêm vai trò mới thông qua plugin mechanism theo R32-C1 — với các trường bắt buộc: tên (tối đa 100 ký tự), vai trò, mô hình AI sử dụng; và các trường tùy chọn: tham số cấu hình bổ sung.
2. WHEN một agent được đăng ký thành công, THE SDLC_System SHALL gán cho agent một định danh duy nhất trong phạm vi toàn hệ thống (agent_id) và xác nhận trạng thái "active".
3. IF cấu hình agent thiếu bất kỳ trường bắt buộc nào (tên, vai trò, mô hình AI), THEN THE SDLC_System SHALL trả về thông báo lỗi liệt kê cụ thể từng trường còn thiếu và không đăng ký agent.
4. IF một agent đang được cập nhật cấu hình trong khi có task đang chạy, THEN THE SDLC_System SHALL ghi nhận thay đổi và áp dụng cấu hình mới sau khi task hiện tại của agent đó hoàn thành.
5. WHEN một agent bị vô hiệu hóa (deactivated), THE SDLC_System SHALL chuyển trạng thái agent sang "inactive", ngăn agent đó nhận nhiệm vụ mới, và cho phép task đang chạy hoàn thành bình thường trước khi deactivate có hiệu lực.
6. IF một agent mới được đăng ký với tên và vai trò trùng với một agent đang ở trạng thái "active", THEN THE SDLC_System SHALL từ chối đăng ký và trả về thông báo lỗi xung đột.
7. THE SDLC_System SHALL cho phép cấu hình tham số temperature (giá trị từ 0.0 đến 2.0) cho từng agent; WHEN temperature = 0, agent SHALL tạo ra output deterministic (cùng input cho cùng output); WHEN temperature > 0, agent SHALL cho phép output đa dạng hơn tỷ lệ thuận với giá trị temperature; IF không được cấu hình, THEN temperature SHALL mặc định là 0.7.

---

### Requirement 2: Agent Phân tích Yêu cầu (Requirements_Agent)

**User Story:** Là một Business Analyst, tôi muốn AI tự động phân tích và cấu trúc hóa yêu cầu từ mô tả thô, để rút ngắn thời gian viết tài liệu yêu cầu.

#### Acceptance Criteria

1. WHEN một mô tả yêu cầu thô (raw requirement description) được cung cấp, THE Requirements_Agent SHALL phân tích và tạo ra tài liệu yêu cầu có cấu trúc bao gồm các phần bắt buộc: tiêu đề, mô tả chi tiết, loại phân loại và ít nhất một tiêu chí chấp nhận, trong vòng 60 giây.
2. THE Requirements_Agent SHALL phân loại từng yêu cầu thành một trong các loại: functional, non-functional, hoặc constraint.
3. IF một yêu cầu có thể thuộc nhiều loại, THEN THE Requirements_Agent SHALL chọn loại phù hợp nhất và ghi lý do phân loại kèm theo yêu cầu đó.
4. WHEN yêu cầu đầu vào chứa mâu thuẫn logic giữa hai hay nhiều điều kiện, THE Requirements_Agent SHALL gắn cờ (flag) các yêu cầu mâu thuẫn kèm: định danh từng yêu cầu xung đột và phát biểu cụ thể các điều kiện mâu thuẫn nhau.
5. THE Requirements_Agent SHALL liên kết từng yêu cầu với ít nhất một tiêu chí chấp nhận đo lường được, mỗi tiêu chí phải chứa: actor, action và observable outcome.
6. IF đầu vào không chứa đủ thông tin để tạo ít nhất một yêu cầu hoàn chỉnh (bao gồm tiêu đề + mô tả + ít nhất một điều kiện chấp nhận), THEN THE Requirements_Agent SHALL trả về danh sách câu hỏi làm rõ cho Human_Reviewer.
7. WHEN tài liệu yêu cầu được lưu hoặc cập nhật, THE Requirements_Agent SHALL ghi Artifact vào Context_Store với số phiên bản nguyên tăng thêm 1 so với phiên bản trước đó.

---

### Requirement 3: Agent Thiết kế Hệ thống (Design_Agent)

**User Story:** Là một Software Architect, tôi muốn AI tự động tạo tài liệu thiết kế từ tài liệu yêu cầu, để giảm thời gian thiết kế kiến trúc ban đầu.

#### Acceptance Criteria

1. WHEN tài liệu yêu cầu đã được phê duyệt được cung cấp, THE Design_Agent SHALL tạo tài liệu thiết kế bao gồm: sơ đồ kiến trúc (architecture diagram), mô hình dữ liệu (data model entities) và đặc tả API (API endpoints) trong vòng 120 giây.
2. WHEN tài liệu thiết kế được tạo xong, THE Design_Agent SHALL đảm bảo mọi yêu cầu được gắn nhãn "functional" trong tài liệu yêu cầu đều được ánh xạ tới ít nhất một thành phần thiết kế (named element trong architecture diagram, data model entity, hoặc API endpoint).
3. WHEN tài liệu yêu cầu thay đổi phiên bản, THE Design_Agent SHALL phát hiện sự thay đổi và cập nhật tài liệu thiết kế tương ứng trong vòng 120 giây, gắn annotation "CHANGED" trên mỗi phần bị ảnh hưởng kèm danh sách tên thành phần thay đổi.
4. IF tài liệu yêu cầu đầu vào chưa được Human_Reviewer phê duyệt (trạng thái khác "approved"), THEN THE Design_Agent SHALL từ chối xử lý và trả về thông báo yêu cầu phê duyệt.
5. WHEN tài liệu thiết kế được tạo thành công, THE Design_Agent SHALL lưu dưới dạng Artifact vào Context_Store với phiên bản tương ứng với phiên bản tài liệu yêu cầu đầu vào.

---

### Requirement 4: Agent Thiết kế UI/UX (UIUX_Agent)

**User Story:** Là một Product Designer, tôi muốn AI tự động sinh HTML prototype từ tài liệu thiết kế, để stakeholder có thể xem và confirm giao diện trực quan trên browser trước khi bắt đầu code.

#### Acceptance Criteria

1. WHEN tài liệu thiết kế có trạng thái "approved" được cung cấp, THE UIUX_Agent SHALL sinh HTML prototype bao gồm: một file HTML cho mỗi screen/page, sử dụng Bootstrap (mặc định) hoặc CSS framework được cấu hình, với layout responsive (mobile/tablet/desktop).
2. THE UIUX_Agent SHALL tạo navigation links giữa các screens để người dùng có thể click và test user flow trực tiếp trên browser mà không cần backend.
3. THE UIUX_Agent SHALL sinh kèm file spec Markdown cho mỗi screen, mô tả: interaction logic (click, hover, form submit behavior), states (loading, empty, error, success), edge cases (long text, no data), và accessibility notes (ARIA labels, keyboard navigation).
4. THE UIUX_Agent SHALL đảm bảo prototype tuân thủ data model từ Design_Agent: hiển thị đúng fields, đúng data types, và đúng relationships được mô tả trong tài liệu thiết kế.
5. THE UIUX_Agent SHALL sinh prototype dưới dạng static HTML + CSS (plaintext, diff-able trên Git theo R16-C2); KHÔNG sử dụng JavaScript phức tạp hoặc build tools — prototype phải mở được trực tiếp bằng browser mà không cần server.
6. THE SDLC_System SHALL cho phép cấu hình CSS framework mặc định per-project: Bootstrap (default), TailwindCSS, Material UI, hoặc custom CSS; IF không cấu hình, THEN SHALL sử dụng Bootstrap latest stable version.
7. WHEN Human_Reviewer review prototype, THE SDLC_System SHALL cho phép annotate trực tiếp trên từng screen (comment per-element hoặc per-area) để UIUX_Agent sửa lại; annotations SHALL được lưu kèm artifact.
8. THE UIUX_Agent SHALL tổ chức output theo cấu trúc thư mục chuẩn nằm trong folder feature spec tương ứng (theo R16-C7):
   ```
   /docs/specs/{feature-name}/ui/
   ├── index.html          — landing/home + navigation tới tất cả screens
   ├── screens/            — từng screen (*.html)
   ├── specs/              — interaction specs per screen (*.md)
   ├── assets/             — images, icons nếu có
   └── design-tokens.json  — colors, spacing, typography tokens
   ```
9. WHEN prototype được tạo thành công, THE UIUX_Agent SHALL lưu dưới dạng Artifact vào Context_Store với phiên bản tương ứng với phiên bản tài liệu thiết kế đầu vào.
10. THE Task_Agent SHALL sử dụng UI prototype làm input bổ sung khi sinh task list: mỗi screen/component trong prototype SHALL được map thành ít nhất một task frontend cụ thể.

---

### Requirement 5: Agent Sinh Task List (Task_Agent)

**User Story:** Là một Tech Lead, tôi muốn AI tự động phân rã tài liệu thiết kế thành danh sách task triển khai cụ thể, để đội nhóm có thể bắt tay vào code ngay mà không mất thời gian breakdown thủ công.

#### Acceptance Criteria

1. WHEN tài liệu thiết kế có trạng thái "approved" được cung cấp, THE Task_Agent SHALL phân rã thành danh sách task triển khai, mỗi task bao gồm: tiêu đề, mô tả chi tiết, acceptance criteria, estimated effort (S/M/L/XL), priority (P0-P3), dependencies (task nào cần hoàn thành trước), và suggested assignee role.
2. THE Task_Agent SHALL đảm bảo mọi thành phần trong tài liệu thiết kế (architecture components, data models, API endpoints) đều được cover bởi ít nhất một task; IF phát hiện thành phần chưa có task, THEN SHALL tự động tạo task bổ sung.
3. THE Task_Agent SHALL sắp xếp tasks theo thứ tự thực hiện hợp lý dựa trên dependency graph: tasks không có dependency có thể chạy song song, tasks có dependency phải chạy sau task phụ thuộc.
4. THE Task_Agent SHALL nhóm tasks theo feature/module và gắn labels tương ứng để dễ lọc và theo dõi tiến độ.
5. IF tài liệu thiết kế chứa thành phần quá lớn để thực hiện trong một task đơn lẻ (estimated effort > XL hoặc > 5 ngày công), THEN THE Task_Agent SHALL tự động chia nhỏ thành sub-tasks với dependency nội bộ.
6. WHEN task list được tạo thành công, THE Task_Agent SHALL lưu dưới dạng Artifact vào Context_Store kèm metadata: phiên bản thiết kế tương ứng, tổng số tasks, phân bố effort, và critical path (chuỗi tasks dài nhất quyết định timeline).
7. THE Task_Agent SHALL hỗ trợ cập nhật task list khi tài liệu thiết kế thay đổi phiên bản: phát hiện thành phần thay đổi, thêm/sửa/xóa tasks tương ứng, và đánh dấu tasks bị ảnh hưởng với annotation "UPDATED".

---

### Requirement 6: Agent Lập trình (Coding_Agent)

**User Story:** Là một Developer, tôi muốn AI sinh mã nguồn từ tài liệu thiết kế, để tăng tốc độ triển khai các tính năng mới.

#### Acceptance Criteria

1. WHEN tài liệu thiết kế có trạng thái "approved" được cung cấp, THE Coding_Agent SHALL sinh mã nguồn cho từng thành phần được mô tả trong tài liệu thiết kế theo ngôn ngữ lập trình được cấu hình.
2. WHEN mã nguồn được sinh ra, THE Coding_Agent SHALL sinh kèm unit test cho mỗi function/method public được tạo ra, đảm bảo độ phủ (code coverage) tối thiểu 80% trên mã sinh ra.
3. WHEN mã nguồn được sinh ra, THE Coding_Agent SHALL kiểm tra mã theo bộ quy tắc linting được cấu hình; IF mã không vượt qua kiểm tra linting, THEN THE Coding_Agent SHALL tự động sửa lỗi linting và chỉ xuất ra mã đã pass; IF auto-fix không thể giải quyết lỗi linting (ví dụ: vi phạm complexity limit), THEN SHALL gắn cờ các lỗi còn lại kèm mô tả và thông báo cho Human_Reviewer.
4. IF tài liệu thiết kế chứa thành phần không đủ thông tin để sinh mã (thiếu input/output types, thiếu business logic description, hoặc thiếu API contract), THEN THE Coding_Agent SHALL gắn cờ thành phần đó kèm danh sách thông tin còn thiếu và gửi yêu cầu bổ sung đặc tả đến Design_Agent.
5. WHEN mã nguồn được lưu, THE Coding_Agent SHALL ghi vào Context_Store kèm metadata: ngôn ngữ lập trình, phiên bản thiết kế tương ứng, danh sách file được tạo và timestamp.
6. FOR ALL mã nguồn được sinh ra, THE Coding_Agent SHALL quét và đảm bảo không chứa thông tin xác thực nhúng cứng (hardcoded credentials, API keys, passwords) trong mã; IF phát hiện, THEN SHALL thay thế bằng tham chiếu đến biến môi trường.

---

### Requirement 7: Agent Kiểm thử (Testing_Agent)

**User Story:** Là một QA Engineer, tôi muốn AI tự động tạo và thực thi ca kiểm thử, để đảm bảo chất lượng phần mềm trước khi triển khai.

#### Acceptance Criteria

1. WHEN mã nguồn và tài liệu yêu cầu được cung cấp, THE Testing_Agent SHALL tạo bộ ca kiểm thử bao gồm: unit test, integration test và acceptance test (test theo tiêu chí chấp nhận của từng yêu cầu).
2. WHEN bộ ca kiểm thử được tạo xong, THE Testing_Agent SHALL thực thi toàn bộ và tạo báo cáo kết quả bao gồm: tên từng test case, loại test (unit/integration/acceptance), trạng thái pass/fail, tỷ lệ độ phủ mã, và mô tả lỗi chi tiết cho các ca fail.
3. WHEN kết quả kiểm thử có ít nhất một ca fail, THE Testing_Agent SHALL phân loại từng lỗi theo mức độ nghiêm trọng: critical (gây crash hoặc mất dữ liệu), major (chức năng chính không hoạt động đúng), hoặc minor (lỗi UI/cosmetic không ảnh hưởng logic).
4. IF tỷ lệ pass của bộ ca kiểm thử dưới 90%, THEN THE Testing_Agent SHALL chặn việc chuyển Artifact sang Deployment_Agent và gửi thông báo cho Human_Reviewer kèm: tỷ lệ pass thực tế, số ca fail, và danh sách các lỗi critical.
5. WHEN mã nguồn chứa các thành phần parser/serializer, THE Testing_Agent SHALL kiểm tra tính chất round-trip: với mọi đối tượng hợp lệ, quá trình serialize rồi deserialize SHALL tạo ra đối tượng deep-equal với đối tượng gốc trên toàn bộ thuộc tính có giá trị.
6. WHEN quá trình thực thi kiểm thử hoàn tất, THE Testing_Agent SHALL lưu báo cáo kiểm thử vào Context_Store kèm trạng thái tổng thể: "passed" (nếu tỷ lệ pass ≥ 90%) hoặc "failed" (nếu tỷ lệ pass < 90%).

---

### Requirement 8: Agent Triển khai (Deployment_Agent)

**User Story:** Là một DevOps Engineer, tôi muốn AI tự động thực hiện quy trình CI/CD và triển khai ứng dụng, để giảm lỗi thủ công trong quá trình release.

#### Acceptance Criteria

1. WHEN báo cáo kiểm thử với trạng thái "passed" được xác nhận, THE Deployment_Agent SHALL khởi động pipeline CI/CD theo cấu hình môi trường đích (development, staging, production).
2. WHEN triển khai hoàn tất, THE Deployment_Agent SHALL gửi health check request mỗi 10 giây trong tối đa 300 giây; IF ít nhất một health check trả về HTTP 200, THEN triển khai được coi là thành công; IF tất cả health check thất bại sau 300 giây, THEN THE Deployment_Agent SHALL thực hiện rollback về phiên bản trước đó.
3. IF môi trường triển khai là "production", THEN THE Deployment_Agent SHALL yêu cầu xác nhận tường minh từ Human_Reviewer trước khi thực thi triển khai; IF Human_Reviewer không phản hồi trong vòng 60 phút, THEN THE Deployment_Agent SHALL hủy triển khai và thông báo timeout.
4. THE Deployment_Agent SHALL ghi nhật ký (log) toàn bộ các bước thực thi pipeline, bao gồm: thời gian bắt đầu (ISO 8601), thời gian kết thúc, trạng thái từng bước (success/failed/skipped) và thông báo lỗi nếu có.
5. WHEN triển khai hoàn tất thành công, THE Deployment_Agent SHALL cập nhật Context_Store với thông tin: phiên bản đã triển khai, môi trường đích và thời điểm triển khai (ISO 8601).
6. IF rollback cũng thất bại (health check sau rollback không thành công trong 300 giây), THEN THE Deployment_Agent SHALL đánh dấu trạng thái "critical_failure", gửi cảnh báo khẩn cấp đến Human_Reviewer và ghi nhật ký chi tiết nguyên nhân.

---

### Requirement 9: Agent Vận hành và Giám sát (Operations_Agent)

**User Story:** Là một Site Reliability Engineer, tôi muốn AI liên tục giám sát ứng dụng và cảnh báo sự cố, để phản ứng kịp thời với các vấn đề trong môi trường production.

#### Acceptance Criteria

1. WHILE ứng dụng đang vận hành, THE Operations_Agent SHALL thu thập các chỉ số (metrics): CPU utilization (%), memory utilization (%), response latency (ms) và error rate (%) theo chu kỳ không quá 60 giây.
2. WHEN một chỉ số vượt ngưỡng cảnh báo mức "warning" được cấu hình (nhưng chưa đạt mức sự cố), THE Operations_Agent SHALL gửi cảnh báo mức "warning" đến kênh thông báo được cấu hình trong vòng 30 giây kể từ khi phát hiện; IF kênh thông báo không khả dụng, THEN SHALL thử lại tối đa 3 lần với interval 10 giây và ghi log lỗi gửi cảnh báo.
3. WHEN một sự cố được phát hiện (định nghĩa: error rate vượt ngưỡng mức "critical" HOẶC service không phản hồi health check trong 3 chu kỳ liên tiếp — khác với cảnh báo warning ở C2 vì sự cố yêu cầu phân tích root cause), THE Operations_Agent SHALL phân tích nhật ký hệ thống trong cửa sổ 60 phút gần nhất và đề xuất ít nhất một giải pháp khắc phục dưới dạng các bước hành động cụ thể (plain-language steps), kèm mức độ tin cậy (confidence score từ 0.0 đến 1.0).
4. IF tỷ lệ lỗi vượt quá 5% trong cửa sổ thời gian 5 phút liên tiếp, THEN THE Operations_Agent SHALL tự động kích hoạt cảnh báo mức "critical" và thông báo cho Human_Reviewer trong vòng 60 giây.
5. THE Operations_Agent SHALL lưu trữ lịch sử chỉ số và sự cố trong Context_Store với thời gian lưu giữ tối thiểu 30 ngày, dữ liệu cũ hơn 30 ngày có thể được archive hoặc xóa theo chính sách được cấu hình.

---

### Requirement 10: Agent Theo dõi Sức khỏe Dự án (ProjectHealth_Agent)

**User Story:** Là một Project Manager, tôi muốn AI tự động tổng hợp và báo cáo tình trạng sức khỏe của dự án (tiến độ, chất lượng, rủi ro, chi phí), để tôi nắm bắt tổng quan nhanh chóng và phát hiện vấn đề sớm mà không phải tự thu thập số liệu từ nhiều nguồn.

#### Acceptance Criteria

1. THE ProjectHealth_Agent SHALL tự động thu thập và tổng hợp dữ liệu từ tất cả các nguồn trong hệ thống: task status (Task_Agent — done/in_progress/blocked), test results (Testing_Agent — pass rate, coverage), pipeline status (Orchestrator — success/failed/running), Git activity (commits/merges per day), evaluation scores (Eval_Store), user feedback (positive/negative ratio), sentiment escalations, token usage/cost, và review gate wait times.
2. THE ProjectHealth_Agent SHALL tạo báo cáo định kỳ tự động theo 3 mức: (a) **Daily Summary** — tóm tắt hoạt động hôm nay (tasks completed, commits, issues), (b) **Weekly Report** — tiến độ tuần, velocity, blockers, quality trend, (c) **Sprint/Release Report** — tổng kết sprint/release với metrics đầy đủ. Tần suất và thời điểm gửi báo cáo SHALL configurable per-project.
3. THE ProjectHealth_Agent SHALL tính và hiển thị các chỉ số sức khỏe dự án: **Progress Score** (% tasks done vs planned), **Quality Score** (weighted average: test pass rate, AI eval score, feedback rating), **Risk Score** (based on: blocked tasks, overdue reviews, sentiment escalations, quality regressions), **Velocity** (tasks/story points completed per sprint), và **Cost Efficiency** (token cost per task completed).
4. WHEN Risk Score vượt ngưỡng cảnh báo được cấu hình (mặc định > 7/10), THE ProjectHealth_Agent SHALL tự động gửi alert "project_at_risk" cho Project Manager và Tech Lead kèm: danh sách risk factors cụ thể, impact assessment, và suggested actions.
5. THE ProjectHealth_Agent SHALL phát hiện và cảnh báo proactive các dấu hiệu rủi ro sớm: (a) velocity giảm > 20% so với 2 sprint trước, (b) test pass rate giảm > 10% trong 7 ngày, (c) số blocked tasks > 3 đồng thời, (d) average review wait time > 24 giờ, (e) budget usage > 80% quota tháng khi mới qua 60% thời gian.
6. THE ProjectHealth_Agent SHALL cung cấp Project Health Dashboard hiển thị: timeline tiến độ (planned vs actual), burn-down/burn-up chart, quality trend over time, risk radar chart, cost burn rate, và team productivity metrics — tất cả configurable per-project.
7. THE ProjectHealth_Agent SHALL hỗ trợ truy vấn bằng ngôn ngữ tự nhiên: PM có thể hỏi (ví dụ: "Tiến độ sprint này thế nào?", "Task nào đang block?", "Chất lượng code tuần này so với tuần trước?") và nhận câu trả lời tóm tắt dựa trên dữ liệu thực tế.
8. THE ProjectHealth_Agent SHALL lưu tất cả báo cáo đã sinh vào Context_Store (format Markdown theo R16-C2) và Git repository, cho phép xem lại lịch sử báo cáo và so sánh giữa các kỳ.
9. THE SDLC_System SHALL cho phép cấu hình kênh gửi báo cáo per-project: email, Slack, Teams, hoặc chỉ hiển thị trên dashboard; mỗi loại báo cáo có thể gửi đến nhóm người nhận khác nhau.
10. THE SDLC_System SHALL cho phép enable/disable ProjectHealth_Agent per-project; WHEN disabled, dữ liệu vẫn được thu thập (cho các dashboard khác) nhưng không sinh báo cáo tự động và không gửi alerts.

---

### Requirement 11: Agent Phân tích Cảm xúc và Chuyển hướng Hỗ trợ (Sentiment_Agent)

**User Story:** Là một Product Owner, tôi muốn hệ thống tự động nhận diện cảm xúc tiêu cực của người dùng trong quá trình tương tác với AI Agent, để kịp thời chuyển hướng sang chuyên gia hỗ trợ khi AI không đáp ứng được nhu cầu.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp Sentiment_Agent có khả năng phân tích cảm xúc (sentiment analysis) trên mỗi prompt input của người dùng, phân loại thành một trong các mức: positive, neutral, frustrated, hoặc angry, kèm confidence score (0.0 đến 1.0).
2. THE Sentiment_Agent SHALL chạy song song (không blocking) với agent chính đang xử lý task; kết quả phân tích cảm xúc SHALL được gắn kèm vào metadata của mỗi interaction mà không làm tăng latency phản hồi quá 100ms.
3. WHEN Sentiment_Agent phát hiện cảm xúc "frustrated" hoặc "angry" với confidence ≥ 0.7 trong 2 lần tương tác liên tiếp của cùng một người dùng trong cùng session, THE SDLC_System SHALL kích hoạt cơ chế escalation.
4. WHEN cơ chế escalation được kích hoạt, THE SDLC_System SHALL: (a) gửi thông báo cho người dùng rằng hệ thống đang chuyển yêu cầu sang chuyên gia, (b) tự động tạo issue trên hệ thống SKU kèm: tóm tắt vấn đề, lịch sử tương tác, sentiment timeline, agent_id đang xử lý, và priority dựa trên sentiment severity.
5. THE SDLC_System SHALL tích hợp với hệ thống SKU thông qua API có thể cấu hình; IF SKU không khả dụng, THEN SHALL queue issue và retry tối đa 3 lần với interval 60 giây, đồng thời gửi thông báo fallback qua kênh cảnh báo được cấu hình.
6. THE SDLC_System SHALL cho phép cấu hình ngưỡng escalation cho từng agent hoặc toàn hệ thống: số lần frustrated/angry liên tiếp trước khi escalate (mặc định 2), mức confidence tối thiểu (mặc định 0.7), và có cho phép người dùng tự request escalation thủ công hay không (mặc định: có).
7. THE SDLC_System SHALL cho phép người dùng chủ động yêu cầu chuyển sang chuyên gia bất kỳ lúc nào (manual escalation); hệ thống SHALL tạo SKU issue tương tự C4 với priority "user_requested".
8. THE Sentiment_Agent SHALL lưu trữ kết quả phân tích cảm xúc cho mỗi interaction vào Context_Store kèm: user_id, session_id, timestamp, sentiment label, confidence score, và escalation_triggered (true/false); dữ liệu SHALL được lưu giữ tối thiểu 90 ngày.
9. THE SDLC_System SHALL cung cấp dashboard phân tích sentiment tổng hợp theo: agent/team/project/thời gian, bao gồm: phân bố sentiment, tỷ lệ escalation, thời gian trung bình trước khi escalate, và correlation giữa sentiment xấu với agent/model cụ thể.
10. THE SDLC_System SHALL cho phép enable/disable Sentiment_Agent cho từng agent hoặc toàn hệ thống; WHEN disabled, hệ thống SHALL không phân tích cảm xúc nhưng vẫn cho phép manual escalation (C7).

---

### Requirement 12: Điều phối Pipeline (Orchestrator)

**User Story:** Là một Engineering Manager, tôi muốn hệ thống tự động điều phối các agent theo quy trình SDLC, để có thể chạy toàn bộ pipeline phát triển từ đầu đến cuối với sự can thiệp tối thiểu.

#### Acceptance Criteria

1. THE Orchestrator SHALL hỗ trợ cấu hình Pipeline theo hai chế độ: tuần tự (sequential — mỗi bước chạy sau khi bước trước hoàn thành) và song song có điều kiện (conditional parallel — các nhánh được kích hoạt dựa trên biểu thức điều kiện boolean do người dùng cấu hình, ví dụ: `artifact.status == "passed" AND target.env != "production"`).
2. WHEN một bước trong Pipeline thất bại, THE Orchestrator SHALL dừng các bước phụ thuộc trực tiếp, ghi nhận nguyên nhân thất bại, và gửi thông báo cho Human_Reviewer trong vòng 60 giây; các nhánh độc lập (không có dependency vào bước thất bại) SHALL tiếp tục chạy bình thường.
3. THE Orchestrator SHALL theo dõi và cập nhật trạng thái của từng agent trong Pipeline với độ trễ không quá 5 giây, hiển thị trạng thái: "pending", "running", "completed", "failed", "timeout".
4. WHEN tất cả các bước trong Pipeline hoàn thành với trạng thái "completed", THE Orchestrator SHALL tạo báo cáo tổng kết Pipeline bao gồm: tên từng bước, agent thực thi, trạng thái cuối cùng, thời gian thực thi từng bước và tổng thời gian pipeline.
5. IF một agent không gửi heartbeat hoặc acknowledgement trong vòng 300 giây kể từ khi được giao nhiệm vụ, THEN THE Orchestrator SHALL đánh dấu agent đó là "timeout", thử lại sau 60 giây (tối đa 2 lần retry) và chuyển sang trạng thái "failed" nếu vẫn không phản hồi sau tổng cộng 3 lần thử.

---

### Requirement 13: Chế độ Tương tác (Interaction Modes)

**User Story:** Là một Developer, tôi muốn chọn chế độ tương tác phù hợp khi bắt đầu làm việc với AI Agent, để hệ thống điều chỉnh quy trình xử lý phù hợp với mục đích.

#### Acceptance Criteria

1. THE SDLC_System SHALL yêu cầu người dùng chọn một trong ba chế độ tương tác (Interaction Mode) khi khởi tạo phiên làm việc mới: **Spec Mode** ("Plan first, then build" — tạo requirements và design trước khi coding), **Fix Mode** ("Investigate, then fix" — phân tích root cause trước khi sửa lỗi), **Vibe Mode** ("Just code, no ceremony" — sinh code trực tiếp từ yêu cầu).
2. WHEN người dùng chọn Spec Mode, THE SDLC_System SHALL thực thi pipeline đầy đủ: Requirements_Agent → (approval) → Design_Agent → (approval) → UIUX_Agent → (approval) → Task_Agent → Coding_Agent → Testing_Agent → Deployment_Agent; review gates SHALL được kích hoạt tự động theo R14.
3. WHEN người dùng chọn Fix Mode, THE SDLC_System SHALL thực thi pipeline rút gọn: thu thập thông tin lỗi → phân tích root cause → đề xuất giải pháp + impact analysis → (xác nhận) → Coding_Agent sinh patch → Testing_Agent chạy regression test; review gate chỉ bắt buộc trước deploy production.
4. WHEN người dùng chọn Vibe Mode, THE SDLC_System SHALL bỏ qua bước requirements/design/task, cho phép Coding_Agent sinh code trực tiếp từ prompt; Testing_Agent chạy basic validation (linting + unit test); review gates tắt trừ khi bật thủ công.
5. THE SDLC_System SHALL cho phép chuyển đổi mode giữa chừng mà không mất context đã tích lũy; các Artifact đã tạo trong mode trước SHALL được giữ lại.
6. THE SDLC_System SHALL cho phép Admin cấu hình mode mặc định và giới hạn mode khả dụng cho từng team/project (ví dụ: disable Vibe Mode cho production-critical projects).
7. THE SDLC_System SHALL ghi log mode được chọn cho mỗi session kèm metadata: user_id, mode, timestamp, mode changes, và pipeline steps thực tế được thực thi.
8. THE SDLC_System SHALL hiển thị rõ ràng mode hiện tại trong giao diện suốt phiên làm việc kèm mô tả ngắn về quy trình đang áp dụng.

---

### Requirement 14: Phê duyệt của Con người (Human-in-the-Loop)

**User Story:** Là một Tech Lead, tôi muốn kiểm soát và phê duyệt các Artifact quan trọng trước khi chuyển sang công đoạn tiếp theo, để đảm bảo chất lượng và kiểm soát rủi ro.

#### Acceptance Criteria

1. THE SDLC_System SHALL cho phép Human_Reviewer cấu hình các điểm kiểm duyệt (review gate) bắt buộc tại bất kỳ chuyển tiếp công đoạn nào trong Pipeline.
2. WHEN một Artifact đến điểm kiểm duyệt, THE Orchestrator SHALL tạm dừng Pipeline và gửi thông báo đến Human_Reviewer kèm: nội dung Artifact, tên công đoạn hiện tại, agent_id của agent tạo Artifact và timestamp tạo Artifact.
3. WHEN Human_Reviewer phê duyệt Artifact, THE Orchestrator SHALL tiếp tục Pipeline từ bước tiếp theo trong vòng 10 giây sau khi nhận xác nhận.
4. WHEN Human_Reviewer từ chối Artifact kèm phản hồi, THE Orchestrator SHALL gửi Artifact và phản hồi trở lại agent tạo ra Artifact đó để xử lý lại.
5. IF Artifact chờ phê duyệt quá 24 giờ mà không có phản hồi từ Human_Reviewer, THEN THE SDLC_System SHALL gửi nhắc nhở; IF vẫn không có phản hồi sau 48 giờ, THEN SHALL leo thang (escalate) thông báo đến người giám sát được cấu hình kèm thông tin artifact_id và thời gian chờ.
6. IF một Artifact bị từ chối 3 lần liên tiếp tại cùng một review gate, THEN THE Orchestrator SHALL dừng Pipeline tại bước đó, đánh dấu trạng thái "blocked" và thông báo cho Human_Reviewer rằng cần can thiệp thủ công để tránh vòng lặp rework vô hạn.

---

### Requirement 15: Chia sẻ Ngữ cảnh giữa các Agent (Context_Store)

**User Story:** Là một Developer, tôi muốn các agent tự động chia sẻ thông tin và Artifact với nhau, để không phải truyền dữ liệu thủ công giữa các công đoạn.

#### Acceptance Criteria

1. WHEN một agent ghi Artifact, THE Context_Store SHALL lưu trữ với định danh duy nhất (artifact_id, độ dài 1-128 ký tự) kết hợp với phiên bản (version, số nguyên dương); WHEN một agent đọc Artifact, THE Context_Store SHALL trả về nội dung theo artifact_id và version được yêu cầu.
2. THE Context_Store SHALL đảm bảo read-after-write consistency: trong cùng phiên ghi, thao tác đọc ngay sau khi ghi thành công SHALL trả về đúng nội dung đã ghi.
3. WHEN một Artifact được cập nhật (version mới), THE Context_Store SHALL lưu giữ tối đa 100 phiên bản trước đó và cho phép truy xuất bất kỳ phiên bản nào theo artifact_id + version; WHEN số phiên bản vượt quá 100, THE Context_Store SHALL tự động xóa phiên bản cũ nhất (FIFO) để duy trì giới hạn.
4. IF một agent cố gắng ghi Artifact với artifact_id và version đã tồn tại, THEN THE Context_Store SHALL từ chối thao tác ghi và trả về lỗi xung đột phiên bản (version conflict) kèm thông tin phiên bản hiện tại.
5. THE Context_Store SHALL đảm bảo rằng truy cập không được xác thực (unauthenticated) vào Artifact sẽ bị từ chối và trả về lỗi; dữ liệu Artifact không thể truy cập ngoài phiên xác thực hợp lệ, cả khi lưu trữ (at rest) và khi truyền tải (in transit).
6. IF một agent yêu cầu đọc artifact_id hoặc version không tồn tại, THEN THE Context_Store SHALL trả về lỗi "not found" kèm thông tin artifact_id và version được yêu cầu.

---

### Requirement 16: Quản lý Phiên bản và Lưu trữ (Version Control & Git Integration)

**User Story:** Là một Developer, tôi muốn toàn bộ sản phẩm của hệ thống (code và tài liệu) được quản lý bằng Git với commit tự động tại mỗi mốc hoàn thành, để có lịch sử thay đổi rõ ràng, dễ review, rollback và collaboration.

#### Acceptance Criteria

1. THE SDLC_System SHALL sử dụng Git làm hệ thống quản lý phiên bản duy nhất cho TẤT CẢ output của pipeline, bao gồm cả source code VÀ tài liệu (requirements, design, task list, test reports, deployment logs); mọi Artifact SHALL được lưu dưới dạng file trong Git repository.
2. THE SDLC_System SHALL quy định tất cả tài liệu output của mọi công đoạn phải ở dạng plaintext có thể diff được: requirements (Markdown), design documents (Markdown + Mermaid/PlantUML cho diagrams), task lists (Markdown hoặc YAML), test reports (Markdown hoặc JSON), deployment logs (JSON), và configuration files (YAML/JSON); KHÔNG sử dụng binary format (DOCX, PDF) cho tài liệu working — chỉ export binary khi cần chia sẻ bên ngoài.
3. WHEN một agent hoàn thành xử lý một prompt (mỗi lần agent sinh output từ một prompt input), THE SDLC_System SHALL tự động tạo Git commit cho các file thay đổi, với message format: `[agent_role] prompt summary` (ví dụ: `[coding] Add JWT token generation to AuthService`), kèm metadata trong commit body: pipeline_id, task_id, agent_id, prompt_log_id (tham chiếu R24); điều này cho phép rollback chính xác đến từng prompt thay vì chỉ từng task.
4. WHEN một sub-task hoặc task hoàn thành (bao gồm nhiều prompts), THE SDLC_System SHALL tạo Git tag nhẹ (lightweight tag) đánh dấu mốc: format `task/{task_id}/done` để dễ tìm kiếm trong lịch sử.
5. WHEN một pipeline stage hoàn thành (Requirements → Design → Tasks → Code → Test → Deploy), THE SDLC_System SHALL tạo Git annotated tag đánh dấu mốc: format `stage/{stage_name}/v{version}` (ví dụ: `stage/design/v2`, `stage/testing/v1`).
6. THE SDLC_System SHALL sử dụng branching strategy phân cấp theo session/task/sub-task:
   - **Pipeline branch** (tạo khi session bắt đầu): `{mode}/{feature-slug}` — ví dụ: `spec/user-authentication`, `fix/payment-timeout`, `vibe/prototype-dashboard`.
   - **Stage branch** (tạo khi mỗi stage bắt đầu): `{mode}/{feature-slug}/{stage}` — ví dụ: `spec/user-authentication/design`.
   - **Task branch** (tạo khi task bắt đầu): `{mode}/{feature-slug}/{task-id}` — ví dụ: `spec/user-authentication/task-001`.
   - **Sub-task branch** (tạo khi sub-task bắt đầu): `{mode}/{feature-slug}/{task-id}/{sub-task-id}` — ví dụ: `spec/user-authentication/task-001/sub-001`.
   
   Merge flow: sub-task → task branch (khi sub-task xong) → pipeline branch (khi task pass tests) → main (khi pipeline pass final review gate R14). Stage branches (req, design) merge vào pipeline branch khi Human_Reviewer approved.
7. THE SDLC_System SHALL tổ chức repository theo cấu trúc thư mục chuẩn tách biệt code và tài liệu:
   ```
   /docs/requirements/    — requirements documents (*.md)
   /docs/design/          — design documents + diagrams (*.md)
   /docs/tasks/           — task lists (*.md hoặc *.yaml)
   /docs/specs/           — feature specs (requirements + design + tasks + radio per feature)
   /docs/bugs/            — bugfix documents (bugfix.md, design.md, tasks.md per bug)
   /docs/change-request/  — CR backlog + per-CR specs
   /docs/qa/              — Q&A index + threads
   /docs/deliverables/    — deliverables + limitations per sprint
   /docs/decisions/       — decision logs (*.md)
   /src/                  — source code
   /tests/                — test code
   /config/               — configuration files
   /reports/              — test reports, deployment logs (*.md, *.json)
   ```
8. THE SDLC_System SHALL cho phép cấu hình Git remote (GitHub, GitLab, Bitbucket, self-hosted) per-project; hệ thống SHALL push changes sau mỗi commit hoặc theo batch configurable (mặc định: push sau mỗi task hoàn thành).
9. WHEN Human_Reviewer approve/reject tại review gate, THE SDLC_System SHALL ghi nhận quyết định trong Git: approval → merge commit với message `[review] Approved by {user} - {comment}`; rejection → commit trên branch với message `[review] Rejected by {user} - {reason}` để giữ lịch sử.
10. THE SDLC_System SHALL hỗ trợ rollback: IF cần quay lại phiên bản trước của bất kỳ Artifact nào (code hoặc tài liệu), THE SDLC_System SHALL sử dụng Git revert/checkout thay vì cơ chế riêng, đảm bảo single source of truth.
11. THE SDLC_System SHALL đảm bảo Context_Store (R15) đồng bộ với Git repository: mọi Artifact trong Context_Store SHALL có tham chiếu đến Git commit SHA tương ứng; truy vấn Artifact bằng version number SHALL map được sang Git commit cụ thể.

---

### Requirement 17: Cơ sở Tri thức Doanh nghiệp (Knowledge Base & Vector Store)

**User Story:** Là một Engineering Manager, tôi muốn các agent có khả năng truy vấn kiến thức nội bộ của doanh nghiệp/dự án, để các câu trả lời và sản phẩm của agent phù hợp với bối cảnh cụ thể của tổ chức.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp thành phần Knowledge_Store (vector database) cho phép lưu trữ tài liệu nội bộ dưới dạng vector embeddings, hỗ trợ semantic similarity search với độ trễ không quá 200ms cho mỗi truy vấn.
2. THE SDLC_System SHALL cho phép người dùng nạp (ingest) tài liệu vào Knowledge_Store theo nhiều định dạng: plain text, Markdown, PDF, DOCX; hệ thống SHALL tự động chia nhỏ (chunk) tài liệu, tạo embeddings và lập chỉ mục (index).
3. WHEN một agent thực thi task, THE SDLC_System SHALL cho phép agent truy vấn Knowledge_Store bằng ngữ cảnh hiện tại để lấy top-K tài liệu liên quan nhất (K configurable, mặc định K=5) và sử dụng làm ngữ cảnh bổ sung cho việc sinh output (RAG pattern).
4. THE SDLC_System SHALL cho phép cấu hình Knowledge_Store theo phạm vi: toàn hệ thống (global), theo dự án (project-scoped), hoặc theo agent (agent-scoped).
5. WHEN tài liệu nguồn trong Knowledge_Store được cập nhật hoặc xóa, THE SDLC_System SHALL tự động cập nhật embeddings tương ứng trong vòng 300 giây.
6. THE SDLC_System SHALL cho phép cấu hình enable/disable việc sử dụng Knowledge_Store cho từng agent; WHEN disabled, agent SHALL hoạt động chỉ dựa trên input trực tiếp.
7. THE SDLC_System SHALL ghi log mỗi lần agent truy vấn Knowledge_Store, bao gồm: agent_id, query summary, số kết quả trả về, và relevance score trung bình.

---

### Requirement 18: Hệ thống FAQ Best-Match (FAQ Direct Response)

**User Story:** Là một Engineering Manager, tôi muốn các câu hỏi thường gặp (FAQ) được trả lời trực tiếp từ kho câu trả lời đã duyệt sẵn thay vì sinh bởi AI, để đảm bảo tính chính xác, nhất quán và tiết kiệm chi phí.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp thành phần FAQ_Store cho phép người dùng quản lý (CRUD) các cặp câu hỏi-câu trả lời kèm metadata: category, tags, ngày tạo, ngày cập nhật và trạng thái (active/inactive).
2. WHEN một agent nhận câu hỏi đầu vào, THE SDLC_System SHALL thực hiện so khớp ngữ nghĩa (semantic matching) với toàn bộ câu hỏi trong FAQ_Store và tính relevance score cho từng cặp FAQ, với độ trễ không quá 200ms cho mỗi lần matching.
3. IF relevance score của FAQ best-match vượt ngưỡng confidence được cấu hình (mặc định ≥ 0.85), THEN THE SDLC_System SHALL trả về trực tiếp câu trả lời đã duyệt sẵn mà KHÔNG gọi mô hình AI.
4. IF relevance score của tất cả FAQ đều dưới ngưỡng confidence, THEN THE SDLC_System SHALL chuyển câu hỏi sang agent AI xử lý bình thường (có thể kết hợp Knowledge_Store/RAG theo R17).
5. THE SDLC_System SHALL cho phép cấu hình ngưỡng confidence (0.0 đến 1.0) cho từng agent hoặc toàn hệ thống.
6. WHEN FAQ_Store trả về best-match, THE SDLC_System SHALL kèm theo: FAQ_id, relevance score, và gắn nhãn nguồn "faq_direct" trong response.
7. THE SDLC_System SHALL cho phép enable/disable cơ chế FAQ best-match cho từng agent.
8. THE SDLC_System SHALL ghi log mọi lần FAQ best-match được kích hoạt, bao gồm: agent_id, câu hỏi gốc, FAQ_id, relevance score, và kết quả (direct_response hoặc fallback_to_ai).

---

### Requirement 19: Kho Source Code Tiêu chuẩn (Baseline Template Registry)

**User Story:** Là một Developer, tôi muốn hệ thống tự động sử dụng source code tiêu chuẩn đã qua kiểm thử khi tạo project mới hoặc thêm tính năng phổ biến, để tiết kiệm thời gian và đảm bảo chất lượng nền tảng.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp thành phần Template_Registry cho phép lưu trữ và quản lý các source code template tiêu chuẩn (baseline), mỗi template bao gồm: tên, mô tả, tags, source code repository URL hoặc embedded code, danh sách dependencies, hướng dẫn sử dụng, và trạng thái (active/deprecated).
2. THE SDLC_System SHALL cho phép đăng ký template theo nhiều loại: project scaffold, feature module, và architecture pattern.
3. WHEN Coding_Agent nhận yêu cầu tạo project hoặc thêm tính năng, THE SDLC_System SHALL tự động tìm kiếm Template_Registry bằng semantic matching; IF tìm thấy template với relevance score ≥ 0.8, THEN SHALL đề xuất sử dụng template.
4. IF người dùng chấp nhận sử dụng template, THEN THE Coding_Agent SHALL scaffold từ template và chỉ sử dụng AI để customize phần riêng (business logic, config, extension points).
5. THE SDLC_System SHALL cho phép cấu hình hành vi: "auto_use", "suggest" (mặc định), hoặc "ignore".
6. THE SDLC_System SHALL hỗ trợ versioning cho template; WHEN template được cập nhật, phiên bản cũ vẫn khả dụng.
7. THE SDLC_System SHALL cho phép quản lý Template_Registry qua Admin Portal: CRUD, stats, "verified" badge sau security review.
8. THE SDLC_System SHALL đảm bảo mọi template active đã pass: linting, security scan, và có unit test cho core components.
9. THE SDLC_System SHALL hỗ trợ template scoping: global, organization, team-private (theo R34).
10. THE SDLC_System SHALL ghi log mỗi lần template được sử dụng kèm: template_id, version, agent_id, project_id, và % code giữ nguyên vs AI sinh thêm.

---

### Requirement 20: Kho Template Tài liệu và Cấu hình (Document & Configuration Template Registry)

**User Story:** Là một Tech Lead, tôi muốn có kho template chuẩn cho các tài liệu quy trình và cấu hình hệ thống, để đảm bảo mọi đội nhóm tạo tài liệu theo đúng format và tuân thủ quy chuẩn nội bộ.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp thành phần Doc_Template_Registry cho phép lưu trữ và quản lý các template theo 6 loại: requirements template, design template, task template, skills template, hooks template, và steering template.
2. THE SDLC_System SHALL cho phép mỗi template bao gồm: tên, loại, mô tả, nội dung template (với placeholders), danh sách required sections, ví dụ mẫu đã điền, hướng dẫn sử dụng, tags, phiên bản, và trạng thái (active/draft/deprecated).
3. WHEN Requirements_Agent, Design_Agent hoặc Task_Agent tạo tài liệu mới, THE SDLC_System SHALL tự động áp dụng template active mặc định (hoặc template được chỉ định bởi project config) làm cấu trúc nền.
4. THE SDLC_System SHALL cho phép người dùng tạo và quản lý skills/hooks/steering templates theo format chuẩn với schema validation tự động.
5. THE SDLC_System SHALL hỗ trợ nhiều template cho cùng một loại; người dùng hoặc Admin có thể chọn template mặc định theo project/team.
6. THE SDLC_System SHALL hỗ trợ template inheritance: template con kế thừa từ template cha và override/thêm phần riêng.
7. THE SDLC_System SHALL hỗ trợ versioning cho document templates với khả năng diff giữa các phiên bản.
8. THE Admin Portal SHALL cung cấp trang Doc Template Management: CRUD, preview, stats, import/export JSON/YAML.
9. THE SDLC_System SHALL hỗ trợ template scoping: global, organization, team-private với override hierarchy (theo R34).
10. WHEN Output Validation (R22) kiểm tra output, THE SDLC_System SHALL sử dụng required sections và schema từ template đang áp dụng làm validation rules tự động.

---

### Requirement 21: Bộ nhớ Dự án (Project Second Brain)

**User Story:** Là một Developer, tôi muốn AI tự động tích lũy và tổ chức kiến thức riêng của dự án trong quá trình làm việc, để khi dự án phát triển lớn, agent vẫn nhanh chóng tìm được thông tin liên quan mà không cần tôi chỉ dẫn thủ công.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp thành phần Project_Brain (tách biệt với Knowledge_Store) cho mỗi project, tự động thu thập và lập chỉ mục kiến thức sinh ra trong quá trình làm việc, bao gồm: Artifacts (requirements, designs, task lists, code, test reports), decision logs (lý do chọn giải pháp A thay vì B), conversation summaries (tóm tắt các cuộc trao đổi quan trọng), và project-specific terminology.
2. THE Project_Brain SHALL tự động cập nhật khi có Artifact mới được lưu vào Context_Store hoặc khi có thay đổi trong repository: tạo embeddings, trích xuất entities/relationships, và lập chỉ mục toàn văn (full-text index) trong vòng 120 giây sau mỗi thay đổi.
3. THE Project_Brain SHALL duy trì knowledge graph liên kết giữa các thành phần dự án: requirement ↔ design component ↔ task ↔ code file ↔ test case; cho phép agent truy vấn theo quan hệ (ví dụ: "code nào implement requirement X?", "test nào cover module Y?").
4. WHEN một agent thực thi task trong project, THE SDLC_System SHALL tự động truy vấn Project_Brain để bổ sung context liên quan (top-K relevant items, K configurable, mặc định K=10) VÀ Knowledge_Store (R17) cho context chung; Project_Brain SHALL được ưu tiên cao hơn Knowledge_Store khi có kết quả trùng lặp.
5. THE Project_Brain SHALL tự động ghi nhận decision logs: WHEN Human_Reviewer approve/reject Artifact kèm comment, hoặc WHEN người dùng giải thích lý do chọn giải pháp trong conversation, THE SDLC_System SHALL trích xuất và lưu thành decision entry kèm: context, alternatives considered, decision made, rationale, và timestamp.
6. THE Project_Brain SHALL hỗ trợ project-specific terminology: tự động phát hiện thuật ngữ riêng (từ/cụm từ xuất hiện thường xuyên trong project nhưng không có trong từ điển chung) và cho phép người dùng confirm/define meaning; agent SHALL sử dụng terminology này khi sinh output.
7. THE SDLC_System SHALL cung cấp giao diện cho phép người dùng xem, tìm kiếm và bổ sung thủ công vào Project_Brain: thêm decision logs, chỉnh sửa relationships, và đánh dấu thông tin outdated/deprecated.
8. THE Project_Brain SHALL hỗ trợ truy vấn bằng ngôn ngữ tự nhiên: người dùng hoặc agent có thể hỏi (ví dụ: "Tại sao chọn PostgreSQL thay vì MongoDB?", "Module nào phụ thuộc vào AuthService?") và nhận câu trả lời dựa trên dữ liệu đã tích lũy, với độ trễ không quá 500ms.
9. THE SDLC_System SHALL cho phép cấu hình retention policy cho Project_Brain per-project: dữ liệu nào giữ vĩnh viễn (decisions, terminology), dữ liệu nào có thể archive sau thời gian configurable (conversation summaries — mặc định 180 ngày).
10. THE SDLC_System SHALL cung cấp metrics về Project_Brain: tổng số items indexed, kích thước storage, tần suất truy vấn, hit rate (% truy vấn có kết quả hữu ích dựa trên user feedback), và top queries không có kết quả (gaps) để cải thiện coverage.

---

### Requirement 22: Kiểm tra Đầu ra AI (Output Validation & Guardrails)

**User Story:** Là một Tech Lead, tôi muốn hệ thống tự động kiểm tra output của AI trước khi lưu thành Artifact chính thức, để phát hiện sớm output lỗi format, thiếu nội dung hoặc vi phạm quy tắc.

#### Acceptance Criteria

1. THE SDLC_System SHALL cho phép cấu hình validation rules cho output của từng loại agent: schema validation, required sections check, và custom regex/pattern rules.
2. WHEN một agent sinh output, THE SDLC_System SHALL tự động chạy toàn bộ validation rules TRƯỚC khi lưu output thành Artifact vào Context_Store.
3. IF output vi phạm validation rule, THEN THE SDLC_System SHALL từ chối lưu Artifact, gửi kết quả validation trở lại agent, và yêu cầu agent sửa + sinh lại (tối đa 3 lần retry).
4. IF sau 3 lần retry output vẫn không pass validation, THEN THE SDLC_System SHALL đánh dấu Artifact là "validation_failed" với trạng thái "draft" và thông báo cho Human_Reviewer.
5. THE SDLC_System SHALL hỗ trợ guardrail rules cấp doanh nghiệp cho TẤT CẢ agents: cấm PII/credentials trong output, giới hạn độ dài output, và blacklist từ/cụm từ.
6. THE SDLC_System SHALL ghi log mỗi lần validation: agent_id, artifact_id, rules applied, rules passed/failed, số lần retry, và kết quả cuối cùng.
7. THE SDLC_System SHALL cho phép Admin quản lý validation rules qua Admin Portal: CRUD rules, gán cho agent types, và xem violation rate statistics.

---

### Requirement 23: Đánh giá Chất lượng AI và Nghiệm thu (AI Quality Evaluation & Benchmarking)

**User Story:** Là một AI Engineer, tôi muốn có hệ thống đánh giá chất lượng output của AI Agent một cách khách quan, để biết agent hoạt động tốt hay kém và có cơ sở nghiệm thu chất lượng.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp Eval_Store cho phép tạo và quản lý evaluation test suites cho từng agent: tập input, ground truth, scoring criteria, và metadata.
2. THE SDLC_System SHALL hỗ trợ 3 phương pháp đánh giá: Automated Metrics (semantic similarity, BLEU/ROUGE, factual accuracy, completeness, code metrics), LLM-as-Judge (AI evaluator chấm 1-5 theo accuracy/relevance/coherence/harmfulness), và Human Evaluation.
3. THE SDLC_System SHALL cho phép chạy evaluation theo: manual, scheduled (daily/weekly), hoặc on-change (khi agent config thay đổi).
4. WHEN evaluation chạy xong, THE SDLC_System SHALL tạo báo cáo: aggregate score (0-100), điểm theo tiêu chí, bottom 10% test cases, delta vs lần trước, và pass/fail status.
5. THE SDLC_System SHALL cho phép cấu hình ngưỡng nghiệm thu (mặc định 70/100); IF agent không đạt, THEN SHALL đánh dấu "below_quality_threshold" và alert Admin.
6. WHEN điểm đánh giá giảm quá 10% so với baseline, THE SDLC_System SHALL tạo alert "quality_regression" kèm chi tiết.
7. THE SDLC_System SHALL cung cấp Human Evaluation workflow: auto-sampling outputs → expert review → update quality score → đánh dấu good outputs làm ground truth mới.
8. THE SDLC_System SHALL cung cấp Quality Dashboard: trend theo thời gian, so sánh agents, correlation với user feedback (R25/R26), heat map categories.
9. THE SDLC_System SHALL hỗ trợ A/B evaluation: so sánh config cũ vs mới trên cùng test suite với statistical significance (p < 0.05).
10. THE SDLC_System SHALL lưu trữ evaluation results tối thiểu 365 ngày, liên kết với agent config snapshot tại thời điểm chạy.
11. THE SDLC_System SHALL hỗ trợ evaluation scoping: global, project-specific, agent-specific test suites.
12. THE Admin Portal SHALL cung cấp trang Evaluation Management: CRUD test suites, import CSV/JSON, schedules, thresholds, Human Evaluation assignments.

---

### Requirement 24: Ghi nhật ký Prompt Input/Output (AI Interaction Logging)

**User Story:** Là một AI Engineer, tôi muốn ghi lại toàn bộ prompt gửi đến mô hình AI và response trả về, để phục vụ debug, phát hiện hallucination, fine-tuning và compliance audit.

#### Acceptance Criteria

1. WHEN một agent gọi mô hình AI, THE SDLC_System SHALL ghi log: agent_id, pipeline_id, timestamp (ISO 8601), full prompt input, full response output, mô hình AI, và tham số inference (temperature, max_tokens, top_p).
2. THE SDLC_System SHALL ghi log cho MỌI lần gọi AI, bao gồm retry attempts và gọi nội bộ giữa agents.
3. THE SDLC_System SHALL lưu trữ prompt/output logs tối thiểu 90 ngày.
4. THE SDLC_System SHALL cho phép cấu hình verbosity per-agent: "full", "summary" (truncated 500 ký tự), hoặc "metadata_only"; mặc định "full".
5. THE SDLC_System SHALL đảm bảo prompt/output logs immutable (append-only).
6. THE SDLC_System SHALL cung cấp API truy vấn logs theo: agent_id, pipeline_id, khoảng thời gian, mô hình AI, từ khóa.
7. IF prompt chứa dữ liệu nhạy cảm (PII, credentials, tag "sensitive"), THEN SHALL auto-redact thành `[REDACTED]` trước khi lưu.
8. THE SDLC_System SHALL tính và ghi kèm metrics: token count (input + output), latency (ms), và estimated cost.

---

### Requirement 25: Phản hồi Chất lượng Output (Feedback Loop)

**User Story:** Là một Developer, tôi muốn đánh giá chất lượng output của agent sau mỗi lần sử dụng, để hệ thống liên tục cải thiện.

#### Acceptance Criteria

1. THE SDLC_System SHALL cho phép người dùng đánh giá output (thumbs up/down + comment tối đa 1000 ký tự) sau mỗi lần agent sinh Artifact.
2. THE SDLC_System SHALL lưu feedback kèm: agent_id, pipeline_id, artifact_id, user_id, timestamp, rating, comment, và tham chiếu prompt log (R24).
3. THE SDLC_System SHALL cung cấp dashboard feedback theo agent/model/team/project: tỷ lệ positive/negative, xu hướng, top 10 negative gần nhất.
4. WHEN tỷ lệ negative vượt 30% trong 7 ngày (tối thiểu 10 feedback), THE SDLC_System SHALL alert Admin.
5. THE SDLC_System SHALL cho phép Admin đánh dấu feedback "actionable" hoặc "dismissed"; "actionable" liên kết với improvement task.
6. THE SDLC_System SHALL lưu feedback tối thiểu 180 ngày.

---

### Requirement 26: Hệ thống Đánh giá Người dùng (User Feedback & Rating System)

**User Story:** Là một Product Owner, tôi muốn có hệ thống tiếp nhận phản hồi toàn diện từ người dùng về trải nghiệm sử dụng, để liên tục cải thiện chất lượng dịch vụ.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp đánh giá đa chiều: rating 1-5 sao, đánh giá theo tiêu chí (accuracy, speed, relevance, completeness), và comment (tối đa 2000 ký tự).
2. THE SDLC_System SHALL hỗ trợ: prompt sau session, feedback widget, và khảo sát định kỳ (configurable, mặc định 2 tuần).
3. THE SDLC_System SHALL auto-classify feedback: bug_report, quality_issue, feature_request, praise, general; IF confidence < 0.7 THEN "unclassified".
4. WHEN feedback có rating ≤ 2 sao hoặc category "bug_report", THE SDLC_System SHALL tạo SKU issue (1 sao → critical, 2 sao → major).
5. THE SDLC_System SHALL lưu feedback kèm metadata và trạng thái xử lý (new/acknowledged/in_progress/resolved/dismissed).
6. THE SDLC_System SHALL cung cấp dashboard: average rating, phân bố, top issues, satisfaction trend, NPS.
7. THE SDLC_System SHALL alert team owner khi: avg rating < 3.5 (7 ngày), bug_report mới, feature_request ≥ 5 upvotes.
8. THE SDLC_System SHALL cho phép upvote feedback/feature_request trong cùng Team/Project.
9. THE SDLC_System SHALL cho phép Admin response feedback → thông báo cho user.
10. THE SDLC_System SHALL lưu feedback 365 ngày + export CSV/JSON.

---

### Requirement 27: Quản lý SKU Tickets và Feedback (Ticket & Feedback Management Portal)

**User Story:** Là một Support Manager, tôi muốn có màn hình quản lý tập trung SKU tickets và user feedback để theo dõi tiến độ xử lý.

#### Acceptance Criteria

1. THE Admin Portal SHALL cung cấp SKU Ticket Management: danh sách tickets (ticket_id, tiêu đề, nguồn, priority, trạng thái, assignee, ngày tạo, thời gian chờ).
2. THE Admin Portal SHALL cho phép lọc/tìm kiếm tickets theo: trạng thái, priority, nguồn, assignee, agent, team/project, khoảng thời gian.
3. THE Admin Portal SHALL cho phép: xem chi tiết ticket, phân công assignee, cập nhật trạng thái, thêm internal notes, phản hồi user — tất cả ghi audit log.
4. THE Admin Portal SHALL cung cấp User Feedback Management: kanban board, lọc theo category/rating/agent/team, bulk actions, liên kết feedback → SKU ticket.
5. THE Admin Portal SHALL hiển thị dashboard SKU: open/resolved trend, MTTR, tickets quá SLA (critical 48h, major 5 ngày).
6. THE Admin Portal SHALL hiển thị dashboard Feedback: rating trend, category distribution, top feature requests, top negative agents.
7. WHEN SKU ticket critical chưa assign trong 30 phút, SHALL alert Support Manager + hiển thị "Requires Immediate Attention".
8. THE Admin Portal SHALL cho phép cấu hình SLA per priority (first response + resolution time); WHEN breached SHALL đánh dấu + notify assignee + manager.
9. THE Admin Portal SHALL tuân thủ phân quyền R34: scoped visibility theo role/team/project.

---

### Requirement 28: Hiệu năng (Performance)

**User Story:** Là một Engineering Manager, tôi muốn hệ thống phản hồi nhanh và xử lý pipeline hiệu quả, để không tạo nút thắt cổ chai.

#### Acceptance Criteria

1. THE SDLC_System SHALL xử lý tối thiểu 10 pipeline chạy đồng thời mà không làm giảm thời gian phản hồi quá 20% so với đơn lẻ.
2. THE Context_Store SHALL hoàn thành đọc Artifact (dưới 50MB) trong vòng 500ms.
3. THE Context_Store SHALL hoàn thành ghi Artifact (dưới 50MB) trong vòng 2000ms.
4. THE Orchestrator SHALL khởi tạo Pipeline mới trong vòng 5 giây.
5. THE SDLC_System SHALL duy trì API response time dưới 1000ms cho 95th percentile request quản trị.

---

### Requirement 29: Khả năng mở rộng (Scalability)

**User Story:** Là một Platform Engineer, tôi muốn hệ thống mở rộng được khi số lượng dự án và agent tăng.

#### Acceptance Criteria

1. THE SDLC_System SHALL hỗ trợ tối thiểu 50 agent đồng thời mà không vi phạm SLA hiệu năng R28.
2. THE Context_Store SHALL hỗ trợ tối thiểu 10.000 Artifact mà không giảm hiệu năng.
3. THE SDLC_System SHALL hỗ trợ horizontal scaling: tự động phân phối tải trong vòng 60 giây khi thêm instance.
4. WHEN pipeline vượt capacity, THE SDLC_System SHALL queue và xử lý theo priority thay vì từ chối.

---

### Requirement 30: Tính khả dụng (Availability)

**User Story:** Là một SRE, tôi muốn hệ thống luôn sẵn sàng phục vụ.

#### Acceptance Criteria

1. THE SDLC_System SHALL duy trì uptime tối thiểu 99.5%/tháng.
2. THE Context_Store SHALL duy trì uptime tối thiểu 99.9%/tháng.
3. IF một thành phần sự cố, THEN các thành phần khác tiếp tục hoạt động; pipeline bị ảnh hưởng tạm dừng và tự động tiếp tục khi phục hồi.
4. THE SDLC_System SHALL hỗ trợ graceful shutdown trong tối đa 120 giây.

---

### Requirement 31: Độ tin cậy (Reliability)

**User Story:** Là một Developer, tôi muốn hệ thống không mất dữ liệu và phục hồi được sau sự cố.

#### Acceptance Criteria

1. THE Context_Store SHALL đảm bảo durability: Artifact không bị mất khi single node failure.
2. THE SDLC_System SHALL backup Context_Store mỗi 24 giờ tại vị trí vật lý khác.
3. WHEN pipeline gián đoạn do sự cố hệ thống, THE Orchestrator SHALL lưu checkpoint và cho phép tiếp tục từ checkpoint khi phục hồi.
4. THE SDLC_System SHALL có RTO ≤ 15 phút.
5. THE SDLC_System SHALL có RPO ≤ 1 giờ.

---

### Requirement 32: Khả năng bảo trì (Maintainability)

**User Story:** Là một Platform Engineer, tôi muốn hệ thống dễ mở rộng và bảo trì, để thêm agent mới hoặc thay đổi mô hình AI mà không ảnh hưởng toàn hệ thống.

#### Acceptance Criteria

1. THE SDLC_System SHALL hỗ trợ thêm loại agent mới thông qua plugin/extension mechanism mà không cần thay đổi mã nguồn core.
2. THE SDLC_System SHALL cho phép thay đổi mô hình AI qua cấu hình mà không cần triển khai lại; thay đổi có hiệu lực cho task tiếp theo.
3. THE SDLC_System SHALL tuân thủ loose coupling: mỗi agent giao tiếp qua interface chuẩn hóa; thay đổi nội bộ agent không ảnh hưởng agent khác.
4. THE SDLC_System SHALL cung cấp API versioning: phiên bản cũ hỗ trợ tối thiểu 6 tháng trước khi deprecated.
5. THE SDLC_System SHALL cung cấp technical documentation tự động cập nhật cho mọi API và agent interface.

---

### Requirement 33: Bảo mật (Security)

**User Story:** Là một Security Engineer, tôi muốn hệ thống bảo mật dữ liệu và kiểm soát truy cập chặt chẽ.

#### Acceptance Criteria

1. THE SDLC_System SHALL yêu cầu authentication cho mọi tương tác; mỗi agent có credential riêng biệt.
2. THE SDLC_System SHALL thực thi RBAC: agent chỉ đọc/ghi Artifact thuộc phạm vi vai trò, trừ khi được cấp quyền tường minh.
3. THE SDLC_System SHALL ghi audit log cho mọi thao tác thay đổi trạng thái: actor, action, target, timestamp (ISO 8601), kết quả.
4. THE SDLC_System SHALL lưu audit log tối thiểu 90 ngày, immutable.
5. IF 5 lần xác thực thất bại liên tiếp trong 5 phút, THEN SHALL lock 15 phút + alert admin.
6. THE SDLC_System SHALL mã hóa giao tiếp bằng TLS 1.2+.

---

### Requirement 34: Phân quyền Người dùng (User Authorization & Access Control)

**User Story:** Là một System Administrator, tôi muốn kiểm soát chi tiết ai được phép làm gì trong hệ thống.

#### Acceptance Criteria

1. THE SDLC_System SHALL hỗ trợ 5 vai trò: Viewer, Developer, Tech Lead, Admin, Super Admin (quyền tăng dần).
2. THE SDLC_System SHALL phân quyền theo phạm vi Organization → Team → Project; quyền truy cập bị giới hạn trong phạm vi được gán.
3. IF thao tác vượt quyền, THEN SHALL từ chối + trả lỗi "access denied" + audit log.
4. THE SDLC_System SHALL cho phép custom permission sets ngoài 5 vai trò mặc định.
5. THE SDLC_System SHALL hỗ trợ delegation phê duyệt (tối đa 30 ngày, tự động thu hồi khi hết hạn).
6. THE SDLC_System SHALL cách ly dữ liệu giữa Teams/Projects; cross-team cần Super Admin cấp quyền tường minh.
7. WHEN user bị xóa/disable, SHALL thu hồi toàn bộ session/credential/delegation trong 60 giây; approval chờ chuyển sang reviewer thay thế theo R14-C5.
8. THE SDLC_System SHALL cung cấp giao diện quản trị users/roles/permissions cho Super Admin.

---

### Requirement 35: Giới hạn Tài nguyên và Quota (Rate Limiting & Quota Management)

**User Story:** Là một Engineering Manager, tôi muốn kiểm soát mức sử dụng tài nguyên AI theo team/project để quản lý chi phí.

#### Acceptance Criteria

1. THE SDLC_System SHALL cho phép cấu hình quota: token limit, số lần gọi AI, số pipeline đồng thời — per agent/team/project per chu kỳ.
2. WHEN usage đạt 80% quota, SHALL alert "approaching limit" cho team owner và Admin.
3. WHEN usage đạt 100%, SHALL chặn request mới + trả lỗi "quota exceeded"; pipeline đang chạy được hoàn thành.
4. THE SDLC_System SHALL hỗ trợ rate limiting per AI model (requests/phút).
5. THE SDLC_System SHALL cho phép burst allowance (mặc định 20%, tối đa 1 giờ) trước hard-block.
6. THE SDLC_System SHALL cung cấp usage report real-time và lịch sử theo team/project/agent/model.

---

### Requirement 36: Khả năng quan sát (Observability)

**User Story:** Là một DevOps Engineer, tôi muốn giám sát và debug hệ thống dễ dàng.

#### Acceptance Criteria

1. THE SDLC_System SHALL tạo structured log (JSON) cho mọi thao tác: timestamp, agent_id, action, pipeline_id, trạng thái, thời gian thực thi.
2. THE SDLC_System SHALL hỗ trợ distributed tracing với trace_id duy nhất per request.
3. THE SDLC_System SHALL expose Prometheus-compatible metrics endpoint.
4. THE SDLC_System SHALL cung cấp dashboard real-time (≤ 10 giây delay).
5. WHEN lỗi xảy ra, THE SDLC_System SHALL tạo correlation report liên kết logs + traces + metrics.

---

### Requirement 37: Khả năng tích hợp (Compatibility & Integration)

**User Story:** Là một Developer, tôi muốn hệ thống tích hợp với công cụ phát triển hiện có.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp REST API và/hoặc gRPC API với tài liệu OpenAPI/Protobuf.
2. THE Deployment_Agent SHALL tích hợp ít nhất 3 nền tảng CI/CD (GitHub Actions, GitLab CI, Jenkins).
3. THE SDLC_System SHALL hỗ trợ webhook notifications cho sự kiện pipeline.
4. THE Coding_Agent SHALL tích hợp VCS (Git: GitHub, GitLab, Bitbucket).
5. THE SDLC_System SHALL tích hợp hệ thống thông báo: Slack, Microsoft Teams, Email.

---

### Requirement 38: Quản trị Hệ thống (System Administration Portal)

**User Story:** Là một System Administrator, tôi muốn có giao diện quản trị tập trung để quản lý toàn bộ cấu hình, tài nguyên và trạng thái hệ thống.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp Admin Portal (web-based) tập trung toàn bộ chức năng quản trị, bao gồm: Agent Management, Pipeline Management, User & Permissions, Knowledge_Store, FAQ_Store, Template_Registry, Doc_Template_Registry, AI Models, AI Evaluation, System Settings, Logs & Audit, System Health, SKU Tickets, và User Feedback.
2. THE Admin Portal SHALL cung cấp System Settings: default temperature, ngưỡng FAQ confidence, Knowledge_Store top-K, retention policies, ngưỡng cảnh báo.
3. THE Admin Portal SHALL cung cấp AI Model Management: đăng ký/xóa models, rate limits, usage statistics theo model/agent/team.
4. THE Admin Portal SHALL cung cấp System Health: real-time status tất cả thành phần (healthy/degraded/down), uptime, resource usage.
5. THE Admin Portal SHALL cung cấp Backup & Recovery: xem history, manual backup, point-in-time restore trong phạm vi RPO (R31-C5).
6. THE Admin Portal SHALL cung cấp Usage & Billing: token consumption, cost tracking, trend charts, budget alerts.
7. THE Admin Portal SHALL tuân thủ phân quyền R34: mỗi role chỉ thấy phần phù hợp.
8. THE Admin Portal SHALL ghi audit log cho mọi thay đổi cấu hình qua portal (old value → new value) theo R33-C3.

---

### Requirement 39: Lựa chọn Loại AI Phù hợp (AI Model Selection Strategy)

**User Story:** Là một AI Engineer, tôi muốn hệ thống hỗ trợ nhiều loại AI model (không chỉ LLM) và cho phép cấu hình loại model phù hợp nhất cho từng task, để tối ưu chi phí, tốc độ và độ chính xác — thay vì mặc định dùng LLM cho mọi thứ.

#### Acceptance Criteria

1. THE SDLC_System SHALL hỗ trợ đăng ký và sử dụng nhiều loại AI model, bao gồm nhưng không giới hạn: LLM (Large Language Models — cho sinh text, code, phân tích phức tạp), ML Classification models (cho phân loại sentiment, categorization, spam detection), Embedding models (cho semantic search, similarity matching), Code-specific models (cho code completion, linting, vulnerability scan), và Rule-based engines (cho validation, pattern matching, workflow logic).
2. THE SDLC_System SHALL cho phép cấu hình model type cho từng task cụ thể của agent, không chỉ ở cấp agent: ví dụ Sentiment_Agent có thể dùng ML Classification model (nhẹ, nhanh) cho sentiment detection nhưng dùng LLM cho việc tạo summary khi escalation; Coding_Agent có thể dùng code-specific model cho linting nhưng LLM cho code generation.
3. THE SDLC_System SHALL cung cấp cơ chế model routing: dựa trên task type, complexity, và cấu hình, hệ thống SHALL tự động chọn model phù hợp; IF task đơn giản (classification, pattern matching), THEN ưu tiên model nhẹ/chuyên biệt; IF task phức tạp (generation, reasoning, multi-step), THEN sử dụng LLM.
4. THE SDLC_System SHALL cho phép Admin cấu hình model routing rules qua Admin Portal, bao gồm: mapping task_type → model_type, fallback model khi model chính không khả dụng, và điều kiện chuyển đổi (ví dụ: dùng ML model trước, nếu confidence < threshold thì fallback sang LLM).
5. THE SDLC_System SHALL tracking và báo cáo hiệu quả sử dụng model theo từng task type: so sánh latency, cost, accuracy giữa các model types cho cùng loại task, giúp Admin quyết định model routing tối ưu.
6. THE SDLC_System SHALL cho phép cấu hình "cost-performance profile" cho từng agent: "cost_optimized" (ưu tiên model rẻ/nhẹ khi có thể), "performance_optimized" (ưu tiên model mạnh nhất), hoặc "balanced" (mặc định — cân bằng chi phí và chất lượng).
7. THE SDLC_System SHALL đảm bảo tính pluggable: thêm loại model mới (ví dụ: multimodal model, audio model) SHALL chỉ cần đăng ký adapter mới mà không thay đổi core system hoặc agent logic hiện tại.
8. THE SDLC_System SHALL ghi log model selection decision cho mỗi task: task_type, models considered, model selected, routing rule applied, và lý do (ví dụ: "ML classifier selected — task is sentiment classification, cost_optimized profile").

---

### Requirement 40: Chế độ Triển khai Model AI (AI Model Deployment Modes)

**User Story:** Là một Platform Engineer, tôi muốn hệ thống hỗ trợ cả model chạy local (self-hosted) và gọi qua cloud API, để linh hoạt lựa chọn dựa trên yêu cầu bảo mật dữ liệu, chi phí và hiệu năng của từng tổ chức.

#### Acceptance Criteria

1. THE SDLC_System SHALL hỗ trợ 3 chế độ triển khai model AI: (a) **Cloud API** — gọi model qua API của provider bên ngoài (OpenAI, Anthropic, Google, AWS Bedrock...), (b) **Self-hosted** — model chạy trên infrastructure nội bộ của tổ chức (via Ollama, vLLM, TGI, hoặc custom inference server), (c) **Hybrid** — kết hợp cả hai, routing theo rules.
2. THE SDLC_System SHALL cho phép cấu hình deployment mode per-model: mỗi model đăng ký trong hệ thống (R38-C3) SHALL có trường deployment_mode (cloud/self-hosted) kèm connection config tương ứng — Cloud: API endpoint + API key + region; Self-hosted: internal endpoint + health check URL + GPU resource info.
3. THE SDLC_System SHALL cho phép cấu hình routing rules dựa trên data sensitivity: IF dữ liệu đầu vào chứa tag "confidential" hoặc "pii" hoặc thuộc project có policy "data_local_only", THEN SHALL route đến self-hosted model; IF không có constraint, THEN SHALL route theo cost-performance profile (R39-C6).
4. THE SDLC_System SHALL hỗ trợ fallback giữa các deployment modes: IF self-hosted model không khả dụng (health check fail hoặc queue quá tải), THEN SHALL fallback sang cloud API (nếu data policy cho phép); IF cloud API bị rate-limited hoặc unavailable, THEN SHALL fallback sang self-hosted (nếu có).
5. THE SDLC_System SHALL cung cấp monitoring riêng cho từng deployment mode: Cloud — tracking API latency, rate limit remaining, cost per request; Self-hosted — GPU utilization, queue depth, inference latency, model load status.
6. THE SDLC_System SHALL cho phép Admin cấu hình data residency policy per-team/project: "cloud_allowed" (mặc định — có thể dùng cloud API), "local_preferred" (ưu tiên self-hosted, fallback cloud nếu cần), hoặc "local_only" (nghiêm cấm gửi data ra cloud API — vi phạm SHALL bị block và ghi alert).
7. THE SDLC_System SHALL đảm bảo interface giữa agent và model inference là thống nhất bất kể deployment mode: agent không cần biết model đang chạy local hay cloud; routing layer SHALL transparent handle connection, retry, và failover.
8. THE SDLC_System SHALL ghi log deployment mode cho mỗi lần gọi model (bổ sung vào R24): model_id, deployment_mode (cloud/self-hosted), endpoint used, data_policy applied, và fallback triggered (true/false kèm lý do nếu có).

---

### Requirement 41: Cấu hình Mặc định Ban đầu (Default Hooks, Steering & Skills)

**User Story:** Là một Platform Engineer, tôi muốn hệ thống có sẵn bộ hooks, steering và skills cơ bản ngay khi khởi tạo, để các đội nhóm có thể bắt đầu sử dụng ngay mà không phải cấu hình từ đầu.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp sẵn bộ default hooks khi khởi tạo hệ thống, bao gồm tối thiểu:
   - **lint-on-code-save**: Tự động chạy linting khi file code thay đổi (fileEdited, patterns: *.ts, *.py, *.java, *.go).
   - **validate-output-before-save**: Kiểm tra output theo template/schema trước khi ghi Artifact (preToolUse, toolTypes: write).
   - **run-tests-after-task**: Chạy test suite sau mỗi task hoàn thành (postTaskExecution).
   - **security-scan-on-code**: Quét credentials và vulnerabilities khi tạo file code mới (fileCreated, patterns: *.ts, *.py, *.java, *.go).
   - **notify-on-pipeline-complete**: Tạo summary và gửi thông báo khi pipeline hoàn tất (agentStop).
   - **sentiment-check**: Phân tích cảm xúc người dùng mỗi lần gửi prompt (promptSubmit).
   - **template-match-on-new-project**: Tìm và đề xuất template phù hợp khi phát hiện yêu cầu tạo project mới (promptSubmit).

2. THE SDLC_System SHALL cung cấp sẵn bộ default steering files khi khởi tạo, bao gồm tối thiểu:
   - **coding-standards.md** (inclusion: auto): Quy tắc coding conventions, naming, folder structure, design patterns chuẩn.
   - **security-guidelines.md** (inclusion: auto): Quy tắc bảo mật — không hardcode credentials, input validation, OWASP top 10 awareness.
   - **architecture-principles.md** (inclusion: auto): Nguyên tắc kiến trúc — clean architecture, separation of concerns, dependency injection.
   - **documentation-standards.md** (inclusion: auto): Quy tắc viết tài liệu — format, required sections, language style.
   - **git-workflow.md** (inclusion: auto): Quy trình git — branching strategy, commit message format, PR conventions.
   - **testing-strategy.md** (inclusion: fileMatch, pattern: *.test.*): Chiến lược testing — coverage targets, naming conventions, test types.
   - **api-design-guidelines.md** (inclusion: fileMatch, pattern: *controller*, *route*): Quy tắc thiết kế API — REST conventions, versioning, error response format.
   - **deployment-checklist.md** (inclusion: manual): Checklist trước khi deploy — review points, rollback plan, monitoring setup.

3. THE SDLC_System SHALL cung cấp sẵn bộ default skills khi khởi tạo, bao gồm tối thiểu:
   - **database-migration**: Sinh migration files từ data model changes (Coding_Agent).
   - **api-documentation**: Sinh OpenAPI/Swagger docs từ code (Coding_Agent, Design_Agent).
   - **changelog-generator**: Tự động sinh changelog từ commits/PRs (Deployment_Agent).
   - **dependency-audit**: Kiểm tra dependencies outdated/vulnerable (Testing_Agent, Coding_Agent).
   - **code-review**: Review code theo coding standards + suggest improvements (Coding_Agent).
   - **performance-analysis**: Phân tích bottleneck từ metrics/logs (Operations_Agent).
   - **incident-postmortem**: Sinh incident report từ alert history (Operations_Agent).
   - **task-estimation**: Ước lượng effort dựa trên historical data (Task_Agent).
   - **translation**: Dịch tài liệu/UI text sang ngôn ngữ khác (Requirements_Agent).
   - **diagram-generator**: Sinh diagrams (Mermaid/PlantUML) từ mô tả text (Design_Agent).

4. THE SDLC_System SHALL cho phép Admin customize, disable, hoặc xóa bất kỳ default hook/steering/skill nào; các thay đổi SHALL chỉ ảnh hưởng đến team/project được cấu hình mà không ảnh hưởng global defaults.

5. THE SDLC_System SHALL cho phép Admin reset về bộ defaults ban đầu cho một team/project nếu cần; reset SHALL khôi phục toàn bộ hooks/steering/skills về trạng thái mặc định mà không ảnh hưởng custom items đã tạo riêng.

6. WHEN hệ thống được cập nhật phiên bản mới có bổ sung default hooks/steering/skills mới, THE SDLC_System SHALL thông báo cho Admin về items mới và cho phép opt-in kích hoạt mà không tự động áp dụng lên teams/projects đang hoạt động.

---

### Requirement 42: Hệ thống Báo cáo RADIO Tự động (Automated RADIO Reporting)

**User Story:** Là một Project Manager, tôi muốn hệ thống tự động sinh báo cáo RADIO (Review, Action, Difficulty, Information, Outcome) từ dữ liệu thực thi, để nắm bắt tiến độ chính xác mà không phụ thuộc báo cáo chủ quan từ developer.

#### Acceptance Criteria

1. THE SDLC_System SHALL tự động sinh RADIO report theo format 5 thành phần bắt buộc: **R** (Review — trạng thái hiện tại so với spec), **A** (Action — công việc đang thực hiện), **D** (Difficulty — blockers, risks cần escalation), **I** (Information — thông tin bổ sung ảnh hưởng tiến độ), **O** (Outcome — kết quả đạt được, metrics cụ thể).
2. THE SDLC_System SHALL kích hoạt sinh RADIO report tự động khi xảy ra bất kỳ trigger nào sau: (a) task hoàn thành (task status chuyển sang done), (b) test thất bại (CI/CD báo fail), (c) phát hiện blocker/risk mới, (d) cuối ngày làm việc (nếu có progress), (e) cuối sprint, (f) hoàn thành wave trong task dependency graph; tần suất tối thiểu: ít nhất 1 lần/ngày nếu đang active.
3. THE SDLC_System SHALL sinh RADIO report từ dữ liệu thực tế (commits, test results, spec compliance, pipeline status, task status) — KHÔNG dựa trên self-report chủ quan.
4. THE SDLC_System SHALL lưu RADIO report vào Git repository theo đường dẫn chuẩn `docs/specs/{feature-name}/radio.md` với version history đầy đủ.
5. WHEN RADIO report chứa mục "D" (Difficulty) với severity ≥ High, THE SDLC_System SHALL tự động gửi alert cho Tech Lead và PM kèm nội dung cụ thể của blocker.
6. THE SDLC_System SHALL cho phép Human_Reviewer xem lịch sử RADIO reports theo timeline và so sánh giữa các kỳ để phát hiện xu hướng.

---

### Requirement 43: Vòng lặp AI Tự Kiểm tra (AI Self-Review Loop)

**User Story:** Là một Tech Lead, tôi muốn AI tự kiểm tra output trước khi trình duyệt, để giảm số lần reject và nâng cao chất lượng artifact ngay từ lần đầu.

#### Acceptance Criteria

1. WHEN một agent sinh output (spec, design, tasks, code), THE SDLC_System SHALL kích hoạt vòng lặp self-review TRƯỚC KHI chuyển sang Output Validation (R22): agent tự đánh giá output theo các tiêu chí: clarity (không mơ hồ), completeness (đầy đủ), consistency (không mâu thuẫn nội bộ), compliance (tuân thủ spec/design input), và risk detection (phát hiện rủi ro tiềm ẩn). Thứ tự xử lý: Self-Review (R43) → Output Validation (R22) → Human Review (R14).
2. IF self-review phát hiện vấn đề, THEN agent SHALL tự sửa và lặp lại self-review; vòng lặp tối đa 3 lần trước khi trình Human.
3. WHEN self-review hoàn tất (pass hoặc hết 3 lần), THE SDLC_System SHALL ghi kèm metadata: số lần self-review loop, danh sách issues phát hiện và đã sửa, confidence score cuối cùng (0.0–1.0), và các issues còn tồn đọng (nếu có).
4. IF sau 3 lần self-review vẫn còn issues chưa giải quyết được, THEN agent SHALL trình output kèm danh sách issues tồn đọng để Human biết điểm cần chú ý khi review.
5. THE SDLC_System SHALL cho phép cấu hình self-review criteria per-agent và per-artifact-type; Admin có thể thêm/bớt tiêu chí kiểm tra.
6. THE SDLC_System SHALL ghi log toàn bộ self-review iterations: input, issues found, fixes applied, final decision (pass/escalate).

---

### Requirement 44: Quản lý Change Request (Change Request Management)

**User Story:** Là một Project Manager, tôi muốn hệ thống quản lý Change Request theo lifecycle rõ ràng, để không có CR nào bị bỏ sót và mọi thay đổi đều được theo dõi từ tiếp nhận đến hoàn thành.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp hệ thống quản lý Change Request với lifecycle: Pending → Approved → In Progress → Review → Done; hoặc Pending → Rejected. Mỗi CR có định danh duy nhất (CR-XXX).
2. THE SDLC_System SHALL lưu trữ CR backlog tập trung tại `docs/change-request/backlog.md` (plaintext, diff-able) bao gồm: CR_id, tiêu đề, mô tả, nguồn (client/internal), priority, status, ngày tạo, estimated effort, và assigned sprint.
3. WHEN CR được approve, THE SDLC_System SHALL tự động tạo thư mục `docs/change-request/CR-XXX/` chứa requirements.md, design.md (nếu effort > 1 ngày), và tasks.md — tuân theo cùng quy trình spec-driven như feature mới.
4. THE SDLC_System SHALL yêu cầu Human_Reviewer (PM hoặc Tech Lead) phê duyệt trước khi AI bắt đầu thực thi bất kỳ CR nào.
5. WHEN CR effort < 1 ngày (small CR), THE SDLC_System SHALL cho phép bỏ qua bước design và chỉ yêu cầu requirements + tasks.
6. WHEN CR hoàn thành, THE SDLC_System SHALL tự động cập nhật backlog.md (status → Done), sinh RADIO report cho CR, và chạy regression test.
7. THE SDLC_System SHALL cho phép AI đánh giá impact analysis cho mỗi CR: components bị ảnh hưởng, estimated effort, risks, và dependencies với features/CRs khác.
8. THE SDLC_System SHALL cung cấp dashboard CR: open/in-progress/done trend, throughput per sprint, average cycle time, overdue CRs.

---

### Requirement 45: Quản lý Q&A (Question & Answer Management)

**User Story:** Là một Business Analyst, tôi muốn hệ thống tự động phát hiện điểm mơ hồ trong spec và quản lý các câu hỏi làm rõ theo thread, để không có ambiguity nào bị bỏ qua và mọi kết luận đều được phản ánh lại vào spec.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp hệ thống Q&A Management với cấu trúc: mỗi câu hỏi là một thread riêng (QA-XXX) có lifecycle: Open → In Discussion → Answered → Closed.
2. THE SDLC_System SHALL lưu trữ Q&A index tại `docs/qa/index.md` và mỗi thread tại `docs/qa/QA-XXX/thread.md`, bao gồm: QA_id, câu hỏi, context (spec/design liên quan), priority (High/Medium/Low), target (ai trả lời), status, và deadline.
3. WHEN một agent (đặc biệt Requirements_Agent hoặc Design_Agent) phát hiện ambiguity trong input, THE SDLC_System SHALL tự động tạo QA thread mới với status Open, gắn reference đến phần spec/design mơ hồ.
4. THE SDLC_System SHALL hỗ trợ multi-round discussion: mỗi thread cho phép nhiều lượt trao đổi (follow-up) giữa AI, Human_Reviewer, và external stakeholder.
5. WHEN QA thread được trả lời đầy đủ, THE SDLC_System SHALL yêu cầu ghi Conclusion (kết luận tóm tắt) và Action Items (hành động tiếp theo: update spec, tạo CR, v.v.).
6. WHEN QA có conclusion approved, THE SDLC_System SHALL tự động cập nhật spec/design liên quan và ghi reference ngược (`Updated from QA-XXX`).
7. THE SDLC_System SHALL theo dõi deadline: High priority QA phải có answer trong 48h, Medium trong 5 ngày; WHEN overdue, SHALL flag trong RADIO "D" (R42) và escalate theo escalation matrix (R51).
8. THE SDLC_System SHALL đảm bảo spec KHÔNG được chuyển sang giai đoạn tiếp theo nếu còn QA Open liên quan chưa được giải quyết.

---

### Requirement 46: Quản lý Deliverables và Limitations (Deliverables & Limitations Management)

**User Story:** Là một Delivery Manager, tôi muốn hệ thống tự động tổng hợp danh sách sản phẩm đã hoàn thành và các giới hạn chưa giải quyết, để có cái nhìn rõ ràng về trạng thái delivery mà không phải thu thập thủ công.

#### Acceptance Criteria

1. THE SDLC_System SHALL tự động consolidate deliverables từ dữ liệu thực thi: tasks completed, test results, deployment status, spec compliance — tạo file `docs/deliverables/sprint-XX/deliverables.md` tại cuối mỗi sprint hoặc phase.
2. THE SDLC_System SHALL phân loại deliverables theo: feature (fully implemented + tested), partial (implemented nhưng chưa đầy đủ), và documentation (tài liệu đã tạo).
3. THE SDLC_System SHALL tự động phát hiện và ghi nhận limitations từ: tasks chưa hoàn thành, test cases đang fail, known issues, và requirements chưa implement — mỗi limitation có ID duy nhất (LIM-XXX).
4. THE SDLC_System SHALL phân loại limitations theo type: Not Implemented (yêu cầu tồn tại nhưng chưa làm), Partial (làm chưa xong), Technical Constraint (giới hạn kỹ thuật không thể vượt qua), Known Issue (bug đã biết chưa fix).
5. FOR EACH limitation, THE SDLC_System SHALL yêu cầu ghi handling plan: "Next Sprint", "Phase 2", "Won't Fix" kèm lý do; Human_Reviewer phải confirm plan trước khi gửi cho stakeholder.
6. THE SDLC_System SHALL duy trì file `docs/deliverables/baseline.md` ghi nhận baseline metrics từ sprint đầu tiên để so sánh improvement qua thời gian.
7. THE SDLC_System SHALL cung cấp bidirectional links: limitation references spec/CR/task, và spec/CR/task references limitation.

---

### Requirement 47: Approve Checklist Enforcement (Quản lý Checklist Phê duyệt)

**User Story:** Là một Tech Lead, tôi muốn hệ thống enforce checklist bắt buộc tại mỗi review gate, để đảm bảo Human_Reviewer không bỏ sót tiêu chí quan trọng khi approve.

#### Acceptance Criteria

1. THE SDLC_System SHALL cung cấp approve checklist bắt buộc cho mỗi loại review gate: Approve Requirements, Approve Design, Approve Tasks, và Approve PR (Code); mỗi checklist gồm danh sách items cần tick.
2. THE SDLC_System SHALL yêu cầu Human_Reviewer tick tất cả checklist items trước khi cho phép approve; IF bất kỳ item nào chưa tick, THEN SHALL chặn approve và hiển thị danh sách items còn thiếu.
3. THE SDLC_System SHALL cung cấp default checklist per gate type và cho phép Admin customize thêm/bớt items per-project hoặc per-team. Default checklist tối thiểu bao gồm: (a) Approve Requirements — complete, clear, EARS format, testable, no contradictions, edge cases, glossary, no assumptions, risks flagged, Q&A resolved; (b) Approve Design — covers requirements, architecture sound, security, performance, DB design, API contract, GUI prototype reviewable, error handling, risks flagged; (c) Approve Tasks — covers design, granularity < 4h, clear dependencies, requirements reference, testable outcome, test tasks included; (d) Approve PR — spec compliance, design compliance, tests pass, coverage ≥ 80%, security, performance, code quality.
4. THE SDLC_System SHALL ghi nhận kết quả checklist vào Git kèm approve decision: reviewer, timestamp, danh sách items đã tick, và comments (nếu có).
5. THE SDLC_System SHALL hỗ trợ conditional checklist items: items chỉ xuất hiện khi đáp ứng điều kiện (ví dụ: "Security review" chỉ hiện khi artifact chứa auth/payment logic).
6. THE SDLC_System SHALL cung cấp metrics: tỷ lệ first-time-pass (approve không reject), average review time per gate, và items thường bị bỏ sót nhất.

---

### Requirement 48: Definition of Done Enforcement (Kiểm tra Hoàn thành Tự động)

**User Story:** Là một Delivery Manager, tôi muốn AI tự động kiểm tra Definition of Done (DoD) trước khi báo task/feature hoàn thành, để đảm bảo không có artifact nào được đánh dấu "Done" khi chưa đạt đủ tiêu chí.

#### Acceptance Criteria

1. THE SDLC_System SHALL định nghĩa DoD cho mỗi loại artifact/phase: Spec (approved + versioned + no open QA + correct EARS format), Design (approved + GUI reviewable + covers all requirements + no open RISK), Tasks (approved + each task < 4h + clear dependencies + full traceability), Feature (all tasks done + all tests pass + spec compliance ≥ 95% + PR merged + clean RADIO + deliverables updated), Bugfix (root cause confirmed + fix merged + regression pass + no unchanged behavior broken + prevention applied).
2. WHEN AI hoặc Human đánh dấu artifact/phase là "Done", THE SDLC_System SHALL tự động chạy DoD check: xác minh từng tiêu chí trong DoD tương ứng.
3. IF bất kỳ tiêu chí DoD nào không đạt, THEN THE SDLC_System SHALL chặn chuyển status sang "Done", trả về danh sách tiêu chí chưa đạt kèm chi tiết (ví dụ: "2 tests failing", "QA-003 still Open").
4. THE SDLC_System SHALL cho phép Admin customize DoD per-project: thêm/bớt/sửa tiêu chí cho từng artifact type.
5. THE SDLC_System SHALL ghi log mỗi lần DoD check: artifact_id, tiêu chí checked, pass/fail per criterion, và kết quả tổng thể.
6. THE SDLC_System SHALL cung cấp override mechanism: Tech Lead có thể force-complete với lý do ghi nhận (ví dụ: "accepted technical debt") — decision SHALL được ghi vào audit log và RADIO "I".

---

### Requirement 49: Quy trình Bugfix Chuyên biệt (Bugfix-Specific Workflow)

**User Story:** Là một QA Engineer, tôi muốn hệ thống có quy trình bugfix rõ ràng với phân tích root cause (5WHY), xác định hành vi không thay đổi (regression prevention), và áp dụng biện pháp phòng ngừa, để bug không tái diễn và không gây regression.

#### Acceptance Criteria

1. WHEN Fix Mode được kích hoạt, THE SDLC_System SHALL yêu cầu tạo bugfix document (`docs/bugs/BUG-XXX/bugfix.md`) với format bắt buộc gồm 3 phần: (a) Current Behavior (Defect) — mô tả hành vi lỗi hiện tại, (b) Expected Behavior (Correct) — mô tả hành vi đúng theo spec, (c) Unchanged Behavior (Regression Prevention) — liệt kê hành vi KHÔNG được thay đổi.
2. THE SDLC_System SHALL yêu cầu AI thực hiện phân tích 5WHY (Five Whys) cho mỗi bug: hỏi "Why?" liên tiếp tối thiểu 3, tối đa 5 lần để tìm root cause; root cause phải là nguyên nhân có thể fix được (không phải "human error").
3. THE SDLC_System SHALL yêu cầu bugfix document bao gồm Prevention Plan: biện pháp ngăn bug tái diễn (ví dụ: thêm linter rule, thêm validation, thêm test case).
4. WHEN bugfix code được sinh ra, THE Testing_Agent SHALL tạo regression test cases bao phủ toàn bộ "Unchanged Behavior" đã liệt kê — đảm bảo fix không phá hỏng hành vi hiện tại.
5. THE SDLC_System SHALL kiểm tra DoD cho bugfix: root cause confirmed + fix merged + regression test pass + unchanged behavior verified + prevention applied; KHÔNG cho phép close bug nếu thiếu bất kỳ tiêu chí nào.
6. THE SDLC_System SHALL lưu bugfix document vào Git (`docs/bugs/BUG-XXX/`) kèm design.md (5WHY + solution) và tasks.md, tuân theo quy trình version control R16.

---

### Requirement 50: Chính sách Xử lý Lỗi và Retry Thống nhất (Unified Failure Handling & Retry Policy)

**User Story:** Là một Tech Lead, tôi muốn hệ thống có chính sách retry và xử lý lỗi thống nhất cho tất cả agents, để hành vi khi gặp lỗi là nhất quán, predictable và có escalation path rõ ràng.

#### Acceptance Criteria

1. THE SDLC_System SHALL áp dụng retry policy thống nhất cho tất cả agents khi sinh output thất bại: Attempt 1 — agent tự sửa dựa trên error message/feedback; Attempt 2 — agent thử approach khác (different algorithm, different structure); Attempt 3 — agent tổng hợp root cause analysis + báo cáo escalation.
2. IF sau 3 lần retry agent vẫn thất bại, THEN THE SDLC_System SHALL: (a) dừng agent tại task đó, (b) sinh failure report gồm: 3 attempts summary, root cause hypothesis, suggested approach cho Human, (c) chuyển task sang "blocked" status, (d) ghi vào RADIO "D", (e) tạo ISSUE-XXX nếu severity ≥ Medium.
3. THE SDLC_System SHALL hỗ trợ Human override: sau khi agent thất bại 3 lần, Human (Developer role) có thể can thiệp trực tiếp hoặc cung cấp hướng dẫn cụ thể để agent retry lần 4.
4. THE SDLC_System SHALL áp dụng rollback strategy khi output sai: wrong code → `git revert`, wrong spec/design → revert version, entire feature sai hướng → revert branch + quay lại spec phase.
5. THE SDLC_System SHALL cho phép cấu hình số lần retry tối đa per-agent (mặc định 3) và timeout per-attempt (mặc định theo R12-C5).
6. THE SDLC_System SHALL ghi log toàn bộ retry attempts: agent_id, task_id, attempt number, approach used, error encountered, và outcome (success/fail/escalate).

---

### Requirement 51: Giao thức Giao tiếp và Thông báo (Communication Protocol)

**User Story:** Là một Project Manager, tôi muốn hệ thống có giao thức giao tiếp rõ ràng với SLA response time và reminder tự động, để không có approve/QA/escalation nào bị bỏ quên.

#### Acceptance Criteria

1. THE SDLC_System SHALL phân loại giao tiếp theo 5 loại với SLA khác nhau: Approve Request (4h High, 24h Medium), Clarification/QA (24h High, 48h Medium), Escalation (1h Critical, 4h High), Information/FYI (không yêu cầu response), và CR notification (24h acknowledge).
2. THE SDLC_System SHALL hỗ trợ cấu hình kênh giao tiếp per-type: Slack, Microsoft Teams, Email, In-app notification; mỗi loại giao tiếp có thể gửi qua kênh khác nhau.
3. WHEN AI cần Human response (approve, answer QA, confirm escalation), THE SDLC_System SHALL thực hiện reminder strategy tự động: Attempt 1 (ngay lập tức) — gửi notification; Attempt 2 (sau 24h) — remind + flag trong RADIO "D"; Attempt 3 (sau 48h) — escalate lên PM/người giám sát; Attempt 4 (sau 72h) — pause task liên quan, chuyển sang task khác.
4. THE SDLC_System SHALL cho phép cấu hình SLA thời gian response per-role và per-priority; WHEN SLA bị vi phạm, SHALL ghi log + cảnh báo manager.
5. THE SDLC_System SHALL cung cấp metrics giao tiếp: average response time per-role, SLA compliance rate, bottleneck roles (response chậm nhất), và communication volume trend.
6. THE SDLC_System SHALL hỗ trợ delegation: khi Human không available (nghỉ phép, busy), có thể delegate approve/response cho người thay thế với thời hạn configurable (theo R34-C5).

---

### Requirement 52: Chiến lược Phiên bản Spec (Spec Versioning Strategy)

**User Story:** Là một Tech Lead, tôi muốn spec documents có semantic versioning rõ ràng với quy tắc khi nào cần re-approve, để kiểm soát thay đổi chặt chẽ mà không tạo overhead cho những sửa đổi nhỏ.

#### Acceptance Criteria

1. THE SDLC_System SHALL áp dụng semantic versioning cho tất cả spec documents (requirements, design, tasks): **Major** (X.0 — thay đổi scope/logic), **Minor** (X.Y — bổ sung chi tiết, thêm edge case), **Patch** (X.Y.Z — sửa typo, format).
2. WHEN spec có thay đổi Major hoặc Minor, THE SDLC_System SHALL yêu cầu re-approval từ Human_Reviewer bắt buộc trước khi phiên bản mới có hiệu lực.
3. WHEN spec có thay đổi Patch (chỉ typo, format), THE SDLC_System SHALL cho phép auto-approve mà không cần Human review.
4. THE SDLC_System SHALL yêu cầu mỗi spec document ghi rõ: version number, last updated date, updated by, approved by, và change log table (version, date, changes, approved by).
5. THE SDLC_System SHALL tự động phát hiện loại thay đổi (Major/Minor/Patch) dựa trên diff analysis: thêm/xóa requirement → Major; thêm AC, clarify → Minor; typo/format → Patch; Human có thể override phân loại.
6. THE SDLC_System SHALL đảm bảo downstream artifacts (design, tasks, code) reference đúng version của upstream spec; WHEN upstream spec thay đổi Major/Minor, SHALL flag downstream artifacts cần cập nhật.

---

### Requirement 53: Quy tắc Ngoại lệ Quy trình (Process Exception Rules)

**User Story:** Là một Tech Lead, tôi muốn hệ thống có quy tắc rõ ràng về khi nào KHÔNG cần chạy full spec flow, để đội nhóm linh hoạt xử lý hotfix, config change hay typo mà không bị chậm bởi ceremony không cần thiết.

#### Acceptance Criteria

1. THE SDLC_System SHALL hỗ trợ các exception scopes sau — cho phép bỏ qua một phần hoặc toàn bộ spec flow: (a) **Production Hotfix** (critical, < 30 phút) — fix trực tiếp → PR → approve → merge; tạo bugfix.md SAU khi fix, (b) **Config Change** (env variable, feature flag) — PR trực tiếp, không cần spec, (c) **Typo/Copy Change** — PR trực tiếp, không cần spec, (d) **Prototype/POC** — chỉ cần requirements.md ngắn gọn, skip design + tasks, (e) **Dependency Update** (security patch) — PR + tests pass → approve, không cần spec, (f) **Small UI Tweak** (< 1h effort) — requirements + tasks, skip design.
2. THE SDLC_System SHALL cho phép cấu hình exception rules per-project: Admin có thể thêm/bớt/sửa exception types và điều kiện trigger.
3. WHEN người dùng chọn exception scope, THE SDLC_System SHALL ghi nhận lý do exception và giảm pipeline steps tương ứng — nhưng vẫn yêu cầu PR approval và test pass (tối thiểu).
4. THE SDLC_System SHALL áp dụng rule mặc định: nếu không chắc cần full spec hay exception → default là full spec (safer).
5. THE SDLC_System SHALL ghi audit log cho mọi exception usage: user_id, exception type, lý do, và artifact_id liên quan — để theo dõi nếu exception bị lạm dụng.
6. THE SDLC_System SHALL cung cấp metrics: tỷ lệ exception vs full-spec per sprint, exception types phổ biến nhất, và correlation giữa exception usage với defect rate.
