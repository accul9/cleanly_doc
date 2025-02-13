# 家事タスク管理アプリのテーブル設計

## **概要**

家事タスクを管理する Web アプリのデータベース設計。

### **基本機能**

- タスクの CRUD（追加、表示、更新、削除）
- タスクの状態：「未着手」「完了」
- ユーザー登録必須（マルチユーザー対応）
- 家事タスクの分類表示（「日」「週間」「月間」）
- テンプレートの呼び出し機能

---

## **テーブル設計（Django + SQLite に適応）**

### **1. users（ユーザー）**

**目的:** ユーザーのアカウント情報を管理する。

| カラム名   | 型                                | 制約                      | 説明                         |
| ---------- | --------------------------------- | ------------------------- | ---------------------------- |
| id         | INTEGER PRIMARY KEY AUTOINCREMENT | -                         | ユーザーの一意識別子         |
| name       | TEXT                              | NOT NULL                  | ユーザー名                   |
| email      | TEXT                              | NOT NULL, UNIQUE          | メールアドレス（ログイン用） |
| password   | TEXT                              | NOT NULL                  | ハッシュ化されたパスワード   |
| created_at | DATETIME                          | DEFAULT CURRENT_TIMESTAMP | 作成日時                     |
| updated_at | DATETIME                          | DEFAULT CURRENT_TIMESTAMP | 更新日時                     |

---

### **2. tasks（家事タスク）**

**目的:** 家事タスクの情報を管理する。

| カラム名    | 型                                | 制約                                               | 説明                      |
| ----------- | --------------------------------- | -------------------------------------------------- | ------------------------- |
| id          | INTEGER PRIMARY KEY AUTOINCREMENT | -                                                  | タスクの一意識別子        |
| user_id     | INTEGER                           | FOREIGN KEY REFERENCES users(id) ON DELETE CASCADE | タスクの所有者            |
| title       | TEXT                              | NOT NULL                                           | タスク名                  |
| memo        | VARCHAR                           | NULL                                               | タスクの詳細説明          |
| is_complete | BOOLEAN                           | DEFAULT FALSE                                      | 未着手(False), 完了(True) |
| due_date    | DATE                              | NULL                                               | 期限（オプション）        |
| frequency   | INT                               | CHECK(frequency IN ('日', '週', '月'))             | タスクの分類（表示用）    |
| created_at  | DATETIME                          | DEFAULT CURRENT_TIMESTAMP                          | 作成日時                  |
| updated_at  | DATETIME                          | DEFAULT CURRENT_TIMESTAMP                          | 更新日時                  |

---

### **3. task_templates（タスクテンプレート）**

**目的:** ルーティンタスクのテンプレートを管理する。

| カラム名    | 型                                | 制約                                   | 説明                     |
| ----------- | --------------------------------- | -------------------------------------- | ------------------------ |
| id          | INTEGER PRIMARY KEY AUTOINCREMENT | -                                      | テンプレートの一意識別子 |
| title       | TEXT                              | NOT NULL                               | テンプレート名           |
| description | TEXT                              | NULL                                   | テンプレートの説明       |
| frequency   | TEXT                              | CHECK(frequency IN ('日', '週', '月')) | タスクの頻度             |
| created_at  | DATETIME                          | DEFAULT CURRENT_TIMESTAMP              | 作成日時                 |
| updated_at  | DATETIME                          | DEFAULT CURRENT_TIMESTAMP              | 更新日時                 |

---

### **4. completed_tasks（完了済みタスクのログ）**

**目的:** 完了済みタスクの履歴を保持する。

| カラム名     | 型                                | 制約                                               | 説明             |
| ------------ | --------------------------------- | -------------------------------------------------- | ---------------- |
| id           | INTEGER PRIMARY KEY AUTOINCREMENT | -                                                  | 一意識別子       |
| task_id      | INTEGER                           | FOREIGN KEY REFERENCES tasks(id) ON DELETE CASCADE | 対応するタスク   |
| user_id      | INTEGER                           | FOREIGN KEY REFERENCES users(id) ON DELETE CASCADE | 完了したユーザー |
| completed_at | DATETIME                          | DEFAULT CURRENT_TIMESTAMP                          | 完了日時         |

---

## **追加の考慮点**

### **1. タスクの共有機能（マルチユーザー対応）**

タスクを家族やグループで共有する場合、以下のテーブルを追加。

#### **task_users（タスクの共有）**

| カラム名   | 型                                | 制約                                               | 説明                     |
| ---------- | --------------------------------- | -------------------------------------------------- | ------------------------ |
| id         | INTEGER PRIMARY KEY AUTOINCREMENT | -                                                  | 一意識別子               |
| task_id    | INTEGER                           | FOREIGN KEY REFERENCES tasks(id) ON DELETE CASCADE | 対象のタスク             |
| user_id    | INTEGER                           | FOREIGN KEY REFERENCES users(id) ON DELETE CASCADE | タスクに関与するユーザー |
| role       | TEXT                              | CHECK(role IN ('作成者', '実行者'))                | タスクの権限             |
| created_at | DATETIME                          | DEFAULT CURRENT_TIMESTAMP                          | 作成日時                 |

---

## **SQL クエリ例**

### **1. 新規タスク作成**

```sql
INSERT INTO tasks (user_id, title, description, status, due_date, frequency)
VALUES (1, 'ゴミ出し', '可燃ごみを出す', FALSE, '2025-02-15', '週');
```

### **2. ユーザーの全タスク取得**

```sql
SELECT * FROM tasks WHERE user_id = 1 ORDER BY due_date;
```

### **3. タスクの状態を「完了」に更新**

```sql
UPDATE tasks SET status = TRUE WHERE id = 5;
INSERT INTO completed_tasks (task_id, user_id) VALUES (5, 1);
```

---
