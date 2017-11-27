---
title: "不適當的具現化反模式"
description: "避免對只需建立一次然後進行共用的物件持續建立新執行個體。"
author: dragon119
ms.date: 06/05/2017
ms.openlocfilehash: 3387eca45119b105b68cd60ab842d033df84727c
ms.sourcegitcommit: b0482d49aab0526be386837702e7724c61232c60
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2017
---
# <a name="improper-instantiation-antipattern"></a><span data-ttu-id="4a723-103">不適當的具現化反模式</span><span class="sxs-lookup"><span data-stu-id="4a723-103">Improper Instantiation antipattern</span></span>

<span data-ttu-id="4a723-104">如果對只需建立一次然後進行共用的物件持續建立新執行個體，會對效能造成傷害。</span><span class="sxs-lookup"><span data-stu-id="4a723-104">It can hurt performance to continually create new instances of an object that is meant to be created once and then shared.</span></span> 

## <a name="problem-description"></a><span data-ttu-id="4a723-105">問題說明</span><span class="sxs-lookup"><span data-stu-id="4a723-105">Problem description</span></span>

<span data-ttu-id="4a723-106">許多程式庫會提供外部資源的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="4a723-106">Many libraries provide abstractions of external resources.</span></span> <span data-ttu-id="4a723-107">就內部而言，這些類別通常會管理自己的資源連線，並作為用戶端可用來存取資源的代理程式。</span><span class="sxs-lookup"><span data-stu-id="4a723-107">Internally, these classes typically manage their own connections to the resource, acting as brokers that clients can use to access the resource.</span></span> <span data-ttu-id="4a723-108">以下是一些與 Azure 應用程式相關的代理程式類別範例：</span><span class="sxs-lookup"><span data-stu-id="4a723-108">Here are some examples of broker classes that are relevant to Azure applications:</span></span>

- <span data-ttu-id="4a723-109">`System.Net.Http.HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="4a723-109">`System.Net.Http.HttpClient`.</span></span> <span data-ttu-id="4a723-110">使用 HTTP 與 Web 服務通訊。</span><span class="sxs-lookup"><span data-stu-id="4a723-110">Communicates with a web service using HTTP.</span></span>
- <span data-ttu-id="4a723-111">`Microsoft.ServiceBus.Messaging.QueueClient`。</span><span class="sxs-lookup"><span data-stu-id="4a723-111">`Microsoft.ServiceBus.Messaging.QueueClient`.</span></span> <span data-ttu-id="4a723-112">發佈和接收服務匯流排佇列的訊息。</span><span class="sxs-lookup"><span data-stu-id="4a723-112">Posts and receives messages to a Service Bus queue.</span></span> 
- <span data-ttu-id="4a723-113">`Microsoft.Azure.Documents.Client.DocumentClient`。</span><span class="sxs-lookup"><span data-stu-id="4a723-113">`Microsoft.Azure.Documents.Client.DocumentClient`.</span></span> <span data-ttu-id="4a723-114">連線至 Cosmos DB 執行個體</span><span class="sxs-lookup"><span data-stu-id="4a723-114">Connects to a Cosmos DB instance</span></span>
- <span data-ttu-id="4a723-115">`StackExchange.Redis.ConnectionMultiplexer`。</span><span class="sxs-lookup"><span data-stu-id="4a723-115">`StackExchange.Redis.ConnectionMultiplexer`.</span></span> <span data-ttu-id="4a723-116">連線至 Redis，包括 Azure Redis 快取。</span><span class="sxs-lookup"><span data-stu-id="4a723-116">Connects to Redis, including Azure Redis Cache.</span></span>

<span data-ttu-id="4a723-117">這些類別的目的是要具現化一次，並在應用程式的整個存留期間重複使用。</span><span class="sxs-lookup"><span data-stu-id="4a723-117">These classes are intended to be instantiated once and reused throughout the lifetime of an application.</span></span> <span data-ttu-id="4a723-118">不過，這些類別通常都會被曲解為，應只在需要時取得，並且要快速發行。</span><span class="sxs-lookup"><span data-stu-id="4a723-118">However, it's a common misunderstanding that these classes should be acquired only as necessary and released quickly.</span></span> <span data-ttu-id="4a723-119">(此處所列的項目剛好發生於 .NET 程式庫，但該模式不是只適用於 .NET)。</span><span class="sxs-lookup"><span data-stu-id="4a723-119">(The ones listed here happen to be .NET libraries, but the pattern is not unique to .NET.)</span></span>

<span data-ttu-id="4a723-120">下列 ASP.NET 範例會建立 `HttpClient` 的執行個體，以便與遠端服務進行通訊。</span><span class="sxs-lookup"><span data-stu-id="4a723-120">The following ASP.NET example creates an instance of `HttpClient` to communicate with a remote service.</span></span> <span data-ttu-id="4a723-121">您可以在[這裡][sample-app]找到完整的範例。</span><span class="sxs-lookup"><span data-stu-id="4a723-121">You can find the complete sample [here][sample-app].</span></span>

```csharp
public class NewHttpClientInstancePerRequestController : ApiController
{
    // This method creates a new instance of HttpClient and disposes it for every call to GetProductAsync.
    public async Task<Product> GetProductAsync(string id)
    {
        using (var httpClient = new HttpClient())
        {
            var hostName = HttpContext.Current.Request.Url.Host;
            var result = await httpClient.GetStringAsync(string.Format("http://{0}:8080/api/...", hostName));
            return new Product { Name = result };
        }
    }
}
```

<span data-ttu-id="4a723-122">在 Web 應用程式中，此技術無法進行調整。</span><span class="sxs-lookup"><span data-stu-id="4a723-122">In a web application, this technique is not scalable.</span></span> <span data-ttu-id="4a723-123">新的 `HttpClient` 物件會針對每個使用者的要求而建立。</span><span class="sxs-lookup"><span data-stu-id="4a723-123">A new `HttpClient` object is created for each user request.</span></span> <span data-ttu-id="4a723-124">如果負載過重，網頁伺服器可能會耗盡可用的通訊端，導致出現 `SocketException` 錯誤。</span><span class="sxs-lookup"><span data-stu-id="4a723-124">Under heavy load, the web server may exhaust the number of available sockets, resulting in `SocketException` errors.</span></span>

<span data-ttu-id="4a723-125">此問題不限定於 `HttpClient` 類別。</span><span class="sxs-lookup"><span data-stu-id="4a723-125">This problem is not restricted to the `HttpClient` class.</span></span> <span data-ttu-id="4a723-126">其他包裝資源或建立成本高的類別也可能會造成類似問題。</span><span class="sxs-lookup"><span data-stu-id="4a723-126">Other classes that wrap resources or are expensive to create might cause similar issues.</span></span> <span data-ttu-id="4a723-127">下列範例會建立 `ExpensiveToCreateService` 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="4a723-127">The following example creates an instances of the `ExpensiveToCreateService` class.</span></span> <span data-ttu-id="4a723-128">此處的問題不一定是通訊端耗盡，有可能只是與建立每個執行個體的所需時間有關。</span><span class="sxs-lookup"><span data-stu-id="4a723-128">Here the issue is not necessarily socket exhaustion, but simply how long it takes to create each instance.</span></span> <span data-ttu-id="4a723-129">持續建立和終結此類別的執行個體可能會對系統延展性有不好的影響。</span><span class="sxs-lookup"><span data-stu-id="4a723-129">Continually creating and destroying instances of this class might adversely affect the scalability of the system.</span></span>

```csharp
public class NewServiceInstancePerRequestController : ApiController
{
    public async Task<Product> GetProductAsync(string id)
    {
        var expensiveToCreateService = new ExpensiveToCreateService();
        return await expensiveToCreateService.GetProductByIdAsync(id);
    }
}

public class ExpensiveToCreateService
{
    public ExpensiveToCreateService()
    {
        // Simulate delay due to setup and configuration of ExpensiveToCreateService
        Thread.SpinWait(Int32.MaxValue / 100);
    }
    ...
}
```

## <a name="how-to-fix-the-problem"></a><span data-ttu-id="4a723-130">如何修正問題</span><span class="sxs-lookup"><span data-stu-id="4a723-130">How to fix the problem</span></span>

<span data-ttu-id="4a723-131">如果包裝外部資源的類別可共用且是安全執行緒，請為該類別的可重複使用執行個體，建立共用的單一執行個體或集區。</span><span class="sxs-lookup"><span data-stu-id="4a723-131">If the class that wraps the external resource is shareable and thread-safe, create a shared singleton instance or a pool of reusable instances of the class.</span></span>

<span data-ttu-id="4a723-132">下列範例使用靜態 `HttpClient` 執行個體，因此可在所有要求間共用連線。</span><span class="sxs-lookup"><span data-stu-id="4a723-132">The following following example uses a static `HttpClient` instance, thus sharing the connection across all requests.</span></span>

```csharp
public class SingleHttpClientInstanceController : ApiController
{
    private static readonly HttpClient HttpClient;

    static SingleHttpClientInstanceController()
    {
        HttpClient = new HttpClient();
    }

    // This method uses the shared instance of HttpClient for every call to GetProductAsync.
    public async Task<Product> GetProductAsync(string id)
    {
        var hostName = HttpContext.Current.Request.Url.Host;
        var result = await HttpClient.GetStringAsync(string.Format("http://{0}:8080/api/...", hostName));
        return new Product { Name = result };
    }
}
```

## <a name="considerations"></a><span data-ttu-id="4a723-133">考量</span><span class="sxs-lookup"><span data-stu-id="4a723-133">Considerations</span></span>

- <span data-ttu-id="4a723-134">此反模式的重點是會重複建立和終結*可共用*物件的執行個體。</span><span class="sxs-lookup"><span data-stu-id="4a723-134">The key element of this antipattern is repeatedly creating and destroying instances of a *shareable* object.</span></span> <span data-ttu-id="4a723-135">如果類別不可共用 (不安全的執行緒)，則不適用此反模式。</span><span class="sxs-lookup"><span data-stu-id="4a723-135">If a class is not shareable (not thread-safe), then this antipattern does not apply.</span></span>

- <span data-ttu-id="4a723-136">共用資源類型可能會要求您是否應使用單一執行個體或建立集區。</span><span class="sxs-lookup"><span data-stu-id="4a723-136">The type of shared resource might dictate whether you should use a singleton or create a pool.</span></span> <span data-ttu-id="4a723-137">`HttpClient` 類別的設計主要用於共用，而非用於集區。</span><span class="sxs-lookup"><span data-stu-id="4a723-137">The `HttpClient` class is designed to be shared rather than pooled.</span></span> <span data-ttu-id="4a723-138">其他物件可能支援集區，可讓系統將工作負載分散到多個執行個體。</span><span class="sxs-lookup"><span data-stu-id="4a723-138">Other objects might support pooling, enabling the system to spread the workload across multiple instances.</span></span>

- <span data-ttu-id="4a723-139">您在多個要求間共用的物件「必須」是安全執行緒。</span><span class="sxs-lookup"><span data-stu-id="4a723-139">Objects that you share across multiple requests *must* be thread-safe.</span></span> <span data-ttu-id="4a723-140">`HttpClient` 類別是為使用此方法而設計，但其他類別可能不支援並行要求，因此請查看相關文件。</span><span class="sxs-lookup"><span data-stu-id="4a723-140">The `HttpClient` class is designed to be used in this manner, but other classes might not support concurrent requests, so check the available documentation.</span></span>

- <span data-ttu-id="4a723-141">某些資源類型微不足道，應不要保留。</span><span class="sxs-lookup"><span data-stu-id="4a723-141">Some resource types are scarce and should not be held onto.</span></span> <span data-ttu-id="4a723-142">資料庫連接就是個例子。</span><span class="sxs-lookup"><span data-stu-id="4a723-142">Database connections are an example.</span></span> <span data-ttu-id="4a723-143">保留不需要的開放式資料庫連接，可能會阻止其他並行使用者存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="4a723-143">Holding an open database connection that is not required may prevent other concurrent users from gaining access to the database.</span></span>

- <span data-ttu-id="4a723-144">在.NET Framework 中，許多與外部資源建立連線的物件，皆是在管理這些連線的其他類別上使用靜態 Factory 方法所建立。</span><span class="sxs-lookup"><span data-stu-id="4a723-144">In the .NET Framework, many objects that establish connections to external resources are created by using static factory methods of other classes that manage these connections.</span></span> <span data-ttu-id="4a723-145">這些處理站物件的用途是儲存和重複使用，而不是處置和重新建立。</span><span class="sxs-lookup"><span data-stu-id="4a723-145">These factories  objects are intended to be saved and reused, rather than disposed and recreated.</span></span> <span data-ttu-id="4a723-146">例如，在 Azure 服務匯流排中，`QueueClient` 物件是透過 `MessagingFactory` 物件建立。</span><span class="sxs-lookup"><span data-stu-id="4a723-146">For example, in Azure Service Bus, the `QueueClient` object is created through a `MessagingFactory` object.</span></span> <span data-ttu-id="4a723-147">就內部而言，`MessagingFactory` 會管理連線。</span><span class="sxs-lookup"><span data-stu-id="4a723-147">Internally, the `MessagingFactory` manages connections.</span></span> <span data-ttu-id="4a723-148">如需詳細資訊，請參閱[使用服務匯流排傳訊的效能改進最佳作法][service-bus-messaging]。</span><span class="sxs-lookup"><span data-stu-id="4a723-148">For more information, see [Best Practices for performance improvements using Service Bus Messaging][service-bus-messaging].</span></span>

## <a name="how-to-detect-the-problem"></a><span data-ttu-id="4a723-149">如何偵測問題</span><span class="sxs-lookup"><span data-stu-id="4a723-149">How to detect the problem</span></span>

<span data-ttu-id="4a723-150">此問題的徵兆包括輸送量降低或錯誤率增加，以及以下一個或多個狀況：</span><span class="sxs-lookup"><span data-stu-id="4a723-150">Symptoms of this problem include a drop in throughput or an increased error rate, along with one or more of the following:</span></span> 

- <span data-ttu-id="4a723-151">例外狀況增加，這表示通訊端、資料庫連接或檔案控制代碼等資源耗盡。</span><span class="sxs-lookup"><span data-stu-id="4a723-151">An increase in exceptions that indicate exhaustion of resources such as sockets, database connections, file handles, and so on.</span></span> 
- <span data-ttu-id="4a723-152">記憶體使用量和記憶體回收量增加。</span><span class="sxs-lookup"><span data-stu-id="4a723-152">Increased memory use and garbage collection.</span></span>
- <span data-ttu-id="4a723-153">網路、磁碟或資料庫活動增加。</span><span class="sxs-lookup"><span data-stu-id="4a723-153">An increase in network, disk, or database activity.</span></span>

<span data-ttu-id="4a723-154">您可以執行下列步驟來協助識別此問題：</span><span class="sxs-lookup"><span data-stu-id="4a723-154">You can perform the following steps to help identify this problem:</span></span>

1. <span data-ttu-id="4a723-155">執行生產系統的處理程序監視，以識別回應時間變慢的時間點，或系統因缺少資源而失敗的時間點。</span><span class="sxs-lookup"><span data-stu-id="4a723-155">Performing process monitoring of the production system, to identify points when response times slow down or the system fails due to lack of resources.</span></span>
2. <span data-ttu-id="4a723-156">請檢查從這些點上擷取到的遙測資料，以判斷哪些作業可能正在建立或終結資源耗用物件。</span><span class="sxs-lookup"><span data-stu-id="4a723-156">Examine the telemetry data captured at these points to determine which operations might be creating and destroying resource-consuming objects.</span></span>
3. <span data-ttu-id="4a723-157">在受控制的測試環境中 (不要使用生產系統)，對每個可疑作業進行負載測試。</span><span class="sxs-lookup"><span data-stu-id="4a723-157">Load test each suspected operation, in a controlled test environment rather than the production system.</span></span>
4. <span data-ttu-id="4a723-158">檢閱原始碼，並檢查代理程式物件的管理方式。</span><span class="sxs-lookup"><span data-stu-id="4a723-158">Review the source code and examine the how broker objects are managed.</span></span>

<span data-ttu-id="4a723-159">針對負載系統上執行緩慢或產生例外狀況的作業，查看堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="4a723-159">Look at stack traces for operations that are slow-running or that generate exceptions when the system is under load.</span></span> <span data-ttu-id="4a723-160">這項資訊可協助識別這些作業使用資源的方式。</span><span class="sxs-lookup"><span data-stu-id="4a723-160">This information can help to identify how these operations are utilizing resources.</span></span> <span data-ttu-id="4a723-161">例外狀況可以協助判斷錯誤是否是因為共用資源即將耗盡所致。</span><span class="sxs-lookup"><span data-stu-id="4a723-161">Exceptions can help to determine whether errors are caused by shared resources being exhausted.</span></span> 

## <a name="example-diagnosis"></a><span data-ttu-id="4a723-162">範例診斷</span><span class="sxs-lookup"><span data-stu-id="4a723-162">Example diagnosis</span></span>

<span data-ttu-id="4a723-163">下列各節會將這些步驟套用到稍早所述的範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="4a723-163">The following sections apply these steps to the sample application described earlier.</span></span>

### <a name="identify-points-of-slow-down-or-failure"></a><span data-ttu-id="4a723-164">識別速度變慢或失敗的點</span><span class="sxs-lookup"><span data-stu-id="4a723-164">Identify points of slow down or failure</span></span>

<span data-ttu-id="4a723-165">下圖顯示使用 [New Relic APM][new-relic] 產生的結果，其中顯示回應時間不佳的作業。</span><span class="sxs-lookup"><span data-stu-id="4a723-165">The following image shows results generated using [New Relic APM][new-relic], showing operations that have a poor response time.</span></span> <span data-ttu-id="4a723-166">在此情況下，`NewHttpClientInstancePerRequest` 控制器中的 `GetProductAsync` 方法值得您深入調查。</span><span class="sxs-lookup"><span data-stu-id="4a723-166">In this case, the `GetProductAsync` method in the `NewHttpClientInstancePerRequest` controller are worth investigating further.</span></span> <span data-ttu-id="4a723-167">請注意，當這些作業正在執行時，錯誤率也會增加。</span><span class="sxs-lookup"><span data-stu-id="4a723-167">Notice that the error rate also increases these operations are running.</span></span> 

![顯示範例應用程式為每個要求建立新 HttpClient 物件執行個體的 New Relic 監視器儀表板][dashboard-new-HTTPClient-instance]

### <a name="examine-telemetry-data-and-find-correlations"></a><span data-ttu-id="4a723-169">檢查遙測資料並尋找相互關聯</span><span class="sxs-lookup"><span data-stu-id="4a723-169">Examine telemetry data and find correlations</span></span>

<span data-ttu-id="4a723-170">下圖顯示使用執行緒分析擷取的資料，使用的期間與上一張圖相同。</span><span class="sxs-lookup"><span data-stu-id="4a723-170">The next image shows data captured using thread profiling, over the same period corresponding as the previous image.</span></span> <span data-ttu-id="4a723-171">系統會花很長的時間來開啟通訊端連線，且甚至花更多時間來關閉這些連線並處理通訊端例外狀況。</span><span class="sxs-lookup"><span data-stu-id="4a723-171">The system spends a significant time opening socket connections, and even more time closing them and handling socket exceptions.</span></span>

![顯示範例應用程式為每個要求建立新 HttpClient 物件執行個體的 New Relic 執行緒分析工具][thread-profiler-new-HTTPClient-instance]

### <a name="performing-load-testing"></a><span data-ttu-id="4a723-173">執行負載測試</span><span class="sxs-lookup"><span data-stu-id="4a723-173">Performing load testing</span></span>

<span data-ttu-id="4a723-174">使用負載測試來模擬使用者可能會執行的一般作業。</span><span class="sxs-lookup"><span data-stu-id="4a723-174">Use load testing to simulate the typical operations that users might perform.</span></span> <span data-ttu-id="4a723-175">這有助於識別系統中的哪些組件，會在不同負載下受到資源耗盡問題的影響。</span><span class="sxs-lookup"><span data-stu-id="4a723-175">This can help to identify which parts of a system suffer from resource exhaustion under varying loads.</span></span> <span data-ttu-id="4a723-176">請在受控制的環境執行這些測試，而不是在生產系統中執行。</span><span class="sxs-lookup"><span data-stu-id="4a723-176">Perform these tests in a controlled environment rather than the production system.</span></span> <span data-ttu-id="4a723-177">下圖顯示當使用者負載增加至 100 位並行使用者時，`NewHttpClientInstancePerRequest` 控制器處理的要求輸送量。</span><span class="sxs-lookup"><span data-stu-id="4a723-177">The following graph shows the throughput of requests handled by the `NewHttpClientInstancePerRequest` controller as the user load increases to 100 concurrent users.</span></span>

![範例應用程式為每個要求建立新 HttpClient 物件執行個體的輸送量][throughput-new-HTTPClient-instance]

<span data-ttu-id="4a723-179">首先，工作負載增加時，每秒處理的要求數量就會增加。</span><span class="sxs-lookup"><span data-stu-id="4a723-179">At first, the volume of requests handled per second increases as the workload increases.</span></span> <span data-ttu-id="4a723-180">但是，在大約有 30 位使用者時，成功的要求數量達到限制，而且系統開始產生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="4a723-180">At about 30 users, however, the volume of successful requests reaches a limit, and the system starts to generate exceptions.</span></span> <span data-ttu-id="4a723-181">從那時起，例外狀況的數量會隨著使用者負載量增加而逐漸增加。</span><span class="sxs-lookup"><span data-stu-id="4a723-181">From then on, the volume of exceptions gradually increases with the user load.</span></span> 

<span data-ttu-id="4a723-182">負載測試會將這些失敗回報為 HTTP 500 (內部伺服器) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="4a723-182">The load test reported these failures as HTTP 500 (Internal Server) errors.</span></span> <span data-ttu-id="4a723-183">檢閱遙測資料後顯示，造成這些錯誤的原因是系統即將用完通訊端資源，因為建立了越來越多的 `HttpClient` 物件。</span><span class="sxs-lookup"><span data-stu-id="4a723-183">Reviewing the telemetry showed that these errors were caused by the system running out of socket resources, as more and more `HttpClient` objects were created.</span></span>

<span data-ttu-id="4a723-184">下圖會顯示類似的測試，但測試對象是建立自訂 `ExpensiveToCreateService` 物件的控制器。</span><span class="sxs-lookup"><span data-stu-id="4a723-184">The next graph shows a similar test for a controller that creates the custom `ExpensiveToCreateService` object.</span></span>

![範例應用程式為每個要求建立新 ExpensiveToCreateService 物件執行個體的輸送量][throughput-new-ExpensiveToCreateService-instance]

<span data-ttu-id="4a723-186">這次，控制器不會產生任何例外狀況，但輸送量仍在平均回應時間增加到 20 時達到平穩狀態。</span><span class="sxs-lookup"><span data-stu-id="4a723-186">This time, the controller does not generate any exceptions, but throughput still reaches a plateau, while the average response time increases by a factor of 20.</span></span> <span data-ttu-id="4a723-187">(圖表使用對數刻度作為回應時間與輸送量。)遙測資料顯示，建立 `ExpensiveToCreateService` 的新執行個體是此問題的主要原因。</span><span class="sxs-lookup"><span data-stu-id="4a723-187">(The graph uses a logarithmic scale for response time and throughput.) Telemetry showed that creating new instances of the `ExpensiveToCreateService` was the main cause of the problem.</span></span>

### <a name="implement-the-solution-and-verify-the-result"></a><span data-ttu-id="4a723-188">實作解決方案並確認結果</span><span class="sxs-lookup"><span data-stu-id="4a723-188">Implement the solution and verify the result</span></span>

<span data-ttu-id="4a723-189">將 `GetProductAsync` 方法替換為共用單一 `HttpClient` 執行個體後，第二個負載測試結果顯示效能已獲改善。</span><span class="sxs-lookup"><span data-stu-id="4a723-189">After switching the `GetProductAsync` method to share a single `HttpClient` instance, a second load test showed improved performance.</span></span> <span data-ttu-id="4a723-190">沒有任何錯誤回報，且系統能夠處理增加的負載 (每秒最多 500 個要求)。</span><span class="sxs-lookup"><span data-stu-id="4a723-190">No errors were reported, and the system was able to handle an increasing load of up to 500 requests per second.</span></span> <span data-ttu-id="4a723-191">相較於先前的測試，平均回應時間已減半。</span><span class="sxs-lookup"><span data-stu-id="4a723-191">The average response time was cut in half, compared with the previous test.</span></span>

![範例應用程式為每個要求重新使用相同 HttpClient 物件執行個體的輸送量][throughput-single-HTTPClient-instance]

<span data-ttu-id="4a723-193">為方便比較，下圖顯示堆疊追蹤遙測資料。</span><span class="sxs-lookup"><span data-stu-id="4a723-193">For comparison, the following image shows the stack trace telemetry.</span></span> <span data-ttu-id="4a723-194">這次，系統會花費大部分時間執行實際工作，而不是開啟和關閉通訊端。</span><span class="sxs-lookup"><span data-stu-id="4a723-194">This time, the system spends most of its time performing real work, rather than opening and closing sockets.</span></span>

![顯示範例應用程式為所有要求建立單一 HttpClient 物件執行個體的 New Relic 執行緒分析工具][thread-profiler-single-HTTPClient-instance]

<span data-ttu-id="4a723-196">下圖顯示使用 `ExpensiveToCreateService` 物件的共用執行個體所進行的類似測試。</span><span class="sxs-lookup"><span data-stu-id="4a723-196">The next graph shows a similar load test using a shared instance of the `ExpensiveToCreateService` object.</span></span> <span data-ttu-id="4a723-197">同樣地，已處理的要求數量會隨著使用者負載量增加而增加，但平均回應時間仍然很低。</span><span class="sxs-lookup"><span data-stu-id="4a723-197">Again, the volume of handled requests increases in line with the user load, while the average response time remains low.</span></span> 

![範例應用程式為每個要求重新使用相同 HttpClient 物件執行個體的輸送量][throughput-single-ExpensiveToCreateService-instance]



[sample-app]: https://github.com/mspnp/performance-optimization/tree/master/ImproperInstantiation
[service-bus-messaging]: /azure/service-bus-messaging/service-bus-performance-improvements
[new-relic]: https://newrelic.com/application-monitoring
[throughput-new-HTTPClient-instance]: _images/HttpClientInstancePerRequest.jpg
[dashboard-new-HTTPClient-instance]: _images/HttpClientInstancePerRequestWebTransactions.jpg
[thread-profiler-new-HTTPClient-instance]: _images/HttpClientInstancePerRequestThreadProfile.jpg
[throughput-new-ExpensiveToCreateService-instance]: _images/ServiceInstancePerRequest.jpg
[throughput-single-HTTPClient-instance]: _images/SingleHttpClientInstance.jpg
[throughput-single-ExpensiveToCreateService-instance]: _images/SingleServiceInstance.jpg
[thread-profiler-single-HTTPClient-instance]: _images/SingleHttpClientInstanceThreadProfile.jpg