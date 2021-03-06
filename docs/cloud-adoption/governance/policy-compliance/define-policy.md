---
title: CAF：定義公司原則聲明
description: 您要如何建立原則，以反映並降低風險？
author: BrianBlanchard
ms.date: 01/02/2019
ms.openlocfilehash: 97a2c25fea621e5418e505375eb0e80007ddb0de
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901108"
---
<!---
I understand risk and tolerance, now what do I do?
Define the policy... [aspirational statement to move towards 2/1] If you need help defining policies, each discipline includes references to common business risks and policies to mitigate the risks...
--->

# <a name="defining-corporate-policy-for-cloud-governance"></a>定義雲端治理的公司原則

一旦您已針對組織分析雲端轉型過程中的已知風險和相關風險承受度，下一步就是建立明確處理這些風險的原則，並盡可能定義緩解這些風險所需的步驟。

<!-- markdownlint-disable MD026 -->

## <a name="how-can-corporate-it-policy-become-cloud-ready"></a>公司 IT 原則如何成為雲端就緒？

在傳統治理和雲端治理中，公司原則建立作用的治理定義。 大部分的「IT 治理」動作的目的是要實作技術，以監視、強制執行、操作及自動化那些公司原則。 雲端治理建立在類似的概念上。

![公司治理和治理專業領域](../../_images/operational-transformation-govern.png)

*圖 1.公司治理和治理專業領域。*

上圖示範業務風險、原則與合規性，以及監視與強制執行之間對於建立治理原則的互動。 其後是實現您原則的五個雲端治理專業領域。

雲端治理是在一段時間內持續進行採用工作的成果，因為轉換不會在一夕之間發生。 使用快速積極的方法，嘗試在解決關鍵公司原則變更之前提供完整雲端治理，這樣很少會產生想要的結果。 相反地，我們建議漸進的方法。

雲端採用架構的不同之處在於購買週期，以及該週期如何產生有效的轉換影響。 由於沒有龐大的基本建設費用 (CapEx) 收購需求，工程師可以更快開始實驗及採用。 在大部分公司文化中，消除採用的 CapEx 障礙可能導致回饋循環、有機成長及漸進執行變得緊迫。

雲端採用的移轉，需要是受治理的移轉。 在許多組織中，公司原則轉換透過漸進原則變更和自動化那些變更強制執行 (與雲端服務提供者一同定義的新能力)，而達到改善的治理和更高的遵守率。

<!-- markdownlint-enable MD026 -->

## <a name="review-existing-policies"></a>檢閱現有原則

隨著您的雲端部署的成熟以及遷移到雲端中的 IT 資產數量增加，您的風險及相關聯的原則需求也會改變。 治理是一個持續不斷的過程，應該定期與 IT 人員和專案關係人一起檢閱原則，以確保裝載在雲端的資源會繼續保持遵守公司整體的目標和需求。 您對於新風險和容忍程度的了解可記錄成[現有原則評論](what-is-a-cloud-policy-review.md)，以判斷適合您組織的必要治理層級。

> [!TIP]
> 如果您的組織受第三方合規性治理，要考慮的一個最大業務風險為遵守[法規合規性](what-is-regulatory-compliance.md)的風險。 通常不能降低此風險，反之可能需要嚴格遵守。 開始原則檢閱之前，請務必了解您的第三方合規性需求。

## <a name="create-cloud-policy-statements"></a>建立雲端原則陳述式

雲端型 IT 原則可確定 IT 人員和自動化系統必須支援的需求、標準和目標。 原則決策是您的[雲端架構設計](align-governance-journeys.md)中的主要因素，以及您要如何實作[原則遵循流程](processes.md)。

個別的雲端原則聲明是用來解決在風險評估流程中識別出之特定風險的方針。 雖然這些原則可以整合到更廣泛的公司原則文件中，但 CAF 指南中討論的雲端原則聲明，往往是對風險和處理計劃的更簡潔的摘要。 每個定義定義都應該包含下列資訊：

- **業務風險**。 此原則將解決的風險摘要。
- **原則聲明**。 對原則需求和目標的簡要說明。
- **設計或技術指導**。 支援並強制執行此原則的可操作建議、規格或其他指引，IT 團隊和開發人員在設計與建置其雲端部署時可以使用該原則。

如果您需要協助開始定義原則，請參閱治理章節概觀中介紹的[治理專業領域](../governance-disciplines.md)。 這些專業領域的文章都包含移到雲端時遇到的常見商務風險的範例，以及用來減輕這些風險的範例原則 (例如，請參閱「成本管理法則」的[範例原則定義](../cost-management/policy-statements.md)）。

## <a name="incremental-governance-and-integrating-with-existing-policy"></a>增量治理並與現有原則整合

對雲端環境的計劃新增項目應一律進行審核，以確保其符合現有原則，並更新原則以解決未涵蓋的任何問題。 您也應該定期執行[雲端原則檢閱](what-is-a-cloud-policy-review.md)，以確保您的雲端原則已更新，並與任何新的公司原則同步處理。

將雲端原則與傳統 IT 原則整合的需求，主要取決於雲端治理程序的成熟度和雲端資產的規模。 如需在雲端轉換期間處理原則整合的更廣泛的討論，請參閱有關[增量治理和原則 MVP](overview.md) 一文。

## <a name="next-steps"></a>後續步驟

定義原則之後，草擬架構設計指南，為 IT 人員和開發人員提供可採取動作的指導方針。

> [!div class="nextstepaction"]
> [草擬架構設計指南](align-governance-journeys.md)
