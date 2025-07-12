# BitzFlow

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

BitzFlowは、BitzLabsエコシステム内で、ガントチャート、シーケンス図、Gitグラフなど、時間や順序の「流れ」を持つ多種多様なダイアグラムを扱うための専門ライブラリです。

## 主な機能

-   **`FlowAST`の定義**: レーン(Lane)、ノード(Node)、リンク(Link)という汎用的な概念に基づき、様々なフロー系ダイアグラムを統一的に表現するAST。
-   **2つの入力ルート**:
    1.  **パーサー**: PlantUMLのシーケンス図や、ガントチャート用の簡易言語など、それぞれの図に特化したDSLを解釈し、`FlowAST`を生成します。
    2.  **専用API (Builder API)**: C#やTypeScriptのコードから、図の種類に応じたビルダー（`GanttChartBuilder`など）を使って、直感的に`FlowAST`をプログラム的に構築できます。
-   **レイアウトエンジン**: `FlowAST`を受け取り、`diagramType`に応じて適切なレイアウトアルゴリズム（時間軸の計算、シーケンスの配置など）を実行し、その結果を描画プリミティブの集合である**`LayoutAST`**として出力します。

## このライブラリの位置づけ

主に`BitzDoc`から利用され、Markdown文書内にプロジェクト計画やシステムの相互作用を視覚的に埋め込む機能を提供します。

このライブラリの最終的な出力は`LayoutAST`です。これをSVGなどの具体的な画像形式に変換する処理は、`BitzRenderers`に委譲されます。

### 利用シナリオ

-   **DSL経由**: プロジェクトの進捗報告書にガントチャートを、設計書にAPIのシーケンス図を記述する。
-   **API経由**: バージョン管理システムのログから、リポジトリのブランチ履歴を動的にGitグラフとして描画する。

### 依存関係

-   `BitzAstCore`
-   `BitzParser`
-   `BitzLayout` (出力型として)

---
