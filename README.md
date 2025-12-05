# Traditional Mirai Research Center Web

このプロジェクトは、MarkdownファイルをベースにしたSPA（Single Page Application）ライクな静的ウェブサイトです。
HTML/CSS/JavaScriptのみで構成されており、ビルドプロセスを必要とせずにMarkdownを動的にレンダリングして表示します。

## システム概要

- **Markdown駆動**: コンテンツは全て `md/` ディレクトリ内のMarkdownファイルとして管理されます。
- **動的ルーティング**: URLパラメータ (`?p=...`) またはパスに基づいて、対応するMarkdownファイルを非同期で読み込み、ブラウザ上でHTMLに変換して表示します。
- **SPAライクな挙動**: ページ遷移時にフルリロードを行わず、JavaScriptでコンテンツを書き換えます（`History API`を使用）。
- **メタデータ管理**: `content.json` でサイト内のページ構成やメタデータ（タイトル、日付、カテゴリ、レイアウト等）を一元管理しています。

## ディレクトリ構成

```
.
├── md/                 # マークダウンファイル格納ディレクトリ (コンテンツ本体)
│   ├── index.md        # トップページ
│   ├── news/           # ニュース記事
│   ├── research/       # 研究プロジェクト記事
│   └── parts/          # ヘッダー・フッター等の共通パーツ
├── css/                # コンポーネント別CSSファイル
├── js/                 # アプリケーションロジック (main.js)
├── images/             # 画像ファイル
├── content.json        # サイト構成定義ファイル
└── index.html          # エントリーポイント
```

## コンテンツの管理方法

### 1. 新しい記事・ページの追加

1. **Markdownファイルの作成**:
   `md/` ディレクトリ内の適切な場所にMarkdownファイルを作成します。例えばニュース記事なら `md/news/YYYY-MM-DD-title.md` とします。

2. **フロントマターの記述**:
   ファイルの先頭にYAML形式でメタデータを記述してください。

   ```markdown
   ---
   title: "記事のタイトル"
   date: "2025-01-01"
   category: "news"    # news, research, etc.
   layout: "article"   # article (詳細), list (一覧), top (トップ)
   description: "記事の短い説明"
   image: "/images/news/thumbnail.jpg" # (任意) サムネイル画像
   ---
   ```

3. **content.json への登録**:
   `/content.json` に新しいエントリを追加します。ここで定義した情報がルーティングや一覧表示に使用されます。

   ```json
   {
     "url": "/news/new-article",
     "path": "md/news/new-article.md",
     "title": "記事のタイトル",
     "date": "2025-01-01",
     "category": "news",
     "layout": "article",
     "description": "記事の短い説明"
   }
   ```

### 2. リンクの貼り方

- **内部リンク**: `?p=md/path/to/file.md` の形式で記述します。
  - 例: `[お知らせ](?p=md/news/index.md)`

- **外部リンク**: 通常通り `https://...` で記述します。

## 開発・ローカル実行

特別なビルドツールは不要ですが、`fetch` APIを使用しているためローカルサーバーが必要です。

```bash
# Pythonを使用する場合
python3 -m http.server 8000

# Node.js (http-server) を使用する場合
npx http-server .
```

ブラウザで `http://localhost:8000` にアクセスしてください。

## 技術スタック

- **HTML5 / CSS3**: Vanilla CSS (CSS Variables使用)
- **JavaScript**: Vanilla JS (ES Modules不使用、単一ファイル)
- **markdown-it**: ブラウザ側でのMarkdownレンダリングエンジン
- **Google Fonts**: Noto Serif JP, Outfit
