---
title: "設計與實作模式"
description: "良好的設計會包含一些要素，例如元件設計和部署中的一致性及連貫性、用於簡化管理及開發的可維護性，以及可讓元件和子系統在其他應用程式和其他案例中使用的重複使用性。 設計和實作階段所做的決策，會對雲端上裝載的應用程式和服務在品質和擁有權總成本上產生重大影響。"
keywords: "設計模式"
author: dragon119
ms.date: 06/23/2017
pnp.series.title: Cloud Design Patterns
ms.openlocfilehash: 3d6e528b8c88c6fcc265d6425dcdae6fae5166fb
ms.sourcegitcommit: b0482d49aab0526be386837702e7724c61232c60
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2017
---
# <a name="design-and-implementation-patterns"></a><span data-ttu-id="90baa-105">設計與實作模式</span><span class="sxs-lookup"><span data-stu-id="90baa-105">Design and Implementation patterns</span></span>

<span data-ttu-id="90baa-106">良好的設計會包含一些要素，例如元件設計和部署中的一致性及連貫性、用於簡化管理及開發的可維護性，以及可讓元件和子系統在其他應用程式和其他案例中使用的重複使用性。</span><span class="sxs-lookup"><span data-stu-id="90baa-106">Good design encompasses factors such as consistency and coherence in component design and deployment, maintainability to simplify administration and development, and reusability to allow components and subsystems to be used in other applications and in other scenarios.</span></span> <span data-ttu-id="90baa-107">設計和實作階段所做的決策，會對雲端上裝載的應用程式和服務在品質和擁有權總成本上產生重大影響。</span><span class="sxs-lookup"><span data-stu-id="90baa-107">Decisions made during the design and implementation phase have a huge impact on the quality and the total cost of ownership of cloud hosted applications and services.</span></span>

| <span data-ttu-id="90baa-108">模式</span><span class="sxs-lookup"><span data-stu-id="90baa-108">Pattern</span></span> | <span data-ttu-id="90baa-109">摘要</span><span class="sxs-lookup"><span data-stu-id="90baa-109">Summary</span></span> |
| ------- | ------- |
| [<span data-ttu-id="90baa-110">外交官 (Ambassador)</span><span class="sxs-lookup"><span data-stu-id="90baa-110">Ambassador</span></span>](../ambassador.md) | <span data-ttu-id="90baa-111">建立會代表取用者服務或應用程式傳送網路要求的協助程式服務。</span><span class="sxs-lookup"><span data-stu-id="90baa-111">Create helper services that send network requests on behalf of a consumer service or application.</span></span> |
| [<span data-ttu-id="90baa-112">防損毀層</span><span class="sxs-lookup"><span data-stu-id="90baa-112">Anti-Corruption Layer</span></span>](../anti-corruption-layer.md) | <span data-ttu-id="90baa-113">在現代應用程式和舊版系統間實作外觀或配接器層。</span><span class="sxs-lookup"><span data-stu-id="90baa-113">Implement a façade or adapter layer between a modern application and a legacy system.</span></span> |
| [<span data-ttu-id="90baa-114">前端的後端</span><span class="sxs-lookup"><span data-stu-id="90baa-114">Backends for Frontends</span></span>](../backends-for-frontends.md) | <span data-ttu-id="90baa-115">建立由特定前端應用程式或介面取用的個別後端服務。</span><span class="sxs-lookup"><span data-stu-id="90baa-115">Create separate backend services to be consumed by specific frontend applications or interfaces.</span></span> |
| [<span data-ttu-id="90baa-116">CQRS</span><span class="sxs-lookup"><span data-stu-id="90baa-116">CQRS</span></span>](../cqrs.md) | <span data-ttu-id="90baa-117">隔離自使用個別介面來更新資料的作業中讀取資料的作業。</span><span class="sxs-lookup"><span data-stu-id="90baa-117">Segregate operations that read data from operations that update data by using separate interfaces.</span></span> |
| [<span data-ttu-id="90baa-118">計算資源彙總</span><span class="sxs-lookup"><span data-stu-id="90baa-118">Compute Resource Consolidation</span></span>](../compute-resource-consolidation.md) | <span data-ttu-id="90baa-119">將多個工作或作業合併為單一計算單位</span><span class="sxs-lookup"><span data-stu-id="90baa-119">Consolidate multiple tasks or operations into a single computational unit</span></span> |
| [<span data-ttu-id="90baa-120">外部組態存放區</span><span class="sxs-lookup"><span data-stu-id="90baa-120">External Configuration Store</span></span>](../external-configuration-store.md) | <span data-ttu-id="90baa-121">將設定資訊從應用程式部署套件移至集中位置。</span><span class="sxs-lookup"><span data-stu-id="90baa-121">Move configuration information out of the application deployment package to a centralized location.</span></span> |
| [<span data-ttu-id="90baa-122">閘道彙總</span><span class="sxs-lookup"><span data-stu-id="90baa-122">Gateway Aggregation</span></span>](../gateway-aggregation.md) | <span data-ttu-id="90baa-123">您可以使用閘道來將多個個別的要求彙總成單一要求。</span><span class="sxs-lookup"><span data-stu-id="90baa-123">Use a gateway to aggregate multiple individual requests into a single request.</span></span> |
| [<span data-ttu-id="90baa-124">閘道卸載</span><span class="sxs-lookup"><span data-stu-id="90baa-124">Gateway Offloading</span></span>](../gateway-offloading.md) | <span data-ttu-id="90baa-125">將共用或特殊服務功能卸載至閘道 Proxy。</span><span class="sxs-lookup"><span data-stu-id="90baa-125">Offload shared or specialized service functionality to a gateway proxy.</span></span> |
| [<span data-ttu-id="90baa-126">閘道路由</span><span class="sxs-lookup"><span data-stu-id="90baa-126">Gateway Routing</span></span>](../gateway-routing.md) | <span data-ttu-id="90baa-127">使用單一端點將要求路由至多個服務。</span><span class="sxs-lookup"><span data-stu-id="90baa-127">Route requests to multiple services using a single endpoint.</span></span> |
| [<span data-ttu-id="90baa-128">選出領導者</span><span class="sxs-lookup"><span data-stu-id="90baa-128">Leader Election</span></span>](../leader-election.md) | <span data-ttu-id="90baa-129">選取一個執行個體作為領導者，負責管理其他執行個體，協調分散式應用程式中共同作業工作執行個體集合執行的動作。</span><span class="sxs-lookup"><span data-stu-id="90baa-129">Coordinate the actions performed by a collection of collaborating task instances in a distributed application by electing one instance as the leader that assumes responsibility for managing the other instances.</span></span> |
| [<span data-ttu-id="90baa-130">管道和篩選器</span><span class="sxs-lookup"><span data-stu-id="90baa-130">Pipes and Filters</span></span>](../pipes-and-filters.md) | <span data-ttu-id="90baa-131">將執行複雜處理程序的工作，細分成一系列可重複使用的個別元素。</span><span class="sxs-lookup"><span data-stu-id="90baa-131">Break down a task that performs complex processing into a series of separate elements that can be reused.</span></span> |
| [<span data-ttu-id="90baa-132">Sidecar</span><span class="sxs-lookup"><span data-stu-id="90baa-132">Sidecar</span></span>](../sidecar.md) | <span data-ttu-id="90baa-133">將應用程式的元件部署到個別的處理序或容器，以提供隔離和封裝。</span><span class="sxs-lookup"><span data-stu-id="90baa-133">Deploy components of an application into a separate process or container to provide isolation and encapsulation.</span></span> |
| [<span data-ttu-id="90baa-134">靜態內容裝載</span><span class="sxs-lookup"><span data-stu-id="90baa-134">Static Content Hosting</span></span>](../static-content-hosting.md) | <span data-ttu-id="90baa-135">將靜態內容部署到可以直接將其交付給用戶端的雲端儲存體服務。</span><span class="sxs-lookup"><span data-stu-id="90baa-135">Deploy static content to a cloud-based storage service that can deliver them directly to the client.</span></span> |
| [<span data-ttu-id="90baa-136">Strangler</span><span class="sxs-lookup"><span data-stu-id="90baa-136">Strangler</span></span>](../strangler.md) | <span data-ttu-id="90baa-137">透過將功能的特定片段逐漸取代成新的應用程式和服務，來逐步移轉舊版系統。</span><span class="sxs-lookup"><span data-stu-id="90baa-137">Incrementally migrate a legacy system by gradually replacing specific pieces of functionality with new applications and services.</span></span> |