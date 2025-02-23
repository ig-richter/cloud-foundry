---

copyright:
  years: 2018
lastupdated: "2019-04-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# サービスの処理
{: #workingwith-services}

{{site.data.keyword.cfee_full_notm}} にデプロイされているアプリケーションは、以下の 2 つのタイプのサービス・インスタンスにバインドできます。

1. {{site.data.keyword.Bluemix}} カタログから作成され、{{site.data.keyword.Bluemix}} アカウントで使用可能なパブリック・サービス・インスタンス。  
{{site.data.keyword.Bluemix}} アカウントで使用可能なパブリック・サービス・インスタンスは、それが存在するだけで自動的に CFEE 環境で使用できるわけではありません。パブリック・サービス・インスタンス ({{site.data.keyword.Bluemix}} アカウントで使用可能) を CFEE 環境内のスペースで使用できるようにするためには、ターゲット CFEE スペースに明示的に追加する必要があります。CFEE のユーザー・インターフェースによってパブリック・サービス・インスタンスを CFEE に_追加_ すると、パブリック・サービス・インスタンスの別名が CFEE 内に配置されます。CFEE スペースに追加 (別名割り当て) された {{site.data.keyword.Bluemix}} サービス・インスタンスは、その CFEE スペース内のアプリケーションにバインドできます。これにより、開発者は {{site.data.keyword.Bluemix}} サービスの膨大なカタログを CFEE 環境にデプロイしたアプリケーションで利用でき、それらの {{site.data.keyword.Bluemix}} サービスへのアクセス制御も可能になります。

   既存の {{site.data.keyword.Bluemix}} サービス・インスタンスを CFEE スペースに追加することに加えて、新規 {{site.data.keyword.Bluemix}} サービス・インスタンスを CFEE スペース内から作成することもでき、そうすると、そのサービス・インスタンスは自動的にその CFEE スペースに追加されます。
  
2. ローカル Cloud Foundry サービス・ブローカー (CFEE に対してローカル) によって管理されているサービス・インスタンス。 これは、以下の 2 つのタイプのいずれかです。
   *  2a. 現在の {{site.data.keyword.cfee_full_notm}} インスタンスの Cloud Foundry マーケットプレイスから作成されたサービス・オファリングのインスタンス。 このタイプのサービス・インスタンスでは、環境内で使用可能な登録されたサービス・ブローカーが必要です。 サービス・ブローカーにより、サービス・オファリングおよびプランのカタログが示されるとともに、該当するサービス・オファリングからのインスタンスを作成、削除、バインド、およびアンバインドできます。 詳しくは、Cloud Foundry 資料の『[Managing Service Brokers](https://docs.cloudfoundry.org/services/managing-service-brokers.html)』を参照してください。
   * 2b. ユーザー提供のサービス・インスタンス。 このタイプの作成は、コマンド・ライン・インターフェース (CLI) ではサポートされますが、ユーザー・インターフェースではサポートされません。 それにもかかわらず、ユーザー提供のサービス・インスタンスは、ユーザー・インターフェースにリストされます。
   
CFEE のユーザー・インターフェースから、または CLI を使用して、CFEE スペースからサービス・インスタンスを作成することもできます (以下のセクションを参照してください)。CLI のコマンドを使用して CFEE からサービス・インスタンスを作成すると、次の 2 つの結果が生じます。
* パブリック {{site.data.keyword.Bluemix}} 内にサービス・インスタンスが作成されます。
* サービス・インスタンスが作成された CFEE スペース内にパブリック・サービス・インスタンスの別名が作成されます。
    
以下のセクションでは、サービス・インスタンスの追加、作成、アクセス制御、およびバインドを行う方法を、ユーザー・インターフェースによる方法と、Cloud Foundry CLI による方法の両方で説明します。
   

## すべての CFEE 環境の {{site.data.keyword.Bluemix_notm}} サービス・インスタンスの表示
{: #viewing-services_across}

[Cloud Foundry サービス・ダッシュボード](https://cloud.ibm.com/dashboard/cloudfoundry/services)に移動して、{{site.data.keyword.Bluemix_notm}} アカウント内の (アクセス権限を持っている) すべての {{site.data.keyword.Bluemix_notm}} サービス・インスタンスの統合ビューを表示し、どのサービス・インスタンスがどの CFEE スペースに追加済みなのかを確認することができます。

このビューには、アカウント内の使用可能な {{site.data.keyword.Bluemix_notm}} サービス・インスタンスがすべて表示され、どのサービス・インスタンスがどの CFEE スペースに追加されたか (別名を割り当てられたか) が示されます。 サービス・インスタンスを展開すると、そのサービス・インスタンスが追加された CFEE スペースが表示されます。  さらに展開すると、これらのサービス・インスタンスがバインドされた先の CFEE スペース内のアプリケーションを表示することができます。  

このビューから、それらのサービス・インスタンスが追加された CFEE スペースにデプロイされたアプリケーションをバインドすることもできます。 対応するサービス (特定の CFEE スペースで使用可能) の表の行の右端にあるメニューに進み、**「アプリケーションにバインド」**を選択します。

## CFEE スペースへの既存の {{site.data.keyword.Bluemix_notm}} サービス・インスタンスの追加
{: #adding-services-inspace}

{{site.data.keyword.Bluemix_notm}} サービス・インスタンスを、CFEE スペースにデプロイされたアプリケーションにバインドできます。  IBM Cloud サービス・インスタンスを CFEE にデプロイされたアプリケーションにバインドするには、その前に、そのサービス・インスタンスを、そのアプリケーションがデプロイされている CFEE スペースに追加する必要があります。 {{site.data.keyword.Bluemix_notm}} サービス・インスタンスを CFEE スペースに追加するためには、前もって以下の要件が満たされている必要があります。
* Cloud Foundry サービスのインスタンスを追加することはできません。  CFEE に追加 (別名割り当て) できるのは、リソース・グループと IAM をサポートしている {{site.data.keyword.Bluemix_notm}} サービスだけです。 パブリック Cloud Foundry の組織、スペース、および役割にインスタンス化されているサービスは、CFEE に追加 (別名割り当て) できません。  {{site.data.keyword.Bluemix_notm}} サービスは、徐々にリソース・グループを活用する方向に移行しています。  サービスが Cloud Foundry の組織やスペースの代わりにリソース・グループを使用するように移行すると、そのサービスの既存のインスタンスをリソース・グループにマイグレーションできるようになり、その時点でサービスを CFEE に追加できます。  [リソース・グループへの Cloud Foundry サービス・インスタンスのマイグレーション](https://cloud.ibm.com/docs/resources/instance_migration.html#migrate)を参照してください。
* その {{site.data.keyword.Bluemix_notm}} サービスは、CFEE インスタンスがあるのと同じ {{site.data.keyword.Bluemix_notm}} アカウント内で使用可能である必要があります。
* {{site.data.keyword.Bluemix_notm}} サービス・インスタンスそのものに対する_オペレーター_以上のプラットフォーム役割が必要です。 詳しくは、{{site.data.keyword.Bluemix_notm}} ヘッダーの**「管理」>「ユーザー」** メニューにある「ID およびアクセス」ページを参照して、現在のアカウント・アクセス・ポリシーを確認してください。

既存の {{site.data.keyword.Bluemix_notm}} サービス・インスタンスを CFEE スペースに追加するには、次のようにします。
1. [Cloud Foundry サービス・ダッシュボード](https://cloud.ibm.com/dashboard/cloudfoundry/services)に移動します。  
2. リスト内で対象の {{site.data.keyword.Bluemix_notm}} サービス・インスタンスを見つけて、対応する表の行の右端にあるメニューを起動します。
3. **「サービスの追加」**を選択します。
4. _「サービスの追加」_ダイアログで、サービスを追加する先の CFEE、組織、およびスペースを選択し、**「追加」**をクリックします。
5. ターゲット {{site.data.keyword.Bluemix_notm}} サービスを展開して、サービスが追加された新しい CFEE を確認します。


代わりに以下のようにして、{{site.data.keyword.Bluemix_notm}} サービス・インスタンスを、それを追加したい CFEE スペース内から追加することができます。
1. {{site.data.keyword.Bluemix_notm}} [ダッシュボード](https://cloud.ibm.com/dashboard)または [Cloud Foundry サービス・ダッシュボード](https://cloud.ibm.com/dashboard/cloudfoundry/services)で、アプリケーションがホストされている Cloud Foundry Enterprise Environment を見つけます。
2. ナビゲーション・ペインで**「組織」**に移動し、アプリケーションがある組織およびスペースを開きます。
3. **「スペース」**タブに移動し、アプリケーションが含まれているスペースを見つけます。
4. ターゲット・スペースのページで、**「サービス」**タブに移動し、**「サービスの追加」**をクリックします。  これにより、__「サービスの追加」__ダイアログが開きます。  {{site.data.keyword.Bluemix_notm}} アカウント内で使用可能なサービス・インスタンスのリストがページに表示されます。
5. 使用可能なサービス・インスタンスから CFEE スペースに追加するものを見つけて選択します。 
6. CFEE スペース内で使用可能なサービスのリストにそのサービス・インスタンスが追加されます。

   **注:** {{site.data.keyword.Bluemix_notm}} サービスを CFEE スペースに追加すると、そのサービス・インスタンスの別名がそのスペース内で作成されます。 サービス・インスタンスを CFEE スペースから**削除**する (表内の対応するサービス・インスタンス行の右端にあるメニューから行えます) 場合、サービスはパブリック・アカウントからは削除されません。  この操作は、特定の CFEE アカウントからの削除を行うだけであり、その CFEE スペース内のアプリケーションへのバインドができなくなります。また、サービス・インスタンス自体を {{site.data.keyword.Bluemix_notm}} アカウントから削除すると、そのサービスはどの CFEE スペースでも使用可能ではなくなります。 

## CFEE スペースのユーザー・インターフェースからの {{site.data.keyword.Bluemix_notm}} サービス・インスタンスの作成
{: #creating-services-inspace}

CFEE スペース内から {{site.data.keyword.Bluemix_notm}} サービス・インスタンスを作成できます。  {{site.data.keyword.Bluemix_notm}} サービス・インスタンスの作成の結果、次のようになります。
* {{site.data.keyword.Bluemix_notm}} サービス・インスタンスが IBM Cloud 内に作成されます。  これは、{{site.data.keyword.Bluemix_notm}} [カタログ](https://cloud.ibm.com/catalog)からのサービス・インスタンスの作成と等価です。
* {{site.data.keyword.Bluemix_notm}} サービス・インスタンスの別名が、作成操作を開始した CFEE スペースに追加 (別名割り当て) されます。 (CFEE スペース内の) 別名サービス・インスタンスは、(IBM Cloud アカウント内の) 実際のサービス・インスタンスへの参照です。 サービス・インスタンス別名により、開発者は、サービス・インスタンスとの対話 (アプリケーションへのバインドなど) に必要なすべての資格情報の処理を自動的に行うことでサービス・インスタンスと対話できます。

CFEE スペース内からサービス・インスタンスを作成するには、次のようにします。
1. ターゲット・スペース・ページで、**「サービス」**タブに移動し、**「サービスの作成」**をクリックします。  これにより、{{site.data.keyword.cfee_full_notm}} の**「マーケットプレイス」**ビューが開きます。  このビューでは、以下の 2 つのソースからのサービス・オファリングがリストされます (「__ソース__」列を参照)。
   * {{site.data.keyword.Bluemix_notm}} カタログからのオファリング。
   * ローカル {{site.data.keyword.cfee_full_notm}} マーケットプレイスからのオファリング。
5. 使用可能なサービス・オファリングからインスタンスを生成するものを見つけて選択します。 
6. サービス・オファリングのソース (IBM Cloud カタログまたはローカル CFEE マーケットプレイス) に応じて、以下のいずれかに進みます。
   * 選択したサービス・オファリングが {{site.data.keyword.Bluemix_notm}} カタログからのオファリングである場合、IBM Cloud サービス・オファリング作成ページに進むための**「続行」**ボタンが表示されます。  これは、{{site.data.keyword.Bluemix_notm}} カタログから使用できるのと同じページです。  このページで、新規サービス・インスタンスの名前、地域、プラン、および IBM Cloud 内でサービスを作成するために必要なその他の特性を指定します。  サービス・インスタンスを作成すると、それは IBM Cloud 内に作成され、自動的に現行 CFEE スペースに追加されます。
   * 選択したサービス・オファリングがローカル CFEE カタログからのものである場合、サービス・インスタンスを作成するための**「作成」**ボタンが表示されます。 サービス・インスタンスが作成される現行 CFEE 内の組織およびスペースを指定するように求めるプロンプトが出されます。

## Cloud Foundry CLI を使用した CFEE スペースからの {{site.data.keyword.Bluemix_notm}} サービス・インスタンスの作成
{: #creating-services_cli}

CFEE 内のスペースから CLI を使用してサービス・インスタンスを作成できます。  CFEE スペースからの CLI を使用したサービスの作成は、そのスペース内での UI を使用した作成と等価です。 つまり、サービスを作成すると、以下の 2 つの結果が生じます。
* {{site.data.keyword.Bluemix_notm}} サービス・インスタンスが IBM Cloud 内に作成されます。  これは、{{site.data.keyword.Bluemix_notm}} [カタログ](https://cloud.ibm.com/catalog)からのサービス・インスタンスの作成と等価です。
* {{site.data.keyword.Bluemix_notm}} サービス・インスタンスが、作成操作が開始された CFEE スペースに追加 (別名割り当て) されます。

CFEE スペースから作成したサービス・インスタンスと CLI から作成したサービス・インスタンスの違いは、サービス・インスタンスのライフサイクルです。  CFEE スペース UI でサービス・インスタンスを作成した場合、作成されたサービス・インスタンスのライフサイクルは、{{site.data.keyword.Bluemix_notm}} アカウント内のサービス・インスタンス自体から制御されます。  これは、サービス・プランの更新が、CFEE スペースからではなく、{{site.data.keyword.Bluemix_notm}} アカウント内のインスタンスから制御されることを意味します。  一方、CLI から作成したサービス・インスタンスのライフサイクルは、CFEE スペースから制御されます (サービスは CFEE スペースから更新されます)。

CLI から作成したサービス・インスタンスも UI に表示されます。このようなサービス・インスタンスは名前の横に ` * ` が付いています。

CLI を使用してサービス・インスタンスを作成するには、以下の手順に従います。

1. {{site.data.keyword.Bluemix}} コマンド・ライン・インターフェースをダウンロードしてインストールします (まだ行っていない場合)。 [Cloud Foundry CLI をダウンロードします](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")。

2. {{site.data.keyword.cfee_full}} の概要ページに移動し、環境の API エンドポイントを見つけます。

3. コマンド・ライン・インターフェースで、以下のように API エンドポイントを CFEE 環境のエンドポイントに設定します。

  ```
  cf api <api_enpoint>
  ```
  {: pre}

4. CFEE 環境にログインします。

  ```
  cf login -u <username> -o <org_name> -s <space_name>
  ```
  {: pre}

  フェデレーテッド ID を使用する場合、`-sso` オプションを使用してください。

  ```
  cf login -o <org_name> -s <space_name> -sso
  ```
  {: pre}
  
5. 以下の方法でサービスを作成します。
`cf create-service` コマンドを発行して、コマンドが発行された CFEE スペース内にサービス・インスタンスを作成できます。CLI のコマンドを使用して CFEE からサービス・インスタンスを作成すると、次の 2 つの結果が生じます。
    * `instance_name` パラメーターで指定された名前で、パブリック {{site.data.keyword.Bluemix}} 内にサービス・インスタンスが作成されます。コマンドに `instance_name` が指定されていない場合は、{{site.data.keyword.Bluemix}} リソース・コントローラーによって名前が指定されます。  
    * `SERVICE_INSTANCE` で指定された名前で、コマンドが発行された CFEE スペース内にパブリック・サービス・インスタンスの別名が作成されます。パブリック・サービス・インスタンスの名前は、CFEE 内のサービス・インスタンスの名前と自動的に同じにはならないことに注意してください。そのため、同じ名前をパブリック・サービス・インスタンスと CFEE 内のサービス・インスタンス (パブリックのものの別名) の両方に付ける場合は、以下のコマンドで `instance_name` と `SERVICE_INSTANCE` の両方を (同じ値で) 指定する必要があります。

  ```
  cf create-service SERVICE PLAN SERVICE_INSTANCE -c '{"instance_name":"value", "resource_group":"value", "target":"value"}'
  ```
  {: pre}
  
以下の例では、「myCloudant」という名前 (パブリック・インスタンスと CFEE 別名の両方で同じ名前) の Cloudant サービス (標準プラン) のインスタンスが「bluemix-us-south 地域」に作成されて、特定のリソース・グループにグループ化されます。

  ```
  cf create-service cloudant standard myCloudant -c '{"instance_name":"myCloudant", "resource_group":"b0daaf6c3ccd4392a266da916cce2e8c", "target":"bluemix-us-south"}'
  ```
  {: pre}

 `create-service` コマンドは、特定のユース・ケースを処理するオプション・パラメーターを指定して発行できます。
 
   * インスタンス名 (`instance_name`): パブリック {{site.data.keyword.Bluemix}} で作成されたサービス・インスタンスのカスタム名を指定できます。`instance_name` 値を指定しない場合、パブリックに含まれるパブリック・サービス・インスタンスのデフォルト名は `SERVICE_INSTANCE` (CFEE 内のインスタンスの名前) と異なるものになります。パブリック・サービス・インスタンスの名前を制御するため、またはそれに CFEE 内のサービス・インスタンス (パブリックのものの別名) と同じ名前を指定する場合に、このパラメーターを使用することをお勧めします。
   * リソース・グループ (`resource_group`)。新しいインスタンスを配置するリソース・グループを指定できます。<!-- The    "resource_group" (`resource_group`). -->このパラメーターの値は、リソース・グループの名前 (_resource group name_) ではなく、リソース・グループの ID (_resource group ID_) でなければなりません。コマンド `ibmcloud resource groups` を使用して、特定のリソース・グループの_リソース・グループ ID_ を見つけることができます。
   * ターゲット・デプロイメント地域 (`target`)。サービス・インスタンスがプロビジョンされる地域。一部のサービスではターゲットを指定する必要がないことに注意してください。ただし、作成中のサービスでターゲットの指定が必要な場合にそれが指定されていないと、コマンドは失敗します。

オプションで、それらのパラメーターを格納する JSON ファイルを提供することもできます。以下に例を示します。
  ```
  {
    "NewCloudant": 
        {  
        "instance_name":"myCloudant",
        "resource_group": "b0daaf6c3ccd4392a266da916cce2e8c",
        "target": "bluemix-us-south"
        }
   }
  ```
  {: pre}
  
その後、`cf create-service` コマンドの一部として JSON ファイルを呼び出すことができます。JSON ファイルへのパスは、絶対でも相対でも構いません。
  ```
  cf create-service SERVICE PLAN SERVICE_INSTANCE -c PATH_TO_FILE
  ```
  {: pre}
   
その他の詳細を表示するには、以下を発行します。
  ```
  cf create-service -help
  ```
<br>
詳細情報については、Cloud Foundry 資料の [cf CLI を使用したサービス・インスタンスの管理](https://docs.cloudfoundry.org/devguide/services/managing-services.html){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。
  
## CFEE のユーザー・インターフェースからのアプリケーションへのサービスのバインド
{: #bind-services-ui}

{{site.data.keyword.Bluemix_notm}} サービス・インスタンスに対する_オペレーター_ 以上のプラットフォーム役割および_ライター_ 以上のサービス役割を持っているユーザーは、[Cloud Foundry サービス・ダッシュボード](https://cloud.ibm.com/dashboard/cloudfoundry/services)から、または CFEE スペースのユーザー・インターフェースの「サービス」タブから、CFEE スペースにデプロイされたアプリケーションに、そのインスタンスをバインドできます。   

アプリケーションへのサービス・インスタンスのバインドを [Cloud Foundry サービス・ダッシュボード](https://cloud.ibm.com/dashboard/cloudfoundry/services)から行うには、次のようにします。
1. [Cloud Foundry サービス・ダッシュボード](https://cloud.ibm.com/dashboard/cloudfoundry/services)に移動して、{{site.data.keyword.Bluemix_notm}} アカウント内の使用可能なすべての {{site.data.keyword.Bluemix_notm}} サービス・インスタンスの統合ビューを表示します。  どの {{site.data.keyword.Bluemix_notm}} サービス・インスタンスがどの CFEE スペース内で使用可能である (別名が割り当てられている) のかを表示することもこのビューの目的です。 サービス・インスタンスを展開して、そのサービス・インスタンスが追加された CFEE スペースを表示することができます。  さらに展開すると、これらのサービス・インスタンスがバインドされた先の CFEE スペース内のアプリケーションを表示することができます。 
2. バインドする {{site.data.keyword.Bluemix_notm}} サービス・インスタンスを見つけます。
3. 対応するビューを展開して、サービス・インスタンスが追加されたすべての CFEE スペースを表示します。 そのサービス・インスタンスが、アプリケーションがデプロイされた CFEE スペースに追加されていない場合、ターゲット CFEE スペース内でそのサービス・インスタンスを使用可能にするため、行の右端にあるメニューから**「追加」**を選択します。
4. サービス・インスタンスが使用可能な CFEE スペースに対応している行の右端にあるメニューに移動し、**「アプリケーションのバインド」**を選択します。
5. __「アプリケーションのバインド」__ダイアログで、バインドするアプリケーションを選択します。 

   **注:** バインド予定のサービスがパブリック (外部) とプライベート (内部) の両方のエンドポイントをサポートする場合 ([IBM Cloud サービス・エンドポイント](https://cloud.ibm.com/docs/services/service-endpoint?topic=service-endpoint-about#about)を参照)、または複数の[サービス・アクセス役割](https://cloud.ibm.com/docs/iam?topic=iam-iamconcepts#am) (バインディングによって実行できる、サービス・インスタンスに対して許可されるアクションを指定する) をサポートする場合、またはその両方の場合には、エンドポイント・タイプ (パブリックまたはプライベート) または特定のサービス役割、またはその両方を選択するように促す複数のステップからなるダイアログが表示されます。
6. これで、サービス・インスタンスにアプリケーションがバインドされました。  表内のサービス・インスタンスを展開すると、(特定の CFEE スペース内で) そのサービス・インスタンスにバインドされているアプリケーションを確認できます。


サービス・インスタンスへのアプリケーションのバインドを CFEE スペースの__「サービス」__ページから行うには、次のようにします。

1. アプリケーションがデプロイされた CFEE ユーザー・インターフェースを開きます。
2. 左側のナビゲーション・ペインで**「組織」**に移動し、アプリケーションがデプロイされた組織およびスペースを開きます。
3. **「スペース」**タブに移動し、アプリケーションが含まれているスペースを見つけます。
4. ターゲット・スペース・ページで**「サービス」**タブに移動します。
5. **「サービス・インスタンス」**の表で、バインドしたいサービス・インスタンスに対応している行の右端にある__「アクション」__メニューに移動し、**「バインド」**を選択します。
6. サービス・インスタンスにバインドするアプリケーションを選択します。

アプリケーションをサービス・インスタンスからアンバインドするには、以下のようにします。

1. スペースの**「サービス」**タブで、ターゲット・サービス・インスタンスを展開して、そのインスタンスにバインドされているアプリを表示します。
2. アプリケーションの行にある「アクション」メニューに移動し、**「サービスのアンバインド」**を選択します。

## Cloud Foundry CLI を使用したアプリケーションへのサービスのバインド
{: #bind-services-cli}

`cf bind-service` コマンドを使用して、サービス・インスタンスを CFEE 内のアプリケーションにバインドできます。

 ```
  cf bind-service APP_NAME SERVICE_INSTANCE -c '{"parameter": "value"}' 
  ```
  {: pre}

以下のケースでは、コマンドに特別なパラメーターが必要です。

* `cf bind-service` コマンドを使用して、IBM Cloud プライベート・ネットワーク上の IBM Cloud サービスに接続する [IBM Cloud サービス・エンドポイント](https://cloud.ibm.com/docs/services/service-endpoint?topic=service-endpoint-about#about)をサポートするサービス・インスタンスにアプリをバインドする場合。パブリック (外部) とプライベート (内部) の両方のエンドポイントをサポートするサービスに (CFEE 内にデプロイされた) アプリケーションをバインドするには、どちらのエンドポイントをバインドに使用するのかを指定する必要があります。以下の例では、コマンドが「内部 (internal)」エンドポイントを使用してサービスをバインドします。

  ```
  cf bind-service myApplication myServiceInstance -c '{"service-endpoints":"internal"}' 
  ```
  {: pre}

* サービスに複数の[サービス・アクセス役割](https://cloud.ibm.com/docs/iam?topic=iam-iamconcepts#am)がある場合。サービス・アクセス役割は、バインディングによって実行できる、サービス・インスタンスに対して許可されるアクションを指定します。`role` パラメーターによって、特定のサービス・アクセス役割を指定できます。以下の例では、バインディングによって「ライター (writer)」のサービス・アクセス役割が指定されます。

  ```
  cf bind-service myApplication myServiceInstance -c '{"role": "writer"}' 
  ```
  {: pre}
<br>
`cf bind-service` CLI コマンドを使用したアプリケーションのバインドについて詳しくは、Cloud Foundry 資料で[サービス・インスタンスのバインド](https://docs.cloudfoundry.org/devguide/services/application-binding.html){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。

## サービスの可視性
{: #service_visibility}

お客様は、どのユーザーが CFEE 環境から新規 IBM Cloud サービスを作成できるかを制御できます。  また、どのユーザーが既存の IBM Cloud サービスを CFEE スペースに追加できるかを制御することもできます。  この制御は、ユーザーに対するサービス・オファリング (またはそれらのサービス・オファリングのインスタンス、あるいはその両方) の可視性を制御することによって行います。

### サービス・インスタンスの作成の制御
{: #control_servicecreation}

サービスの作成を制御するには、IBM Cloud アカウント内のすべてのユーザー、または特定の Cloud Foundry 組織内のユーザーのいずれかを対象に、IBM Cloud カタログ内のサービス・オファリングの可視性を制御します。

#### IBM Cloud アカウント内のユーザーに対する IBM Cloud カタログの可視性の制御
{: #creation_accountvisibility}

IBM Cloud アカウント管理者 (すべての IAM 対応サービスに対して_管理者_ 役割を持っているユーザー) は、特定の **IBM Cloud アカウント**内のすべてのユーザーを対象に、サービスまたはサービス・プランがカタログに表示されないようにすることができます。  アカウント管理者は、アカウント内のすべてのユーザーを対象にサービスまたはサービス・プランをブラックリストに登録することで、すべてのアカウント・ユーザーがサービスを使用できないようにすることができます。 サービスをブラックリストに登録すると、そのアカウントのユーザーには、そのサービス (またはサービス・プラン) が IBM Cloud カタログに表示されることさえなくなります。  これを行うには、`ibmcloud catalog blacklist` コマンドを使用します。このコマンドの構文は以下のとおりです。

  ```
  ibmcloud catalog blacklist [--add service NAME or entry ID] [--remove service NAME or entry ID] [--service-list] [--output TYPE]
  ```

最も単純な形式では、`catalog backlist` コマンドを使用して、サービス・オファリング (そのすべてのプラン) を現在のアカウント内のすべてのユーザーに対して非表示にすることができます。

  ```
  Ibmcloud catalog blacklist [--add service NAME or entry ID] [--remove service NAME or entry ID] [--service-list] [--output TYPE]

  ```
  {: pre}

可視性のブラックリスト登録は、サービス・オファリング全体を対象に大まかに行うだけでなく、特定の地域の特定のプランを対象に行うこともできます。  ただし、そのためには、名前ではなく、グローバル・カタログ内の対応するエントリーの ID を使用する必要があります。   特定の地域の特定のプランをユーザーに表示しないようにするには、次のコマンドを実行します。

  ```
  Ibmcloud catalog blacklist -add <catalogEntryID>
  ```
  {: pre}
  
 ブラックリストからサービスを削除するには、以下のようにします。
 ```
  Ibmcloud catalog blacklist -remove <catalogEntryID>
  ```
  {: pre}
 
特定のカタログ・エントリー (例えば、サービスのプランとそのデプロイメント地域など) の ID は、次のコマンドで調べられます。 このコマンドは、特定のサービスのすべてのプランとそのデプロイメント地域を返します。

  ```
  ibmcloud catalog service <thisService>
  ```
  {: pre}

次のコマンドを実行すると、`ibmcloud catalog blacklist` に関する詳細情報が表示されます。

  ```
  Ibmcloud catalog blacklist -help
  ```
  {: pre}


#### Cloud Foundry 組織内のユーザーに対する可視性の制御
{: #creation_orgvisibility}

`cf enable-service-access` コマンドおよび `cf disable-service-access` コマンドを使用して、特定の **Cloud Foundry 組織**を対象にサービスへのアクセスを制御できます。  この場合、アクセスの制御点は Cloud Foundry です (IBM Cloud アカウントではありません)。  これはネイティブ `cf` コマンドである (`ibmcloud` コマンドではない) ことに注意してください。 

`cf` CLI を使用してサービス可視性を設定する場合は、以下の点を考慮してください。

*  `cf` コマンドは、特定の Cloud Foundry 組織のすべてのユーザーを対象に、サービス・オファリング (オプションで特定のサービス・オファリング・プラン) を有効または無効にします。
* 有効化は、組織の「ホワイトリスト」を作成することで機能します。 これは、サービスにアクセスできる Cloud Foundry 組織の包含リストです (前述の `ibmcloud` コマンドの「ブラックリスト」アプローチとは逆です。ブラックリストは、アカウント・ユーザーの可視範囲から除外するサービスのリストを定義することで機能します)。 つまり、このアプローチによるカタログ・サービスに対するアクセス制御は、どの組織がサービスを表示できないかを定義すること (ブラックリスティング) ではなく、どの組織がサービスを表示できるかを定義すること (ホワイトリスティング) によって機能します。
*  ある組織をホワイトリストに追加してその組織にサービス (またはサービス・プラン) に対するアクセス権限を付与すると、事実上、それ以外の組織によるアクセスを無効にする (除外する) ことになります。  つまり、いったん組織をホワイトリストに登録すると、他のすべての組織によるそのサービス (またはサービス・プラン) へのアクセスを無効にすることになります。
*  「表示可能な組織」リストに組織を追加する前に、すべての組織に対して可視性を無効にする必要があります。  サービス・プランがすべての組織で現在使用可能な場合に、そのプランへのアクセスを無効にすることはできません。

上記の一般的な動作を踏まえ、以下のコマンドを実行してサービスへの組織のアクセスを制御することをお勧めします。

  * すべての組織を対象にサービス・プランへのアクセスを**無効**にします。
  ```
  cf disable-service-access SERVICE [-p PLAN] [-o ORG]
  ```
  {: pre}
  
  以下の例では、MyOrg のすべてのメンバーに対して、Cloudant サービスの標準プランへのアクセス権限を無効にします。
  ```
  cf disable-service-access cloudant -p standard -o MyOrg
  ```
  {: pre}

  
  * 特定の CFEE 組織を対象にサービス・プランへのアクセスを**有効**にします。  これにより、このコマンドで特別に有効にした組織以外には、このサービス・プランは無効になります。 
  ```
  cf enable-service-access SERVICE [-p PLAN] [-o ORG]
  ```
  {: pre}

<br>  
**注:** 特定のプランではなくサービス全体が対象の場合は、Cloud Foundry のサービス有効化/無効化コマンドを実行することをお勧めします。


### 既存のサービス・インスタンスへのアクセスの制御
{: #control_serviceaddition}

CFEE スペース内の開発者は、IBM Cloud アカウントで使用可能な既存のサービス・インスタンスを使用できます。  CFEE の特定のスペース内で、ユーザーはそのスペースにサービス・インスタンスを**追加**できます (上記の[既存のサービス・インスタンスの追加](https://cloud.ibm.com/docs/cloud-foundry/add-serv-inst.html#workingwith-services#adding-services-inspace)を参照)。 サービス・インスタンスを CFEE スペースに追加すると、そのサービス・インスタンスの別名 (参照) が作成されます。これにより、開発者は、そのサービス・インスタンスを、あたかも実際のサービス・インスタンスであるかのように、その CFEE スペースにデプロイされたアプリケーションにバインドできます。

アカウント管理者は、[IAM アクセス・ポリシー](https://cloud.ibm.com/docs/iam/iamusermanage.html#iamusermanage)を使用してサービス・インスタンスの使用を制御できます。  アカウント管理者は UI または ibmcloud CLI から、サービス・インスタンス自体に対する、またはそれらのインスタンスが存在するリソース・グループに対する役割を割り当てることができます。  開発者がスペースにサービス・インスタンスを追加するためには、そのインスタンス (またはそのリソース・グループ) に対して_ビューアー_ 以上の役割を持っている必要があります。

CFEE サービスについて詳しく解説し、実演しているビデオが、[CFEE ビデオの再生リスト](https://ibm.biz/CFEE-Playlist){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") にあります。
{:tip}
