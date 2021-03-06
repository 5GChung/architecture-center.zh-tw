---
title: CAF：大型企業 – 有關治理 MVP 的其他技術細節
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 大型企業 – 有關治理 MVP 的其他技術細節
author: BrianBlanchard
ms.openlocfilehash: 50b0ae1ebb386d2f0b693a347d599ff29680a9cb
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900955"
---
# <a name="large-enterprise-best-practice-explained"></a>大型企業：最佳做法說明

治理旅程會從一組最初的[公司原則](./initial-corporate-policy.md)開始。 這些原則會用來建立治理的最小可行產品 (MVP)，以反映出[最佳做法](./overview.md)。

在本文中，我們會討論建立治理 MVP 所需的策略概要。 治理 MVP 的核心在於[部署加速](../../deployment-acceleration/overview.md)專業領域。 在此階段套用的工具和模式有助於累進式發展，而這是在未來擴大治理成效所需的。

## <a name="governance-mvp-cloud-adoption-foundation"></a>治理 MVP (雲端採用基礎)

快速採用治理和企業原則是可行的，因為有一些簡單的原則和雲端治理工具。 在任何治理程序中都有要達成的三個治理專業領域，而這些是第一個。 每一個項目都將以本文為基礎來展開。

為建立起點，本文將針對建立治理 MVP (所有採用的基礎) 所需的身分識別基準、安全性基準和部署加速，討論其背後的策略概要。

![漸進式治理 MVP 的範例](../../../_images/governance/governance-mvp.png)

## <a name="implementation-process"></a>實作程序

治理 MVP 的實作相依於身分識別、安全性和網路。 解決相依性後，雲端治理小組就會決定治理的幾個層面。 雲端治理小組和支援小組所做的決策會透過單一強制資產套件來實作。

![漸進式治理 MVP 的範例](../../../_images/governance/governance-mvp-implementation-flow.png)

這項實作也可以使用簡單的檢查清單來說明：

1. 尋求有關以下核心相依性的決策：身分識別、網路和加密。
2. 判斷在強制執行公司原則時要使用的模式。
3. 針對資源一致性、資源標記及記錄與報告專業領域，判斷適當的治理模式。
4. 實作符合所選原則強制執行模式的治理工具，以套用相依性決策和治理決策。

[!INCLUDE [implementation-process](../../../../../includes/cloud-adoption/governance/implementation-process.md)]

## <a name="application-of-governance-defined-patterns"></a>治理定義模式的應用程式

雲端治理小組會負責執行下列決策和實作。 許多服務會要求來自其他小組的輸入，但雲端治理小組可負責決策和實作。 下列各節將概述為此使用案例所做的決策，以及每個決策的詳細資料。

### <a name="subscription-model"></a>訂用帳戶模型

已選擇**混合**作為 Azure 訂用帳戶的模式。

- 出現有關 Azure 資源的新要求時，應針對每個營運地理位置中的每個主要業務單位建立「部門」。 在每個部門內，應為每個應用程式原型建立「訂用帳戶」。
- 應用程式原型是以類似需求將應用程式分組的方式。 常見範例包括：具有受保護資料的應用程式、受到控管的應用程式 (例如 HIPAA 或 FedRAMP)、低風險應用程式、具有內部部署相依性的應用程式、SAP 或 Azure 中的其他大型主機，或是擴充內部部署 SAP 或大型主機的應用程式。 每個組織在資料分類和支援業務的應用程式類型上都有獨特需求。 數位資產的相依性對應可協助定義組織中的應用程式原型。
- 根據上述兩點，應同意將常見的命名慣例列為訂用帳戶設計的一部分。

### <a name="resource-consistency"></a>資源一致性

已選擇**階層式一致性**作為資源一致性模式。

- 應針對每個應用程式建立資源群組。 應針對每個應用程式原型建立管理群組。 Azure 原則應該套用至相關管理群組中的所有訂用帳戶。
- 作為部署程序的一部分，所有資產的資源一致性範本應該儲存在原始檔控制中。
- 每個資源群組應符合特定工作負載或應用程式。
- 所定義的 Azure 管理群組階層應以巢狀群組表示計費責任歸屬和應用程式擁有權。
- Azure 原則的廣泛實作可能會超出小組的時間承諾，並可能無法在此時提供更多價值。 不過，應建立簡單的預設原則並套用至每個資源群組，以強制執行最初的幾個雲端治理原則聲明。 這會用來定義特定治理需求的實作。 然後，這些實作可以套用到所有已部署的資產上。

### <a name="resource-tagging"></a>資源標記

已選擇**會計**作為資源標記的模式。

- 已部署的資產應該標記取代為下列項目的值：部門/計費單位、地理位置、資料分類、重要性、SLA、環境、應用程式原型、應用程式及應用程式擁有者。
- 這些值搭配 Azure 管理群組及已部署資產的相關訂用帳戶，將可擬定治理、營運和安全性決策。

### <a name="logging-and-reporting"></a>記錄與報告

在此時，我們建議開發小組使用**混合式**作為記錄與報告的模式，但並非必要。

- 在為了進行記錄或報告而收集的特定資料點上，目前並未設定治理需求。 此功能是此虛構敘述特有的，而且應將其視為反模式。 應儘速決定和強制執行記錄標準。
- 發行任何受保護的資料或任務關鍵性工作負載之前，需執行額外的分析。
- 在支援受保護的資料或任務關鍵性工作負載之前，現有的內部部署作業監視解決方案必須獲派工作區的存取權，以便進行記錄。 如果應用程式要由定義的 SLA 來支援，則應用程式必須符合使用該租用戶時的相關安全性和記錄需求。

## <a name="evolution-of-governance-processes"></a>治理程序的發展

某些原則聲明無法或不應該由自動化工具來控制。 而其他原則需要 IT 安全性和內部部署身分識別基準小組的持續努力。 雲端治理小組需要監看下列程序來實作最後八個原則聲明：

**公司原則變更**：雲端治理小組會變更治理 MVP 的設計，以採用新的原則。 治理 MVP 的值可用於自動強制執行新原則。

**加速採用**：雲端治理小組已檢閱多個小組的部署指令碼。 他們已維護一組能作為部署範本的指令碼。 這些範本可供雲端採用小組和 DevOps 小組使用，進而更快速地定義部署。 每個指令碼都包含強制執行治理原則的需求，而且不需要雲端採用工程師執行額外的工作。 作為這些指令碼的規劃者，他們能更快速地實作原則變更。 此外，這些範本可視為採用的加速器。 這可以在沒有嚴格的強制遵循要求下，確保部署的一致性。

**工程師訓練**：雲端治理小組提供兩個月的訓練課程，並且已為工程師建立兩段影片。 這兩種資源可協助工程師快速了解治理特性，以及如何執行部署。 小組正在新增訓練資產，以示範生產與非生產部署之間的差異，而這可協助工程師了解新原則如何影響採用。 這可以在沒有嚴格的強制遵循要求下，確保部署的一致性。

**部署規劃**：在部署任何包含受保護資料的資產之前，雲端治理小組會負責檢閱部署指令碼，以驗證治理的一致性。 若現有小組使用先前已核准的部署，則會使用程式設計工具對其進行稽核。

**每月稽核和報告**：雲端治理小組會每月對所有雲端部署執行稽核，以驗證原則是否仍保持一致。 如果發現偏差，他們就會記下這些偏差，然後將其分享給雲端採用小組。 如果強制作業沒有造成業務中斷或資料流失的風險，就會自動強制執行原則。 在稽核結束時，雲端治理小組會為雲端策略小組和每個雲端採用小組編譯報告，以告知整體的原則遵循狀況。 報告也會儲存以便用於稽核和法律用途。

**每季原則檢閱**：雲端治理小組和雲端策略小組會每季檢閱稽核結果，並提出公司原則的建議變更。 這些建議中有許多都是持續改進與觀察使用量模式的結果。 核准的原則變更會在後續的稽核週期內整合至治理工具中。

## <a name="alternative-patterns"></a>替代模式

如果為此治理旅程選擇的模式不符合讀者需求，以下是每個模式的替代方案：

- [加密模式](../../../decision-guides/encryption/overview.md)
- [身分識別模式](../../../decision-guides/identity/overview.md)
- [記錄與報告模式](../../../decision-guides/log-and-report/overview.md)
- [原則強制執行模式](../../../decision-guides/policy-enforcement/overview.md)
- [資源一致性模式](../../../decision-guides/resource-consistency/overview.md)
- [資源標記模式](../../../decision-guides/resource-tagging/overview.md)
- [軟體定義網路模式](../../../decision-guides/software-defined-network/overview.md)
- [訂用帳戶設計模式](../../../decision-guides/subscriptions/overview.md)

## <a name="next-steps"></a>後續步驟

在實作本指南後，每個雲端採用小組即可使用可靠的治理基礎繼續進行下去。 雲端治理小組會齊頭並進，持續更新公司原則和治理專業領域。

這兩個小組會使用承受度指標，來識別要繼續支援雲端採用所需要的下一個發展目標。 在這趟旅程中，公司的下一步是發展其治理基準來支援使用傳統或第三方多重要素驗證 (MFA) 的應用程式需求。

> [!div class="nextstepaction"]
> [身分識別基準演進](./identity-baseline-evolution.md)
