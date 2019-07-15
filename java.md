# java代码规范
## 中文版
https://github.com/alibaba/p3c/blob/master/%E9%98%BF%E9%87%8C%E5%B7%B4%E5%B7%B4Java%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C%EF%BC%88%E5%8D%8E%E5%B1%B1%E7%89%88%EF%BC%89.pdf

---
* **接口**  
    以I开头命名 `ICompare`
* **类名**  
    首字母大写 `TaskItem`
* **属性名称**  
    首字母大写 `public string Name`
* **字段名称**  
    下划线开头，首字母小写，其余跟其属性一样 `private string _name`
* **方法**   
    首字母大写，参数命名规则于变量一致 `void SayHello(TimeSpan timeSpan){}`
* **异步方法**
    以`Async`结尾，尽可能支持`CancellationToken` 参数支持

    ``` C#
    //错误示例
    public Task Run(){
        
    }

    //正确示例
    public Task RunAsync(CancellationToken cancellation =  default){

    }
    ```
* **变量**  
    首字母小写 `int leftNumber = 10;`

## 方法
1. 每个方法的内的代码不应该超过100行
2. 适当的作用域定义
3. 参数检查

## 释放资源
使用Using释放资源

## 异常处理
下层尽量避免try，上层统一处理异常

## 日志

## 工程结构

1. 要有条理
2. 同层之间不允许引用。
3. 入口于入口不应该相互调用 `Controler A 调用 Controler B`
4. 库工程避免隐性依赖，比如ConfigurationManager

## MVC

* 避免不明确的Action重载  
    `public string Get(int id)`与`public string Get(string id)`
* 指定HttpMethod
* 授权认证
* API参数要明确，避免在方法内去到Request里找
* 避免啰嗦的参数
* API请指明返回类型  
    避免`IActionResult`

## Bad Practice Need to Avoid

1. If variable is compile time constant, use const instead of static readonly to declare constant. Use static readonly to declare runtime constant only when it need be initialized at runtime.

    Bad practice example:

    ```cs
        private static readonly string rijndaelIV = "DAIMLERS_MBAFCLC";//128
    ```

2. If instance variable is reference type, instantiate it just before using it to avoid unnecessary memory allocation, because not all instance methods need it.
    Bad practice example:

    ```cs
        public class SendEmailbyHelm{
            List<QuotationDetailDataModel> updatelist = new List<QuotationDetailDataModel>();
            ……
    ```

3. Do not use exception to control the execution flow of a program, because CLR start to trace stack from the point of throw statement, this incurs inevitable overhead.

    Bad practice example:

    ```cs
        if (quotation == null)
            throw new Exception("Quotation not found.");
    ```

4. Add suffix “Async” for name of asynchronous method, it is common convention.

    Bad practice example:

    ```cs
        public async Task<IHttpActionResult> CheckDocumentUpload(int companyId, int applicationId, int btype)
    ```

5. Do not use Task.Run to call synchronous IO-bound method, this is called fake asynchronous method just faking it by doing synchronous work on background thread. It does not increase any scalability, but add unnecessary synchronization context switch. Use Task.Run to call CPU-bound method to add responsiveness.

    Bad practice example:

    ```cs
        return await Task.Run(() => InvokeCheckDocumentUpload(companyId, applicationId, btype));
    ```

6. Use StringBuilder instead of string concatenation avoid unnecessary heap space allocation.

    Bad practice example:

    ```cs
        string cmdText = "select ListPrice from AssetModel where AssetModel.ID = @AssetModelID;"
                        * " select RentalModeType as paymentType,percentage from vFinancialProductDetailListCustomer "
    ```

7. Declare all instance, static variable, property, delegate, event before all method.

    Bad practice example:

    ```cs
        private string CreateMD5Hash(string inputString)
        {
            MD5CryptoServiceProvider md5Hash;
            md5Hash = new MD5CryptoServiceProvider();
            byte[] inputBytes = System.Text.Encoding.Unicode.GetBytes(inputString);
            byte[] hashBytes = md5Hash.ComputeHash(inputBytes);
            return Convert.ToBase64String(hashBytes);
        }

        public string parameterReq { get; set; }

        static System.Threading.ReaderWriterLockSlim LogWriteLock = new System.Threading.ReaderWriterLockSlim();

        public DealerDiaryViewModel diaryModel = null;

        public string HttpPostJsonRequestForFIM(string Url, string parameters,string guid = "", Guid threadID = new Guid())
        {
            var startTime = DateTime.Now;
            var watch = Stopwatch.StartNew();
        }
    ```

8. It is better to insert proper blank line, like between variable declaration and following logic, logic and return statement.

    Bad practice example:

    ```cs
        var userController = new UserAccountController();
        var token = userController.GetDigitalFormTokenInfo();
        args.Signature.BankReservedContact = token.DafBankReservedContact;
        args.Signature.IdCardNumber = token.DafIdCardNumber;

        rtn.result = true;
        rtn.resultcode = "200";
        return rtn;
    ```

9. It is better to explicitly declare instance variable with access modifier 

    Bad practice example:
    
    ```cs
        public class DealerApplicationBusiness
        {
            ServiceClient sc;
            IFIMInterface iFimInterface;
    ```

10.	It is better to use value type enum instead of reference type class only contain integer constant but no any behavior. 

    Bad practice example:

    ```cs
        public class FpTypeCycle
        {
            public static readonly int Yearly = 12;
            public static readonly int Semi = 6;
            public static readonly int Quarterly = 3;
            public static readonly int Other = 1;
        }
    ```

11.	Do not call asynchronous method synchronously, because it runs synchronously and blocks calling thread.

    Bad practice example:

    ```cs
        //异步更新其它数据
        UpdateStatusAndOfficerByFIM(startDate, endDate, createdBy, -1, resultSet, applicationApprovedList);

        private async void UpdateStatusAndOfficerByFIM(string fromDate, string toDate, string createdBy, int dealerId, FIMInterface.Proposal.ProposalControlPanelInfo[] fimResultSet, List<ApplComWorkQueueDataModel> applicationApprovedList)
    ```

12.	Do not define async void method except async event handler, because void is not awaitable type and any exceptions happen in method cannot be propagated to callers, so basically it equivalents to fire-and-crash pattern.

    Bad practice example:

    ```cs
        private async void UpdateStatusAndOfficerByFIM()
    ```