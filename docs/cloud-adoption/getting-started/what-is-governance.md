---
title: 企業雲端採用：什麼是雲端資源治理？
description: Azure 上資源存取權治理的概念說明
author: petertaylor9999
ms.date: 09/10/2018
ms.openlocfilehash: 14c26cbccdbea524a7c7220b3bb98ed2a118c30d
ms.sourcegitcommit: c49aeef818d7dfe271bc4128b230cfc676f05230
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/11/2018
ms.locfileid: "44389208"
---
# <a name="enterprise-cloud-adoption-what-is-cloud-resource-governance"></a>企業雲端採用：什麼是雲端資源治理？

在 [Azure 如何運作？](what-is-azure.md)中，您已了解 Azure 是代替使用者執行虛擬化硬體和軟體的伺服器和網路硬體的集合。 Azure 可藉由讓建立、讀取、更新和刪除資源變得簡單，使貴組織的開發和 IT 部門更加敏捷。

不過，雖然為開發人員提供不受限制的資源存取可以讓他們非常敏捷，但是也會導致非預期的成本後果。 例如，開發小組可能會核准要部署一組測試資源，但是在完成測試之後忘記刪除。 這些資源將會繼續累計成本，即使已不再核准或需要使用它們。 

這個問題的解決方案是資源存取**治理**。 治理指的是管理、監視及稽核 Azure 資源的使用符合組織目標和需求的進行中程序。 

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2ii94] 

這些目標和需求對於每個組織都是獨特的，所以不可能有一體適用的治理方法。 相反地，Azure 會實作兩個主要的治理工具，**資源型存取控制 (RBAC)** 和**資源原則**，由每個組織設計其治理模型並使用。

RBAC 會定義角色，而角色會定義獲指派角色的使用者功能。 例如，**擁有者**角色可以對資源啟用所有功能 (建立、讀取、更新和刪除)，而**讀者**角色僅可啟用讀取功能。 角色可以定義為套用至許多資源類型的廣範圍，或者套用至少數資源的狹窄範圍。 

資源原則會定義資源建立的規則。 例如，資源原則可以將虛擬機器的 SKU 限制為特定預先核准大小。 或者，資源原則可以在要求建立資源時，強制新增成本中心的標記。 

在設定這些工具時，重要的考量是平衡治理與組織靈活度。 也就是，治理原則越嚴格，您的開發人員和 IT 工作者就會變得越不敏捷。 這是因為嚴格的治理原則可能需要更多手動步驟，例如需要開發人員填寫表單或傳送電子郵件給治理小組的某人，以手動建立資源。 治理小組具有有限的功能並且可能會積存，導致開發小組等候其資源建立而無效率，以及在等候不需要的資源刪除時持續累計成本。

## <a name="next-steps"></a>後續步驟

既然您已了解雲端資源治理的概念，請繼續深入了解在 Azure 中[如何管理資源存取權](azure-resource-access.md)，以準備學習如何設計[單一小組](../governance/governance-single-team.md)和[多個小組](../governance/governance-multiple-teams.md)的治理模型。

> [!div class="nextstepaction"]
> [深入了解 Azure 中的資源存取](azure-resource-access.md)