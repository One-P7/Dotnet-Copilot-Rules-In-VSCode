GitHub Copilot Test Code Style Guidelines Prompt
1. 使用套件：
   - MSTest
   - NSubstitute
   - AutoFixture
   - FluentAssertions
2. 命名慣例：
   - 測試輔助方法可使用中文命名，提升可讀性。
   - 標準方法與變數使用駝峰式命名（例如：TestInitialize, _tutorialFileRepository）。
   - 私有成員變數以底線前綴（例如：_sno, _loginUser）。
   - 測試輔助方法命名：
        • Arrange 區塊：使用 Set 前綴（例如：Set已驗證的使用者身分為）。
        • Act 區塊：使用 When 前綴（例如：When執行刪除文件）。
3. 測試結構與組織：
   - 採用 AAA 模式（Arrange-Act-Assert）明確分段。
   - 測試方法名稱應清楚描述場景與預期結果。
   - 測試初始化統一在 TestInitialize 方法中設定相依項。
   - 使用測試屬性標記：
        • [Owner] 指定開發者
        • [TestCategory] 指定測試類別
        • [TestProperty] 指定目標方法
4. 測試輔助方法：
   - 創建語義化的輔助方法模擬真實操作。
   - 提供專用的工廠方法建立測試對象（例如：一個教學pdf檔）。
   - Arrange 區塊使用 Set 前綴；Act 區塊使用 When 前綴。
5. Assert(驗證)風格：
   - 優先使用 FluentAssertions 進行流式驗證（例如：result.Should().BeTrue()）。
   - 偶爾混用 MSTest 驗證（例如：Assert.AreEqual）。
   - 方法呼叫檢查建議使用 NSubstitute 語法（例如：Received(1), DidNotReceive()）。
6. 異常處理：
   - 使用 [ExpectedException] 標記預期會拋出的例外（例如：不支援的檔案類型）。
7. 程式碼格式：
   - 使用適當縮排與空行分隔邏輯區塊，並保持方法間空行提高可讀性。
   - 將相關測試方法按功能分組排列。
8. 測試覆蓋：
   - 全面覆蓋正常與邊緣情境（例如：資料不存在或部分文件存在）。
   - 測試應涵蓋資料庫與檔案系統操作的交互，並使用模擬隔離測試單元。
Tip:
   - 使用 Set 前綴標示 Arrange 輔助方法，使用 When 前綴標示 Act 輔助方法，
     以使測試代碼的意圖更為明確與易讀。