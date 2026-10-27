# 唯音個人版本

這個分支從官方 4.9.2 建立。修改目的：在中文注音的「中英混合輸入回退」模式下，以 `Shift + .` 輸入 `>`、以 `Shift + ,` 輸入 `<`、以 `Shift + [` 輸入 `{`、以 `Shift + ]` 輸入 `}`，沿用混打的組字／遞交方式。它們不再優先查詢中文標點；其他按鍵保留既有規則。

修改僅適用於中英混打，未涵蓋關閉混打的純中文模式。候選視窗中既有的選字操作仍優先。ASCII 符號可能先顯示在組字區，按 Enter 遞交；不強制清空尚未完成的讀音。

## 自動更新

- fork 的預設分支設為 `personal`，排程工作流程才會執行。
- `main` 保留官方程式，不放個人修改；個人版本以官方正式 release 為更新來源，並非每個主線提交。
- 每天約台灣時間 10:23 檢查新正式版。GitHub 排程可能延遲，長期無活動的公開倉庫排程也可能被停用。
- 新版本先合併進 `automation/upstream-*` 分支並建立 PR，明確呼叫驗證流程，不仰賴機器人 PR 自動觸發另一個 workflow。
- Linux 引擎測試、macOS 引擎測試、macOS 通用版本建置全部成功，才合併 PR。需要分支審查時不會繞過審查。
- 衝突、測試失敗、工具鏈無法取得、辭典下載失敗，都會停止更新。舊 `personal` 不受影響。請在 GitHub 通知設定啟用 Actions 失敗通知。
- 在 fork 的 Settings → Actions → General 允許 GitHub Actions 建立 PR。受組織政策限制時需要管理員開啟。

## 下載與安裝

在 Actions 的成功驗證執行中下載 `vChewing-personal-*` artifact。內有安裝程式 ZIP、SHA256 與完整來源 commit。這是本機臨時簽署的個人版本，不是作者簽署／Apple 公證的官方版本。此流程不自動安裝、不發布 GitHub Release。

## 本機驗證

需要 Swift 6.4 以上與相容的 macOS SDK。Command Line Tools 若缺少 Foundation 巨集外掛，改用完整 Xcode 的工具鏈；不應為此修改引擎程式。

```sh
swift test --package-path Packages/vChewing_OSNeutral_LibVanguard --filter test_PersonalMixedShiftAngleBrackets
swift test --package-path Packages/vChewing_OSNeutral_LibVanguard
make release
```

測試包含半形／全形標點設定、有無英文或未完成讀音、已組好的中文，以及僅提供基底字元的鍵盤事件。正式使用前仍應在 VS Code 實測中文模式輸入 `<`、`>`、`{`、`}`、Enter 遞交、候選選取及 `Command + /`。
