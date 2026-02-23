# CompanySuggestPanel

OpenCorporates API 連携による会社名サジェスト機能を提供する Salesforce Lightning Web Component (LWC) です。

## 概要

CompanySuggestPanel は、ユーザーが会社名を入力する際にリアルタイムで会社候補を提案する Lightning Web Component です。OpenCorporates API と連携し、複数の管轄区域にわたって会社情報を検索できます。

### 主な機能

- **リアルタイム検索**: デバウンス処理により効率的な API 呼び出しを実現
- **複数管轄区域対応**: 異なる管轄区域での会社検索に対応
- **会社情報表示**: 会社名、管轄区域コード、会社番号、ステータスを表示
- **エラーハンドリング**: 包括的なエラーメッセージとユーザーフィードバック
- **デバッグモード**: トラブルシューティング用の組み込みデバッグログ機能
- **モックモード**: API トークンなしでのテストモード対応

## コンポーネント構成

### Lightning Web Components (LWC)

- `companySuggestPanel` - メイン検索・サジェストコンポーネント
  - リアルタイム会社検索機能
  - 候補リスト表示（ステータス情報付き）
  - 会社選択時のイベント発火

### Apex クラス

- `CompanySuggestService` - 会社検索サービスクラス
  - `searchCompanies()` - OpenCorporates API の呼び出し
  - `CompanyCandidate` DTO - API レスポンスのマッピング用

- `CompanySuggestServiceTest` - ユニットテスト
  - モック HTTP レスポンステスト

## セットアップ

### 前提条件

- Salesforce DX CLI がインストール済み
- Org エイリアスが設定済み（デフォルト: `yumiorg`）
- OpenCorporates API トークン（オプション：モックモードの場合は不要）

### API トークンの設定

実際の OpenCorporates API を使用する場合：

1. カスタムラベル `OpenCorporatesApiToken` を作成し、API トークンを設定
2. Salesforce org で名前付き認証情報 `OpenCorporates` を設定
3. コンポーネントは自動的にトークンを使用して API 呼び出しを実行

### モックモード

API トークンなしでの開発・テスト時：
- トークンが利用できない場合、コンポーネントが自動的に検出
- テスト用のモックデータを返却
- デバッグ用コンソールログを出力

## 開発ガイド

### プロジェクト構造

```
.
├── config/
│   └── project-scratch-def.json
├── force-app/
│   └── main/
│       └── default/
│           ├── classes/
│           │   ├── CompanySuggestService.cls
│           │   ├── CompanySuggestService.cls-meta.xml
│           │   ├── CompanySuggestServiceTest.cls
│           │   └── CompanySuggestServiceTest.cls-meta.xml
│           └── lwc/
│               └── companySuggestPanel/
│                   ├── companySuggestPanel.js
│                   ├── companySuggestPanel.html
│                   ├── companySuggestPanel.js-meta.xml
│                   └── __tests__/
│                       └── companySuggestPanel.test.js
├── sfdx-project.json
└── README.md
```

### ビルドとデプロイ

```bash
# スクラッチ org を作成
sfdx force:org:create -f config/project-scratch-def.json -a CompanySuggestPanel

# ソースコードをデプロイ
sfdx project:deploy:start --source-dir force-app/main/default --target-org CompanySuggestPanel

# テストを実行
sfdx force:apex:test:run -u CompanySuggestPanel
```

## 使用方法

Lightning ページにコンポーネントを追加：

```xml
<c-company-suggest-panel></c-company-suggest-panel>
```

`companyselect` イベントをリッスン：

```javascript
const panel = document.querySelector('c-company-suggest-panel');
panel.addEventListener('companyselect', (event) => {
  const selectedCompany = event.detail.company;
  console.log(selectedCompany);
});
```

## テスト

`CompanySuggestServiceTest.cls` に含まれるユニットテスト：

```bash
sfdx force:apex:test:run -u <target-org>
```

## デバッグ機能

- **debugMsg**: 画面にコンポーネント初期化ステータスを表示
- **コンソールログ**: JavaScript コンソールログでライフサイクルを追跡
- **Apex デバッグログ**: Apex クラスで API インタラクションをログ出力

## ライセンス

MIT

## 参考資料

- [Salesforce Lightning Web Components](https://developer.salesforce.com/docs/component-library/overview/components)
- [OpenCorporates API ドキュメント](https://api.opencorporates.com/documentation)
