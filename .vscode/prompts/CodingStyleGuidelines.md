GitHub Copilot C# Style Guidelines Prompt
1. 命名規範：
   - 介面：以 I 開頭，使用大駝峰 (例如：IDataService)
   - 類型（類別、結構、列舉、介面）：大駝峰 (例如：DataService)
   - 方法、屬性、事件、局部函數：大駝峰 (例如：GetData(), ProcessData())
   - 私有實例欄位：小駝峰，前綴底線 (例如：_dataService)
   - 公開/受保護的靜態或唯讀欄位及常數：大駝峰 (例如：DefaultValue, MaxRetries, MaxConnectionPoolSize)
   - 參數與局部變數：小駝峰 (例如：userId)
2. 變數宣告：
   - 強制使用 var 進行宣告。
3. 成員主體表達式：
   - 方法、建構函式、運算子：使用區塊主體。
   - 屬性、索引器、存取器：使用運算式主體。
4. 程式碼區塊與格式：
   - 區塊前（if、else、catch、finally 等）必須換行。
   - 即使單一陳述式，也必須使用大括號 { }。
   - 空格規則：
        • 控制流程關鍵字後需有空格 (if (condition))
        • 冒號、逗號及開啟大括號前需留空格 (class MyClass : BaseClass, DoSomething(a, b), if (condition) {)
        • 方法名稱與括號間不留空格 (DoSomething())
   - 新行與縮排：
        • 每個區塊開頭大括號前換行，內容縮排；case 區塊亦需縮排。
5. 程式碼組織：
   - Using 指令放在命名空間外，System 命名空間優先，並按字母排序。
   - 命名空間：採用檔案範圍命名空間 (C# 10 特性)。
6. 修飾詞順序：
   - 順序：public, private, protected, internal, static, extern, new, virtual, abstract, sealed, override, readonly, unsafe, volatile, async
7. 現代 C# 特性：
   - 使用模式匹配替代 as/空值檢查與 is 型別轉換。
   - 使用 Null 條件運算子 (?.) 與 Null 聯合運算子 (??)。
   - 優先使用 switch 表達式代替傳統 switch 陳述式。
   - 推薦使用內嵌變數宣告、default 常量、以及索引和範圍運算子 (^, ..)。
8. 程式碼實踐：
   - 在存取私有成員時，必須使用 this 關鍵字明確表示該成員屬於實例 (例如：this._dataService, this._loginUser)。
   - 每一個方法上方都必須要有 XML Summary 做解釋，
