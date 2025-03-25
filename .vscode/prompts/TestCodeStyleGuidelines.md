# GitHub Copilot Test Code Style Guidelines Prompt

## 1. 使用套件
- MSTest
- NSubstitute
- AutoFixture
- FluentAssertions

## 2. 命名慣例
- **測試類別命名**: `[被測試類別名稱]Tests`（例如：`FileServiceTests`）
- **測試方法命名**: 方法名稱_情境_預期結果，除了方法名稱以外皆使用正體中文台灣用語命名（例如：`UploadFileAsync_不支援檔案類型_拋出異常`, `Handle_DB存在資料且硬碟存在檔案_正確回傳`）
- **測試輔助方法命名**:
  - Arrange 區塊：使用「Set」前綴（例如：`Set已驗證的使用者身分為`）
  - Act 區塊：使用「When」前綴（例如：`When執行刪除文件`）
- **變數命名**:
  - 測試類別的私有成員變數以底線前綴（例如：`_sut`, `_fileRepository`）
  - 使用 `this` 關鍵字存取私有成員變數以及測試輔助方法（例如：`this._sut`, `this.When執行刪除文件()`）
  - 方法內的區域變數使用駝峰式命名（例如：`fileContent`, `userId`）

## 3. 測試結構與組織
- 採用 AAA 模式（Arrange-Act-Assert）明確分段，並使用註解標記各區段
- 使用標準的測試屬性：
  ```csharp
  [TestMethod]
  [Owner("開發者姓名")]
  [TestCategory("類別名稱")]
  [TestProperty("TargetMethod", "方法名稱")]
  public void 方法名稱_情境_預期結果() { ... }
  ```
- 將相關的測試方法按功能分組
- 每個測試類別使用一個 `TestInitialize` 方法進行共通設定：
  ```csharp
  [TestInitialize]
  public void Initialize()
  {
      this._fixture = new Fixture();
      this._fileRepository = Substitute.For<IFileRepository>();
      this._sut = new FileService(this._fileRepository);
  }
  ```

## 4. 依賴注入與 Mock 設置
- 所有依賴均在 `TestInitialize` 中創建
- 使用 NSubstitute 進行 mock 設置：
  ```csharp
  // 推薦的設置方式
  this._fileRepository.GetAsync(Arg.Any<string>()).Returns(expectedFile);

  // 而非混用其他框架語法
  ```
- 使用 AutoFixture 生成測試資料：
  ```csharp
  var fileModel = this._fixture.Create<FileModel>();
  ```

## 5. 測試輔助方法
- 創建有明確語義的輔助方法：
  ```csharp
  private void Set檔案類型為不支援格式()
  {
      this._file.Extension = ".xyz";
      this._fileValidator.IsSupported(Arg.Any<string>()).Returns(false);
  }

  private async Task<Result> When執行上傳檔案()
  {
      return await this._sut.UploadFileAsync(this._file);
  }
  ```
- 使用工廠方法建立特定測試對象：
  ```csharp
  private FileModel Create一個教學pdf檔()
  {
      return this._fixture.Build<FileModel>()
          .With(f => f.Extension, ".pdf")
          .With(f => f.Type, "application/pdf")
          .Create();
  }
  ```

## 6. 斷言（Assert）風格
- 統一使用 FluentAssertions 進行流式驗證：
  ```csharp
  // 推薦的驗證方式
  result.Should().BeTrue();
  files.Should().HaveCount(3);
  fileContent.Should().StartWith("測試內容");

  // 不建議混用
  // Assert.AreEqual(expected, actual);
  ```
- 測試例外情況：
  ```csharp
  // 推薦方式
  Func<Task> action = async () => await this._sut.UploadFileAsync(invalidFile);
  await action.Should().ThrowAsync<UnsupportedFileTypeException>();

  // 不使用 [ExpectedException]
  ```
- 方法呼叫驗證：
  ```csharp
  this._fileRepository.Received(1).SaveAsync(Arg.Any<FileModel>());
  this._logger.DidNotReceive().LogError(Arg.Any<string>());
  ```

## 7. 程式碼格式
- 使用適當縮排與空行分隔 AAA 各區段
- 測試方法間保持一行空行
- 相關的測試方法分組並用兩行空行分隔
- 使用 `this` 關鍵字呼叫測試輔助方法
- 範例：
  ```csharp
  [TestMethod]
  public void UploadFileAsync_支援檔案類型_成功上傳()
  {
      // Arrange
      var file = this.Create一個教學pdf檔();
      this._fileValidator.IsSupported(Arg.Any<string>()).Returns(true);

      // Act
      var result = await this._sut.UploadFileAsync(file);

      // Assert
      result.Should().BeTrue();
      this._fileRepository.Received(1).SaveAsync(Arg.Is<FileModel>(f => f.Id == file.Id));
  }
  ```

## 8. 測試情境覆蓋
- 確保至少覆蓋以下情境：
  - 正常流程（資料完整、操作成功）
  - 邊緣情境（資料不存在、部分資料存在）
  - 異常情境（操作失敗、異常拋出）
- 測試跨組件交互作用時，使用 mock 隔離測試範圍：
  ```csharp
  // DB 操作成功但檔案系統操作失敗的情境
  this._dbRepository.SaveAsync(Arg.Any<DocumentEntity>()).Returns(Task.FromResult(true));
  this._fileSystem.SaveFileAsync(Arg.Any<Stream>(), Arg.Any<string>()).Returns(Task.FromResult(false));
  ```

## 9. 測試隔離
- 每個測試方法應該獨立且不依賴其他測試的結果
- 使用 `TestCleanup` 在必要時清理資源：
  ```csharp
  [TestCleanup]
  public void Cleanup()
  {
      // 清理測試產生的檔案或資源
      if (File.Exists(this._tempFilePath))
      {
          File.Delete(this._tempFilePath);
      }
  }
  ```

## 10. 完整測試範例

```csharp
[TestClass]
public class FileServiceTests
{
    private IFixture _fixture;
    private IFileRepository _fileRepository;
    private IFileValidator _fileValidator;
    private FileService _sut; // System Under Test

    [TestInitialize]
    public void Initialize()
    {
        this._fixture = new Fixture();
        this._fileRepository = Substitute.For<IFileRepository>();
        this._fileValidator = Substitute.For<IFileValidator>();
        this._sut = new FileService(this._fileRepository, this._fileValidator);
    }

    [TestMethod]
    [Owner("張小明")]
    [TestCategory("檔案操作")]
    [TestProperty("TargetMethod", "UploadFileAsync")]
    public async Task UploadFileAsync_支援檔案類型_成功上傳()
    {
        // Arrange
        var file = Create一個教學pdf檔();
        Set檔案可被驗證通過();

        // Act
        var result = await When執行上傳檔案(file);

        // Assert
        result.Should().BeTrue();
        this._fileRepository.Received(1).SaveAsync(Arg.Is<FileModel>(f => f.Id == file.Id));
    }

    [TestMethod]
    [Owner("張小明")]
    [TestCategory("檔案操作")]
    [TestProperty("TargetMethod", "UploadFileAsync")]
    public async Task UploadFileAsync_不支援檔案類型_拋出異常()
    {
        // Arrange
        var file = Create一個不支援的檔案();
        Set檔案驗證不通過();

        // Act & Assert
        Func<Task> action = async () => await this._sut.UploadFileAsync(file);
        await action.Should().ThrowAsync<UnsupportedFileTypeException>();
        this._fileRepository.DidNotReceive().SaveAsync(Arg.Any<FileModel>());
    }


    // 工廠方法
    private FileModel Create一個教學pdf檔()
    {
        return this._fixture.Build<FileModel>()
            .With(f => f.Extension, ".pdf")
            .With(f => f.Type, "application/pdf")
            .With(f => f.Name, "教學文件.pdf")
            .Create();
    }

    private FileModel Create一個不支援的檔案()
    {
        return this._fixture.Build<FileModel>()
            .With(f => f.Extension, ".xyz")
            .With(f => f.Type, "application/xyz")
            .Create();
    }

    // Arrange 輔助方法
    private void Set檔案可被驗證通過()
    {
        this._fileValidator.IsSupported(Arg.Any<string>()).Returns(true);
    }

    private void Set檔案驗證不通過()
    {
        this._fileValidator.IsSupported(Arg.Any<string>()).Returns(false);
    }

    // Act 輔助方法
    private async Task<bool> When執行上傳檔案(FileModel file)
    {
        return await this._sut.UploadFileAsync(file);
    }
}
```