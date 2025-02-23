---

copyright:

  years: 2018
lastupdated: "2019-01-31"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# 更新及調整
{: #update-scale}

將 {{site.data.keyword.cfee_full_notm}} 服務實例更新至最新版本，以取得最新的 CFEE 函數及修正程式。CFEE 的更新包含新版本的 Cloud Foundry 及 CFEE 支援服務（Kubernetes、Cloud Object Storage 或 Compose for PostgreSQL）。不過，並非每項 CFEE 更新都會包含新版本的 Cloud Foundry 及 CFEE 支援服務。

CFEE 版本更新會在包含 CFEE 元件的控制平面上以及 Cell 上進行。您也可以藉由新增或刪除應用程式 Cell，來調整 CFEE 實例的容量。

**附註：**在版本更新期間或者在新增或刪除 Cell 時，可能無法使用_概觀_、_資源用量_ 及_更新及調整_ 頁面中所顯示的部分度量值（例如，記憶體及 CPU）。

## 更新 CFEE 版本
{: #update}

使用者需要具有下列許可權，才能將 CFEE 實例更新至新版本：
   * CFEE 實例的_編輯者_ 角色或更高角色。
   * CFEE 佈建所在 Kubernetes 叢集的_操作員_ 角色或更高角色。

更新 CFEE 實例的 CFEE 版本是兩步驟處理程序。首先，請更新管理 CFEE 元件項目的控制平面。然後，更新用來管理應用程式的 Cell。

將 CFEE 更新至新版本時，適用下列規則及限制：
* 必須先更新控制平面。更新控制平面之後，即可更新 Cell。
* 應用程式 Cell 只能更新至控制平面的版本。亦即，控制平面的 CFEE 版本層次可以高於應用程式 Cell，但並非反之亦然。

若要更新 CFEE 實例的 CFEE 版本，請執行下列動作：
1. 移至 [{{site.data.keyword.Bluemix_notm}} 儀表板](https://cloud.ibm.com/dashboard/apps/)，然後開啟您要更新的 {{site.data.keyword.cfee_full_notm}}。
2. 在導覽窗格中，移至_作業_ 項目下的**更新與擴充**頁面。
3. （選用）在_控制平面_ 表格中，您可以展開該列，以查看控制平面中的工作者節點。
4. 按一下**更新**。
5. 在「更新_控制平面_」對話框中，選取其中一個可用的 CFEE 版本，然後按一下**更新**。更新大約需要 45 分鐘。版本說明會詳述所選取 CFEE 版本套件中內含的元件版本，以及指向說明該版本中所遞送內容的_新增功能_ 文件鏈結。
6. _節點狀態_ 直欄會顯示更新進度。更新完成之後，_版本_ 直欄會反映新的 CFEE 版本。
7. 完成控制平面 Cell 的更新之後，請尋找 _Cell_表格，然後按一下**更新**。
8. 在_更新控制平面_ 對話框中，選取 CFEE 版本，然後按一下*更新*。只可更新 Cell 的可用版本，因為 Cell 只能更新至控制平面的版本。單一更新動作會更新所有 Cell。
9. 循序更新 Cell。_節點狀態_ 直欄指出每個 Cell 的更新進度。更新 Cell 時，其新版本會反映在_版本_ 直欄中。

## 在版本更新期間中斷
{: #update-disruption}

更新 Cloud Foundry 控制平面並不會造成中斷。不過，將 Cloud Foundry Cell 更新至新的 CFEE 版本可能會中斷 CFEE 環境的作業。一次對一個 Cell 執行更新，因此，會在更新完成之後備份於 Cell 中執行的應用程式實例，而其他 Cell 在更新期間都是關閉的。不過，在部分情況下，於 CFEE 中執行的部分應用程式及服務可能會變成無法使用。例如，如果 CFEE 有兩個具有完整容量的 Cloud Foundry Cell，則會在版本更新期間關閉只有一個實例的應用程式，因為在更新該 Cell 時無法將應用程式實例移至另一個 Cell，因此，應用程式將無法使用。即使用三個 Cell 和每個應用程式三個實例，在版本更新期間的應用程式可用性仍可能會有暫時性的中斷。

在版本更新期間，CFEE _概觀_ 頁面中所顯示「Cell 節點」量規中所報告的「記憶體」及 CPU 度量值（由 Kubernetes 叢集所產生）與 Grafana 之_節點_ 儀表板中所顯示的相同度量值（由作業系統層次所產生）可能會不相符。

進行更新時的 CFEE 元件狀態將會反映在「性能檢查」頁面中。重新啟動大約需要 45 分鐘。

## 版本週期及支援原則
{: #version-cycle}

Cloud Foundry Enterprise Environment 服務通常會定期發行新版本。服務的版本遵循語意版本化格式 _**Major.Minor.Patch**_ (https://semver.org/)

_次要_ 版本會定期遞增（通常是每月）。_主要_ 版本也會遞增，但不常發生，通常是在發生重大變更時。更新至_主要_ 版本可能會導致升級期間發生部分中斷。_修補程式_ 會提供修正程式，並套用至最新可用版本。 

為了防止系統關閉，只容許將 CFEE 實例更新至比其現行版本高 2 個_主要_ 版本。以前一個範例為例，如果 CFEE 實例現行版本為 3.1.2，且目標版本為 7.0.0，則需要先將 CFEE 實例更新至 5.x.x 版，再更新至目標版本 7.0.0。

只有在最新版本中，才能定期提供新功能及現有功能的加強功能。重要修正程式是在只有最新 3 個_主要_ 版本（最新版本及前兩個版本）的最新版本中提供。例如，如果有 4.x.x、3.x.x 及 2.x.x 版可用，則不支援 1.x.x 版（亦即，修補程式將會與 1.x.x 版修正程式一起提供）。  

修補程式會在給定_主要_ 的最新可用版本上提供。例如，如果 CFEE 實例是執行 3.1.2 版，但 3.x.x _主要_ 中的最新可用版本是 3.3.4 版，則會在 3.3.x 版程式碼庫上提供任何修補程式，這表示將會在（新的）3.3.5 版中提供修補程式。具有 3.1.2 版的 CFEE 實例需要更新至 3.3.5 版，才能接收新的修補程式（亦即，僅更新至 3.3.4 版將無法解決修補程式中的已修正問題，而此修補程式僅在 3.3.5 版中提供）。

## 調整 CFEE 基礎架構
{: #scale}

使用者需要具有下列許可權，才能在 CFEE 實例中新增或移除 Cloud Foundry Cell：
* CFEE 實例的_管理者_ 角色或更高角色。
* CFEE 佈建所在 Kubernetes 叢集的_操作員_ 角色或更高角色。

若要在 CFEE 實例中新增應用程式 Cell，請執行下列動作：
1. 移至 [{{site.data.keyword.Bluemix_notm}} 儀表板](https://cloud.ibm.com/dashboard/apps/)，然後開啟您要在其中新增 Cell 的 {{site.data.keyword.cfee_full_notm}}。
2. 按一下 _Cell_ 表格附近的**變更 Cloud Foundry Cell 計數**，以開啟_新增 Cloud Foundry Cell_ 頁面。
3. 在_變更 Cloud Foundry Cell 計數_ 頁面中，選取「目標 Cell 計數」欄位中的 Cell 總數。此頁面也會顯示將在其中新增 Cell 之 CFEE 實例的「地理位置」及「位置」。它也會顯示_現行 Cell 計數_ 欄位中的現行 Cell 數目以及「節點大小」欄位中要新增的 Cell 容量（與現行 Cell 的容量相同）。此頁面的右窗格會顯示已新增 Cell 的預估成本，以及環境的新預估成本總計。
4. 按一下**新增 Cell**。在_新增 Cell_ 對話框中，按一下**新增**。
5. 回到_更新與擴充_ 頁面。針對每個新的 Cell 節點，都會在 _Cell 節點_ 表格中新增一列。「節點狀態」直欄指出在 CFEE 環境中新增及部署 Cell 的進度。
