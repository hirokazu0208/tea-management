# ER Diagram v0.1

茶道教室管理システムの初期データモデル。

```mermaid
erDiagram

    PERSONS {
        string person_id PK
        string name
        string name_kana
        string display_name
        string status
        string note
        datetime created_at
        string created_by
        datetime updated_at
        string updated_by
    }

    EXTERNAL_ACCOUNTS {
        string external_account_id PK
        string person_id FK
        string provider
        string provider_user_id
        string provider_display_name
        datetime linked_at
        string status
        datetime created_at
        datetime updated_at
    }

    CLASSROOMS {
        string classroom_id PK
        string classroom_name
        string display_name
        string status
        string description
        string timezone
        datetime created_at
        string created_by
        datetime updated_at
        string updated_by
    }

    CLASSROOM_MEMBERSHIPS {
        string membership_id PK
        string classroom_id FK
        string person_id FK
        string member_type
        string membership_status
        date joined_at
        date left_at
        string note
        datetime created_at
        string created_by
        datetime updated_at
        string updated_by
    }

    ACTIVITIES {
        string activity_id PK
        string classroom_id FK
        string activity_type
        string title
        datetime start_at
        datetime end_at
        string location
        string status
        datetime attendance_deadline_at
        int preparation_count
        string note
        datetime created_at
        string created_by
        datetime updated_at
        string updated_by
    }

    PERSONS ||--o{ EXTERNAL_ACCOUNTS : "has"
    PERSONS ||--o{ CLASSROOM_MEMBERSHIPS : "belongs to"
    CLASSROOMS ||--o{ CLASSROOM_MEMBERSHIPS : "has"
    CLASSROOMS ||--o{ ACTIVITIES : has
```

## Responsibility

- `PERSONS`: 人そのもの
- `EXTERNAL_ACCOUNTS`: LINE等の外部アカウント
- `CLASSROOMS`: 教室そのもの
- `CLASSROOM_MEMBERSHIPS`: 人と教室の所属関係

## Current business rules

- 本籍生徒は `REGULAR_MEMBER`
- 先生は `TEACHER`
- 所属状態は `ACTIVE / ON_LEAVE / WITHDRAWN`
- 管理者権限は所属種別とは分離する
- 体験者・見学者は教室所属ではなく、将来 `participations` で管理する
- 場所代・先生送迎費は原則として `ACTIVE` な本籍生徒全員を按分対象とする
