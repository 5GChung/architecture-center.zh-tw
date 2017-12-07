---
title: "選擇解決方案以整合內部部署 Active Directory 與 Azure。"
description: "比較用於整合內部部署 Active Directory 與 Azure 的參考架構。"
ms.date: 04/06/2017
ms.openlocfilehash: 413a5463d90547197c4b6834d353b4ecf61483ee
ms.sourcegitcommit: b0482d49aab0526be386837702e7724c61232c60
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2017
---
# <a name="choose-a-solution-for-integrating-on-premises-active-directory-with-azure"></a><span data-ttu-id="558a7-103">選擇解決方案以整合內部部署 Active Directory 與 Azure</span><span class="sxs-lookup"><span data-stu-id="558a7-103">Choose a solution for integrating on-premises Active Directory with Azure</span></span>

<span data-ttu-id="558a7-104">本文會比較整合內部部署 Active Directory (AD) 環境與 Azure 網路的選項。</span><span class="sxs-lookup"><span data-stu-id="558a7-104">This article compares options for integrating your on-premises Active Directory (AD) environment with an Azure network.</span></span> <span data-ttu-id="558a7-105">我們會為每個選項提供一個參考架構和可部署的解決方案。</span><span class="sxs-lookup"><span data-stu-id="558a7-105">We provide a reference architecture and a deployable solution for each option.</span></span>

<span data-ttu-id="558a7-106">許多組織使用 [Active Directory Domain Services (AD DS)][active-directory-domain-services] 來驗證與使用者、電腦、應用程式或包含在安全性邊界內其他資源相關聯的身分識別。</span><span class="sxs-lookup"><span data-stu-id="558a7-106">Many organizations use [Active Directory Domain Services (AD DS)][active-directory-domain-services] to authenticate identities associated with users, computers, applications, or other resources that are included in a security boundary.</span></span> <span data-ttu-id="558a7-107">目錄和身分識別服務通常裝載在內部部署，但如果您的應用程式部分裝載在內部部署且部分裝載在 Azure 中，將驗證要求從 Azure 傳回內部部署時，可能會有延遲。</span><span class="sxs-lookup"><span data-stu-id="558a7-107">Directory and identity services are typically hosted on-premises, but if your application is hosted partly on-premises and partly in Azure, there may be latency sending authentication requests from Azure back to on-premises.</span></span> <span data-ttu-id="558a7-108">在 Azure 中實作目錄和身分識別服務可以減少這種延遲。</span><span class="sxs-lookup"><span data-stu-id="558a7-108">Implementing directory and identity services in Azure can reduce this latency.</span></span>

<span data-ttu-id="558a7-109">Azure 會針對在 Azure 中實作目錄和身分識別服務，提供兩種解決方案：</span><span class="sxs-lookup"><span data-stu-id="558a7-109">Azure provides two solutions for implementing directory and identity services in Azure:</span></span> 

* <span data-ttu-id="558a7-110">使用 [Azure AD][azure-active-directory] 在雲端建立 Active Directory 網域，然後將其連線至您的內部部署 Active Directory 網域。</span><span class="sxs-lookup"><span data-stu-id="558a7-110">Use [Azure AD][azure-active-directory] to create an Active Directory domain in the cloud and connect it to your on-premises Active Directory domain.</span></span> <span data-ttu-id="558a7-111">[Azure AD Connect][azure-ad-connect] 會整合您的內部部署目錄與 Azure AD。</span><span class="sxs-lookup"><span data-stu-id="558a7-111">[Azure AD Connect][azure-ad-connect] integrates your on-premises directories with Azure AD.</span></span>

* <span data-ttu-id="558a7-112">在 Azure 中部署執行 AD DS 作為網域控制站的 VM，以便將現有的內部部署 Active Directory 基礎結構擴充至 Azure 中。</span><span class="sxs-lookup"><span data-stu-id="558a7-112">Extend your existing on-premises Active Directory infrastructure to Azure, by deploying a VM in Azure that runs AD DS as a domain controller.</span></span> <span data-ttu-id="558a7-113">當內部部署網路與 Azure 虛擬網路 (VNet) 透過 VPN 或 ExpressRoute 連線連接時，這個架構更為常見。</span><span class="sxs-lookup"><span data-stu-id="558a7-113">This architecture is more common when the on-premises network and the Azure virtual network (VNet) are connected by a VPN or ExpressRoute connection.</span></span> <span data-ttu-id="558a7-114">此架構可能會有數種變化：</span><span class="sxs-lookup"><span data-stu-id="558a7-114">Several variations of this architecture are possible:</span></span> 

    - <span data-ttu-id="558a7-115">在 Azure 中建立網域，並將其加入至您的內部部署 AD 樹系。</span><span class="sxs-lookup"><span data-stu-id="558a7-115">Create a domain in Azure and join it to your on-premises AD forest.</span></span>
    - <span data-ttu-id="558a7-116">在 Azure 中建立您內部部署樹系之網域所信任的另一個樹系。</span><span class="sxs-lookup"><span data-stu-id="558a7-116">Create a separate forest in Azure that is trusted by domains in your on-premises forest.</span></span>
    - <span data-ttu-id="558a7-117">將 Active Directory Federation Services (AD FS) 部署複寫至 Azure。</span><span class="sxs-lookup"><span data-stu-id="558a7-117">Replicate an Active Directory Federation Services (AD FS) deployment to Azure.</span></span> 

<span data-ttu-id="558a7-118">以下章節詳細說明其中各個選項。</span><span class="sxs-lookup"><span data-stu-id="558a7-118">The next sections describe each of these options in more detail.</span></span>

## <a name="integrate-your-on-premises-domains-with-azure-ad"></a><span data-ttu-id="558a7-119">整合內部部署網域與 Azure AD</span><span class="sxs-lookup"><span data-stu-id="558a7-119">Integrate your on-premises domains with Azure AD</span></span>

<span data-ttu-id="558a7-120">使用 Azure Active Directory (Azure AD) 在 Azure 中建立網域，並將其連結到內部部署 AD 網域。</span><span class="sxs-lookup"><span data-stu-id="558a7-120">Use Azure Active Directory (Azure AD) to create a domain in Azure and link it to an on-premises AD domain.</span></span> 

<span data-ttu-id="558a7-121">Azure AD 目錄不是內部部署目錄的擴充功能。</span><span class="sxs-lookup"><span data-stu-id="558a7-121">The Azure AD directory is not an extension of an on-premises directory.</span></span> <span data-ttu-id="558a7-122">而是包含相同物件和身分識別的複本。</span><span class="sxs-lookup"><span data-stu-id="558a7-122">Rather, it's a copy that contains the same objects and identities.</span></span> <span data-ttu-id="558a7-123">在內部部署對這些項目所做的變更會複製到 Azure AD，但在 Azure AD 中所做的變更不會複寫回內部部署網域。</span><span class="sxs-lookup"><span data-stu-id="558a7-123">Changes made to these items on-premises are copied to Azure AD, but changes made in Azure AD are not replicated back to the on-premises domain.</span></span>

<span data-ttu-id="558a7-124">您也可以使用 Azure AD，而不使用內部部署目錄。</span><span class="sxs-lookup"><span data-stu-id="558a7-124">You can also use Azure AD without using an on-premises directory.</span></span> <span data-ttu-id="558a7-125">在此情況下，Azure AD 會當作所有身分識別資訊的主要來源，而不包含從內部部署目錄複寫的資料。</span><span class="sxs-lookup"><span data-stu-id="558a7-125">In this case, Azure AD acts as the primary source of all identity information, rather than containing data replicated from an on-premises directory.</span></span>


<span data-ttu-id="558a7-126">**優點**</span><span class="sxs-lookup"><span data-stu-id="558a7-126">**Benefits**</span></span>

* <span data-ttu-id="558a7-127">您不需要維護雲端的 AD 基礎結構。</span><span class="sxs-lookup"><span data-stu-id="558a7-127">You don't need to maintain an AD infrastructure in the cloud.</span></span> <span data-ttu-id="558a7-128">Azure AD 會完全由 Microsoft 所管理和維護。</span><span class="sxs-lookup"><span data-stu-id="558a7-128">Azure AD is entirely managed and maintained by Microsoft.</span></span>
* <span data-ttu-id="558a7-129">Azure AD 會提供與內部部署相同的身分識別資訊。</span><span class="sxs-lookup"><span data-stu-id="558a7-129">Azure AD provides the same identity information that is available on-premises.</span></span>
* <span data-ttu-id="558a7-130">驗證可能會在 Azure 中進行，進而減少外部應用程式和使用者與內部部署網域連絡的需求。</span><span class="sxs-lookup"><span data-stu-id="558a7-130">Authentication can happen in Azure, reducing the need for external applications and users to contact the on-premises domain.</span></span>

<span data-ttu-id="558a7-131">**挑戰**</span><span class="sxs-lookup"><span data-stu-id="558a7-131">**Challenges**</span></span>

* <span data-ttu-id="558a7-132">識別服務受到使用者和群組的限制。</span><span class="sxs-lookup"><span data-stu-id="558a7-132">Identity services are limited to users and groups.</span></span> <span data-ttu-id="558a7-133">無法驗證服務與電腦帳戶。</span><span class="sxs-lookup"><span data-stu-id="558a7-133">There is no ability to authenticate service and computer accounts.</span></span>
* <span data-ttu-id="558a7-134">您必須設定與內部部署網域的連線，讓 Azure AD 目錄維持同步。</span><span class="sxs-lookup"><span data-stu-id="558a7-134">You must configure connectivity with your on-premises domain to keep the Azure AD directory synchronized.</span></span> 
* <span data-ttu-id="558a7-135">應用程式可能需要重寫，才能透過 Azure AD 啟用驗證。</span><span class="sxs-lookup"><span data-stu-id="558a7-135">Applications may need to be rewritten to enable authentication through Azure AD.</span></span>

<span data-ttu-id="558a7-136">**[閱讀更多...][aad]**</span><span class="sxs-lookup"><span data-stu-id="558a7-136">**[Read more...][aad]**</span></span>

## <a name="ad-ds-in-azure-joined-to-an-on-premises-forest"></a><span data-ttu-id="558a7-137">Azure 中的 AD DS 會加入內部部署樹系</span><span class="sxs-lookup"><span data-stu-id="558a7-137">AD DS in Azure joined to an on-premises forest</span></span>

<span data-ttu-id="558a7-138">將 AD 網域服務 (AD DS) 伺服器部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="558a7-138">Deploy AD Domain Services (AD DS) servers to Azure.</span></span> <span data-ttu-id="558a7-139">在 Azure 中建立網域，並將其加入至您的內部部署 AD 樹系。</span><span class="sxs-lookup"><span data-stu-id="558a7-139">Create a domain in Azure and join it to your on-premises AD forest.</span></span> 

<span data-ttu-id="558a7-140">如果您需要使用 Azure AD 目前尚未實作的 AD DS 功能，請考慮此選項。</span><span class="sxs-lookup"><span data-stu-id="558a7-140">Consider this option if you need to use AD DS features that are not currently implemented by Azure AD.</span></span> 

<span data-ttu-id="558a7-141">**優點**</span><span class="sxs-lookup"><span data-stu-id="558a7-141">**Benefits**</span></span>

* <span data-ttu-id="558a7-142">可存取與內部部署相同的身分識別資訊。</span><span class="sxs-lookup"><span data-stu-id="558a7-142">Provides access to the same identity information that is available on-premises.</span></span>
* <span data-ttu-id="558a7-143">您可以在內部部署和 Azure 中驗證使用者、服務和電腦帳戶。</span><span class="sxs-lookup"><span data-stu-id="558a7-143">You can authenticate user, service, and computer accounts on-premises and in Azure.</span></span>
* <span data-ttu-id="558a7-144">您不需要管理另一個 AD 樹系。</span><span class="sxs-lookup"><span data-stu-id="558a7-144">You don't need to manage a separate AD forest.</span></span> <span data-ttu-id="558a7-145">Azure 中的網域可以屬於內部部署樹系。</span><span class="sxs-lookup"><span data-stu-id="558a7-145">The domain in Azure can belong to the on-premises forest.</span></span>
* <span data-ttu-id="558a7-146">您可以將內部部署群組原則物件所定義的群組原則套用到 Azure 中的網域。</span><span class="sxs-lookup"><span data-stu-id="558a7-146">You can apply group policy defined by on-premises Group Policy Objects to the domain in Azure.</span></span>

<span data-ttu-id="558a7-147">**挑戰**</span><span class="sxs-lookup"><span data-stu-id="558a7-147">**Challenges**</span></span>

* <span data-ttu-id="558a7-148">您必須在雲端部署並管理您自己的 AD DS 伺服器和網域。</span><span class="sxs-lookup"><span data-stu-id="558a7-148">You must deploy and manage your own AD DS servers and domain in the cloud.</span></span>
* <span data-ttu-id="558a7-149">在雲端的網域伺服器與在內部部署執行的伺服器之間可能會有一些同步處理延遲。</span><span class="sxs-lookup"><span data-stu-id="558a7-149">There may be some synchronization latency between the domain servers in the cloud and the servers running on-premises.</span></span>

<span data-ttu-id="558a7-150">**[閱讀更多...][ad-ds]**</span><span class="sxs-lookup"><span data-stu-id="558a7-150">**[Read more...][ad-ds]**</span></span>

## <a name="ad-ds-in-azure-with-a-separate-forest"></a><span data-ttu-id="558a7-151">在 Azure 中具有不同樹系的 AD DS</span><span class="sxs-lookup"><span data-stu-id="558a7-151">AD DS in Azure with a separate forest</span></span>

<span data-ttu-id="558a7-152">將 AD Domain Services (AD DS) 伺服器部署到 Azure，但建立與內部部署樹系不同的另一個 Active Directory [樹系][ad-forest-defn]。</span><span class="sxs-lookup"><span data-stu-id="558a7-152">Deploy AD Domain Services (AD DS) servers to Azure, but create a separate Active Directory [forest][ad-forest-defn] that is separate from the on-premises forest.</span></span> <span data-ttu-id="558a7-153">此樹系受到內部部署樹系中的網域信任。</span><span class="sxs-lookup"><span data-stu-id="558a7-153">This forest is trusted by domains in your on-premises forest.</span></span>

<span data-ttu-id="558a7-154">這個架構的典型用途包括維持雲端中保留之物件和身分識別的安全性隔離，以及將個別網域從內部部署移轉到雲端。</span><span class="sxs-lookup"><span data-stu-id="558a7-154">Typical uses for this architecture include maintaining security separation for objects and identities held in the cloud, and migrating individual domains from on-premises to the cloud.</span></span>

<span data-ttu-id="558a7-155">**優點**</span><span class="sxs-lookup"><span data-stu-id="558a7-155">**Benefits**</span></span>

* <span data-ttu-id="558a7-156">您可以實作內部部署身分識別，並區隔僅 Azure 的身分識別。</span><span class="sxs-lookup"><span data-stu-id="558a7-156">You can implement on-premises identities and separate Azure-only identities.</span></span>
* <span data-ttu-id="558a7-157">您不需要從內部部署 AD 樹系複寫至 Azure。</span><span class="sxs-lookup"><span data-stu-id="558a7-157">You don't need to replicate from the on-premises AD forest to Azure.</span></span>

<span data-ttu-id="558a7-158">**挑戰**</span><span class="sxs-lookup"><span data-stu-id="558a7-158">**Challenges**</span></span>

* <span data-ttu-id="558a7-159">在 Azure 中為內部部署身分識別進行驗證時，需要到內部部署 AD 伺服器的額外網路躍點。</span><span class="sxs-lookup"><span data-stu-id="558a7-159">Authentication within Azure for on-premises identities requires extra network hops to the on-premises AD servers.</span></span>
* <span data-ttu-id="558a7-160">您必須在雲端部署自己的 AD DS 伺服器和樹系，並在樹系之間建立適當的信任關係。</span><span class="sxs-lookup"><span data-stu-id="558a7-160">You must deploy your own AD DS servers and forest in the cloud, and establish the appropriate trust relationships between forests.</span></span>

<span data-ttu-id="558a7-161">**[閱讀更多...][ad-ds-forest]**</span><span class="sxs-lookup"><span data-stu-id="558a7-161">**[Read more...][ad-ds-forest]**</span></span>

## <a name="extend-ad-fs-to-azure"></a><span data-ttu-id="558a7-162">將 AD FS 擴充至 Azure</span><span class="sxs-lookup"><span data-stu-id="558a7-162">Extend AD FS to Azure</span></span>

<span data-ttu-id="558a7-163">將 Active Directory Federation Services (AD FS) 部署複寫至 Azure，以便針對在 Azure 中執行的元件，執行同盟驗證和授權。</span><span class="sxs-lookup"><span data-stu-id="558a7-163">Replicate an Active Directory Federation Services (AD FS) deployment to Azure, to perform federated authentication and authorization for components running in Azure.</span></span> 

<span data-ttu-id="558a7-164">此架構的一般用途包括：</span><span class="sxs-lookup"><span data-stu-id="558a7-164">Typical uses for this architecture:</span></span>

* <span data-ttu-id="558a7-165">驗證和授權夥伴組織的使用者。</span><span class="sxs-lookup"><span data-stu-id="558a7-165">Authenticate and authorize users from partner organizations.</span></span>
* <span data-ttu-id="558a7-166">可讓使用者從組織防火牆外部執行的網頁瀏覽器進行驗證。</span><span class="sxs-lookup"><span data-stu-id="558a7-166">Allow users to authenticate from web browsers running outside of the organizational firewall.</span></span>
* <span data-ttu-id="558a7-167">可讓使用者從授權的外部裝置 (例如行動裝置) 連線。</span><span class="sxs-lookup"><span data-stu-id="558a7-167">Allow users to connect from authorized external devices such as mobile devices.</span></span> 

<span data-ttu-id="558a7-168">**優點**</span><span class="sxs-lookup"><span data-stu-id="558a7-168">**Benefits**</span></span>

* <span data-ttu-id="558a7-169">您可以運用宣告感知應用程式。</span><span class="sxs-lookup"><span data-stu-id="558a7-169">You can leverage claims-aware applications.</span></span>
* <span data-ttu-id="558a7-170">能夠信任要進行驗證的外部夥伴。</span><span class="sxs-lookup"><span data-stu-id="558a7-170">Provides the ability to trust external partners for authentication.</span></span>
* <span data-ttu-id="558a7-171">與多種驗證通訊協定相容。</span><span class="sxs-lookup"><span data-stu-id="558a7-171">Compatibility with large set of authentication protocols.</span></span>

<span data-ttu-id="558a7-172">**挑戰**</span><span class="sxs-lookup"><span data-stu-id="558a7-172">**Challenges**</span></span>

* <span data-ttu-id="558a7-173">您必須在 Azure 中部署您自己的 AD DS、AD FS 與 AD FS Web 應用程式 Proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="558a7-173">You must deploy your own AD DS, AD FS, and AD FS Web Application Proxy servers in Azure.</span></span>
* <span data-ttu-id="558a7-174">此架構的設定可能相當複雜。</span><span class="sxs-lookup"><span data-stu-id="558a7-174">This architecture can be complex to configure.</span></span>

<span data-ttu-id="558a7-175">**[閱讀更多...][adfs]**</span><span class="sxs-lookup"><span data-stu-id="558a7-175">**[Read more...][adfs]**</span></span>

<!-- links -->

[aad]: ./azure-ad.md
[ad-ds]: ./adds-extend-domain.md
[ad-ds-forest]: ./adds-forest.md
[ad-forest-defn]: https://msdn.microsoft.com/library/ms676906.aspx
[adfs]: ./adfs.md

[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[azure-active-directory]: /azure/active-directory-domain-services/active-directory-ds-overview
[azure-ad-connect]: /azure/active-directory/active-directory-aadconnect