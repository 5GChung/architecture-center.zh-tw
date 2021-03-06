---
title: CAF：監視和強制執行原則遵循
description: 您要如何確保符合已建立的原則？
author: BrianBlanchard
ms.date: 1/4/2019
ms.openlocfilehash: 9066f33c8baa183476a9632e82d6eb960d03752c
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900756"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-processes-can-help-ensure-policy-adherence"></a>哪些流程可協助確保原則遵循？

<!---
I've defined policies, I've provided an architecture guide. Now how do I monitor adherence to policy? If there is a violation, how do I enforce the policy?
--->

建立您的雲端原則聲明並且草擬設計指南之後，您必須建立策略以確保您的雲端部署符合原則需求。 此策略必須包含您雲端治理小組的持續檢閱和通訊流程，建立當違反原則時需要動作的準則，並且定義自動化監視和合規性系統 (將會偵測違規和觸發修復動作) 的需求。

請參閱[可採取動作的治理旅程](../journeys/overview.md)的公司原則區段，以取得原則遵循流程如何符合雲端治理方案的範例。

## <a name="prioritize-policy-adherence-processes"></a>制定原則遵循流程的優先順序

需要在開發流程方面投資多少，才能支援您的原則目標？ 根據您的雲端部署的大小和成熟度，建立支援合規性之流程的投入量，以及與此投入量相關聯的成本可能大不相同。

針對包含開發和測試資源的小型部署，原則需求簡單且需要少數的專用資源就能解決。 相反地，具有高優先順序安全性和效能需求的成熟任務關鍵性雲端部署，可能需要一組人員、廣泛的內部流程以及自訂監視工具，才能支援您的原則目標。

作為定義您原則遵循策略中的第一個步驟，請評估以下討論的流程可以如何支援您的原則需求。 決定投資這些流程值得多少投入量，然後使用這項資訊來建立符合這些需求的實際預算和人員計劃。

## <a name="establish-cloud-governance-team-processes"></a>建立雲端治理小組流程

在定義原則合規性修復的觸發程序之前，您需要建立您的小組將會使用，以及資訊如何在 IT 人員與雲端治理小組之間共用和呈報的整體流程。

### <a name="assign-cloud-governance-team-members"></a>指派雲端治理小組成員

誰會提供原則合規性的持續指引，並且處理在部署及操作您的雲端資產時出現的原則相關問題？ 雲端治理小組的大小和組合取決於原則需求的複雜度，以及您附加至原則合規性的預算和人員優先順序。

選擇已定義原則聲明所涵蓋的區域中，具有專業知識的小組成員。 針對初始測試部署，可以限制為負責建立治理基本的少數系統管理員。 隨著您的部署成熟以及原則變得更加複雜且與更廣泛公司原則需求更緊密地整合，您的雲端治理小組必須變更以支援日益增加的複雜原則需求。

隨著您的治理流程成熟，請定期檢閱雲端指引小組的成員資格，以確保您可以適當地解決最新的原則需求。 識別具有相關經驗或者對於特定治理領域有興趣的 IT 人員成員，並且視需要將他們永久或依臨機操作基準納入小組中。

### <a name="reviews-and-policy-iteration"></a>檢閱和原則反覆項目

隨著額外資源部署，雲端治理小組必須確定新工作負載或資產符合原則需求。 規劃與負責部署任何新資源的小組開會，討論與設計指南保持一致。

隨著整體部署成長，定期評估新的潛在風險並且視需要更新原則聲明和設計指南。 排程五個治理專業領域個別的定期檢閱週期，以確保原則是最新的且符合原則。

### <a name="education"></a>教育訓練

原則合規性需要 IT 人員和開發人員了解影響他們責任領域的原則需求。 規劃投入資源以記錄決策和需求，並且對所有相關小組教育支援您的原則需求的設計指南。

因應原則變更，定期更新文件和訓練教材，並確保教育工作能夠將更新的需求和指引傳授給相關的 IT 人員。  

### <a name="establish-escalation-paths"></a>建立呈核路徑

如果資源不符合規範，誰會收到通知？ 如果 IT 人員偵測到原則合規性問題，他們要連絡誰？ 請確定已清楚定義雲端治理小組的呈報流程。 請確定這些通訊通道會保持更新，以反映人員和組織變更。

## <a name="violation-triggers-and-actions"></a>違規觸發程序和動作

在您定義雲端治理小組及其流程之後，您必須明確地定義將會觸發動作的合規性違規合格項目，以及這些動作是什麼。

### <a name="define-triggers"></a>定義觸發程序

針對每個原則聲明，檢閱需求以決定何者構成原則違規。 使用您在原則定義流程中建立的資訊來產生觸發程序。

* 風險承受度 - 根據您在[風險承受度分析](risk-tolerance.md)中建立的計量和風險指標，建立違規觸發程序。
* 定義原則需求 - 原則聲明可能會提供服務等級協定 (SLA)、商務持續性和災害復原 (BRCD) 或效能需求，這些項目應該用來作為合規性觸發程序的基礎。

### <a name="define-actions"></a>定義動作

每個違規觸發程序都應該有一個對應的動作。 觸發的動作都應該在發生違規時，通知適當的 IT 人員或雲端治理小組成員。 此通知會根據偵測到違規的類型和嚴重性，導致手動檢閱合規性問題及啟動預先建立的修復流程。

違規觸發程序和動作的一些範例：

| 雲端治理專業領域 | 範例觸發程序 | 範例動作 |
|-----------------------------|----------------|---------------|
| 成本管理 | 每月雲端費用高於預期 20%。 | 通知計費單位主管，該主管會開始檢閱資源使用量。 |
| 安全性基準 | 偵測到可疑的使用者登入活動。 | 通知 IT 安全性小組，並停用可疑的使用者帳戶。 |
| 資源一致性 | 工作負載的 CPU 使用率大於 90%。 | 通知 IT 作業小組並且相應放大其他資源來處理負載。 |

## <a name="monitoring-and-compliance-automation"></a>監視與合規性自動化

在您定義合規性違規觸發程序和動作之後，您可以開始規劃如何最佳地使用記錄和報告工具及雲端平台的其他功能，協助自動化監視和原則合規性策略。

請參閱 CAF [記錄和報告決策指南](../../decision-guides/log-and-report/overview.md)主題，以取得選擇部署的最佳監視模式的指引。
