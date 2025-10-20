# PBI-14: Tích hợp Trình chỉnh sửa Trực quan Directus

**Parent PBI**: [@PBI 14: Tích hợp Trình chỉnh sửa Trực quan Directus](../backlog.md)

## Overview

Dự án này nhằm mục đích tích hợp tính năng Trình chỉnh sửa Trực quan (Visual Editor) của Directus vào trang web hiện tại. Việc này sẽ trao quyền cho các biên tập viên chỉnh sửa nội dung trực tiếp ngay trên giao diện trang web, giảm thiểu việc chuyển đổi ngữ cảnh giữa CMS và trang live, từ đó tăng tốc độ và hiệu quả của quy trình cập nhật nội dung.

## Problem Statement

Hiện tại, quy trình làm việc của biên tập viên bị gián đoạn. Họ phải chỉnh sửa trong một giao diện (Directus Admin) và xem kết quả ở một giao diện khác (trang web). Điều này tạo ra một khoảng cách nhận thức, làm chậm quá trình và có thể dẫn đến lỗi. Việc tích hợp Trình chỉnh sửa Trực quan sẽ kết nối hai môi trường này, tạo ra một vòng lặp phản hồi tức thì và cải thiện trải nghiệm làm việc của đội ngũ nội dung.

## User Stories

- **US1 (Parent PBI Story):** Với tư cách là một biên tập viên, tôi muốn chỉnh sửa nội dung trực tiếp từ giao diện trang web để có thể tăng tốc quy trình cập nhật và giảm thiểu lỗi.
- **US2:** Là một nhà phát triển, tôi muốn cấu hình và khởi tạo thư viện Trình chỉnh sửa Trực quan để nó có thể giao tiếp với Directus và có thể được bật/tắt một cách linh hoạt.
- **US3:** Là một nhà phát triển, tôi muốn tạo một "wrapper component" và áp dụng nó cho một thành phần nội dung đơn giản (ví dụ: tiêu đề trang) để xác minh chức năng chỉnh sửa trực quan.
- **US4:** Là một biên tập viên, tôi muốn thấy các chỉ báo chỉnh sửa trên hầu hết các thành phần nội dung chính của trang (ví dụ: khối văn bản, hình ảnh, thẻ thông tin) để tôi có thể chỉnh sửa chúng một cách dễ dàng.
- **US5:** Là một biên tập viên, tôi muốn có một trải nghiệm chỉnh sửa mượt mà, với các chỉ báo rõ ràng và khả năng chuyển đổi giữa chế độ xem và chế độ chỉnh sửa.

## Technical Approach

- **Tích hợp Frontend:**
  - Cần tạo một component bậc cao (Higher-Order Component - HOC) hoặc một "wrapper component" (`VisualEditorWrapper`) để bọc các thành phần nội dung có thể chỉnh sửa.
  - Wrapper này sẽ nhận các props bao gồm `collection` và `item_id` từ dữ liệu được fetch từ Directus để khởi tạo trình chỉnh sửa, hiển thị chỉ báo "edit" và xử lý sự kiện nhấp chuột để điều hướng đến Directus.
- **Quản lý Cấu hình:**
  - Sử dụng biến môi trường (`NEXT_PUBLIC_VISUAL_EDITOR_ENABLED`) để kiểm soát việc bật/tắt tính năng.
  - Các thông tin như URL của Directus cũng phải được quản lý qua biến môi trường.
- **Khởi tạo:**
  - Viết một module hoặc hook (`useVisualEditor`) để khởi tạo trình chỉnh sửa, đọc các biến môi trường và thiết lập kết nối với Directus, đồng thời kiểm tra xem người dùng có được xác thực hay không.

## UX/UI Considerations

- Giao diện của các chỉ báo chỉnh sửa phải rõ ràng, dễ nhận biết nhưng không gây rối cho bố cục chung của trang.
- Các chỉ báo chỉ hiển thị cho người dùng đã được xác thực và có quyền.
- Cân nhắc thêm một nút gạt (toggle switch) để biên tập viên có thể chủ động bật/tắt chế độ chỉnh sửa trên giao diện.
- Cần có phản hồi cho người dùng (ví dụ: loading indicator) khi họ nhấp vào một liên kết chỉnh sửa.

## Acceptance Criteria

- **AC1:** Hệ thống hiển thị các chỉ báo "edit" bên cạnh các thành phần nội dung có thể chỉnh sửa khi biên tập viên đã xác thực đang xem trang.
- **AC2:** Nhấp vào chỉ báo "edit" sẽ điều hướng biên tập viên đến đúng mục và trường trong giao diện quản trị Directus.
- **AC3:** Tính năng chỉnh sửa trực quan chỉ được kích hoạt cho người dùng có quyền và có thể bị vô hiệu hóa hoàn toàn thông qua biến môi trường.
- **AC4:** Việc tích hợp không làm ảnh hưởng đến hiệu suất tải trang đối với người dùng thông thường hoặc phá vỡ các chức năng hiện có.
- **AC5:** `VisualEditorWrapper` được tạo và áp dụng thành công cho các thành phần nội dung chính.
- **AC6:** Trải nghiệm chỉnh sửa mượt mà, với các chỉ báo được thiết kế tốt và có tài liệu hướng dẫn ngắn cho biên tập viên.

## Dependencies

- **Framework:** Next.js/React, TypeScript
- **CMS:** Directus (Cloud hoặc Self-hosted)
- **Thư viện:** `@directus/visual-editing`

## Open Questions

- (Không có)

## Related Tasks

- [@tasks.md](./tasks.md)
- [directus-next-cms](https://github.com/krharsh17/directus-next-cms)
- https://github.com/luochuanyuewu/nextus
