# Workflow: Chi tiết hóa EPIC

## Description
Quy trình này hướng dẫn Lina truy xuất một EPIC đã được duyệt từ hệ thống và triển khai chi tiết các bộ Spec cho các User Story.

## Triggers
- **Manual Command (Thủ công):** Hệ thống hoặc PM thông báo một EPIC đã được duyệt và yêu cầu Lina chi tiết hóa.
   > *"EPIC-123 đã được duyệt, hãy tiến hành phân rã và chi tiết hóa Specs."*

## Mermaid Diagram

```mermaid
flowchart TD
  Start([Nhận lệnh chi tiết hóa EPIC]) --> GetEpicContext[Skill: get-epic-context]
  GetEpicContext --> CheckEpic{Epic tồn tại & hợp lệ?}
  CheckEpic -->|Không| ErrorEnd([Báo lỗi & Kết thúc])
  CheckEpic -->|Có| BreakStory[Phân rã User Story]
  
  BreakStory --> WriteSpecs[Skill: write-story-specs]
  WriteSpecs --> CheckSpecs{Specs hợp lệ & Nhất quán?}
  
  CheckSpecs -->|Không| RefineSpecs[Chỉnh sửa & Hoàn thiện Specs]
  RefineSpecs --> WriteSpecs
  
  CheckSpecs -->|Có| SaveStory[Skill: save-story-local]
  SaveStory --> End([Kết thúc quy trình])
```

## Steps

| # | Bước                              | Actor | Tool/Skill mã hóa                                                                                | Kết quả đầu ra (Output)                                                                          |
|---|-----------------------------------| ----- |--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| 1 | Truy xuất tài liệu Epic           | Lina  | `[../skills/lina-mcp/get-epic-context/SKILL.md](../skills/lina-mcp/get-epic-context/SKILL.md)`   | Bối cảnh Epic (epicTitle, storyList, epicDocuments, epicContext).                                |
| 2 | Phân rã Story & Viết Specs        | Lina  | `[../skills/write-story-specs/SKILL.md](../skills/write-story-specs/SKILL.md)`                   | Các file Spec: `user-story`, `concept_note`, `user-flow`, `data-dictionary`, `api-spec`, `db_design`. |
| 3 | Lưu Story Specs cục bộ            | Lina  | `[../skills/save-story-local/SKILL.md](../skills/save-story-local/SKILL.md)`                     | Toàn bộ 6 file Spec được lưu trữ thành công trong workspace.                                     |

## Definition of Done

* [ ] Đã kéo thành công bối cảnh EPIC bằng skill `get-epic-context`.
* [ ] Danh sách User Story được phân rã hợp lý từ bối cảnh EPIC đã lấy.
* [ ] Đã hoàn thiện đủ 6 file Spec (`user-story`, `concept_note`, `user-flow`, `data-dictionary`, `api-spec`, `db_design`) với dữ liệu đồng bộ và nhất quán.
* [ ] Toàn bộ các file Spec của Story đã được lưu trữ thành công cục bộ trong workspace qua skill `save-story-local`.
