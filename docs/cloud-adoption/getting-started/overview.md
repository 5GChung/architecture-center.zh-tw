---
title: 企業雲端採用：快速入門
description: 描述在採用 Azure 雲端技術中企業數位轉型第一個階段的概觀
author: petertaylor9999
ms.openlocfilehash: 25d1afe4d048b07a350dfc1b5c463b1d5425a8a4
ms.sourcegitcommit: ae8a1de6f4af7a89a66a8339879843d945201f85
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2018
ms.locfileid: "43327202"
---
# <a name="enterprise-cloud-adoption-getting-started"></a>企業雲端採用：快速入門 

**數位轉型**為雲端運算表示從運作內部部署跳脫到雲端中運作。 這項轉變包含進行商務往來的新方法 - 例如，從軟體和資料中心硬體的資本支出數位轉型到雲端資源使用量的運作支出。 

## <a name="digital-transformation-process"></a>數位轉型：程序

若要成功採用雲端，企業必須備妥其組織、人員和程序以進行此數位轉型。 每個企業的組織結構都不同，因此沒有組織整備的一體適用方法。 本文件概述企業準備就緒可能需要的高階步驟。 貴組織必須花時間開發詳細的計劃，來完成每個列出的步驟。

數位轉型的高階程序是：

1. 建立雲端策略小組。 這個小組負責帶領數位轉型。 務必也在這個階段形成治理小組和安全性小組以進行數位轉型。
2. 雲端策略小組的成員會了解新功能和雲端技術之間的差異。  
3. 雲端策略小組會建置數位轉型的商務案例來讓企業就緒 - 列舉目前商務策略中所有的間距，並判斷排除這些間隙的高階解決方案。
4. 對應高階解決方案與商務群組。 識別每個商務群組中的專案關係人，以擁有每個解決方案的設計和實作。
5. 轉換現有的角色、技能與程序以包含雲端角色、技術和程序。  
<!--6. Develop processes for operating in the cloud to make solutions more robust in terms of availability, resiliency, and security. 
7. Optimize solutions for performance, scalability, and cost efficiency.-->

## <a name="step-1-create-a-cloud-strategy-team"></a>步驟 1：建立雲端策略小組

您企業數位轉型的第一個步驟是納入跨組織的商務領導人，以建立雲端策略小組 (CST)。 此小組是由財務、IT 基礎結構和應用程式群組的商務領導人所組成。 這些小組可協助雲端分析和測試階段。

例如，雲端策略小組可由 CTO 推動，並由企業架構小組的成員、IT 財務、來自各種 IT 應用程式群組 (人力資源、財務等等) 的資深技術人員，以及基礎結構、安全性和網路小組的領導人組成。  

也務必形成兩個其他高階小組：治理小組和安全性小組。 這些小組負責設計、實作和持續稽核企業的治理與安全性原則。 治理小組需要曾用過資產保護、成本管理、群組原則及相關主題的成員。 安全性小組需要十分熟悉最新業界安全性標準以及企業安全性需求的成員。

治理小組負責在雲端中設計和實作企業的治理模型，以及部署和維護屬於數位轉型的共用基礎結構資產。 這些資產包括將內部部署網路連線到雲端中虛擬網路所需的硬體、軟體和雲端資源。

安全性小組會負責在雲端中設計和實作企業的安全性原則，與治理小組密切合作。 安全性小組擁有內部部署網路安全性界限的延伸，以包括雲端中的虛擬網路。 這可能會採用下列形式：擁有及維護雲端虛擬網路上的輸入和輸出防火牆，以及確保工具和原則防止未經授權的資源部署。

## <a name="step-2-learn-whats-new-in-the-cloud"></a>步驟 2：了解雲端中的新功能
 
您企業數位轉型的下一個步驟，是要讓雲端策略小組的成員了解雲端技術將如何變更企業執行業務的方式。 這是您的業務、人員和技術變更的準備及規劃。 雲端策略小組的成員務必了解新功能，以及雲端與內部部署之間的差異。

了解雲端的起點是學習高階 [Azure 運作方式](what-is-azure.md)。 接下來，深入了解[在 Azure 中治理](what-is-governance.md)的基本概念以準備[了解資源存取管理](azure-resource-access.md)。

如需進階學習，治理小組應檢閱目錄的治理小節中的概念和設計指南。 基礎結構和工作負載各節適合用於了解一般架構和雲端中的工作負載。

## <a name="step-3-identify-gaps-in-business-strategy"></a>步驟 3：識別商務策略中的間距

下一個步驟是讓雲端策略團隊列舉需要數位轉型解決方案的商務問題。 例如，企業可能會有硬體生命週期結束而需要更換的現有內部部署資料中心。 在另一個範例中，企業可能會遇到新功能和服務上市時間的困難，並可能會落後競爭。 這些間距代表貴企業數位轉型的「目標」。

商務策略中的間距可以分為下列類別：

|類別|說明|
|-----|-----|
|成本管理|代表企業支付技術方式的間距。|
|控管|代表企業用來保護其資產，避免不當使用而造成成本過度使用、安全性問題或相容性問題的間距。 | 
|法規遵循|代表企業符合其本身內部處理程序和原則，以及外部法律、規定和標準方式的間距。 |
|安全性|代表企業保護其技術和資料資產免於外部威脅方式的間距。 |
|資料治理|代表企業管理其資料，特別是客戶資料方式的間距。 例如，歐盟新的一般資料保護規定 (GDPR) 對於保護客戶資料有嚴格的要求，可能需要新的硬體和軟體。|    

一旦貴企業已將所有商務策略間距分類為這些類別後，下一個步驟是決定每個問題的高階解決方案。

下表說明數個範例：

|商務策略的間距|類別 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|解決方案 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
|-----|-----|-----|
| 目前在內部部署裝載的服務在需求尖峰期間會遇到可用性、復原性及延展性的問題，也就是大約 10%的使用量。 內部部署資料中心內的伺服器生命週期結束。 企業 IT 建議為資料中心購買可因應尖峰需求規格的新內部部署硬體。| 成本管理 | 將受影響的現有內部部署工作負載移轉到雲端中可調整的資源，僅需支付使用量。 |
| 外部資料管理的法律和規定要求企業遵循一組需要加密待用資料的標準控制項，且需要新的硬體和軟體。 | 資料治理 | 將資料移至待用資料的 Azure 儲存體服務加密。 |
| 裝載於內部部署資料中心的服務在公眾對應的服務上遇到分散式阻斷服務 (DDoS) 攻擊。 攻擊難以降低，且需要新的硬體、軟體和安全性人員有效地處理。 | 安全性 | 將服務遷移至 Azure，並利用 Azure DDoS 保護。|

當您已列舉所有商務策略中的間距，且已判斷高階解決方案時，請設定清單的優先順序。 透過將商務策略的間距與每個類別中企業的短期與長期目標對齊，來設定清單的優先順序。 例如，如果企業的短期目標是要減少下兩個會計季度中的 IT 支出，則「成本管理」類別中的商務間距就可透過與每個關聯的預估成本節省來設定優先順序。

此程序的輸出是配合商務分類的高階解決方案堆疊排名清單。 

## <a name="step-4-align-high-level-solutions-with-business-groups-to-design-solutions"></a>步驟 4：對應高階解決方案與商務群組來設計解決方案 

現在已列舉數位轉型的目標、設定優先順序，且提議高階解決方案；下一個步驟是讓雲端策略小組對齊每個高階解決方案，與每個企業群組中的設計和實作小組。 

小組採用優先設定的清單，並逐步完成每個高階解決方案來設計每個解決方案。 設計程序會牽涉到新基礎結構和新工作負載的規格。 它們所遵循的人員及程序的角色也可能會變更。 每個設計小組也務必在這個階段包含治理和安全性小組以檢閱每一項設計。 每一項設計都必須落在治理與安全性小組所定義的原則和程序中，且這些小組必須包含在每一項設計的最終登出。

每個解決方案的設計都是非一般的工作，必須與其他小組的其他解決方案設計一起考量。 例如，如果數個設計會導致現有內部部署應用程式和服務移轉至雲端，將這些群組在一起並設計整體的移轉策略可能更有效率。 另舉一例，可能無法遷移某些現有的內部部署應用程式和服務，解決方案可能是要將其取代為新的開發或第三方服務。 在此情況下，將這些群組在一起，並判斷它們判斷之間的重疊，以判斷第三方服務是否可用於多個解決方案，可能更有效率。

一旦解決方案的設計完成之後，小組會繼續進行每一項設計的實作階段。 每個解決方案設計的實作階段可以使用標準專案管理程序來執行。

## <a name="step-5-translate-existing-roles-skills-and-process-for-the-cloud"></a>步驟 5：轉譯雲端的現有角色、技術和程序

在 IT 產業歷程記錄期間的每個發展階段，最重要的產業變更通常會依人員角色中的變更標示。 從大型主機轉換到用戶端/伺服器模型期間，電腦操作員角色會大量消失，並由系統管理員所取代。 當虛擬化的時代來臨時，個人使用實體伺服器的需求將會消失，並取代為虛擬化專業人員的需求。 同樣地，隨著機構轉移至雲端運算，角色很可能會再次變更。 例如，資料中心專家可能取代雲端財務分析師。 即使是在 IT 職稱維持不變的情況下，每日的工作角色已大幅發展。 

IT 部門人員可能會對其角色和職位覺得有點焦慮，因為他們了解到支援雲端解決方案需要一組不同的技巧。 但是，探索並了解新雲端技術的敏捷員工不必感到這種恐懼。 它們可以主導採用雲端服務，並協助組織了解並運用相關聯的變更。 

### <a name="capturing-concerns"></a>擷取的考量

在數位轉型期間，每個小組應該在任何員工考量出現時加以擷取。 在擷取考量時，識別下列項目： 
* 考量的類型。 例如，背景工作角色可能可以因應數位轉型伴隨而來的工作職責變更。
* 考量的影響 (如果未解決)。 例如，因應數位轉型可能會導致背景工作角色執行所需變更速度緩慢。 
* 解決考量所需具備的領域。 例如，如果 IT 部門中的背景工作角色都不願意獲取新的技能，最好具備 IT 專案關係人的領域來解決這個問題。 識別領域對於某些問題可能很清晰，而在這些情況下，您可能必須呈報領導主管。 

### <a name="identify-gaps"></a>找出間距

透過貴企業數位轉型問題的另一個層面是找出**間距**。 間距是您數位轉型所需但目前不存在貴企業中的角色、技能或程序。 

開始列舉伴隨數位轉型的新責任，特別強調新責任和即將淘汰的目前責任。 識別與每個責任對齊的區域。 針對新責任，決定與區域對齊的緊密程度。 部分責任可能會跨越數個區域，而這代表更好的對齊機會，應該要擷取成考量。 在沒有任何識別為需負責區域的情況下，擷取這個作為間距。

接下來，找出支援責任所需的技能。 判斷貴企業是否具有包含這些技能的現有資源。 如果沒有任何現有的資源，判斷哪些訓練課程或人才收購是必要的。 決定必須支援責任以保持數位轉型順利進行的時間範圍。

最後，找出將執行這些技能的角色。 您的某些現有工作力會假設部分角色，在其他情況下可能需要全新的角色。 

### <a name="partner-across-teams"></a>跨小組的夥伴

填滿貴組織數位轉型間距所需的技能，通常不會侷限於單一角色或甚至是單一部門。 技能都會有關聯性和相依性，可橫跨單一角色或多個角色，而這些角色可能存在於多個部門。 例如，工作負載擁有者可能需要 IT 角色中的人員佈建核心資源，例如訂用帳戶和資源群組。

這些相依性表示貴組織實作來管理角色之間工作流程的新處理序。 在上述範例中，有許多不同類型的程序可支援工作負載擁有者和 IT 角色之間的關聯性。 例如，您可以建立工作流程工具來管理程序，或可以使用簡單的電子郵件範本。

追蹤這些相依性，並記下可支援它們的程序，以及程序目前是否存在。 對於需要工具的程序，請確定部署任何工具的時間軸對齊整體數位轉型的排程。

## <a name="next-steps"></a>後續步驟

數位轉型是反覆的程序，且小組涉及每個反覆項目都將會變得更有效率。 

> [!div class="nextstepaction"]
> [了解 Azure 如何運作](what-is-azure.md)