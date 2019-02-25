---
title: CAF：資源一致性的計量、指標及風險承受度
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 資源一致性的計量、指標及風險承受度
author: BrianBlanchard
ms.openlocfilehash: 3a2561a6d1d81a6395bb256e921a7a26898b45a0
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901032"
---
# <a name="resource-consistency-metrics-indicators-and-risk-tolerance"></a>資源一致性的計量、指標及風險承受度

本文旨在協助您量化商務風險承受度，因為這與資源一致性息息相關。 定義計量和指標可協助您建立商務案例，藉此讓資源一一致性專業領域變得更完善。

## <a name="metrics"></a>度量

資源一致性專業領域著重於解決與雲端部署的作業管理相關的風險。 若要進行風險分析，您可以收集與 IT 作業相關的資料，以判斷要面對多少風險，以及針對您規劃的雲端部署投資資源一致性治理有多重要。

每個組織都有不同的作業案例，但下列項目表示了在評估資源一致性專業領域內的風險承受度時，應收集之計量的實用範例：

- **雲端資產**。 雲端部署的資源總數。
- **未標記的資源**。 沒有必要帳戶處理、業務衝擊或組織標記的資源數目。
- **未充分使用的資產**。 其中記憶體、CPU 或網路功能一致未被充分利用的資源數目。
- **資源耗盡**。 其中記憶體、CPU 或網路功能因負載而耗盡的資源數目。
- **資源存留期**。 自上次部署或修改資源以來的時間。
- **服務可用性**。 與預期執行時間相比，裝載於雲端之工作負載的實際執行時間百分比。
- **危急條件中的 VM**。 已部署的 VM 數目，其中已偵測到一或多個重大問題，您必須解決這類問題，才能還原正常的功能。
- **依嚴重性排序的警示**。 有關已部署資產的警示總數 (依嚴重性細分)。
- **狀況不良的子網路連結**。 有網路連線問題的資源數目。
- **狀況不良的服務端點**。 外部網路端點的問題數目。
- **雲端提供者服務健康狀態事件**。 雲端提供者造成的中斷或效能事件的數目。
- **備份健康情況**。 主動同步處理的備份數目。
- **復原健康情況**。 已成功執行復原作業的次數。

## <a name="risk-tolerance-indicators"></a>風險承受度指標

雲端平台提供了一組基本功能，使部署小組無需大量額外規劃或程序，即可有效地管理小型部署。 因此，包含相當少量雲端型資產的小型開發/測試或實驗性第一工作負載代表低層級的風險，並且可能不需要採用正式的資源一致性原則。

但是，隨著雲端資產規模的擴大，管理資產的複雜度就變得更加困難。 隨著雲端上的資產越來越多，識別資源擁有權和控制資源的能力，對於將風險降至最低來說非常重要。 隨著更多任務關鍵型工作負載部署至雲端，服務的執行時間變得十分重要，並且對服務中斷的容錯潛在成本超支會迅速減少。

在雲端採用的早期階段，您可以與 IT 作業小組和商務專案關係人合作，以識別與資源一致性相關的[業務風險](business-risks.md)，然後決定可接受的風險承受度基準。 這一節的 CAF 會提供範例，但您公司或部署的詳細風險和基準可能會有所不同。

擁有基準之後，請建立最低基準，來指出所識別的風險增加到什麼程度會讓您無法接受。 這些基準可充當觸發條件，一旦觸及，就是您必須採取行動來減輕這些風險的時候。 以下幾個範例會說明，作業計量 (例如，前面討論過的那些) 如何證明您應該提高對資源一致性規則的投資。

- **標記和命名觸發程序**。 如果某公司有超過 X% 的資源缺少所需標記資訊或不遵守命名標準，則應考慮投資資源一致性專業領域，以協助改善這些標準，並確保將它們一致地應用於雲端部署資產。
- **過度佈建的資源觸發程序**。 如果某公司有超過 X% 的資產定期使用非常少量的可用記憶體、CPU 或網路功能，則建議對資源一致性專業領域進行投資，以協助最佳化這些項目的資源使用狀況。
- **佈建不足的資源觸發程序**。 如果某公司有超過 X% 的資產定期耗盡其大部分可用記憶體、CPU 或網路功能，則建議對資源一致性專業領域進行投資，以協助確保這些資產具有避免服務中斷所需的資源。
- **資源存留期觸發程序**。 擁有超過 X 個資源且超過 X 個月未更新的公司，可以受益於資源一致性專業領域的投資，旨在確保作用中資源得到修補且健康情況良好，同時淘汰過時或其他未使用的資產。  
- **服務可用性觸發程序**。 在任務關鍵服務經歷過低於 X % 執行時間的公司，應該投資資源一致性專業領域，以改善其服務的可靠性。
- **VM 健康狀態觸發程序**。 擁有超過 X% 的 VM 遇到重大健康情況問題的公司，應該投資資源一致性專業領域，來找出問題並改善 VM 穩定性。
- **網路健康情況觸發程序**。 擁有超過 X% 的網路子網路或端點發生連線問題的公司，應該投資資源一致性專業領域，以找出並解決網路問題。
- **備份涵蓋範圍觸發程序**。 擁有 X% 任務關鍵資產且沒有最新備份的公司，將受益於對資源一致性專業領域的投資增加，以確保一致的備份策略。
- **備份健康情況觸發程序**。 發生超過 X% 還原作業失敗的公司，應該投資資源一致性專業領域，來識別備份的問題並確保重要資源受到保護。

用來測量風險承受度的確切計量和觸發條件，以及資源一致性專業領域的投資層級皆取決於您的組織，但您應將上述範例作為與雲端治理小組討論的實用基礎。  

## <a name="next-steps"></a>後續步驟

使用[雲端管理範本](./template.md)，記錄與目前雲端採用方案一致的計量和承受度指標。

根據風險和承受度建立流程來治理和傳達資源一致性原則的遵循情況。

> [!div class="nextstepaction"]
> [建立原則合規性流程](compliance-processes.md)