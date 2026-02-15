# 공지사항/알림 스키마

```mermaid
erDiagram
    members {
        uuid id PK
        varchar name
    }

    notices {
        uuid id PK
        varchar title
        text content
        uuid category_id FK
        uuid author_id FK
        boolean pinned
        int views
        timestamp created_at
    }

    notice_categories {
        uuid id PK
        varchar name UK
        timestamp created_at
    }

    notifications {
        uuid id PK
        uuid recipient_id FK
        varchar title
        varchar description
        uuid reference_id "nullable"
        varchar reference_type "nullable"
        boolean is_read
        timestamp created_at
    }

    announcements {
        uuid id PK
        varchar title
        varchar description
        uuid reference_id "nullable"
        varchar reference_type "nullable"
        timestamp created_at
    }

    announcement_reads {
        uuid id PK
        uuid announcement_id FK
        uuid member_id FK
        timestamp read_at
    }

    notices }o--|| notice_categories : "categorized"
    notices }o--|| members : "authored by"
    notifications }o--|| members : "sent to"
    announcements ||--|{ announcement_reads : "tracked by"
    announcement_reads }o--|| members : "read by"
```
