# 🐍 Python Runner for Unity Editor

Unity Editor 上から Python スクリプトをワンクリックで実行・停止できるエディタ拡張です。  
プロジェクトルート基準の「相対パス」指定、マシンごとのローカル設定保存、起動時ポップアップ実行などに対応します。

---

## ✅ 機能概要

- Unity のメニューから Python スクリプトを実行 / 停止
- プロジェクトルート基準の **相対パス指定**（`Assets` の 1 つ上が基準）
- `python` / `python3` の切り替えトグル
- macOS / Windows / Linux 対応
- 標準出力 / 標準エラーをウィンドウ内にストリーム表示
- 設定は **Library 配下の JSON** に保存され、Git 共有されない
- 「プロジェクト起動時に実行するか？」を聞くポップアップ（オプション）
- すでに実行中であればポップアップを出さない安全設計
- macOS / Linux では `/usr/bin/env` 経由で python を解決し、PATH 問題に強い

---

## 📦 導入方法

1. このスクリプトファイルをプロジェクトに配置します。  
   例:  
   `Assets/IGNORANZ PROJECT/PythonRunner/Editor/PythonRunnerWindow.cs`

2. Unity がコンパイルを完了すると、メニューに項目が追加されます。  
   メニュー: **Tools → Python Runner**

3. クリックすると専用ウィンドウが開きます。

---

## 🖥 ウィンドウの使い方

### 1. Python ファイルの指定

**Python file (relative to project)** に、プロジェクトルートからの相対パスを指定します。

例:

```text
Assets/IGNORANZ PROJECT/FileSync/FileSync.py
```

### 2. 引数（Arguments）の指定

コマンドライン引数を、Python に渡す形のまま記述します。  
パスに **空白や日本語、`#` などの記号**が含まれる場合は、必ず **二重引用符 `"` で囲んでください。

例:

```text
"/Users/arata/Downloads/#unity/computer-game-IGNORANZ_PROJECT-Holo_Board/ProjectSettings/CollabSyncLocal.json" "/Users/arata/Library/CloudStorage/OneDrive-個人用/ドキュメント/IGNORANZ PROJECT/Holo Board/共有ファイル/CollabSync/CollabSyncLocal.json" --loop --interval 10 --verbose --collab-sync
```

> 🔸 注意:  
> 第 1 引数 = 読み込み用 JSON ファイル  
> 第 2 引数 = 書き込み先 JSON ファイル（フォルダではなく **ファイルパス**）である必要があります。

### 3. インタプリタの切り替え

- チェックボックス: **Use python3**
- macOS / Linux では `python3` の使用を推奨
- 右側のボタンで `python` ↔ `python3` をワンタッチ切り替え可能

### 4. ボタンの説明

| ボタン名              | 説明                                               |
|-----------------------|----------------------------------------------------|
| ▶ Run                 | Python プロセスを起動します                        |
| ■ Stop                | プロセスを通常停止します                           |
| Force Stop            | 子プロセスごと強制終了を試みます                   |
| Ask on project open   | Unity 起動時に「実行するか？」を確認するポップアップを有効化 |

---

## 🚀 起動時ポップアップ

`Ask on project open` が ON の場合、Unity 起動時に次のようなダイアログが表示されます。

- **Run**: 現在設定されているスクリプトを即時実行  
- **Cancel**: 何もしない  
- **Open Settings**: Python Runner ウィンドウを開く

さらに、以下の条件ではポップアップは **表示されません**:

- すでに Python Runner 経由で実行中のプロセスがある
- 前回記録された PID のプロセスがまだ生きていると判断できる場合

---

## 🧾 設定ファイルについて

設定は **プロジェクト共有されない** `Library` フォルダに保存されます。

パス:

```text
<ProjectRoot>/Library/PythonRunner/settings.json
```

例:

```json
{
  "pythonRelativePath": "Assets/IGNORANZ PROJECT/FileSync/FileSync.py",
  "arguments": ""/Users/.../ProjectSettings/CollabSyncLocal.json" "/Users/.../CollabSyncLocal.json" --loop --interval 10 --verbose --collab-sync",
  "usePython3": true,
  "askOnOpen": true,
  "lastPid": 12345
}
```

> 💡 ポイント:  
> - `Library` フォルダは通常 Git の管理外です。  
> - 各開発者が自分のマシンに合わせた Python パスやオプションを保存できます。

---

## 🛠 トラブルシューティング

### 「Python 実行エラー: ApplicationName='python' ...」と出る

- Unity から起動した環境に `python` というコマンドが無い可能性があります。
- 対処:
  - **Use python3** を ON にする（macOS では `python3` が標準的）
  - スクリプト内部では macOS / Linux 時に `/usr/bin/env python3` で解決するようになっています。

### JSON が出力されない

- 第 2 引数が **フォルダパスになっていないか**を確認してください。
- `.../CollabSyncLocal.json` のように、ファイル名まで含めたパスを渡す必要があります。

### ログが長くなりすぎる

- ログビューには最新 **200 行まで**が保持されます。
- 「Clear」ボタンで消去、「Copy」でクリップボードにコピーできます。

---

## 📂 想定フォルダ構成

```text
Assets/
└── IGNORANZ PROJECT/
    ├── PythonRunner/
    │   ├── Editor/
    │   │   └── PythonRunnerWindow.cs
    │   └── README.md
    └── FileSync/
        └── FileSync.py

Library/
└── PythonRunner/
    └── settings.json   （自動生成・Git共有なし）
```

---

## 👤 ライセンス
MIT LICENSE
