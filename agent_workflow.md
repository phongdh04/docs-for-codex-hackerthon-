# Agentic Workflow - Ezi Solutions Project Management

Tài liệu này mô tả quy trình phối hợp giữa 5 Agent (Lina, Mattin, Bob, David, Sarah) và PM/PO.

## 1. Sơ đồ Workflow tổng thể (Cập nhật có Sarah)

```mermaid
graph TD
    User([Yêu cầu thô]) --> Lina[Lina - Senior BA]
    Lina --> QA_Batch{Lina đặt câu hỏi Q&A}
    QA_Batch -- Trả lời --> Lina
    
    subgraph "Vòng lặp Phê duyệt Specs & Test Scenarios"
    Lina -- "Viết Specs" --> Mattin[Mattin - BA Lead]
    Mattin -- "Audit Nghiệp vụ" --> David[David - Architect]
    David -- "Audit Kiến trúc" --> Mattin
    
    Lina -.->|Specs| Sarah[Sarah - QC Lead]
    Sarah -- "Viết Test Scenarios" --> Mattin
    Sarah -- "Viết Automation Spec" --> David
    
    Mattin -- "Yêu cầu sửa" --> Lina
    Mattin -- "Yêu cầu sửa Test" --> Sarah
    end
    
    Mattin -- "Trình duyệt (Pass)" --> PMPO[PM/PO - Người dùng]
    
    PMPO -- "Phê duyệt" --> Bob[Bob - Tech Lead]
    
    subgraph "Giai đoạn Thiết kế & Đặc tả UI"
    Lina -- "Ý tưởng" --> Robin[Robin - Designer]
    Robin -- "Đọc STITCH.md" --> Robin
    Robin -- "Concept Image / Stitch UI" --> Lina
    Lina -- "Chốt thiết kế" --> Robin
    Robin -- "Viết UI/UX Spec & Export Assets" --> Lina
    Lina -- "Hoàn tất bộ Spec (Specs + UI/UX)" --> Mattin
    end

    subgraph "Đội ngũ Thực thi (Devs)"
    Devs_BE[Kevin - Senior BE Dev]
    Devs_Ang[Luffy - Senior Angular Dev]
    Devs_React[Sanji - Senior React Dev]
    Devs_Mob[Zoro - Senior Mobile Dev]
    end
    
    subgraph "Vòng lặp Thực thi & Kiểm thử"
    Bob -- "Phân rã Task Specs" --> David_Final[David - Architect]
    David_Final -- "Audit Task Specs" --> Bob
    Bob -- "Chốt Task Specs" --> Devs_BE
    Bob -- "Chốt Task Specs" --> Devs_Ang
    Bob -- "Chốt Task Specs" --> Devs_React
    Bob -- "Chốt Task Specs" --> Devs_Mob
    
    Devs_BE -- "API Contract / Code" --> Bob_Review[Bob - Tech Lead]
    Devs_Ang -- "Angular UI" --> Bob_Review
    Devs_React -- "React UI" --> Bob_Review
    Devs_Mob -- "Mobile UI" --> Bob_Review
    
    Bob_Review -- "Task OK" --> Sarah_Task[Sarah - Task Test]
    Sarah_Task -- "Bug (Task)" --> Bob_Filter
    
    Sarah_Task -- "Pass (All Tasks)" --> Sarah_Story[Sarah - Story E2E]
    
    Sarah_Story -- "Phát hiện Bug (Story)" --> Bob_Filter[Bob - Bug Filtering]
    Bob_Filter -- "Yêu cầu Fix" --> Devs_BE
    Bob_Filter -- "Yêu cầu Fix" --> Devs_Ang
    Bob_Filter -- "Yêu cầu Fix" --> Devs_React
    Bob_Filter -- "Yêu cầu Fix" --> Devs_Mob
    Sarah_Story -- "Test Report (Pass)" --> PMPO
    end
    
    Bob_Review -- "Review Report" --> PMPO
    
    style David fill:#f96,stroke:#333,stroke-width:2px
    style David_Final fill:#f96,stroke:#333,stroke-width:2px
    style Sarah fill:#dfd,stroke:#333,stroke-width:2px
    style Sarah_Test fill:#dfd,stroke:#333,stroke-width:2px
    style PMPO fill:#bbf,stroke:#333,stroke-width:4px
    style Robin fill:#fbc,stroke:#333,stroke-width:2px
    style Devs_BE fill:#ffd,stroke:#333,stroke-width:2px
    style Devs_Ang fill:#ffd,stroke:#333,stroke-width:2px
    style Devs_React fill:#ffd,stroke:#333,stroke-width:2px
    style Devs_Mob fill:#ffd,stroke:#333,stroke-width:2px
```

## 2. Chi tiết vai trò các Agent

### [Lina - Senior Business Analyst](file:///c:/ezi-solutions/autodev/autodev.docs/lina-senior-ba/AGENTS.md)
Sản xuất Spec chi tiết. Chốt ý tưởng thiết kế với Robin.

### [Robin - UI/UX Designer](file:///c:/ezi-solutions/autodev/autodev.docs/robin-ui-ux-designer/AGENTS.md)
"Người gác cổng thẩm mỹ". Quản trị thiết kế thông qua **[STITCH.md](file:///c:/ezi-solutions/autodev/autodev.docs/guidelines/repository-level/STITCH.md)**, sinh thiết kế qua Stitch/Image Gen và viết `ui_ux_spec.md`.

### [Mattin - BA Lead](file:///c:/ezi-solutions/autodev/autodev.docs/mattin-ba-lead/AGENTS.md)
"Người gác cổng nghiệp vụ". Audit Lina, Robin và Sarah.

### [David - System Architect](file:///c:/ezi-solutions/autodev/autodev.docs/david-system-architect/AGENTS.md)
"Người gác cổng kiến trúc". Audit Specs, giải pháp kỹ thuật và phương pháp Automation.

### [Sarah - QC Lead](file:///c:/ezi-solutions/autodev/autodev.docs/sarah-qc-lead/AGENTS.md)
"Cảnh sát chất lượng". Blackbox tester (Pytest/Playwright/Appium/Requests).

### [Bob - Tech Lead](file:///c:/ezi-solutions/autodev/autodev.docs/bob-tech-lead/AGENTS.md)
"Người thực thi chiến lược". Phân rã task, review code và lọc bug.

### [Kevin - Senior Backend Dev](file:///c:/ezi-solutions/autodev/autodev.docs/kevin-senior-be-dev/AGENTS.md)
"Bàn tay thực thi BE". Chuyên gia Clean Code (Java/Spring Boot).

### [Luffy - Senior Angular Dev](file:///c:/ezi-solutions/autodev/autodev.docs/luffy-senior-angular-dev/AGENTS.md)
"Bàn tay thực thi FE (Angular)". Chuyên gia Pixel Perfect (Angular/Nebular).

### [Sanji - Senior React Dev](file:///c:/ezi-solutions/autodev/autodev.docs/sanji-senior-react-dev/AGENTS.md)
"Bàn tay thực thi FE (React/Next)". Chuyên gia Next.js và TailwindCSS.

### [Zoro - Senior Mobile Dev](file:///c:/ezi-solutions/autodev/autodev.docs/zoro-senior-mobile-dev/AGENTS.md)
"Bàn tay thực thi Mobile". Chuyên gia Flutter và React Native.

## 3. Quy tắc Audit chéo
- **Sarah (QC)** bị Mattin audit về nghiệp vụ và David audit về kỹ thuật test.
- **Mattin (BA)** bị David audit về ranh giới hệ thống.
- **Bob (Tech)** bị David audit về kiến trúc task.
- **PM/PO** là người chốt cuối cùng mọi tranh chấp.
