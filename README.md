# Dotnet-Copilot-Rules-In-VSCode

這是一份關於我平常使用 VSCode 編寫 .NET 專案時針對 GitHub Copilot 調教的相關設定，GitHub Copilot 在生成程式碼、提交訊息、審查建議以及測試代碼前，會先閱讀 .vscode 資料夾中所存放的 Prompt Files ，在根據 Prompt Files 中所規定好的規範來生成結果。因此， 只要將 .vscode 放入你的專案中，並使用 VS Code 開啟，就可以開始使用這些設定了。
 
> [!Tip]
>
> 這些 Prompt 設定是根據我平時的 Coding 習慣，並不是通用準則，使用前可根據自身習慣更改 Prompt 內容來調教自己的 Copilot 
>

## 參考
- [Prompt 長度議題](https://waytoagi.feishu.cn/wiki/KLcEwKbDgir6JXkamVlcDO6en0b)
- [Prompt 手冊](https://github.com/Acmesec/PromptJailbreakManual)
- [參考專案](https://github.com/NikiforovAll/dotnet-copilot-rules)