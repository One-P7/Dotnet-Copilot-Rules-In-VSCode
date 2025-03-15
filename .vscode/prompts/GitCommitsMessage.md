GitHub Copilot Git Commit Message Guidelines Prompt
Commit Message 結構:
  <type>(<scope>): <description> <footer>
1. 目的:
   - 描述程式碼變更的內容、目的、設計思路與業務邏輯，方便 Code Review 與 Bug 追蹤。
2. 基本結構:
   - <type>(<scope>): <description> <footer>
     • type: 此次 commit 的類型。
     • scope: 受影響的模塊或專案區域 (多個 scope 用逗號分隔，若超過三個可用 * 表示)。
     • description: 簡短描述此次變更。
     • footer: 關聯 issue 編號 (例如: #1234)。
3. Commit Type (類型):
   - feat: 新增或修改功能。
   - fix: 修補 bug。
   - docs: 文件修改。
   - style: 格式調整 (不影響程式碼執行，例如空白、格式、漏掉分號)。
   - refactor: 重構程式碼 (非新功能或 bug 修正)。
   - perf: 改善效能。
   - test: 新增或補充測試。
   - chore: 建構程序或輔助工具相關變更。
4. Commit Scope (範圍):
   - 常見範圍包括：
        api, logic, repo, sql, ef, infra, common, arch, security, ui, domain
5. 提示:
   - 異動範圍若為複數，請以逗號分隔；若超過三個，可用 * 替代。
   - 每次 commit 應盡量保持單一變更意圖，避免混合多個無關變更。
6. 範例:
   - feat(*): 新增買方貼標歷程查詢功能，包括 DTO、Repository、Controller 及相關設定 #16183
   - feat(ef): TrustMail Entity 設定自動更新欄位掛載 Attribute #9453
   - feat(logic): 調整私買刪除邏輯，移除檢查特定代碼買方紀錄的邏輯 #9453
   - feat(ef): 反向工程生成助購屋需求歷程的 EF Core Entity 至 Temp 資料夾 #16184