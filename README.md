# apple-app-template

SwiftUI マルチプラットフォーム（iOS/macOS）アプリのテンプレートリポジトリ。
`.xcodeproj` はコミットせず、[XcodeGen](https://github.com/yonaskolb/XcodeGen) の `project.yml` から生成する。

## 特徴

- **iOS / macOS 両対応** — SwiftUI マルチプラットフォームターゲット1本（不要な方は `project.yml` の `supportedDestinations` から外すだけ）
- **XcodeGen** — `.xcodeproj` を git 管理しないので、コンフリクトなし・リネームも一括置換で完了
- **Swift 6 言語モード** — Approachable Concurrency + デフォルト MainActor 分離（Xcode 26 の新規プロジェクト相当）
- **Swift Testing** — `@Test` / `#expect` ベースのテスト雛形付き
- **SwiftLint + SwiftFormat** — スタイル統一設定を同梱
- **GitHub Actions CI** — push / PR で iOS・macOS 両方のテストと Lint を実行

## 必要ツール

```bash
brew install xcodegen swiftlint swiftformat
```

- Xcode 26 以降

## 使い方（新規アプリ作成）

```bash
# 1. テンプレートからリポジトリ作成
gh repo create <your-app> --template akidon0000/apple-app-template --public --clone
cd <your-app>

# 2. アプリ名を一括置換（AppTemplate → 新しい名前）
grep -rl AppTemplate . --exclude-dir=.git | xargs sed -i '' 's/AppTemplate/YourApp/g'

# 3. Xcode プロジェクトを生成して開く
xcodegen generate
open YourApp.xcodeproj
```

必要に応じて `project.yml` の `bundleIdPrefix` と `deploymentTarget` を調整する。

## 開発コマンド

```bash
xcodegen generate      # project.yml 変更後に再生成
swiftlint --strict     # Lint
swiftformat .          # 整形

# テスト
xcodebuild test -project AppTemplate.xcodeproj -scheme AppTemplate \
  -destination 'platform=macOS' CODE_SIGNING_ALLOWED=NO
```

## 構成

```
├── project.yml          # XcodeGen 定義（ターゲット・スキーム・ビルド設定）
├── Sources/             # アプリ本体（SwiftUI）
├── Resources/           # Assets.xcassets（AppIcon / AccentColor）
├── Tests/               # ユニットテスト（Swift Testing）
├── .swiftlint.yml
├── .swiftformat
├── .github/workflows/ci.yml
└── CLAUDE.md            # Claude Code 用プロジェクトメモリ
```

## License

MIT
