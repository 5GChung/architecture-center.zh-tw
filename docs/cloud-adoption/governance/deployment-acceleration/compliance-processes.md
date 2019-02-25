---
title: CAF：部署加速原則合規性流程
description: 部署加速原則合規性流程
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
author: alexbuckgit
ms.openlocfilehash: 08046a4ae199a8714393d2aba3841dd84008d836
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900867"
---
# <a name="deployment-acceleration-policy-compliance-processes"></a>部署加速原則合規性流程

本文討論治理[部署加速](./overview.md)的原則遵循流程方法。 若要有效治理雲端設定，就要從重複執行手動流程來開始，以便偵測問題及強制實行原則來緩解這些風險。 不過，您也可以將這些流程改為自動進行，然後輔以工具來降低治理負擔，並對偏差行為做出更快速的回應。

## <a name="planning-review-and-reporting-processes"></a>規劃、檢閱及報告流程

即使是雲端中最佳的部署加速工具，其效果也就和其所支援的流程與原則相同而已。 以下是一組經常作為安全性基準專業領域一部分的流程範例。 當您規劃可讓您根據業務變更和 IT 小組 (負責將治理指引付諸行動) 意見反應來繼續更新安全性原則的流程時，請使用這些範例作為起點。

**初始風險評估及規劃**：在您初次採用部署加速專業領域時，請識別您的核心業務風險和與商務應用程式部署相關的承受度。 使用此資訊與 IT 營運小組和安全性小組的成員討論特定技術風險，以及開發一組基本的部署和設定原則來降低這些風險，從而建立初始治理策略。

**部署規劃**：在部署任何資產之前，請先執行安全性檢閱來找出任何新的風險，並確定已符合所有的存取和資料安全性原則需求。

**部署測試**：在部署任何資產時，與公司內安全性小組合作的雲端治理小組會負責檢閱部署，以驗證安全性原則的合規性。

**年度規劃**：進行年度的部署加速策略高階檢閱。 探索公司未來計劃的優先順序以及更新後的雲端採用策略，以找出可能提高的風險和其他新浮現的設定需求和機會。 同時利用這個機會檢閱最新部署加速最佳做法，並且將其整合至您的原則和檢閱流程。

**每季檢閱與規劃**：進行每季的安全性稽核資料和附隨報告檢閱，以找出部署加速原則需要變更的地方。 在此流程中，請審視目前的網路安全性情勢以主動預料新興的威脅，並適當更新原則。 完成檢閱之後，請讓應用程式和系統的設計指引能與更新後的原則契合。

此規劃流程也是評估您的雲端治理小組目前成員資格，以了解新的或演進原則相關差距和 DevOps 與部署加速相關風險的好時機。 邀請相關 IT 人員來參與檢閱及規劃，無論是作為小組的暫時技術顧問或永久成員。

**教育與訓練**：每兩個月提供訓練課程，以確定 IT 人員和開發人員跟上最新的部署加速策略和需求。 在此流程中檢閱及更新任何文件、指導方針或其他訓練資產，以確保它們與最新的公司原則聲明同步。

**每月稽核和報告檢閱**：對所有雲端部署執行每月稽核，以確保這些部署與設定原則能夠持續保持一致。 檢閱 IT 人員的安全性相關活動，並識別任何未在現行監視和強制程序中處理的合規性問題。 此檢閱的結果會向雲端策略小組和每個雲端採用小組報告，以告知整體的原則遵循狀況。 報表也會儲存以便用於稽核和法律用途。

## <a name="ongoing-monitoring-processes"></a>持續監視流程

根據您雲端基礎結構目前和過去狀態的可見度，判斷您的部署加速治理策略是否成功。 如果無法分析您雲端資源安全性健康情況和活動的相關計量和資料，您就無法識別風險的變更或者偵測是否違反風險承受度。 上面所討論的持續治理流程需要品質資料，以確保可以修改原則來保護您的基礎結構，對抗因為資源設定不正確而不斷變化的威脅和風險。

請確定您的安全性和 IT 小組已針對雲端基礎結構實作自動化監視系統，可擷取評估風險所需的相關記錄資料。 主動監視這些系統以確保立即偵測及降低潛在原則違規，並且確保您的監視策略符合部署和設定需求。

## <a name="violation-triggers-and-enforcement-actions"></a>違規觸發程序及強制執行動作

由於不符合設定原則可能會導致重大的服務中斷風險，因此雲端治理小組應該要能察覺嚴重違反原則的情事。 請確定 IT 人員有清楚的呈報路徑，可將設定合規性問題回報給最適合識別並確認原則問題是否已緩和的治理小組成員。  

偵測到違規時，您應該採取動作以儘速調整原則。 您的 IT 小組可以使用[適用於 Azure 的部署加速工具鏈](toolchain.md)中概述的工具，將大部分違規觸發程序自動化。

下列觸發程序和強制執行動作提供範例，您可以在討論如何使用監視資料以解決原則違規時使用：

- **偵測到設定發生了非預期的變更**。 如果資源的設定發生了非預期的變更，請與 IT 人員及工作負載擁有者一同找出根本原因，並開發修復計劃。
- **新資源的設定未遵守原則。** 請與 DevOps 小組及工作負載擁有者一同在專案啟動期間檢閱部署加速原則，讓所有相關人員都了解相關的原則需求。
- **部署失敗或設定問題造成專案時程延誤。** 請與開發小組及工作負載擁有者一同確定，小組已了解如何自動部署雲端式資源，以獲得一致性和可重複性。 請務必在開發初期就實現完全自動化的部署 &mdash; 開發後期才嘗試實現這一點經常會導致非預期的問題和延誤。

## <a name="next-steps"></a>後續步驟

使用[雲端管理範本](./template.md)，記錄與目前雲端採用方案一致的流程和觸發程序。

如需執行與採用方案一致的雲端管理原則指引，請參閱有關改良專業領域的文章。

> [!div class="nextstepaction"]
> [部署加速專業領域改進](./discipline-improvement.md)