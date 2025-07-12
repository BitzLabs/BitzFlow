# BitzFlow

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

BitzFlowは、BitzLabsエコシステム内で、ガントチャート、シーケンス図、Gitグラフなど、時間や順序の「流れ」を持つ多種多様なダイアグラムを扱うための専門ライブラリです。

## 主な機能

-   **`FlowAST`の定義**: レーン(Lane)、ノード(Node)、リンク(Link)という汎用的な概念に基づき、様々なフロー系ダイアグラムを統一的に表現するAST。
-   **2つの入力ルート**:
    1.  **パーサー**: PlantUMLのシーケンス図や、ガントチャート用の簡易言語など、それぞれの図に特化したDSLを解釈し、`FlowAST`を生成します。
    2.  **専用API (Builder API)**: C#やTypeScriptのコードから、図の種類に応じたビルダー（`GanttChartBuilder`など）を使って、直感的に`FlowAST`をプログラム的に構築できます。
-   **レイアウトエンジン**: `FlowAST`を受け取り、`diagramType`に応じて適切なレイアウトアルゴリズム（時間軸の計算、シーケンスの配置など）を実行し、その結果を描画プリミティブの集合である**`LayoutAST`**として出力します。

## ✅ 初期開発ToDoリスト

1.  **`FlowAST`の型定義**:
    *   `IFlowNode`インターフェース(`IAstNode`継承)を定義。
    *   `FlowDiagramNode`, `LaneNode`, `Node`, `LinkNode`の基本クラスの「殻」を作成。`FlowDiagramNode`には`DiagramType`プロパティを持たせる。
2.  **専用API (ビルダー) の設計**:
    *   まずは`GanttChartBuilder`クラスを設計。`.AddTask("ID", "Label", startDate, endDate)`のような、ガントチャートに特化したメソッドを提供する。
3.  **パーサーの骨格**:
    *   `FlowParser`ディスパッチャークラスを作成。
    *   まずは`GanttDslParser`サブパーサーの骨格を実装。単純なタスク定義をパースして`FlowAST`を生成するロジックを実装。
4.  **レイアウトエンジンのインターフェース**:
    *   `FlowLayoutEngine`クラスを作成し、`Render(IFlowNode node)`メソッドを定義。
    *   内部で`DiagramType`を見て処理を振り分けるロジックの骨格を実装。
    *   まずは`RenderGanttChart`メソッドで、単純なタスクバーを`LayoutAST`の`RectNode`と`TextSpanNode`に変換する処理を実装。

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
