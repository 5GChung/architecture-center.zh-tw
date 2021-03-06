---
title: CAF：合理化的 5R 策略
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
description: 檢閱合理化數位資產的可用選項。
author: BrianBlanchard
ms.date: 12/10/2018
ms.topic: guide
ms.openlocfilehash: ee196487e6f59b1e1b3c63bab9496cbbf805affd
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2019
ms.locfileid: "55897468"
---
# <a name="the-5-rs-of-rationalization"></a>合理化的 5R 策略

雲端合理化是評估資產以判斷何種方法最適合用來移轉或現代化雲端中各個資產的程序。 確定關於合理化程序的詳細資訊，請參閱[什麼是數位資產？](overview.md)

此處所列的「合理化的 5R 策略」說明最常用的合理化選項。

## <a name="rehost"></a>重新裝載

重新裝載工作也稱為「隨即移轉」，可在盡可能不變更整體架構的情況下，將目前的資產移至選擇的雲端提供者。

常見的動因可能包括：

* 降低 CapEx
* 釋放資料中心空間
* 快速實現雲端 ROI

量化分析因素：

* VM 大小 (CPU、記憶體、儲存體)
* 相依性 (篩選網路)
* 資產相容性

定性分析因素：

* 對變更的容忍度
* 商業優先順序
* 重大商業事件
* 程序相依性

## <a name="refactor"></a>重構

平台即服務 (PaaS) 選項可減少許多應用程式的相關作業成本。 稍微重構應用程式以符合 PaaS 模型，是審慎的做法。

重構也是指重構程式碼，讓應用程式能夠因應新商機的應用程式開發程序。

常見的動因可能包括：

* 更快速簡易的更新
* 程式碼可攜性
* 更高的雲端效率 (資源、速度、成本)

量化分析因素：

* 應用程式資產大小 (CPU、記憶體、儲存體)
* 相依性 (篩選網路)
* 使用者流量 (頁面檢視、頁面瀏覽時間、載入時間)
* 開發平台 (語言、資料平台、中介層服務)

定性分析因素：

* 持續性的商業投資
* 負載平衡選項/時間軸
* 商務程序相依性

## <a name="rearchitect"></a>重新架構

某些過時應用程式與雲端提供者無法相容，因為其架構決策是在應用程式建置時所制定的。 在這種情況下，應用程式可能需要先重新架構，再進行轉換。

在其他情況下，與雲端相容、但不屬於雲端原生效益的應用程式，可藉由將解決方案重新架構為雲端原生應用程式，而產生成本效益和操作效率。

常見的動因可能包括：

* 應用程式的調整能力與靈活性
* 更容易採用新的雲端功能
* 混用技術堆疊

量化分析因素：

* 應用程式資產大小 (CPU、記憶體、儲存體)
* 相依性 (篩選網路)
* 使用者流量 (頁面檢視、頁面瀏覽時間、載入時間)
* 開發平台 (語言、資料平台、中介層服務)

定性分析因素：

* 不斷成長的商業投資
* 營運成本
* 潛在的意見反應迴圈與 DevOps 投資

## <a name="rebuild"></a>重建

在某些情況下，可能會因為推動應用程式所需克服的差異過大大，而無法進一步投資。 尤其是在用來符合商務需求的應用程式目前不受支援，或是與目前執行商務程序的方式不一致時，更是如此。 在此情況下，將會依據[雲端原生](https://azure.microsoft.com/overview/cloudnative/)方法建立新的程式碼基底。

常見的動因可能包括：

* 加速推動創新
* 快速建置應用程式
* 降低營運成本

量化分析因素：

* 應用程式資產大小 (CPU、記憶體、儲存體)
* 相依性 (篩選網路)
* 使用者流量 (頁面檢視、頁面瀏覽時間、載入時間)
* 開發平台 (語言、資料平台、中介層服務)

定性分析因素：

* 下降的使用者滿意度
* 商務程序受限於功能
* 潛在的成本、體驗或營收提升

## <a name="replace"></a>Replace

解決方案通常會使用時當前最好的技術和方法來實作。 在某些情況下，軟體即服務 (SaaS) 應用程式足以提供裝載的應用程式所需的所有功能。 在這類情況下，可將工作負載排入未來替換的時程中，以將其從轉換工作中有效移除。

常見的動因可能包括：

* 標準化業界最佳做法
* 加速採用商務程序導向方法
* 將開發投資重新配置於可創造競爭差異或優勢的應用程式上

量化分析因素：

* 降低整體營運成本
* VM 大小 (CPU、記憶體、儲存體)
* 相依性 (篩選網路)
* 即將淘汰的資產

定性分析因素：

* 目前架構與 SaaS 解決方案的成本效益分析
* 商務程序對應
* 資料結構描述
* 自訂或自動化程序

## <a name="next-steps"></a>後續步驟

整體而言，這些「合理化的 5R 策略」可套用至數位資產，以制定關於未來各種應用程式型態的合理化決策。

> [!div class="nextstepaction"]
> [什麼是數位資產？](overview.md)
