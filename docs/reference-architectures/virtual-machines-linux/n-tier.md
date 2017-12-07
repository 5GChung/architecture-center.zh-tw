---
title: "在 Azure 上執行適用於多層式架構應用程式的 Linux VM"
description: "如何在 Microsoft Azure 上執行適用於多層式架構的 Linux VM。"
author: MikeWasson
ms.date: 11/22/2017
pnp.series.title: Linux VM workloads
pnp.series.next: multi-region-application
pnp.series.prev: multi-vm
ms.openlocfilehash: 98814685e0f33f2a1258bf8307a86f92d8a81968
ms.sourcegitcommit: 583e54a1047daa708a9b812caafb646af4d7607b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2017
---
# <a name="run-linux-vms-for-an-n-tier-application"></a><span data-ttu-id="ce32e-103">執行適用於多層式架構應用程式的 Linux VM</span><span class="sxs-lookup"><span data-stu-id="ce32e-103">Run Linux VMs for an N-tier application</span></span>

<span data-ttu-id="ce32e-104">此參考架構顯示一組經過證實的作法，可用來執行適用於多層式架構應用程式的 Linux 虛擬機器 (VM)。</span><span class="sxs-lookup"><span data-stu-id="ce32e-104">This reference architecture shows a set of proven practices for running Linux virtual machines (VMs) for an N-tier application.</span></span> [<span data-ttu-id="ce32e-105">**部署這個解決方案**。</span><span class="sxs-lookup"><span data-stu-id="ce32e-105">**Deploy this solution**.</span></span>](#deploy-the-solution)  

<span data-ttu-id="ce32e-106">![[0]][0]</span><span class="sxs-lookup"><span data-stu-id="ce32e-106">![[0]][0]</span></span>

<span data-ttu-id="ce32e-107">下載這個架構的 [Visio 檔案][visio-download]。</span><span class="sxs-lookup"><span data-stu-id="ce32e-107">*Download a [Visio file][visio-download] of this architecture.*</span></span>

## <a name="architecture"></a><span data-ttu-id="ce32e-108">架構</span><span class="sxs-lookup"><span data-stu-id="ce32e-108">Architecture</span></span>

<span data-ttu-id="ce32e-109">有許多方式可實作多層式架構。</span><span class="sxs-lookup"><span data-stu-id="ce32e-109">There are many ways to implement an N-tier architecture.</span></span> <span data-ttu-id="ce32e-110">下圖顯示典型的 3 層式架構 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ce32e-110">The diagram shows a typical 3-tier web application.</span></span> <span data-ttu-id="ce32e-111">此架構是根據[執行負載平衡的 VM 以獲得延展性和可用性][multi-vm]建置的。</span><span class="sxs-lookup"><span data-stu-id="ce32e-111">This architecture builds on [Run load-balanced VMs for scalability and availability][multi-vm].</span></span> <span data-ttu-id="ce32e-112">Web 及 Business 層會使用負載平衡的 VM。</span><span class="sxs-lookup"><span data-stu-id="ce32e-112">The web and business tiers use load-balanced VMs.</span></span>

* <span data-ttu-id="ce32e-113">**可用性設定組。**</span><span class="sxs-lookup"><span data-stu-id="ce32e-113">**Availability sets.**</span></span> <span data-ttu-id="ce32e-114">針對每一層建立[可用性設定組][azure-availability-sets]，並且在每一層中至少佈建兩個 VM。</span><span class="sxs-lookup"><span data-stu-id="ce32e-114">Create an [availability set][azure-availability-sets] for each tier, and provision at least two VMs in each tier.</span></span>  <span data-ttu-id="ce32e-115">這讓 VM 能夠符合適用於 VM 之較高[服務等級協定 (SLA)][vm-sla] 的資格。</span><span class="sxs-lookup"><span data-stu-id="ce32e-115">This makes the VMs eligible for a higher [service level agreement (SLA)][vm-sla] for VMs.</span></span> <span data-ttu-id="ce32e-116">您可以在可用性設定組中部署單一 VM，但該單一 VM 無法當做 SLA 保證使用，除非該單一 VM 的所有作業系統及資料磁碟是使用 Azure 進階儲存體。</span><span class="sxs-lookup"><span data-stu-id="ce32e-116">You can deploy a single VM in an availability set, but the single VM will not qualify for an SLA guarantee unless the single VM is using Azure Premium Storage for all OS and data disks.</span></span>  
* <span data-ttu-id="ce32e-117">**子網路。**</span><span class="sxs-lookup"><span data-stu-id="ce32e-117">**Subnets.**</span></span> <span data-ttu-id="ce32e-118">針對每一層建立不同的子網路。</span><span class="sxs-lookup"><span data-stu-id="ce32e-118">Create a separate subnet for each tier.</span></span> <span data-ttu-id="ce32e-119">使用 [CIDR] 標記法來指定位址範圍和子網路遮罩。</span><span class="sxs-lookup"><span data-stu-id="ce32e-119">Specify the address range and subnet mask using [CIDR] notation.</span></span> 
* <span data-ttu-id="ce32e-120">**負載平衡器。**</span><span class="sxs-lookup"><span data-stu-id="ce32e-120">**Load balancers.**</span></span> <span data-ttu-id="ce32e-121">使用[網際網路面向的負載平衡器][load-balancer-external]，將連入的網際網路流量散發到 Web 層，並使用[內部負載平衡器][load-balancer-internal]，將來自 Web 層的網路流量散發到 Business 層。</span><span class="sxs-lookup"><span data-stu-id="ce32e-121">Use an [Internet-facing load balancer][load-balancer-external] to distribute incoming Internet traffic to the web tier, and an [internal load balancer][load-balancer-internal] to distribute network traffic from the web tier to the business tier.</span></span>
* <span data-ttu-id="ce32e-122">**Jumpbox。**</span><span class="sxs-lookup"><span data-stu-id="ce32e-122">**Jumpbox.**</span></span> <span data-ttu-id="ce32e-123">也稱為[防禦主機]。</span><span class="sxs-lookup"><span data-stu-id="ce32e-123">Also called a [bastion host].</span></span> <span data-ttu-id="ce32e-124">網路上系統管理員用來連線到其他 VM 的安全 VM。</span><span class="sxs-lookup"><span data-stu-id="ce32e-124">A secure VM on the network that administrators use to connect to the other VMs.</span></span> <span data-ttu-id="ce32e-125">Jumpbox 具有 NSG，只允許來自安全清單上公用 IP 位址的遠端流量。</span><span class="sxs-lookup"><span data-stu-id="ce32e-125">The jumpbox has an NSG that allows remote traffic only from public IP addresses on a safe list.</span></span> <span data-ttu-id="ce32e-126">NSG 應該允許安全殼層 (SSH) 流量。</span><span class="sxs-lookup"><span data-stu-id="ce32e-126">The NSG should permit secure shell (SSH) traffic.</span></span>
* <span data-ttu-id="ce32e-127">**監視。**</span><span class="sxs-lookup"><span data-stu-id="ce32e-127">**Monitoring.**</span></span> <span data-ttu-id="ce32e-128">監視軟體 (例如 [Nagios]、[Zabbix] 或 [Icinga]) 可讓您深入了解回應時間、VM 執行時間，以及系統的整體健康情況。</span><span class="sxs-lookup"><span data-stu-id="ce32e-128">Monitoring software such as [Nagios], [Zabbix], or [Icinga] can give you insight into response time, VM uptime, and the overall health of your system.</span></span> <span data-ttu-id="ce32e-129">在位於個別管理子網路中的 VM 上安裝監視軟體。</span><span class="sxs-lookup"><span data-stu-id="ce32e-129">Install the monitoring software on a VM that's placed in a separate management subnet.</span></span>
* <span data-ttu-id="ce32e-130">**NSG。**</span><span class="sxs-lookup"><span data-stu-id="ce32e-130">**NSGs.**</span></span> <span data-ttu-id="ce32e-131">使用[網路安全性群組][nsg] (NSG) 來限制 VNet 內的網路流量。</span><span class="sxs-lookup"><span data-stu-id="ce32e-131">Use [network security groups][nsg] (NSGs) to restrict network traffic within the VNet.</span></span> <span data-ttu-id="ce32e-132">例如，在如下所示的 3 層式架構中，資料庫層不接受來自 Web 前端的流量，只接受來自 Business 層和管理子網路的流量。</span><span class="sxs-lookup"><span data-stu-id="ce32e-132">For example, in the 3-tier architecture shown here, the database tier does not accept traffic from the web front end, only from the business tier and the management subnet.</span></span>
* <span data-ttu-id="ce32e-133">**Apache Cassandra 資料庫**。</span><span class="sxs-lookup"><span data-stu-id="ce32e-133">**Apache Cassandra database**.</span></span> <span data-ttu-id="ce32e-134">藉由啟用複寫和容錯移轉，在資料層提供高可用性。</span><span class="sxs-lookup"><span data-stu-id="ce32e-134">Provides high availability at the data tier, by enabling replication and failover.</span></span>

## <a name="recommendations"></a><span data-ttu-id="ce32e-135">建議</span><span class="sxs-lookup"><span data-stu-id="ce32e-135">Recommendations</span></span>

<span data-ttu-id="ce32e-136">您的需求可能和此處所述的架構不同。</span><span class="sxs-lookup"><span data-stu-id="ce32e-136">Your requirements might differ from the architecture described here.</span></span> <span data-ttu-id="ce32e-137">請使用以下建議作為起點。</span><span class="sxs-lookup"><span data-stu-id="ce32e-137">Use these recommendations as a starting point.</span></span> 

### <a name="vnet--subnets"></a><span data-ttu-id="ce32e-138">VNet / 子網路</span><span class="sxs-lookup"><span data-stu-id="ce32e-138">VNet / Subnets</span></span>

<span data-ttu-id="ce32e-139">當您建立 VNet 時，請判斷您在每個子網路中的資源需要多少個 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="ce32e-139">When you create the VNet, determine how many IP addresses your resources in each subnet require.</span></span> <span data-ttu-id="ce32e-140">使用 [CIDR] 標記法，針對所需的 IP 位址指定子網路遮罩和夠大的 VNet 位址範圍。</span><span class="sxs-lookup"><span data-stu-id="ce32e-140">Specify a subnet mask and a VNet address range large enough for the required IP addresses using [CIDR] notation.</span></span> <span data-ttu-id="ce32e-141">使用落在標準[私人 IP 位址區塊][private-ip-space]內的位址空間，其為 10.0.0.0/8、172.16.0.0/12 及 192.168.0.0/16。</span><span class="sxs-lookup"><span data-stu-id="ce32e-141">Use an address space that falls within the standard [private IP address blocks][private-ip-space], which are 10.0.0.0/8, 172.16.0.0/12, and 192.168.0.0/16.</span></span>

<span data-ttu-id="ce32e-142">選擇不會與您的內部部署網路重疊的位址範圍，以防您稍後需要在 VNet 和您的內部部署網路之間設定閘道。</span><span class="sxs-lookup"><span data-stu-id="ce32e-142">Choose an address range that does not overlap with your on-premises network, in case you need to set up a gateway between the VNet and your on-premises network later.</span></span> <span data-ttu-id="ce32e-143">一旦建立 VNet 之後，就無法變更位址範圍。</span><span class="sxs-lookup"><span data-stu-id="ce32e-143">Once you create the VNet, you can't change the address range.</span></span>

<span data-ttu-id="ce32e-144">請記得以功能和安全性需求來設計子網路。</span><span class="sxs-lookup"><span data-stu-id="ce32e-144">Design subnets with functionality and security requirements in mind.</span></span> <span data-ttu-id="ce32e-145">位於相同層或角色的所有 VM 都應移入相同的子網路，它可以是安全性界限。</span><span class="sxs-lookup"><span data-stu-id="ce32e-145">All VMs within the same tier or role should go into the same subnet, which can be a security boundary.</span></span> <span data-ttu-id="ce32e-146">如需設計 VNet 和子網路的詳細資訊，請參閱[規劃和設計 Azure 虛擬網路][plan-network]。</span><span class="sxs-lookup"><span data-stu-id="ce32e-146">For more information about designing VNets and subnets, see [Plan and design Azure Virtual Networks][plan-network].</span></span>

<span data-ttu-id="ce32e-147">針對每個子網路，以 CIDR 標記法來指定子網路的位址空間。</span><span class="sxs-lookup"><span data-stu-id="ce32e-147">For each subnet, specify the address space for the subnet in CIDR notation.</span></span> <span data-ttu-id="ce32e-148">例如，'10.0.0.0/24' 會建立 256 個 IP 位址範圍。</span><span class="sxs-lookup"><span data-stu-id="ce32e-148">For example, '10.0.0.0/24' creates a range of 256 IP addresses.</span></span> <span data-ttu-id="ce32e-149">VM 可以使用這其中的 251 個；保留五個。</span><span class="sxs-lookup"><span data-stu-id="ce32e-149">VMs can use 251 of these; five are reserved.</span></span> <span data-ttu-id="ce32e-150">確定位址範圍不會跨子網路重疊。</span><span class="sxs-lookup"><span data-stu-id="ce32e-150">Make sure the address ranges don't overlap across subnets.</span></span> <span data-ttu-id="ce32e-151">請參閱[虛擬網路常見問題集][vnet faq]。</span><span class="sxs-lookup"><span data-stu-id="ce32e-151">See the [Virtual Network FAQ][vnet faq].</span></span>

### <a name="network-security-groups"></a><span data-ttu-id="ce32e-152">網路安全性群組</span><span class="sxs-lookup"><span data-stu-id="ce32e-152">Network security groups</span></span>

<span data-ttu-id="ce32e-153">使用 NSG 規則來限制各層之間的流量。</span><span class="sxs-lookup"><span data-stu-id="ce32e-153">Use NSG rules to restrict traffic between tiers.</span></span> <span data-ttu-id="ce32e-154">例如，在如上所示的 3 層式架構中，Web 層不會直接與資料庫層通訊。</span><span class="sxs-lookup"><span data-stu-id="ce32e-154">For example, in the 3-tier architecture shown above, the web tier does not communicate directly with the database tier.</span></span> <span data-ttu-id="ce32e-155">若要強制執行此動作，資料庫層應該封鎖來自 Web 層子網路的連入流量。</span><span class="sxs-lookup"><span data-stu-id="ce32e-155">To enforce this, the database tier should block incoming traffic from the web tier subnet.</span></span>  

1. <span data-ttu-id="ce32e-156">建立 NSG，並將它關聯至資料庫層子網路。</span><span class="sxs-lookup"><span data-stu-id="ce32e-156">Create an NSG and associate it to the database tier subnet.</span></span>
2. <span data-ttu-id="ce32e-157">新增規則以拒絕來自 VNet 的所有輸入流量</span><span class="sxs-lookup"><span data-stu-id="ce32e-157">Add a rule that denies all inbound traffic from the VNet.</span></span> <span data-ttu-id="ce32e-158">(在規則中使用 `VIRTUAL_NETWORK` 標記)。</span><span class="sxs-lookup"><span data-stu-id="ce32e-158">(Use the `VIRTUAL_NETWORK` tag in the rule.)</span></span> 
3. <span data-ttu-id="ce32e-159">新增具有較高優先順序的規則，允許來自 Business 層子網路的輸入流量。</span><span class="sxs-lookup"><span data-stu-id="ce32e-159">Add a rule with a higher priority that allows inbound traffic from the business tier subnet.</span></span> <span data-ttu-id="ce32e-160">此規則會覆寫上一個規則，並讓 Business 層可與資料庫層對話。</span><span class="sxs-lookup"><span data-stu-id="ce32e-160">This rule overrides the previous rule, and allows the business tier to talk to the database tier.</span></span>
4. <span data-ttu-id="ce32e-161">新增規則，允許來自資料庫層子網路本身的輸入流量。</span><span class="sxs-lookup"><span data-stu-id="ce32e-161">Add a rule that allows inbound traffic from within the database tier subnet itself.</span></span> <span data-ttu-id="ce32e-162">此規則允許資料庫層中 VM 之間的通訊，是進行資料庫複寫和容錯移轉所需的。</span><span class="sxs-lookup"><span data-stu-id="ce32e-162">This rule allows communication between VMs in the database tier, which is needed for database replication and failover.</span></span>
5. <span data-ttu-id="ce32e-163">新增規則，允許來自 Jumpbox 子網路的 SSH 流量。</span><span class="sxs-lookup"><span data-stu-id="ce32e-163">Add a rule that allows SSH traffic from the jumpbox subnet.</span></span> <span data-ttu-id="ce32e-164">此規則讓系統管理員能夠從 Jumpbox 連線到資料庫層。</span><span class="sxs-lookup"><span data-stu-id="ce32e-164">This rule lets administrators connect to the database tier from the jumpbox.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ce32e-165">NSG 的[預設規則][nsg-rules]允許所有來自 VNet 的輸入流量。</span><span class="sxs-lookup"><span data-stu-id="ce32e-165">An NSG has [default rules][nsg-rules] that allow any inbound traffic from within the VNet.</span></span> <span data-ttu-id="ce32e-166">這些規則無法刪除，但您可藉由建立較高優先順序的規則來覆寫它們。</span><span class="sxs-lookup"><span data-stu-id="ce32e-166">These rules can't be deleted, but you can override them by creating higher-priority rules.</span></span>
   > 
   > 

### <a name="load-balancers"></a><span data-ttu-id="ce32e-167">負載平衡器</span><span class="sxs-lookup"><span data-stu-id="ce32e-167">Load balancers</span></span>

<span data-ttu-id="ce32e-168">外部負載平衡器會將網際網路流量散發到 Web 層。</span><span class="sxs-lookup"><span data-stu-id="ce32e-168">The external load balancer distributes Internet traffic to the web tier.</span></span> <span data-ttu-id="ce32e-169">建立此負載平衡器的公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="ce32e-169">Create a public IP address for this load balancer.</span></span> <span data-ttu-id="ce32e-170">請參閱[建立網際網路面向的負載平衡器][lb-external-create]。</span><span class="sxs-lookup"><span data-stu-id="ce32e-170">See [Creating an Internet-facing load balancer][lb-external-create].</span></span>

<span data-ttu-id="ce32e-171">內部負載平衡器會將網路流量從 Web 層散發到 Business 層。</span><span class="sxs-lookup"><span data-stu-id="ce32e-171">The internal load balancer distributes network traffic from the web tier to the business tier.</span></span> <span data-ttu-id="ce32e-172">若要為此負載平衡器提供私人 IP 位址，請建立前端 IP 設定，並將它關聯至 Business 層的子網路。</span><span class="sxs-lookup"><span data-stu-id="ce32e-172">To give this load balancer a private IP address, create a frontend IP configuration and associate it with the subnet for the business tier.</span></span> <span data-ttu-id="ce32e-173">請參閱[開始建立內部負載平衡器][lb-internal-create]。</span><span class="sxs-lookup"><span data-stu-id="ce32e-173">See [Get started creating an Internal load balancer][lb-internal-create].</span></span>

### <a name="cassandra"></a><span data-ttu-id="ce32e-174">Cassandra</span><span class="sxs-lookup"><span data-stu-id="ce32e-174">Cassandra</span></span>

<span data-ttu-id="ce32e-175">我們建議針對生產環境使用 [DataStax Enterprise][datastax]，但這些建議適用於所有的 Cassandra 版本。</span><span class="sxs-lookup"><span data-stu-id="ce32e-175">We recommend [DataStax Enterprise][datastax] for production use, but these recommendations apply to any Cassandra edition.</span></span> <span data-ttu-id="ce32e-176">如需在 Azure 中執行 DataStax 的詳細資訊，請參閱[適用於 Azure 的DataStax Enterprise 部署指南][cassandra-in-azure]。</span><span class="sxs-lookup"><span data-stu-id="ce32e-176">For more information on running DataStax in Azure, see [DataStax Enterprise Deployment Guide for Azure][cassandra-in-azure].</span></span> 

<span data-ttu-id="ce32e-177">將適用於 Cassandra 叢集的 VM 放置於可用性設定組，以確保會將 Cassandra 複本散發到多個容錯網域和升級網域。</span><span class="sxs-lookup"><span data-stu-id="ce32e-177">Put the VMs for a Cassandra cluster in an availability set to ensure that the Cassandra replicas are distributed across multiple fault domains and upgrade domains.</span></span> <span data-ttu-id="ce32e-178">如需容錯網域和升級網域的詳細資訊，請參閱[管理虛擬機器的可用性][azure-availability-sets]。</span><span class="sxs-lookup"><span data-stu-id="ce32e-178">For more information about fault domains and upgrade domains, see [Manage the availability of virtual machines][azure-availability-sets].</span></span> 

<span data-ttu-id="ce32e-179">針對每個可用性設定組設定三個錯誤網域 (最大值)，以及針對每個可用性設定組設定 18 個升級網域。</span><span class="sxs-lookup"><span data-stu-id="ce32e-179">Configure three fault domains (the maximum) per availability set and 18 upgrade domains per availability set.</span></span> <span data-ttu-id="ce32e-180">這提供仍能平均散發到容錯網域的升級網域數目上限。</span><span class="sxs-lookup"><span data-stu-id="ce32e-180">This provides the maximum number of upgrade domains that can still be distributed evenly across the fault domains.</span></span>   

<span data-ttu-id="ce32e-181">在機架感知的模式中設定節點。</span><span class="sxs-lookup"><span data-stu-id="ce32e-181">Configure nodes in rack-aware mode.</span></span> <span data-ttu-id="ce32e-182">在 `cassandra-rackdc.properties` 檔案中，將容錯網域對應到機架。</span><span class="sxs-lookup"><span data-stu-id="ce32e-182">Map fault domains to racks in the `cassandra-rackdc.properties` file.</span></span>

<span data-ttu-id="ce32e-183">您不需要叢集前方的負載平衡器。</span><span class="sxs-lookup"><span data-stu-id="ce32e-183">You don't need a load balancer in front of the cluster.</span></span> <span data-ttu-id="ce32e-184">用戶端會直接連線至用戶端中的節點。</span><span class="sxs-lookup"><span data-stu-id="ce32e-184">The client connects directly to a node in the cluster.</span></span>

### <a name="jumpbox"></a><span data-ttu-id="ce32e-185">Jumpbox</span><span class="sxs-lookup"><span data-stu-id="ce32e-185">Jumpbox</span></span>

<span data-ttu-id="ce32e-186">Jumpbox 將具備最低效能需求，因此，請針對 Jumpbox 選取較小的 VM 大小，例如標準 A1。</span><span class="sxs-lookup"><span data-stu-id="ce32e-186">The jumpbox will have minimal performance requirements, so select a small VM size for the jumpbox such as Standard A1.</span></span> 

<span data-ttu-id="ce32e-187">針對 Jumpbox 建立[公用 IP 位址]。</span><span class="sxs-lookup"><span data-stu-id="ce32e-187">Create a [public IP address] for the jumpbox.</span></span> <span data-ttu-id="ce32e-188">將 Jumpbox 放置於與其他 VM 相同的 VNet，但在個別的管理子網路中。</span><span class="sxs-lookup"><span data-stu-id="ce32e-188">Place the jumpbox in the same VNet as the other VMs, but in a separate management subnet.</span></span>

<span data-ttu-id="ce32e-189">不允許從公用網際網路對執行應用程式工作負載的 VM 進行 SSH 存取。</span><span class="sxs-lookup"><span data-stu-id="ce32e-189">Do not allow SSH access from the public Internet to the VMs that run the application workload.</span></span> <span data-ttu-id="ce32e-190">對這些 VM 所做的所有 SSH 存取都必須改為透過 Jumpbox 進行。</span><span class="sxs-lookup"><span data-stu-id="ce32e-190">Instead, all SSH access to these VMs must come through the jumpbox.</span></span> <span data-ttu-id="ce32e-191">系統管理員會登入 Jumpbox，然後從 Jumpbox 登入其他 VM。</span><span class="sxs-lookup"><span data-stu-id="ce32e-191">An administrator logs into the jumpbox, and then logs into the other VM from the jumpbox.</span></span> <span data-ttu-id="ce32e-192">Jumpbox 允許來自網際網路，但只來自已知且安全 IP 位址的 SSH 流量。</span><span class="sxs-lookup"><span data-stu-id="ce32e-192">The jumpbox allows SSH traffic from the Internet, but only from known, safe IP addresses.</span></span>

<span data-ttu-id="ce32e-193">若要保護 Jumpbox，請建立 NSG 並將它套用到 Jumpbox 子網路。</span><span class="sxs-lookup"><span data-stu-id="ce32e-193">To secure the jumpbox, create an NSG and apply it to the jumpbox subnet.</span></span> <span data-ttu-id="ce32e-194">新增 NSG 規則，只允許來自一組安全公用 IP 位址的 SSH 連線。</span><span class="sxs-lookup"><span data-stu-id="ce32e-194">Add an NSG rule that allows SSH connections only from a safe set of public IP addresses.</span></span> <span data-ttu-id="ce32e-195">NSG 可以連接到子網路或 Jumpbox NIC。</span><span class="sxs-lookup"><span data-stu-id="ce32e-195">The NSG can be attached either to the subnet or to the jumpbox NIC.</span></span> <span data-ttu-id="ce32e-196">在此情況下，我們建議將它連接到 NIC，如此一來，就只允許 SSH 流量到 Jumpbox，即使您將其他 VM 新增至相同的子網路也一樣。</span><span class="sxs-lookup"><span data-stu-id="ce32e-196">In this case, we recommend attaching it to the NIC, so SSH traffic is permitted only to the jumpbox, even if you add other VMs to the same subnet.</span></span>

<span data-ttu-id="ce32e-197">設定其他子網路的 NSG，允許來自管理子網路的 SSH 流量。</span><span class="sxs-lookup"><span data-stu-id="ce32e-197">Configure the NSGs for the other subnets to allow SSH traffic from the management subnet.</span></span>

## <a name="availability-considerations"></a><span data-ttu-id="ce32e-198">可用性考量</span><span class="sxs-lookup"><span data-stu-id="ce32e-198">Availability considerations</span></span>

<span data-ttu-id="ce32e-199">將每個層或 VM 角色放入個別的可用性設定組。</span><span class="sxs-lookup"><span data-stu-id="ce32e-199">Put each tier or VM role into a separate availability set.</span></span> 

<span data-ttu-id="ce32e-200">在資料庫層，具有多個 VM 不會自動會轉譯成高度可用的資料庫。</span><span class="sxs-lookup"><span data-stu-id="ce32e-200">At the database tier, having multiple VMs does not automatically translate into a highly available database.</span></span> <span data-ttu-id="ce32e-201">針對關聯式資料庫，您通常需要使用複寫和容錯移轉來實現高可用性。</span><span class="sxs-lookup"><span data-stu-id="ce32e-201">For a relational database, you will typically need to use replication and failover to achieve high availability.</span></span>  

<span data-ttu-id="ce32e-202">如果您需要比[適用於 VM 的 Azure SLA][vm-sla] 所提供更高的可用性，請跨兩個區域複寫應用程式並使用 Azure 流量管理員進行容錯移轉。</span><span class="sxs-lookup"><span data-stu-id="ce32e-202">If you need higher availability than the [Azure SLA for VMs][vm-sla] provides, replicate the application across two regions and use Azure Traffic Manager for failover.</span></span> <span data-ttu-id="ce32e-203">如需詳細資訊，請參閱[在多個區域中執行 Linux VM 以獲得高可用性][multi-dc]。</span><span class="sxs-lookup"><span data-stu-id="ce32e-203">For more information, see [Run Linux VMs in multiple regions for high availability][multi-dc].</span></span>  

## <a name="security-considerations"></a><span data-ttu-id="ce32e-204">安全性考量</span><span class="sxs-lookup"><span data-stu-id="ce32e-204">Security considerations</span></span>

<span data-ttu-id="ce32e-205">請考慮新增網路虛擬設備 (NVA)，在公用網際網路和 Azure 虛擬網路之間建立 DMZ。</span><span class="sxs-lookup"><span data-stu-id="ce32e-205">Consider adding a network virtual appliance (NVA) to create a DMZ between the public Internet and the Azure virtual network.</span></span> <span data-ttu-id="ce32e-206">NVA 是虛擬設備的通稱，可以執行網路相關的工作，例如防火牆、封包檢查、稽核和自訂路由傳送。</span><span class="sxs-lookup"><span data-stu-id="ce32e-206">NVA is a generic term for a virtual appliance that can perform network-related tasks such as firewall, packet inspection, auditing, and custom routing.</span></span> <span data-ttu-id="ce32e-207">如需詳細資訊，請參閱[實作 Azure 和網際網路之間的 DMZ][dmz]。</span><span class="sxs-lookup"><span data-stu-id="ce32e-207">For more information, see [Implementing a DMZ between Azure and the Internet][dmz].</span></span>

## <a name="scalability-considerations"></a><span data-ttu-id="ce32e-208">延展性考量</span><span class="sxs-lookup"><span data-stu-id="ce32e-208">Scalability considerations</span></span>

<span data-ttu-id="ce32e-209">負載平衡器會將網路流量散發到 Web 和 Business 層。</span><span class="sxs-lookup"><span data-stu-id="ce32e-209">The load balancers distribute network traffic to the web and business tiers.</span></span> <span data-ttu-id="ce32e-210">藉由新增 VM 執行個體進行水平調整。</span><span class="sxs-lookup"><span data-stu-id="ce32e-210">Scale horizontally by adding new VM instances.</span></span> <span data-ttu-id="ce32e-211">請注意，您可以根據負載，分別調整 Web 和 Business 層。</span><span class="sxs-lookup"><span data-stu-id="ce32e-211">Note that you can scale the web and business tiers independently, based on load.</span></span> <span data-ttu-id="ce32e-212">為了降低因需要維護用戶端親和性而造成的可能複雜性，Web 層中的 VM 應該是無狀態的。</span><span class="sxs-lookup"><span data-stu-id="ce32e-212">To reduce possible complications caused by the need to maintain client affinity, the VMs in the web tier should be stateless.</span></span> <span data-ttu-id="ce32e-213">裝載商務邏輯的 VM 也應該是無狀態的。</span><span class="sxs-lookup"><span data-stu-id="ce32e-213">The VMs hosting the business logic should also be stateless.</span></span>

## <a name="manageability-considerations"></a><span data-ttu-id="ce32e-214">管理性考量</span><span class="sxs-lookup"><span data-stu-id="ce32e-214">Manageability considerations</span></span>

<span data-ttu-id="ce32e-215">使用集中式管理工具 (例如 [Azure 自動化][azure-administration]、[Microsoft Operations Management Suite][operations-management-suite]、[Chef][chef] 或 [Puppet][puppet]) 來簡化整個系統的管理。</span><span class="sxs-lookup"><span data-stu-id="ce32e-215">Simplify management of the entire system by using centralized administration tools such as [Azure Automation][azure-administration], [Microsoft Operations Management Suite][operations-management-suite], [Chef][chef], or [Puppet][puppet].</span></span> <span data-ttu-id="ce32e-216">這些工具可以合併擷取自多個 VM 的診斷和健康情況資訊，以提供系統的整體檢視。</span><span class="sxs-lookup"><span data-stu-id="ce32e-216">These tools can consolidate diagnostic and health information captured from multiple VMs to provide an overall view of the system.</span></span>

## <a name="deploy-the-solution"></a><span data-ttu-id="ce32e-217">部署解決方案</span><span class="sxs-lookup"><span data-stu-id="ce32e-217">Deploy the solution</span></span>

<span data-ttu-id="ce32e-218">此參考架構的部署可在 [GitHub][github-folder] 上取得。</span><span class="sxs-lookup"><span data-stu-id="ce32e-218">A deployment for this reference architecture is available on [GitHub][github-folder].</span></span> 

### <a name="prerequisites"></a><span data-ttu-id="ce32e-219">必要條件</span><span class="sxs-lookup"><span data-stu-id="ce32e-219">Prerequisites</span></span>

<span data-ttu-id="ce32e-220">在您可以將參考架構部署到自己的訂用帳戶之前，必須執行下列步驟。</span><span class="sxs-lookup"><span data-stu-id="ce32e-220">Before you can deploy the reference architecture to your own subscription, you must perform the following steps.</span></span>

1. <span data-ttu-id="ce32e-221">複製、派生或下載適用於 [AzureCAT 參考架構][ref-arch-repo] GitHub 存放庫的 zip 檔案。</span><span class="sxs-lookup"><span data-stu-id="ce32e-221">Clone, fork, or download the zip file for the [AzureCAT reference architectures][ref-arch-repo] GitHub repository.</span></span>

2. <span data-ttu-id="ce32e-222">確定您已在電腦上安裝 Azure CLI 2.0。</span><span class="sxs-lookup"><span data-stu-id="ce32e-222">Make sure you have the Azure CLI 2.0 installed on your computer.</span></span> <span data-ttu-id="ce32e-223">若要安裝 CLI，請依照[安裝 Azure CLI 2.0][azure-cli-2] 中的指示進行。</span><span class="sxs-lookup"><span data-stu-id="ce32e-223">To install the CLI, follow the instructions in [Install Azure CLI 2.0][azure-cli-2].</span></span>

3. <span data-ttu-id="ce32e-224">安裝 [Azure 建置組塊][azbb] npm 封裝。</span><span class="sxs-lookup"><span data-stu-id="ce32e-224">Install the [Azure building blocks][azbb] npm package.</span></span>

  ```bash
  npm install -g @mspnp/azure-building-blocks
  ```

4. <span data-ttu-id="ce32e-225">從命令提示字元、bash 提示字元或 PowerShell 提示字元中，使用下列其中一個命令登入 Azure 帳戶，並遵循提示進行。</span><span class="sxs-lookup"><span data-stu-id="ce32e-225">From a command prompt, bash prompt, or PowerShell prompt, login to your Azure account by using one of the commands below, and follow the prompts.</span></span>

  ```bash
  az login
  ```

### <a name="deploy-the-solution-using-azbb"></a><span data-ttu-id="ce32e-226">使用 azbb 部署解決方案</span><span class="sxs-lookup"><span data-stu-id="ce32e-226">Deploy the solution using azbb</span></span>

<span data-ttu-id="ce32e-227">若要以多層式架構 (N-Tier) 應用程式參考架構部署 Linux VM，請依以下步驟進行：</span><span class="sxs-lookup"><span data-stu-id="ce32e-227">To deploy the Linux VMs for an N-tier application reference architecture, follow these steps:</span></span>

1. <span data-ttu-id="ce32e-228">瀏覽至您在上述必要條件步驟 1 中複製存放庫的 `virtual-machines\n-tier-linux` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ce32e-228">Navigate to the `virtual-machines\n-tier-linux` folder for the repository you cloned in step 1 of the pre-requisites above.</span></span>

2. <span data-ttu-id="ce32e-229">參數檔案會指定部署中每個 VM 的預設管理員使用者名稱及密碼。</span><span class="sxs-lookup"><span data-stu-id="ce32e-229">The parameter file specifies a default adminstrator user name and password for each VM in the deployment.</span></span> <span data-ttu-id="ce32e-230">您必須在部署參考架構前先變更這些內容。</span><span class="sxs-lookup"><span data-stu-id="ce32e-230">You must change these before you deploy the reference architecture.</span></span> <span data-ttu-id="ce32e-231">開啟 `n-tier-linux.json` 檔案，並用新設定取代每個 **adminUsername** 和 **adminPassword** 欄位。</span><span class="sxs-lookup"><span data-stu-id="ce32e-231">Open the `n-tier-linux.json` file and replace each **adminUsername** and **adminPassword** field with your new settings.</span></span>   <span data-ttu-id="ce32e-232">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="ce32e-232">Save the file.</span></span>

3. <span data-ttu-id="ce32e-233">使用如下所示的 **azbb** 命令列來部署參考架構。</span><span class="sxs-lookup"><span data-stu-id="ce32e-233">Deploy the reference architecture using the **azbb** command line tool as shown below.</span></span>

  ```bash
  azbb -s <your subscription_id> -g <your resource_group_name> -l <azure region> -p n-tier-linux.json --deploy
  ```

<span data-ttu-id="ce32e-234">如需使用 Azure 組建區塊部署此範例參考架構的詳細資訊，請瀏覽 [GitHub 存放庫][git]。</span><span class="sxs-lookup"><span data-stu-id="ce32e-234">For more information on deploying this sample reference architecture using Azure Building Blocks, visit the [GitHub repository][git].</span></span>

<!-- links -->
[multi-dc]: multi-region-application.md
[dmz]: ../dmz/secure-vnet-dmz.md
[multi-vm]: ./multi-vm.md
[naming conventions]: /azure/guidance/guidance-naming-conventions
[azbb]: https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks
[azure-administration]: /azure/automation/automation-intro
[azure-availability-sets]: /azure/virtual-machines/virtual-machines-linux-manage-availability
[azure-cli-2]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
[防禦主機]: https://en.wikipedia.org/wiki/Bastion_host
[cassandra-in-azure]: https://docs.datastax.com/en/datastax_enterprise/4.5/datastax_enterprise/install/installAzure.html
[cidr]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[datastax]: http://www.datastax.com/products/datastax-enterprise
[git]: https://github.com/mspnp/template-building-blocks
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/virtual-machines/n-tier-linux
[lb-external-create]: /azure/load-balancer/load-balancer-get-started-internet-portal
[lb-internal-create]: /azure/load-balancer/load-balancer-get-started-ilb-arm-portal
[load-balancer-external]: /azure/load-balancer/load-balancer-internet-overview
[load-balancer-internal]: /azure/load-balancer/load-balancer-internal-overview
[nsg]: /azure/virtual-network/virtual-networks-nsg
[nsg-rules]: /azure/azure-resource-manager/best-practices-resource-manager-security#network-security-groups
[operations-management-suite]: https://www.microsoft.com/server-cloud/operations-management-suite/overview.aspx
[plan-network]: /azure/virtual-network/virtual-network-vnet-plan-design-arm
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[公用 IP 位址]: /azure/virtual-network/virtual-network-ip-addresses-overview-arm
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[ref-arch-repo]: https://github.com/mspnp/reference-architectures
[vm-sla]: https://azure.microsoft.com/support/legal/sla/virtual-machines
[vnet faq]: /azure/virtual-network/virtual-networks-faq
[visio-download]: https://archcenter.azureedge.net/cdn/vm-reference-architectures.vsdx
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[0]: ./images/n-tier-diagram.png "使用 Microsoft Azure 的多層式架構"
