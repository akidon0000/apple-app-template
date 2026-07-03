# apple-app-template

SwiftUI マルチプラットフォーム（iOS/macOS）アプリのテンプレートリポジトリ。
`.xcodeproj` は git 管理せず、XcodeGen（`project.yml`）から生成する。

## コマンド

```bash
# Xcode プロジェクト生成（clone 直後・project.yml 変更後に必須）
xcodegen generate

# ビルド
xcodebuild build -project AppTemplate.xcodeproj -scheme AppTemplate \
  -destination 'platform=macOS' CODE_SIGNING_ALLOWED=NO

# テスト（macOS / iOS シミュレータ）
xcodebuild test -project AppTemplate.xcodeproj -scheme AppTemplate \
  -destination 'platform=macOS' CODE_SIGNING_ALLOWED=NO
xcodebuild test -project AppTemplate.xcodeproj -scheme AppTemplate \
  -destination 'platform=iOS Simulator,name=iPhone 17 Pro' CODE_SIGNING_ALLOWED=NO

# Lint / Format
swiftlint --strict
swiftformat --lint .   # チェックのみ
swiftformat .          # 自動整形
```

## 構成

- `project.yml` — XcodeGen 定義。ターゲットは `AppTemplate`（iOS/macOS マルチプラットフォーム）と `AppTemplateTests`
- `Sources/` — アプリ本体（SwiftUI）
- `Resources/` — Assets.xcassets（AppIcon / AccentColor）
- `Tests/` — ユニットテスト（Swift Testing。XCTest ではない）
- Swift 6 言語モード + Approachable Concurrency + デフォルト MainActor 分離

## 規約

- テストは Swift Testing（`import Testing` / `@Test` / `#expect`）で書く
- コードスタイルは SwiftLint / SwiftFormat の設定に従う（コレクションの末尾カンマは必須）
- `.xcodeproj` 内の設定は直接編集しない。必ず `project.yml` を変更して再生成する

## このテンプレートから新規アプリを作る場合

1. GitHub 上で "Use this template" もしくは `gh repo create <name> --template akidon0000/apple-app-template`
2. `AppTemplate` を新しいアプリ名に一括置換:
   `grep -rl AppTemplate . --exclude-dir=.git | xargs sed -i '' 's/AppTemplate/YourApp/g'`
3. 必要なら `project.yml` の `bundleIdPrefix` と deploymentTarget を調整
4. `xcodegen generate` して開発開始
