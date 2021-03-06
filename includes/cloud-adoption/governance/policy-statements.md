<!-- TEMPLATE FILE - DO NOT ADD METADATA -->

## <a name="policy-statements"></a>Policy statements

下列原則聲明將建立要降低確知風險所需符合的要求。 這些原則將定義治理 MVP 的功能需求。 每項原則都會反映在治理 MVP 的實作中。

部署加速：

- 所有資產都必須根據已定義的分組和標記策略進行分組和標記。
- 所有資產都必須使用經核准的部署模型。
- 在建立了雲端提供者的治理基礎之後，任何部署工具必須與治理小組所定義的工具相容。

身分識別基準：

- 部署至雲端的所有資產，均應使用經目前的治理原則核准的身分識別與角色來控管。
- 內部部署 Active Directory 基礎結構中已提高權限的所有群組，均應對應至經核准的 RBAC 角色。

安全性基準：

- 部署至雲端的任何資產，都必須有已核准的資料分類。
- 在可核准並實作足夠的安全性和治理需求之前，任何資產一經識別有受保護的資料層級，即不可部署至雲端。
- 在可驗證並控管最低網路安全需求之前，雲端環境將被視為非管制區域，且應符合與其他資料中心或內部網路類似的連線需求。

成本管理：

- 為了進行追蹤，所有資產皆必須指派給其中一項核心業務功能內的應用程式擁有者。
- 出現成本考量時，將與財務小組一起建立額外的治理需求。

資源一致性：

- 由於在此階段並未部署沒有任務關鍵性工作負載，因此無須控管 SLA、效能或 BCDR 需求。
- 在部署任務關鍵性工作負載時，將會隨 IT 營運一起建立額外的治理需求。

## <a name="processes"></a>處理序

目前對於上述治理原則的監視和施行，並未分配任何預算。 因此，雲端治理小組會以一些暫行措施來監視原則聲明是否受遵行。

- **教育訓練**：雲端治理小組會投注時間，針對支援這些原則的治理旅程為雲端採用小組提供相關教育訓練。
- **部署**檢閱：在部署任何資產之前，雲端治理小組會檢閱雲端採用團隊的治理旅程。
