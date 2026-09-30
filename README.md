# PDF 選頁重組工具

使用 PySide6 製作的 Windows 桌面工具，可預覽 PDF、勾選指定頁面，並依畫面排列順序輸出成新的 PDF。預覽縮圖不會影響輸出品質。

## 安裝與啟動

需要 Python 3.11 以上版本及 [uv](https://docs.astral.sh/uv/)。

```powershell
uv sync
uv run pdfpick
```

## 操作方式

1. 按「選取 PDF」，或從檔案總管將單一 PDF 檔拖曳到程式視窗中開啟來源檔案。
2. 拖曳頁面縮圖或「拖曳排序」把手，調整預覽與輸出的頁面順序。
3. 點選各頁左上角的核取方塊，或使用「單數頁」「雙數頁」批次選取；單雙數依原始頁碼計算。
4. 使用左下角滑桿調整 25%–200% 預覽比例；100% 對應 Windows 的 96 DPI 顯示尺度。
5. 按「輸出選取頁面」並指定新 PDF 的儲存位置。

密碼保護的 PDF 暫不支援。輸出頁面依畫面排列順序；未勾選的頁面不會輸出。

## 測試

```powershell
uv run pytest
```

## 開發與維護

目前架構、背景預覽協定、縮放定義、輸出安全機制與修改入口整理於 [實作與維護指南](docs/IMPLEMENTATION.md)。

## Windows 執行檔與 macOS App／DMG

GitHub Actions 的「Build desktop apps」會在 main 更新、PR 或 Release 發佈時打包，也可在 Actions 頁面以 Run workflow 手動執行並指定分支或標籤。Release 的標籤必須包含打包設定檔。

產物包含 Windows x86_64 單一 EXE、macOS Apple Silicon（arm64）DMG，以及 SHA256 校驗檔。手動執行與一般提交的產物可從 Actions 的 Artifacts 下載；Release 發佈時會自動附加到該 Release。DMG 中可將 PDFPick.app 拖入 Applications，不需要另外安裝 Python。

目前未設定 Windows 程式碼簽章或 macOS Developer ID 簽章／公證，因此下載後可能出現系統的安全提示；這些套件並非已公證的 macOS 發行版。

打包全程在 GitHub 雲端環境執行。流程會先驗證 Qt、圖示、背景 PDF 渲染與重排輸出；macOS 使用系統內建的 ditto 與 hdiutil 建立並驗證 DMG。Actions 產物保留 30 天。
