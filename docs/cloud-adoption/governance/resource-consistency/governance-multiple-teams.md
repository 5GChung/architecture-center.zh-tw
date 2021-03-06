---
title: CAF：在 Azure 中多個小組的治理設計
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
description: 針對多個小組、多個工作負載及多個環境設定 Azure 治理控制的指南
author: petertaylor9999
ms.date: 2/11/2019
ms.openlocfilehash: d0f3256e1d7b752789efd709f917e4eb2e996175
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901019"
---
# <a name="governance-design-for-multiple-teams"></a>多個小組的控管設計

本指南的目標是協助您了解在 Azure 中設計資源治理模型的程序，以支援多個小組、多個工作負載和多個環境。 首先您會查看一組假設性治理需求，然後瀏覽滿足這些需求的數個範例實作。

這些需求包括：

- 企業計劃將新的雲端角色與責任轉換給一組使用者，因此需要在 Azure 中使用不同的資源存取需求，針對多個小組進行身分識別管理。 需要身分識別管理系統來儲存下列使用者的身分識別：
  1. 組織中負責**訂用帳戶**擁有權的個人。
  2. 組織中負責**共用基礎結構資源** (用來將內部部署網路連線到 Azure 虛擬網路) 的個人。
  3. 貴組織中負責管理**工作負載**的兩個個人。
- 支援多個**環境**。 環境是資源 (例如虛擬機器、虛擬網路和網路流量路由服務) 的邏輯群組。 這些資源的群組有類似的管理和安全性需求，且通常用於特定的目的，例如測試或生產環境。 在此範例中，需求適用於三種環境：
  1. **共用基礎結構環境**，其中包含其他環境中工作負載共用的資源。 例如虛擬網路，具有提供與內部部署連線的閘道子網路。
  2. **生產環境**，具有最嚴格的安全性原則。 可能包含內部或外部對向工作負載。
  3. **開發環境**，適用於概念證明和測試工作。 此環境具有與生產環境不同的安全性、合規性和成本原則。
- **最低權限的權限模型**，使用者在其中預設沒有任何權限。 模型必須支援下列項目：
  - 在訂用帳戶範圍中的單一信任使用者，具有指派資源存取權的權限。
  - 每個工作負載擁有者預設會遭到拒絕存取資源。 資源存取權限是由訂用帳戶範圍的單一信任使用者明確授與。
  - 共用基礎結構資源的管理存取權僅限於共用基礎結構擁有者。
  - 每個工作負載的管理存取權僅限於工作負載管理員。
  - 企業不需要必須在三個環境中獨立管理角色，因此需要只使用[內建 RBAC 角色][rbac-built-in-roles]。 如果企業要使用自訂的 RBAC 角色，則需要額外的程序，才能跨三個環境同步處理自訂角色。
- 依工作負載擁有者名稱、環境或兩者所追蹤的成本。

## <a name="identity-management"></a>身分識別管理

在您可以設計治理模型中的身分識別管理之前，請務必了解它所包含的四個主要區域：

- 管理：建立、編輯和刪除使用者身分識別的程序和工具。
- 驗證：藉由驗證認證 (例如使用者名稱和密碼) 來驗證使用者身分識別。
- 授權：決定通過驗證的使用者可以存取哪些資源，或者他們有權限可以執行哪些作業。
- 稽核：定期檢閱記錄和其他資訊，以探索與使用者身分識別相關的安全性問題。 這包括檢閱可疑的使用模式、定期檢閱使用者權限以確認它們都正確，以及其他功能。

Azure 只信任一個服務的身分識別，那就是 Azure Active Directory (Azure AD)。 您會將使用者新增至 Azure AD，並且針對上述所有功能使用它。 但是，在審視如何設定 Azure AD 之前，務必了解用來管理這些服務存取權的特殊權限帳戶。

當貴組織已註冊 Azure 帳戶時，至少會指派一個 Azure **帳戶擁有者**。 此外，已建立 Azure AD **租用戶**，除非現有租用戶已與貴組織使用 Office 365 等其他 Microsoft 服務相關聯。 具有 Azure AD 租用戶完整權限的**全域管理員**，會在建立時產生關聯。

Azure 帳戶擁有者和 Azure AD 全域管理員的使用者身分識別，都會儲存在由 Microsoft 管理的高度安全身分識別系統。 Azure 帳戶擁有者已獲授權可以建立、更新和刪除訂用帳戶。 Azure AD 全域管理員已獲授權可以在 Azure AD 中執行許多動作，但是針對這個設計指南，您將著重於使用者身分識別的建立和刪除。

> [!NOTE]
> 如果有現有 Office 365 或 Intune 授權與您的帳戶相關聯，則貴組織可能已經有現有的 Azure AD 租用戶。

Azure 帳戶擁有者有權可以建立、更新和刪除訂用帳戶：

![具有 Azure 帳戶管理員和 Azure AD 全域管理員的 Azure 帳戶](../../_images/governance-3-0.png)
*圖 1.具有帳戶管理員和 Azure AD 全域管理員的 Azure 帳戶。*

Azure AD **全域管理員**有權可以建立使用者帳戶：

![具有 Azure 帳戶管理員和 Azure AD 全域管理員的 Azure 帳戶](../../_images/governance-3-0a.png)
*圖 2.Azure AD 全域管理員會在租用戶中建立必要的使用者帳戶。*

前兩個帳戶 **App1 工作負載擁有者**和 **App2 工作負載擁有者**，分別都與組織中負責管理工作負載的個人相關聯。 **網路作業**帳戶是由負責共用基礎結構資源的個人所擁有。 最後，**訂用帳戶擁有者**帳戶是與負責訂用帳戶擁有權的個人相關聯。

## <a name="resource-access-permissions-model-of-least-privilege"></a>最低權限的資源存取權限模型

既然您已建立身分識別管理系統和使用者帳戶，您必須決定如何將角色型存取控制 (RBAC) 角色套用到每個帳戶，以支援最低權限的權限模型。

其他需求指出與每個工作負載相關聯的資源要彼此隔離，這樣就不會有工作負載擁有者具備並非他們擁有的其他任何工作負載的管理存取權。 還有需求僅使用[適用於 Azure RBAC 的內建角色][rbac-built-in-roles]來實作這個模型。

每個 RBAC 角色會在 Azure 中三個範圍的其中一個套用：**訂用帳戶**、**資源群組**，然後是個別**資源**。 角色會在較低的範圍繼承。 例如，如果使用者在訂用帳戶層級獲指派[內建擁有者角色][rbac-built-in-owner]，該角色也會在資源群組和個別資源層級指派給該使用者，除非已覆寫。

因此，若要建立最低權限的模型，您必須決定特定類型使用者在這三個範圍中分別可以採取的動作。 例如，需求是工作負載擁有者僅有權可以管理與他們的工作負載相關聯的資源，無法管理其他工作負載。 如果您在訂用帳戶範圍指派[內建擁有者角色][rbac-built-in-owner]，每個工作負載擁有者都會具有所有工作負載的管理存取權。

讓我們看看兩個範例權限模型，再稍微了解這個概念。 在第一個範例中，模型只信任服務管理員來建立資源群組。 在第二個範例中，模型在訂用帳戶範圍將內建擁有者角色指派給每個工作負載擁有者。

在這兩個範例中，有訂用帳戶服務管理員在訂用帳戶範圍獲指派[內建擁有者角色][rbac-built-in-owner]。 請回想[內建擁有者角色][rbac-built-in-owner]會授與所有權限，包括資源存取權的管理。
![具有擁有者角色的訂用帳戶服務管理員](../../_images/governance-2-1.png)
*圖 3.訂用帳戶，具有獲指派內建擁有者角色的服務管理員。*

1. 在第一個範例中，有**工作負載擁有者 A**，他們沒有訂用帳戶範圍的任何權限，也就是預設沒有資源存取權管理權限。 此使用者想要為其工作負載部署和管理資源。 他們必須連絡**服務管理員**以要求建立資源群組。
    ![工作負載擁有者要求建立資源群組 A](../../_images/governance-2-2.png)
2. **服務管理員**檢閱其要求，並且建立**資源群組 A**。此時，**工作負載擁有者 A** 仍然沒有執行任何動作的權限。
    ![服務管理員建立資源群組 A](../../_images/governance-2-3.png)
3. **服務管理員**將**工作負載擁有者 A** 新增至**資源群組 A**，並指派[內建參與者角色](/azure/role-based-access-control/built-in-roles#contributor)。 參與者角色授與**資源群組 A** 的所有權限，管理存取權限除外。
    ![服務管理員將工作負載擁有者新增至資源群組 A](../../_images/governance-2-4.png)
4. 假設**工作負載擁有者 A** 具有需求，需要小組成員配對在工作負載容量規劃時，檢視 CPU 和網路流量監視資料。 因為**工作負載擁有者 A** 獲指派參與者角色，他們沒有權限可以將使用者新增至**資源群組 A**。他們必須將此要求傳送給**服務管理員**。
    ![工作負載擁有者要求將工作負載參與者新增至資源群組](../../_images/governance-2-5.png)
5. **服務管理員**檢閱要求，並且將兩個**工作負載參與者**使用者新增至**資源群組 A**。這兩個使用者都不需要管理資源的權限，因此指派[內建讀者角色](/azure/role-based-access-control/built-in-roles#contributor)給他們。
    ![服務管理員將工作負載參與者新增至資源群組 A](../../_images/governance-2-6.png)
6. 接下來，**工作負載擁有者 B** 也需要資源群組包含其工作負載的資源。 如同**工作負載擁有者 A**，**工作負載擁有者 B** 最初沒有在訂用帳戶範圍採取任何動作的權限，因此他們必須將要求傳送給**服務管理員**。
    ![工作負載擁有者 B 要求建立資源群組 B](../../_images/governance-2-7.png)
7. **服務管理員**檢閱要求，並且建立**資源群組 B**。![服務管理員建立資源群組 B](../../_images/governance-2-8.png)
8. 然後**服務管理員**將**工作負載擁有者 B** 新增至**資源群組 B**，並指派[內建參與者角色](/azure/role-based-access-control/built-in-roles#contributor)。
    ![服務管理員將工作負載擁有者 B 新增至資源群組 B](../../_images/governance-2-9.png)

此時，每個工作負載擁有者會在本身的資源群組中隔離。 工作負載擁有者或其小組成員都沒有任何其他資源群組中資源的管理存取權。

![具有資源群組 A 和 B 的訂用帳戶](../../_images/governance-2-10.png)
*圖 4.具有兩個工作負載擁有者與其本身資源群組隔離的訂用帳戶。*

此模型是最低權限模型，每個使用者在正確的資源管理範圍獲指派正確的權限。

不過，請考量到此範例中的每項工作都是由**服務管理員**執行。 雖然這是簡單的範例而且可能不會造成問題，但是因為只有兩個工作負載擁有者，所以不難想像對於大型組織會造成的問題類型。 例如，**服務管理員**成為瓶頸，具有導致延遲的大量要求待處理項目。

讓我們看看第二個範例，其中減少了由**服務管理員**執行的工作數目。

1. 在此模型中，**工作負載擁有者 A** 在訂用帳戶範圍獲指派[內建擁有者角色][rbac-built-in-owner]，讓他們可以建立自己的資源群組：**資源群組 A**。![服務管理員將工作負載擁有者 A 新增至訂用帳戶](../../_images/governance-2-11.png)
2. 當**資源群組 A** 建立時，**工作負載擁有者 A** 預設會新增，並且從訂用帳戶範圍繼承[內建擁有者][rbac-built-in-owner]角色。
    ![工作負載擁有者 A 建立資源群組 A](../../_images/governance-2-12.png)
3. [內建擁有者角色][rbac-built-in-owner]授與**工作負載擁有者 A** 管理資源群組存取權的權限。 **工作負載擁有者 A** 新增兩個**工作負載參與者**，並指派[內建讀者角色][rbac-built-in-owner]給每個參與者。
    ![工作負載擁有者 A 新增工作負載參與者](../../_images/governance-2-13.png)
4. **服務管理員**現在將**工作負載擁有者 B** 新增至具有內建擁有者角色的訂用帳戶。
    ![服務管理員將工作負載擁有者 B 新增至訂用帳戶](../../_images/governance-2-14.png)
5. **工作負載擁有者 B** 建立**資源群組 B**，預設會新增。 同樣地，**工作負載擁有者 B** 會從訂用帳戶範圍繼承內建擁有者角色。
    ![工作負載擁有者 B 建立資源群組 B](../../_images/governance-2-15.png)

請注意，在此模型中，**服務管理員**執行的動作比在第一個範例中還要少，原因是將管理存取權委派給個別的工作負載擁有者。

![具有資源群組 A 和 B 的訂用帳戶](../../_images/governance-2-16.png)
*圖 5.訂用帳戶，具有服務管理員和兩個工作負載擁有者，都獲指派內建擁有者角色。*

不過，因為**工作負載擁有者 A** 和**工作負載擁有者 B** 都在訂用帳戶範圍獲指派內建擁有者角色，他們各自都繼承彼此資源群組的內建擁有者角色。 這表示他們不只有其他資源的完整存取權，他們還可以將管理存取權委派給彼此的資源群組。 例如，**工作負載擁有者 B** 有權可以將任何其他使用者新增至**資源群組 A**，並且可以將任何角色指派給它們，包括內建擁有者角色。

如果將每個範例與需求相互比較，您會看到這兩個範例都支援在訂用帳戶範圍的單一信任使用者，有權可以將資源存取權限授與兩個工作負載擁有者。 這兩個工作負載擁有者預設都沒有資源管理存取權，需要**服務管理員**明確地將權限指派給他們。 不過，只有第一個範例支援與每個工作負載相關聯的資源要彼此隔離的需求，如此才沒有任何工作負載擁有者具有任何其他工作負載的資源存取權。

## <a name="resource-management-model"></a>資源管理模型

既然您已經設計最低權限的權限模型，讓我們繼續查看這些治理模型的一些實際應用。 請回想需求，您必須支援下列三種環境：

1. **共用基礎結構：** 所有工作負載共用的單一群組資源。 這些資源是網路閘道、防火牆和安全性服務等等。
2. **開發：** 代表多個非生產就緒工作負載的多個資源群組。 這些資源用於概念證明、測試及其他開發人員活動。 這些資源可能會有比較寬鬆的治理模型，以便增加開發人員的靈活度。
3. **生產：** 代表多個生產工作負載的多個資源群組。 這些資源是用來裝載私人和公用對向的應用程式成品。 這些資源通常有最嚴謹的治理和安全性模型，以防止未經授權存取資源、應用程式程式碼和資料。

針對這三種環境，需要依**工作負載擁有者**、**環境**或兩者來追蹤成本資料。 也就是說，您想要知道**共用基礎結構**的進行中成本，**開發**和**生產**環境中個人所產生的成本，最後是**開發**和**生產**的整體成本。

您已了解資源分成兩個層級：**訂用帳戶**和**資源群組**。 因此，第一個決策是如何依據**訂用帳戶**來組織環境。 只有兩種可能性：單一訂用帳戶或多個訂用帳戶。

在您查看每個模型的範例之前，讓我們檢閱 Azure 中訂用帳戶的管理結構。

請回想需求，您在組織中有個人負責訂用帳戶，這個使用者擁有 Azure AD 租用戶中的**訂用帳戶擁有者**帳戶。 不過，此帳戶沒有建立訂用帳戶的權限。 只有 **Azure 帳戶擁有者**有權執行這個操作：

![Azure 帳戶擁有者建立訂用帳戶](../../_images/governance-3-0b.png)
*圖 6。Azure 帳戶擁有者建立訂用帳戶。*

一旦建立訂用帳戶，**Azure 帳戶擁有者**可以將**訂用帳戶擁有者**帳戶新增至具有**擁有者**角色的訂用帳戶：

![Azure 帳戶擁有者將訂用帳戶擁有者使用者帳戶新增至具有擁有者角色的訂用帳戶。](../../_images/governance-3-0c.png)
*圖 7。Azure 帳戶擁有者將**訂用帳戶擁有者**使用者帳戶新增至具有**擁有者**角色的訂用帳戶。*

**訂用帳戶擁有者**現在可以建立**資源群組**，並且委派資源存取權管理。

首先讓我們看看使用單一訂用帳戶的範例資源管理模型。 第一個決策是如何將資源群組對齊至三個環境。 您有兩個選擇：

1. 將每個環境對齊至單一資源群組。 所有共用基礎結構資源會部署到單一**共用基礎結構**資源群組。 與開發工作負載相關聯的所有資源都會部署到單一**開發**資源群組。 與生產工作負載相關聯的所有資源都會都部署到**生產**環境的單一**生產**資源群組。
2. 建立每個工作負載的個別資源群組，使用命名慣例和標記來對齊資源群組與三個環境。

讓我們從評估第一個選項開始。 您將會使用我們在上一節中討論的權限模型，以及單一訂用帳戶服務管理員，他會建立資源群組並且將使用者新增至其中，具有內建**參與者**或**讀者**角色。

1. 所部署的第一個資源群組代表**共用基礎結構**環境。 **訂用帳戶擁有者**會為名為 `netops-shared-rg` 的共用基礎結構資源建立資源群組。
    ![](../../_images/governance-3-0d.png)
2. **訂用帳戶擁有者**會將**網路作業使用者**帳戶新增至資源群組，並且指派**參與者**角色。
    ![](../../_images/governance-3-0e.png)
3. **網路作業使用者**會建立 [VPN 閘道](/azure/vpn-gateway/vpn-gateway-about-vpngateways)，並且設定它來連線到內部部署 VPN 設備。 **網路作業**使用者也會將一組[標記](/azure/azure-resource-manager/resource-group-using-tags)套用至每個資源：environment:shared 和 managedBy:netOps。 當**訂用帳戶服務管理員**匯出成本報告時，成本會與每個標記對齊。 這可讓**訂用帳戶服務管理員**使用 environment 標記和 managedBy 標記來樞紐成本。 請注意圖表右上方的**資源限制**計數器。 每個 Azure 訂用帳戶具有[服務限制](/azure/azure-subscription-service-limits)，為了協助您了解這些限制的影響，您將遵循每個訂用帳戶的虛擬網路限制。 每個訂用帳戶的限制為 1000 個虛擬網路，在部署第一個虛擬網路之後，目前有 999 個可用。
    ![](../../_images/governance-3-1.png)
4. 部署了另外兩個資源群組。 第一個名為 `prod-rg`。 此資源群組會與生產環境對齊。 第二個資源群組的名稱是 `dev-rg`，會與開發環境對齊。 與生產工作負載相關聯的所有資源都會部署至生產環境，與開發工作負載相關聯的所有資源都會部署至開發環境。 在此範例中，您只會將兩個工作負載部署到這兩個環境，這樣我們就不會遇到任何 Azure 訂用帳戶服務限制。 不過，請考慮每個資源群組具有 800 個資源的限制。 如果您繼續向每個資源群組加入工作負載，最終會達到此限制。
    ![](../../_images/governance-3-2.png)
5. 第一個**工作負載擁有者**會將要求傳送給**訂用帳戶服務管理員**，並且新增至每個開發和生產環境中具有**參與者**角色的資源群組。 如同您先前所了解的，**參與者**角色允許使用者執行除了指派角色給其他使用者以外的任何作業。 第一個**工作負載擁有者**現在可以建立與其工作負載相關聯的資源。
    ![](../../_images/governance-3-3.png)
6. 第一個**工作負載擁有者**會在其中包含一組虛擬機器的兩個資源群組中，建立虛擬網路。 第一個**工作負載擁有者**會將 environment 和 managedBy 標記套用至所有資源。 請注意，Azure 服務限制計數器現在剩餘 997 個虛擬網路。
    ![](../../_images/governance-3-4.png)
7. 每個虛擬網路在建立時，並沒有與內部部署的連線。 在這種類型的架構中，每個虛擬網路必須對等互連至**共用基礎結構**環境中的 hub-vnet。 虛擬網路對等互連會建立兩個不同虛擬網路之間的連線，並且允許網路流量在兩者之間傳送。 請注意，虛擬網路對等互連原本並非可轉移。 必須在已連線的兩個虛擬網路其中一個指定對等互連，且只有在其中一個虛擬網路指定對等互連時，連線才會完成。 為了說明其效果，第一個**工作負載擁有者**會指定 **prod-vnet** 與 **hub-vnet** 之間的對等互連。 隨即建立第一個對等互連，但是沒有流量，因為從 **hub-vnet** 到 **prod-vnet** 的互補對等互連尚未指定。 第一個**工作負載擁有者**會連絡**網路作業**使用者，並且要求這個互補對等互連連線。
    ![](../../_images/governance-3-5.png)
8. **網路作業**使用者檢閱要求、核准，然後在 **hub-vnet** 的設定中指定對等互連。 對等互連連線現在已完成，網路流量會在兩個虛擬網路之間流動。
    ![](../../_images/governance-3-6.png)
9. 現在，第二個**工作負載擁有者**會將要求傳送給**訂用帳戶服務管理員**，並且新增至現有**生產**和**開發**環境中具有**參與者**角色的資源群組。 在每個資源群組中，第二個**工作負載擁有者**具有與第一個**工作負載擁有者**相同的所有資源權限。
    ![](../../_images/governance-3-7.png)
10. 第二個**工作負載擁有者**會在 **prod-vnet** 虛擬網路中建立子網路，然後新增兩部虛擬機器。 第二個**工作負載擁有者**會將 environment 和 managedBy 標記套用至每個資源。
    ![](../../_images/governance-3-8.png)

這個範例資源管理模型可讓我們在三個必要的環境中管理資源。 共用基礎結構資源會受到保護，因為訂用帳戶中只有單一使用者有權可以存取這些資源。 每個工作負載擁有者能夠使用共用基礎結構資源，而不需要自己具備實際共用資源的任何權限。 不過，此管理模組不符對於工作負載隔離的需求，兩個**工作負載擁有者**都能夠存取其他工作負載的資源。

此模型還有一個可能無法立竿見影的重要考量。 在範例中，它是 **app1 工作負載擁有者**，會要求與 **hub-vnet** 的網路對等互連連線，以提供與內部部署的連線。 **網路作業**使用者會根據使用該工作負載部署的資源，評估該要求。 當**訂用帳戶擁有者**新增具有**參與者**角色的 **app2 工作負載擁有者**時，該使用者擁有 **prod-rg** 資源群組中所有資源的管理存取權限。

![](../../_images/governance-3-10.png)

這表示 **app2 工作負載擁有者**有權可以使用 **prod-vnet** 虛擬網路中的虛擬機器，來部署他們自己的子網路。 根據預設，這些虛擬機器現在具有內部部署網路的存取權。 **網路作業**使用者並不知道這些機器，且未核准它們與內部部署的連線。

接下來，讓我們看看不同環境和工作負載中，具有多個資源群組的單一訂用帳戶。 請注意，在上一個範例中，因為每個環境的資源是在相同的資源群組中，所以它們很容易識別。 既然您不再有該群組，您必須依賴資源群組命名慣例來提供該功能。

1. **共用基礎結構**資源在此模型中仍然具有個別資源群組，所以維持不變。 每個工作負載需要兩個資源群組，分別適用於**開發**和**生產**環境。 針對第一個工作負載，**訂用帳戶擁有者**會建立兩個資源群組。 第一個資源群組名稱是 **app1-prod-rg**，第二個資源群組名稱是 **app1-dev-rg**。 如前所述，此命名慣例會識別與第一個工作負載相關聯的資源，**app1**，以及**開發**或**生產**環境。 同樣地，*訂用帳戶*擁有者會將 **app1 工作負載擁有者**新增至具有**參與者**角色的資源群組。
    ![](../../_images/governance-3-12.png)
2. 類似於第一個範例，**app1 工作負載擁有者**會將名稱為 **app1-prod-vnet** 的虛擬網路部署至**生產**環境，將另一個名稱為 **app1-dev-vnet** 的虛擬網路部署至**開發**環境。 同樣地，**app1 工作負載擁有者**會將要求傳送給**網路作業**使用者，以建立對等互連連線。 請注意，**app1 工作負載擁有者**會新增與第一個範例相同的標記，限制計數器遞減為在訂用帳戶中剩餘 997 個虛擬網路。
    ![](../../_images/governance-3-13.png)
3. **訂用帳戶擁有者**現在會為 **app2 工作負載擁有者**建立兩個資源群組。 遵循與 **app1 工作負載擁有者**相同的慣例，資源群組的名稱是 **app2-prod-rg** 和 **app2-dev-rg**。 **訂用帳戶擁有者**會將 **app2 工作負載擁有者**新增至具有**參與者**角色的每個資源群組。
    ![](../../_images/governance-3-14.png)
4. *App2 工作負載擁有者*會將虛擬網路和虛擬機器部署到具有相同命名慣例的資源群組。 標記隨即新增，限制計數器遞減為在訂用帳戶中剩餘 995 個虛擬網路。
    ![](../../_images/governance-3-15.png)
5. *App2 工作負載擁有者*會將要求傳送給*網路作業*使用者，以對等互連 app2-prod-vnet 與 hub-vnet。 *網路作業*使用者會建立對等互連連線。
    ![](../../_images/governance-3-16.png)

產生的管理模型類似於第一個範例，有幾項主要差異：

* 兩個工作負載是依據工作負載和環境來隔離。
* 此模型比起第一個範例模型還需要兩個虛擬網路。 雖然因為只有兩個工作負載，所以這不是重要的差異，但是理論上，此模型的工作負載數目限制為 24 個。
* 資源不再針對每個環境，群組在單一資源群組中。 要群組資源必須先了解針對每個環境使用的命名慣例。
* 每個對等互連虛擬網路連線是由網路作業使用者檢閱及核准。

現在讓我們看看使用多個訂用帳戶的資源管理模型。 在此模型中，您會將三種環境對齊至個別的訂用帳戶：**共用服務**訂用帳戶、**生產**訂用帳戶，最後是**開發**訂用帳戶。 此模型的考量類似於使用單一訂用帳戶的模型，您在其中必須決定如何將資源群組對齊至工作負載。 已經判斷為每個工作負載建立資源群組可以滿足工作負載隔離需求，因此您在此範例中會緊緊遵循該模型。

1. 在此模型中，有三個訂用帳戶：共用基礎結構、生產和開發。 三個訂用帳戶都需要訂用帳戶擁有者，在範例中，您會為所有三個訂用帳戶使用相同的使用者帳戶。 共用基礎結構資源的管理方式類似於上述前兩個範例，第一個工作負載是與生產環境中的 app1-rg，以及開發環境中相同名稱的資源群組相關聯。 app1 工作負載擁有者會新增到具有參與者角色的每個資源群組。
    ![](../../_images/governance-3-17.png)
2. 如同先前的範例，app1 工作負載擁有者會建立資源，並且要求與共用基礎結構虛擬網路的對等互連連線。 App1 工作負載擁有者只會新增 managedBy 標記，因為不再需要 environment 標記。 也就是說，每個環境的資源現在會分組在同一個訂用帳戶，environment 標記是多餘的。 限制計數器會遞減為剩餘 999 個虛擬網路。
    ![](../../_images/governance-3-18.png)
3. 最後，訂用帳戶擁有者針對第二個工作負載重複此程序，在 *參與者角色中新增具有 app2 工作負載擁有者的資源群組。 每個環境訂用帳戶的限制計數器會遞減為剩餘 998 個虛擬網路。

此管理模型具有上述第二個範例的優點。 不過，主要差異是，因為它們是分散在兩個訂用帳戶，所以限制比較不成問題。 缺點是標記追蹤的資料成本，必須跨所有三個訂用帳戶進行彙總。

因此，您可以根據您的需求優先順序，選取這兩個範例資源管理模型其中之一。 如果您預期貴組織不會達到單一訂用帳戶的服務限制，則您可以使用單一訂用帳戶與多個資源群組。 相反地，如果貴組織預期有許多工作負載，每個環境有多個訂用帳戶可能比較理想。

## <a name="implementing-the-resource-management-model"></a>實作資源管理模型

您已了解用來管理 Azure 資源存取權的數個不同模型。 現在您會逐步進行所需的步驟，使用一個訂用帳戶為設計指南的每個**共用基礎結構**、**生產**和**開發**環境，實作資源管理模型。 針對全部三個環境，您會有一個**訂用帳戶擁有者**。 每個工作負載會在**資源群組**中隔離，具有新增了**參與者**角色的**工作負載擁有者**。

> [!NOTE]
> 請參閱[了解 Azure 中的資源存取][understand-resource-access-in-azure]以深入了解 Azure 帳戶和訂用帳戶之間的關聯性。

請遵循下列步驟：

1. 如果貴組織還沒有帳戶的話，請建立 [Azure 帳戶](/azure/active-directory/sign-up-organization)。 註冊 Azure 帳戶的人員會成為 Azure 帳戶管理員，而且貴組織的領導必須選取個人來承擔這個角色。 這個個人將負責：
    - 建立訂用帳戶，並且
    - 建立和管理 [Azure Active Directory (AD)](/azure/active-directory/active-directory-whatis) 租用戶，在其中儲存這些訂用帳戶的使用者身分識別。
2. 貴組織的領導小組決定哪些人負責：
    - 使用者身分識別管理，[Azure AD 租用戶](/azure/active-directory/develop/active-directory-howto-tenant)預設會在貴組織的 Azure 帳戶建立時建立，而且帳戶管理員預設會新增為 [Azure AD 全域管理員](/azure/active-directory/active-directory-assign-admin-roles-azure-portal#details-about-the-global-administrator-role)。 貴組織可以選擇另一個使用者來管理使用者身分識別，方法是[將 Azure AD 全域管理員角色指派給該使用者](/azure/active-directory/active-directory-users-assign-role-azure-portal)。
    - 訂用帳戶，這表示這些使用者：
        - 管理與該訂用帳戶中資源使用量相關聯的成本，
        - 實作和維護資源存取權的最小權限模型，以及
        - 持續追蹤服務限制。
    - 共用基礎結構服務 (如果貴組織決定要使用此模型)，這表示此使用者負責：
        - 內部部署至 Azure 網路連線，以及
        - 透過虛擬網路對等互連，在 Azure 內網路連線的擁有權。
    - 工作負載擁有者。
3. Azure AD 全域管理員會為以下人員[建立新的使用者帳戶](/azure/active-directory/add-users-azure-active-directory)：
    - 將成為與每個環境相關聯訂用帳戶**訂用帳戶擁有者**的人員。 請注意，只有當訂用帳戶**服務管理員**不負責管理每個訂用帳戶/環境的資源存取權時，才需要這麼做。
    - 將成為**網路作業使用者**的人員，以及
    - 身為**工作負載擁有者**的人員。
4. Azure 帳戶管理員會使用 [Azure 帳戶入口網站](https://account.azure.com)，建立下列三個訂用帳戶：
    - 適用於**共用基礎結構**環境的訂用帳戶，
    - 適用於**生產**環境的訂用帳戶，以及
    - 適用於**開發**環境的訂用帳戶。
5. Azure 帳戶管理員會[將訂用帳戶服務擁有者新增至每個訂用帳戶](/azure/billing/billing-add-change-azure-subscription-administrator#add-an-rbac-owner-admin-for-a-subscription-in-azure-portal)。
6. 建立**工作負載擁有者**的核准程序，以要求建立資源群組。 核准程序的實作方式有許多種，例如透過電子郵件，或者您可以使用如 [SharePoint 工作流程](https://support.office.com/article/introduction-to-sharepoint-workflow-07982276-54e8-4e17-8699-5056eff4d9e3)的處理程序管理工具。 核准程序可以依照下列步驟：
    - **工作負載擁有者**為**開發**環境、**生產**環境或兩者中的必要 Azure 資源準備用料表，並且將它提交給**訂用帳戶擁有者**。
    - **訂用帳戶擁有者**檢閱用料表並驗證要求的資源，以確保要求的資源適合其規劃使用，例如，檢查要求的[虛擬機器大小](/azure/virtual-machines/windows/sizes)正確無誤。
    - 如果要求未獲得核准，**工作負載擁有者**會收到通知。 如果要求通過核准，**訂用帳戶擁有者**會遵循貴組織的[命名慣例](/azure/architecture/best-practices/naming-conventions)來[建立要求的資源群組](/azure/azure-resource-manager/resource-group-portal#manage-resource-groups)，[新增**工作負載擁有者**](/azure/role-based-access-control/role-assignments-portal#add-access)，其具有[**參與者**角色](/azure/role-based-access-control/built-in-roles#contributor)，並且將通知傳送給已建立資源群組的**工作負載擁有者**。
7. 為工作負載擁有者建立核准程序，以要求來自共用基礎結構擁有者的虛擬網路對等互連連線。 如同上一個步驟，這個核准程序可以使用電子郵件或處理程序管理工具來實作。

既然您已實作治理模型，您可以部署共用基礎結構服務。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [了解如何部署基本基礎結構](../../infrastructure/basic-workload.md)

<!-- links -->
[understand-resource-access-in-azure]: /azure/role-based-access-control/rbac-and-directory-admin-roles

[rbac-built-in-owner]: /azure/role-based-access-control/built-in-roles#owner
[rbac-built-in-roles]: /azure/role-based-access-control/built-in-roles
