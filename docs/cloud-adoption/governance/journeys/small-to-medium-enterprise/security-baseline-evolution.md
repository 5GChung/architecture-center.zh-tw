---
title: CAF：中小型企業 – 安全性基準演進
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 中小型企業 – 安全性基準演進的說明
author: BrianBlanchard
ms.openlocfilehash: 5714b886ef63cc2392905250d97ea905839f6011
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900835"
---
# <a name="caf-small-to-medium-enterprise-security-baseline-evolution"></a>CAF：中小型企業：安全性基準演進

本文透過加入支援將受保護的資料移至雲端的安全性控制項來推演敘述。

## <a name="evolution-of-the-narrative"></a>敘述的演進

IT 和業務領導階層對於 IT、應用程式開發和 BI 小組的早期實驗結果表示滿意。 若要實現這些實驗中的具體商業價值，必須允許這些小組將受保護的資料整合到解決方案中。 這會觸發對公司原則的修改，但也需要在受保護的資料可以登陸雲端之前，落實雲端治理的實施。

### <a name="evolution-of-the-cloud-governance-team"></a>雲端治理小組的演進

考慮到目前為止提供的敘述和支援不斷變化的影響，雲端治理小組現在的看法不同。 發起小組的兩位系統管理員現在被視為經驗豐富的雲端架構設計師。 隨著這種敘述的發展，對於他們的看法將從雲端保管者轉變為更類似雲端守護者的角色。

雖然差異很微妙，但在構建以治理為重點的 IT 文化時，這是一個重要的區別。 雲端保管者會清除由創新的雲端架構設計師造成的混亂。 兩個角色有自然的衝突和相反目標。 在另一方面，雲端守護者可協助保護雲端，讓其他雲端架構設計師可以減少混換，更快速地移轉。 此外，雲端守護者參與建立可加速部署和採用的範本，使其成為創新加速器，以及五大雲端專業領域的捍衛者。

### <a name="evolution-of-the-current-state"></a>目前狀態的演進

在此敘述開始時，應用程式開發小組仍然致力於開發/測試能力，而 BI 小組仍處於實驗階段。 IT 負責運作兩個託管的基礎結構環境，稱為 Prod 和 DR。

從那時起，某些將會影響治理的事項已經改變：

- 應用程式開發小組已實作 CI/CD 管線，以利用改善的使用者體驗，部署雲端原生應用程式。 該應用程式不會與受保護的資料交互作用，因此還沒準備好用於生產環境。
- IT 內的商業智慧小組會從物流、庫存和第三方，積極地籌劃雲端的資料。 此資料將用於推動可能會塑造商業流程的新預測。 但是，在將客戶和財務資料整合到資料平台之前，這些預測和見解是不可行的。
- IT 小組正在進行 CIO 和 CFO 淘汰 DR 資料中心的計劃。 DR 資料中心內的 2,000 個資產中，有 1,000 多個資產已經遭到淘汰或移轉。
- 定義鬆散的 PII 和財務資料相關原則已經過現代化。 不過，新的公司原則取決於相關安全性和治理原則的實施。 小組仍然停滯不前。

### <a name="evolution-of-the-future-state"></a>未來狀態的演進

應用程式開發和 BI 小組的早期實驗顯示在客戶經驗和資料導向決策方面有改善空間。 兩個小組都希望將這些解決方案部署到生產環境，藉此在接下來的 18 個月內擴展雲端的採用率。

在剩下的六個月中，雲端治理小組將實作安全性和治理需求，以允許雲端採用小組移轉這些資料中心內受保護的資料。

目前和未來狀態的變更會產生新風險，需要新的原則聲明。

## <a name="evolution-of-tangible-risks"></a>實質風險的演進

**資料外洩**：採用任何新的資料平台時，與潛在資料外洩相關的負債本身會增加。 採用雲端技術的技術人員已增加實作可能會降低此風險之解決方案的責任。 為確保這些技術人員履行這些責任，必須實作強固的安全性和治理策略。

此業務風險可能會延伸出少數技術風險：

- 您可能會不小心部署任務關鍵性應用程式或受保護的資料。
- 受保護的資料可能會在儲存期間，因為加密決策不佳而遭到洩漏。
- 未經授權的使用者可能會存取受保護的資料。
- 外部入侵可能會導致受保護的資料遭到存取。
- 外部入侵或拒絕服務攻擊可能會導致業務中斷。
- 組織或雇用的變更可能會導致未經授權存取受保護的資料。
- 新的惡意攻擊可能會創造入侵或存取的機會。
- 不一致的部署程序可能導致安全性漏洞，進而造成資料外洩或中斷。
- 設定漂移或遺失修補程式可能會導致非預期的安全性漏洞，進而造成資料外洩或中斷。

## <a name="evolution-of-the-policy-statements"></a>原則聲明的演進

下列原則變更有助於減緩新風險和指導實作。 清單看起來很長，但採用這些原則可能比看起來的簡單。

1. 所有已部署的資產必須依據重要性和資料類別來分類。 分類會由雲端治理小組和應用程式擁有者進行檢閱，然後再部署到雲端。
2. 儲存或存取受保護資料或不受保護資料的應用程式管理方式不同。 至少應該劃分這些資料，以避免非預期地存取受保護的資料。
3. 所有受保護的資料在待用時都必須經過加密。
4. 在包含受保護資料的任何區段中提高權限應該是例外狀況。 任何這類例外狀況都會由雲端治理小組記錄，並定期稽核。
5. 包含受保護資料的網路子網路必須與其他任何子網路隔離。 受保護資料子網路之間的網路流量將定期稽核。
6. 沒有包含受保護資料的任何子網路可以透過公用網際網路或跨資料中心直接存取。 這些子網路的存取必須透過中繼子網路進行路由。 這些子網路的所有存取都必須經過防火牆解決方案，該解決方案可以執行封包掃描和封鎖功能。
7. 治理工具必須稽核並強制執行安全性管理小組所定義的網路設定需求。
8. 治理工具必須限制只對已核准的映像進行 VM 部署。
9. 若有可能，節點設定管理應該將原則需求套用到任何客體作業系統的設定。
10. 治理工具必須強制在所有已部署的資產上啟用自動更新。 必須與作業管理小組一起檢閱違規事件，並根據作業原則來進行修復。 不會自動更新的資產必須包含在 IT 部門所負責的處理程序中。
11. 為任何任務關鍵性應用程式或受保護的資料建立新的訂用帳戶或管理群組將需要經過雲端治理小組審查，以確保已指派適當的藍圖。
12. 最低權限存取模型將會套用到包含任務關鍵性應用程式或受保護資料的任何管理群組或訂用帳戶。
13. 安全性小組應定期檢閱可能影響雲端部署的趨勢與攻擊，以更新雲端中使用的安全性管理工具。
14. 部署工具必須由雲端治理小組核准，以確保能持續治理已部署的資產。
15. 部署指令碼必須在雲端治理小組可存取的中央存放庫中進行維護，以便定進行定期檢閱和稽核。
16. 治理程序必須包含部署期間和一般週期的稽核，以確保所有資產之間的一致性。
17. 部署需要客戶驗證的任何應用程式必須使用適用於內部使用者的主要身分識別提供者核准的身分識別提供者。
18. 雲端治理程序必須包含身分識別管理小組的每季審查。 這些審查有助於識別惡意執行者或雲端資產設定應該避免的使用模式。

## <a name="evolution-of-the-best-practices"></a>最佳做法的演進

治理 MVP 設計將演變為包含新的 Azure 原則和 Azure 成本管理實作。 這兩個設計變更將共同實現新的公司原則聲明。

1. 網路和 IT 安全性小組將會定義的網路需求。 雲端治理小組將會支援對話。
2. 身分識別和 IT 安全性小組將會定義身分識別需求，並對本機 Active Directory 實作進行任何必要的變更。 雲端治理小組將會審查變更。
3. 在 Azure DevOps 中建立存放庫來存放所有相關的 Azure Resource Manager 範本和指令碼式的組態，並為這些項目設定版本。
4. Azure 資訊安全中心實作：
    1. 為包含受保護資料分類的任何管理群組設定 Azure 資訊安全中心。
    2. 依預設，將自動佈建設定為開啟，以確保修補的合規性。
    3. 建立 OS 安全性設定。 IT 安全性小組會定義設定。
    4. 在初次使用資訊安全中心時支援 IT 安全性小組。 將 IT 安全性小組的使用轉變為資訊安全中心，但基於持續改善治理功能而維持存取權。
    5. 建立 Resource Manager 範本，此範本會反映訂用帳戶內資訊安全中心設定所需的變更。
5. 更新所有訂用帳戶的 Azure 原則：
    1. 在所有管理群組和訂用帳戶上稽核並強制執行重要性和資料分類，以識別具有受保護資料分類的任何訂用帳戶。
    2. 僅稽核並強制使用已核准的映像。
6. 為包含受保護資料分類的所有訂用帳戶更新 Azure 原則：
    1. 僅稽核並強制使用標準 Azure RBAC 角色。
    2. 為個別節點上待用的所有儲存體帳戶和檔案，稽核並強制執行加密。
    3. 稽核並強制執行使用 NSG 連到所有 NIC 和子網路的應用程式。 網路和 IT 安全性小組將會定義 NSG。
    4. 稽核並強制對每個網路介面使用已核准的網路子網路和 vNet。
    5. 稽核並強制執行使用者定義路由表的限制。
    6. 針對來賓設定套用內建原則，如下所示：
        1. 稽核 Windows 網頁伺服器目前使用安全的通訊協定
        2. 稽核 Linux 及 Windows 電腦內密碼安全性設定皆設定正確
7. 防火牆組態：
    1. 識別符合所需安全性需求的 Azure 防火牆設定。 或者，識別與 Azure 相容的第三方應用裝置。
    2. 建立 Resource Manager 範本來部署具有必要設定的防火牆。
8. Azure 藍圖：
    1. 建立名為 `protected-data` 的新藍圖。
    2. 對藍圖新增防火牆和 Azure 資訊安全中心範本。
    3. 為受保護資料的訂用帳戶新增原則。
    4. 將藍圖發佈至目前計劃中含有受保護資料的任何管理群組。
    5. 對每個受影響的訂用帳戶及現有藍圖套用新的藍圖。

## <a name="conclusion"></a>結論

對治理 MVP 新增上述流程和變更，有助於降低與安全性治理相關聯的許多風險。 同時，它們會新增網路、身分識別，以及保護資料所需的安全性監視工具。

## <a name="next-steps"></a>後續步驟

隨著雲端採用持續演進，並提供額外的商業價值，風險和雲端治理需求也需要與時俱進。 對於這趟旅程中的虛構公司，下一步是支援任務關鍵性工作負載。 這是需要資源一致性控制項的時機。

> [!div class="nextstepaction"]
> [資源一致性演進](./resource-consistency-evolution.md)