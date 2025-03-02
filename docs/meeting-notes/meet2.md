# **第 2 回 MTG 議事録**

## **基本情報**

| **開催日時** |   2024 年 12 月 19 日（木） 20:00 ～ 21:00   |
| :----------: | :------------------------------------------: |
| **開催場所** |       Discord ボイスチャットチャンネル       |
|  **参加者**  | acccl76, kuwamushi, mickey, あずま, こまいぬ |

## **事前ワーク**

[Miro](https://miro.com/welcomeonboard/Y2wwbFlKbHpiZWtCOWI3Q2pIbCszdHhXUWFlNXhQU3BTa21yVEZrSXVoZDZjSEg2VjNZSW9Oai8yd2M2dUZGSkpkMEk1YnFkWS9wOWV4RXpSYUNBMDk0bzlwTGFaTVp6TGU2UGZhaFhYbW1LOHUxL3gwa0NEdHNMTEU5L2hCdkghZQ==?share_link_id=654086120055)

## **話し合われた内容**

### **1. 開発するアプリの選定**

#### **前提条件**

- **開発対象**: Web アプリ（iPhone や Android のネイティブアプリではない）
- **方式設計**: 設計書のフォーマットや必要事項を「こまいぬさん」が先行して着手。
  - 他にやりたい方がいれば協力可能。
  - 上流工程の経験が積める。

#### **検討アプリ**

- **現実的に作成可能なアプリ**:
  - **記事ストックアプリ**: 複雑度 Low、データ状態管理機能あり。
  - **家事記録アプリ**: 複雑度 Low、タスク管理＋ユニークな機能がポイント（例: ハビティカ風、カンバン式）。
- **難易度が高いアプリ**:
  - スーツケース最適ルート提案、日本語添削、声の分析、レシピ提案。
- **タイマーアプリ**:
  - 最初はタイマー＋ DB 登録機能に限定。
  - 後にデータ分析機能を追加し拡張可能。

#### **決定事項**

- 作成するアプリを投票で決定。
- 投票形式: 各参加者が 3 票を投じる。
- 投票期間: 1 週間。

---

### **2. 外部設計とデザイン**

#### **外部設計**

1. **UML で画面遷移図を作成**
   - 必要な画面の全体像と遷移関係を明確化。
2. **画面デザイン**
   - 白黒のワイヤーフレームレベルで作成。
   - PC 版とモバイル版のレスポンシブ対応を検討。
3. **配色とデザイン**
   - 共通コンポーネントを考案（例: ログインページなど）。
4. **機能一覧作成**
   - 各画面の機能と発火条件を整理（例: 初期表示やボタンのクリック）。

#### **内部設計**

- データベース設計やクラス設計に移行。

---

### **3. チーム編成**

#### **編成方針**

- **人数構成**:
  - 小回りが利く 3 人チームを推奨。
  - 2 チームで同じアプリを作成し、互いにナレッジを共有。
  - チーム間での助け合いを期待。
- **役割分担**:
  - 厳密な役割分担と柔軟な分担のバランスを模索。
  - 途中参加や繁忙期対応を考慮。

#### **次回までのタスク**

- 各参加者のプロフィールとスケジュールを可視化。
- チーム分けの材料として利用。

---

### **4. スケジュール計画**

#### **開発期間**

- **第 1 アプリ目標**:
  - 簡単なアプリを 4 月までに完成。
  - 個人の繁忙期を考慮して 3 ～ 4 か月を想定。
- **意見**:
  - 経験者なら 2 ～ 3 週間で可能だが、個人学習を重視して期間を延長。

---

### **5. 次回までの Action Items**

1. **アプリ投票の実施**
   - 投票形式：3 票制。
   - 結果を基にチーム分けを実施。
2. **プロフィールとスケジュールの提出**
   - チーム編成の参考にする。
3. **デザイン着手**
   - 1 月中にデザインを決定。
   - くわさんがデザイン担当を希望（受験時期を考慮）。

---

### **6. プロジェクト運営のアプローチ**

#### **柔軟な進行管理**

- 進行中に課題が発生した場合は計画を調整。
- チームを統合する選択肢も視野に。

#### **小規模開発の推奨**

- 画面数を 1 ～ 2 枚、メイン機能を 1 つに絞る。
- 短期間での開発を優先し、基盤構築に注力。

---

### **アドバイス**

- **ミッキーさんの提案**:
  - 一般的なフレームワークとメジャーな機能を採用。
  - 小さな単位で作業を分担。

---

### **まとめ**

- 次回ミーティングは年明けを予定。
- 投票とプロフィール可視化を進めた後、スケジュールと計画を確定。
