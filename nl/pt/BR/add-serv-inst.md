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

# Trabalhando com Serviços
{: #workingwith-services}

Os aplicativos implementados em um {{site.data.keyword.cfee_full_notm}} podem ser ligados a dois tipos de instâncias de serviço:

1. Instâncias de serviço público criadas por meio do catálogo do {{site.data.keyword.Bluemix}} e disponíveis na conta do {{site.data.keyword.Bluemix}}.  
As instâncias de serviço público disponíveis na conta do {{site.data.keyword.Bluemix}} não são automaticamente disponíveis, por elas mesmas, para ambientes do CFEE. Para que uma instância de serviço público (disponível na conta do {{site.data.keyword.Bluemix}}) se torne disponível para espaços em um ambiente do CFEE, ela deve ser incluída especificamente no espaço do CFEE de destino. Quando você _incluir_ uma instância de serviço público em um CFEE por meio da interface com o usuário do CFEE, um alias da instância de serviço público será colocado no CFEE. Assim que a instância de serviço do {{site.data.keyword.Bluemix}} for incluída (com alias) no espaço do CFEE, ela poderá ser ligada a aplicativos nesse espaço do CFEE. Isso permite que os desenvolvedores usem o vasto catálogo de serviços do {{site.data.keyword.Bluemix}} em seus aplicativos implementados em ambientes do CFEE, ao mesmo tempo em que permite o controle de acesso a esses serviços do {{site.data.keyword.Bluemix}}.

   Além de incluir instâncias de serviço existentes do {{site.data.keyword.Bluemix}} em um espaço do CFEE, é possível criar uma nova instância de serviço do {{site.data.keyword.Bluemix}} dentro de um espaço do CFEE, o que também a inclui automaticamente nesse espaço do CFEE.
  
2. Instâncias de serviço gerenciadas por um broker de serviço do Cloud Foundry local (local para o CFEE). Estas, por sua vez, podem ser de dois tipos:
   *  2a. Uma instância de uma oferta de serviços criada por meio do marketplace do Cloud Foundry da instância atual do {{site.data.keyword.cfee_full_notm}}. Esse tipo de instância de serviço requer um broker de serviço registrado disponível no ambiente. Um broker de serviço mostra um catálogo de ofertas de serviços e planos, assim como permite a criação, remoção, ligação e desvinculação de instâncias dessas ofertas de serviços. Consulte [Gerenciando brokers de serviço](https://docs.cloudfoundry.org/services/managing-service-brokers.html) na documentação do Cloud Foundry para obter mais informações.
   * 2b. Uma instância de serviço fornecida pelo usuário. A criação desse tipo é suportada por meio da interface
da linha de comandos (CLI), mas não por meio da interface com o usuário. No entanto, as instâncias de serviço fornecidas pelo usuário serão listadas na interface com o usuário.
   
Também é possível criar uma instância de serviço por meio de um espaço do CFEE, por meio da interface com o usuário do CFEE ou usando a CLI (consulte as seções abaixo). A criação de uma instância de serviço por meio de um CFEE usando esse comando tem um resultado duplo:
* Cria uma instância de serviço no {{site.data.keyword.Bluemix}} público.
* Cria um alias dessa instância de serviço público dentro do espaço do CFEE por meio do qual a instância de serviço foi criada.
    
As seções a seguir descrevem como é possível incluir, criar, controlar o acesso e ligar instâncias de serviço na interface com o usuário e com a CLI do Cloud Foundry.
   

## Visualizando instâncias de serviço do {{site.data.keyword.Bluemix_notm}} em todos os ambientes do CFEE
{: #viewing-services_across}

Acesse o [painel de serviços do Cloud Foundry](https://cloud.ibm.com/dashboard/cloudfoundry/services) para ter uma visualização consolidada de todas as instâncias de serviço do {{site.data.keyword.Bluemix_notm}} (às quais você tem acesso) na conta do {{site.data.keyword.Bluemix_notm}} e ver quais foram incluídas em quais espaços do CFEE.

Essa visualização mostra todas as instâncias de serviço do {{site.data.keyword.Bluemix_notm}} disponíveis
na conta e indica quais foram incluídas (com alias) em quais espaços do CFEE. Expanda uma instância de serviço para mostrar os espaços do CFEE nos quais ela foi incluída.  É possível expandi-las ainda mais para ver os aplicativos nos espaços do CFEE aos quais tais instâncias de serviço foram ligadas.  

Por meio dessa visualização, também é possível ligar os aplicativos implementados aos espaços do CFEE nos
quais essas instâncias de serviço foram incluídas. Acesse o menu localizado na extremidade direita da linha da tabela para o serviço correspondente (disponível para um espaço do CFEE específico) e selecione **Ligar aos aplicativos**.

## Incluindo instâncias de serviço existentes do {{site.data.keyword.Bluemix_notm}} em um espaço do CFEE
{: #adding-services-inspace}

É possível ligar uma instância de serviço do {{site.data.keyword.Bluemix_notm}} aos aplicativos implementados em um espaço do CFEE.  Antes que uma instância de serviço do IBM Cloud possa ser ligada ao aplicativo implementado no CFEE, ela deverá ser incluída no espaço do CFEE no qual o aplicativo está implementado. As seguintes condições são necessárias antes de ser possível incluir uma instância de serviço do {{site.data.keyword.Bluemix_notm}} em um Espaço do CFEE:
* O serviço cuja instância deve ser incluída não pode ser um serviço Cloud Foundry.  Apenas os serviços do
{{site.data.keyword.Bluemix_notm}} que suportam grupos de recursos e o IAM podem ser incluídos (com alias) em um
CFEE. Os serviços instanciados em organizações, espaços e funções públicos do Cloud Foundry não podem
ser incluídos (com alias) em um CFEE.  Os serviços do {{site.data.keyword.Bluemix_notm}} estão se
movendo progressivamente para se beneficiar de grupos de recursos.  Depois que um serviço é movido para usar grupos de recursos em vez de organizações e espaços do Cloud Foundry, é possível migrar qualquer instância existente anteriormente desse serviço para um grupo de recursos, quando ela pode ser incluída em um CFEE.  Consulte [Migrando instâncias de
serviço do Cloud Foundry para um grupo de recursos](https://cloud.ibm.com/docs/resources/instance_migration.html#migrate).
* O serviço do {{site.data.keyword.Bluemix_notm}} deve estar disponível na mesma conta do {{site.data.keyword.Bluemix_notm}} na qual a instância do CFEE reside.
* Deve-se ter a função da plataforma de _operador_ (ou superior) para a própria instância de serviço do {{site.data.keyword.Bluemix_notm}}. Para obter mais informações, consulte a página Identidade e acesso sob o menu **Gerenciar > Usuários** no cabeçalho do {{site.data.keyword.Bluemix_notm}} para verificar as suas políticas de acesso de conta atuais.

Para incluir uma instância de serviço existente do {{site.data.keyword.Bluemix_notm}} em um espaço do CFEE:
1. Acesse o [painel de serviços do Cloud Foundry](https://cloud.ibm.com/dashboard/cloudfoundry/services).  
2. Localize a instância de serviço do {{site.data.keyword.Bluemix_notm}} na lista e chame o menu na extremidade direita da linha da tabela correspondente.
3. Selecione  ** Incluir serviço **.
4. No diálogo _Incluir serviço_, selecione o CFEE, a organização e o espaço nos quais você deseja incluir o serviço e clique em **Incluir**.
5. Expanda o serviço de destino do {{site.data.keyword.Bluemix_notm}} para localizar o novo CFEE no qual o serviço foi incluído.


Como alternativa, é possível incluir a instância de serviço do {{site.data.keyword.Bluemix_notm}} dentro do espaço do CFEE no qual você deseja incluí-la:
1. Em seu [painel](https://cloud.ibm.com/dashboard) ou [painel de serviços do Cloud Foundry](https://cloud.ibm.com/dashboard/cloudfoundry/services) do {{site.data.keyword.Bluemix_notm}}, localize o ambiente do Cloud Foundry Enterprise que hospeda o seu aplicativo.
2. Acesse **Organizações** na área de janela de navegação e abra a organização e o espaço nos quais o aplicativo está localizado.
3. Acesse a guia **Espaços** e localize o espaço que contém o aplicativo.
4. Na página de espaço de destino, acesse a guia **Serviços** e clique em **Incluir serviço**.  Isso abrirá o diálogo __Incluir serviço__.  A página lista as instâncias de serviço disponíveis na conta do {{site.data.keyword.Bluemix_notm}}.
5. Localize e selecione uma instância de serviço disponível que deseja incluir no espaço do CFEE. 
6. A instância de serviço será incluída na lista de serviços disponíveis no espaço do CFEE.

   **Nota:** ao incluir um serviço do {{site.data.keyword.Bluemix_notm}} em um espaço do CFEE, um alias para essa instância de serviço é criado nesse espaço. Ao **Remover** a instância de serviço de um espaço do CFEE (no menu localizado na extremidade direita da linha da instância de serviço correspondente na tabela), o serviço não é excluído da conta pública.  A ação simplesmente a remove da conta do CFEE específica e não está mais disponível para ligação a aplicativos nesse espaço do CFEE. Além disso, ao excluir a própria instância de serviço da conta do {{site.data.keyword.Bluemix_notm}}, o serviço não ficará mais disponível para nenhum espaço do CFEE. 

## Criando as instâncias de serviço do {{site.data.keyword.Bluemix_notm}} por meio da interface
com o usuário de um espaço do CFEE
{: #creating-services-inspace}

É possível criar instâncias de serviço do {{site.data.keyword.Bluemix_notm}} dentro de um espaço do CFEE.  A criação de uma instância de serviço do {{site.data.keyword.Bluemix_notm}} resulta no seguinte:
* A instância de serviço do {{site.data.keyword.Bluemix_notm}} é criada no IBM Cloud.  Isso é equivalente a criar a instância de serviço por meio do [catálogo](https://cloud.ibm.com/catalog) do {{site.data.keyword.Bluemix_notm}}.
* Um alias para a instância de serviço do {{site.data.keyword.Bluemix_notm}} é incluído (com alias) no
espaço do CFEE por meio do qual a ação de criação foi iniciada. Uma instância de serviço do alias (em um espaço do CFEE) é uma
referência para a instância de serviço real (na conta do IBM Cloud). O alias da instância de serviço permite que os
desenvolvedores interajam com ela (por exemplo, ligando aos aplicativos) manipulando todas as credenciais
automaticamente para interação com a instância de serviço. .

Para criar uma instância de serviço dentro de um espaço do CFEE:
1. Na página de espaço de destino, acesse a guia **Serviços** e clique em **Criar serviço**.  Isso abrirá a visualização **Mercado** para o {{site.data.keyword.cfee_full_notm}}.  A visualização lista ofertas de serviços de duas origens (veja a coluna __Origem__):
   * Ofertas do catálogo do {{site.data.keyword.Bluemix_notm}}.
   * Ofertas do  {{site.data.keyword.cfee_full_notm}}  marketplace local.
5. Localize e selecione uma oferta de serviço disponível que você deseja instanciar. 
6. Dependendo da origem da oferta de serviços (catálogo do IBM Cloud ou mercado do CFEE local), tome uma das seguintes ações:
   * Se a oferta de serviços selecionada for uma oferta de catálogo do {{site.data.keyword.Bluemix_notm}}, você verá um botão para **Continuar** na página de criação da oferta de serviços do IBM Cloud.  Essa é a mesma página disponível no catálogo do {{site.data.keyword.Bluemix_notm}}.  Nessa página, você fornece o nome da nova instância de serviço, a região, o plano e outras propriedades obrigatórias para criar o serviço no IBM Cloud.  Depois de criar a instância de serviço, ela será criada no IBM Cloud e automaticamente incluída no espaço atual do CFEE.
   * Se a oferta de serviços selecionada for fornecida no catálogo local do CFEE, você verá um botão para **Criar** a instância de serviço. Você será solicitado a fornecer uma organização e um espaço no CFEE atual no qual a instância de serviço será criada.

## Criando instâncias de serviço do {{site.data.keyword.Bluemix_notm}} por meio de um espaço do CFEE usando a CLI do Cloud Foundry
{: #creating-services_cli}

É possível criar uma instância de serviço por meio da CLI de um espaço em um CFEE.  A criação de um serviço por meio da CLI em um espaço do CFEE é equivalente a criá-lo em uma IU nesse espaço. Ou seja, a criação de um serviço tem o resultado duplo a seguir:
* A instância de serviço do {{site.data.keyword.Bluemix_notm}} é criada no IBM Cloud.  Isso é equivalente a criar a instância de serviço por meio do [catálogo](https://cloud.ibm.com/catalog) do {{site.data.keyword.Bluemix_notm}}.
* A instância de serviço do {{site.data.keyword.Bluemix_notm}} é incluída (com alias) no espaço do CFEE no qual a ação de criação foi iniciada.

A diferença entre a criação de uma instância de serviço por meio de um espaço do CFEE e sua criação por meio da CLI
está no ciclo de vida da instância de serviço criada.  Quando você cria a instância de serviço na IU de espaço do CFEE, o ciclo de vida da instância de serviço criada é controlado na própria instância de serviço na conta do {{site.data.keyword.Bluemix_notm}}.  Isso
significa que as atualizações para o plano de serviços são controladas por meio da instância na conta do
{{site.data.keyword.Bluemix_notm}}, não por meio do espaço do CFEE.  Como alternativa, se a instância de serviço
for criada por meio da CLI, o ciclo de vida do serviço será controlado por meio do espaço do CFEE (o serviço será
atualizado por meio do espaço do CFEE).

As instâncias de serviço criadas por meio da CLI também são exibidas na IU e são indicadas por um ` *
` ao lado do nome da instância de serviço.

Para criar instâncias de serviço usando a CLI, siga as etapas a seguir:

1. Faça download e instale a interface da linha de comandos do {{site.data.keyword.Bluemix}} (se você
ainda não tiver feito isso). [Download da CLI do Cloud Foundry](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo").

2. Acesse a página Visão geral do {{site.data.keyword.cfee_full}} e localize o terminal de API do ambiente.

3. Na interface da linha de comandos, configure o terminal de API para o terminal do ambiente do CFEE:

  ```
  cf api <api_enpoint>
  ```
  {: pre}

4. Efetue login no ambiente do CFEE:

  ```
  cf login -u <username> -o <org_name> -s <space_name>
  ```
  {: pre}

  Se você estiver usando um ID federado, use a opção `-sso`:

  ```
  cf login -o <org_name> -s <space_name> -sso
  ```
  {: pre}
  
5. Crie o serviço:
É possível emitir um comando `cf create-service`para criar uma instância de serviço no espaço CFEE a partir do qual o comando é emitido.  A criação de uma instância de serviço por meio de um CFEE usando esse comando tem um resultado duplo:
    * Cria uma instância de serviço no {{site.data.keyword.Bluemix}} público, com um nome fornecido pelo parâmetro `instance_name`. Se nenhum `instance_name` for fornecido no comando, um nome será fornecido pelo controlador de recurso do {{site.data.keyword.Bluemix}}.  
    * Cria um alias dessa instância de serviço público dentro do espaço do CFEE por meio do qual o comando foi emitido, com um nome especificado por `SERVICE_INSTANCE`. Observe que o nome da instância de serviço público não será automaticamente o mesmo nome que a instância de serviço no CFEE. Portanto, se você desejar fornecer o mesmo nome para ambas, a instância de serviço público e a instância de serviço no CFEE (alias do público), será necessário especificar ambos, um `instance_name` e uma `SERVICE_INSTANCE` (com os mesmos valores) no comando a seguir:

  ```
  cf create-service SERVICE PLAN SERVICE_INSTANCE -c '{"instance_name":"value", "resource_group":"value", "target":"value"}'
  ```
  {: pre}
  
O exemplo a seguir cria uma instância do serviço do Cloudant (plano padrão) denominada "myCloudant" (com o mesmo nome em ambos, a instância pública e o alias do CFEE) na "região bluemix-us-south" e agrupa-a em um grupo de recursos específico":

  ```
  cf create-service cloudant standard myCloudant -c '{"instance_name":"myCloudant", "resource_group":"b0daaf6c3ccd4392a266da916cce2e8c", "target":"bluemix-us-south"}'
  ```
  {: pre}

 O comando `create-service` pode ser emitido com parâmetros opcionais para manipular casos de uso específicos:
 
   * Nome da instância (`instance_name`): permite que você especifique um nome customizado para a instância de serviço criada no {{site.data.keyword.Bluemix}} público. Se nenhum valor de `instance_name` for fornecido, o nome padrão da instância de serviço público no público será diferente do `SERVICE_INSTANCE` (o nome da instância no CFEE). Recomendamos que você use esse parâmetro para controlar o nome da instância de serviço público ou se você desejar fornecer a ele o mesmo nome que a instância de serviço no CFEE (alias para o público).
   * Grupo de recursos (`resource_group`). Permite que você especifique um grupo de recursos sob o qual colocar a nova instância.  O "resource_group" (`resource_group`). Observe que o valor desse parâmetro deve ser o ID do grupo de recursos (_ID do grupo de recursos_), não o nome do grupo de recursos (_nome do grupo de recursos_). É possível localizar o _ID do grupo de recursos_ de um grupo de recursos específico com o comando `ibmcloud resource groups`.
   * Região de implementação de destino (`target`). A região na qual a instância de serviço deve ser provisionada. Observe que alguns serviços não requerem especificação de um destino. No entanto, o comando falhará se nenhum destino for fornecido quando o serviço que estiver sendo criado requerer isso.

Opcionalmente, é possível fornecer um arquivo de JSON contendo esses parâmetros. Por exemplo:
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
  
O arquivo de JSON pode ser, então, chamado como parte do comando `cf create-service`. O caminho para o arquivo de JSON pode ser um absoluto ou relativo:
  ```
  cf create-service SERVICE PLAN SERVICE_INSTANCE -c PATH_TO_FILE
  ```
  {: pre}
   
É possível obter mais detalhes emitindo o seguinte:
  ```
  cf create-service -help
  ```
<br>
Veja [Gerenciando instâncias de serviço com a CLI cf](https://docs.cloudfoundry.org/devguide/services/managing-services.html){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") na documentação do Cloud Foundry para obter mais informações.
  
## Ligando serviços a aplicativos por meio da interface com o usuário do CFEE
{: #bind-services-ui}

Os usuários com a função da plataforma _operador_ (ou superior) e a função de serviço de _gravador_ (ou superior) para uma instância de serviço do {{site.data.keyword.Bluemix_notm}} podem ligar essa instância a um aplicativo implementado em um espaço do CFEE, por meio do [painel de serviços do Cloud Foundry](https://cloud.ibm.com/dashboard/cloudfoundry/services) ou por meio da guia Serviços de uma interface com o usuário do espaço do CFEE.   

Para ligar uma instância de serviço a um aplicativo no [painel de serviços do Cloud Foundry](https://cloud.ibm.com/dashboard/cloudfoundry/services):
1. Acesse o [painel de serviços do Cloud Foundry](https://cloud.ibm.com/dashboard/cloudfoundry/services) para ter uma visualização consolidada de todas as instâncias de serviço do {{site.data.keyword.Bluemix_notm}} disponíveis para você na conta do {{site.data.keyword.Bluemix_notm}}.  A visualização também é destinada a mostrar quais instâncias de serviços do {{site.data.keyword.Bluemix_notm}} estão disponíveis (com alias) em quais espaços do CFEE. É possível expandir uma instância de serviço para mostrar os espaços do CFEE nos quais ela foi incluída.  É possível expandi-las ainda mais para ver os aplicativos nos espaços do CFEE aos quais tais instâncias de serviço foram ligadas. 
2. Localize a instância de serviço do {{site.data.keyword.Bluemix_notm}} que você deseja ligar.
3. Expanda a visualização correspondente para ver todos os espaços do CFEE nos quais essa instância de serviço foi incluída. Se essa instância de serviço não tiver sido incluída no espaço do CFEE no qual o aplicativo está implementado, selecione **Incluir** no menu na extremidade direita da linha para tornar a instância de serviço disponível no espaço do CFEE de destino.
4. Acesse o menu localizado na extremidade direita da linha correspondente ao CFEE/espaço no qual a instância de serviço está disponível e selecione **Ligar aplicativo**.
5. No diálogo __Ligar aplicativo__, selecione o aplicativo que você deseja ligar. 

   **Nota:** se o serviço a ser ligado suportar terminais, públicos (externos) e privados (internos) (consulte [IBM Cloud Service Endpoints](https://cloud.ibm.com/docs/services/service-endpoint?topic=service-endpoint-about#about) e/ou suportar múltiplas [funções de acesso ao serviço](https://cloud.ibm.com/docs/iam?topic=iam-iamconcepts#am) (que especificam as ações permitidas sobre a instância de serviço que podem ser executadas por meio da ligação), um diálogo de múltiplas etapas solicitará que você selecione um tipo de terminal (público ou privado) e/ou uma função de serviço específica.
6. Agora, o aplicativo está ligado à instância de serviço.  É possível expandir uma instância de serviço na tabela
para ver os aplicativos ligados a ela (em um espaço do CFEE específico).


Para ligar um aplicativo a uma instância de serviço na página __Serviços__ de um espaço do CFEE:

1. Abra a interface com o usuário do CFEE na qual o aplicativo está implementado.
2. Acesse **Organizações** na área de janela de navegação à esquerda e abra a organização e o espaço nos quais o aplicativo está implementado.
3. Acesse a guia **Espaços** e localize o espaço que contém o aplicativo.
4. Na página de espaço de destino, acesse a guia **Serviços**.
5. Na tabela de **Instâncias de serviço**, acesse o menu __Ações__ na extremidade direita da linha correspondente à instância de serviço que deseja ligar e selecione **Ligar**.
6. Selecione o aplicativo que você deseja ligar à instância de serviço.

Para desvincular um aplicativo de uma instância de serviço:

1. Na guia **Serviços** do espaço, expanda a instância de serviço de destino para mostrar os apps que estão ligados a ela.
2. Acesse o menu Ações em uma linha do aplicativo e selecione **Desvincular serviço**.

## Ligando serviços a aplicativos usando a CLI do Cloud Foundry
{: #bind-services-cli}

É possível usar o comando `cf bind-service` para ligar uma instância de serviço a um aplicativo em um CFEE:

 ```
  cf bind-service APP_NAME SERVICE_INSTANCE -c '{"parameter": "value"}' 
  ```
  {: pre}

Nos casos a seguir, o comando requer parâmetros especiais:

* Quando você estiver usando o comando `cf bind-service` para ligar um app a uma instância de serviço que suporte [IBM Cloud Service Endpoints](https://cloud.ibm.com/docs/services/service-endpoint?topic=service-endpoint-about#about) que se conectam aos serviços do IBM Cloud sobre a rede privada do IBM Cloud. Para ligar um aplicativo (implementado em um CFEE) a um serviço que suporta os terminais públicos (externos) e privados (internos), é necessário especificar qual deles usar para as ligações. No exemplo a seguir, os comandos vinculam o serviço usando o terminal "interno"

  ```
  cf bind-service myApplication myServiceInstance -c '{"service-endpoints":"internal"}' 
  ```
  {: pre}

* Quando o serviço tiver múltiplas [funções de acesso ao serviço](https://cloud.ibm.com/docs/iam?topic=iam-iamconcepts#am). As funções de acesso ao serviço especificam as ações permitidas sobre a instância de serviço que podem ser executadas por meio da ligação. É possível especificar uma função de acesso ao serviço específica por meio do parâmetro `role`. No exemplo a seguir, a ligação especifica uma função de acesso ao serviço "gravador":

  ```
  cf bind-service myApplication myServiceInstance -c '{"role": "writer"}' 
  ```
  {: pre}
<br>
Consulte [Ligar uma instância de serviço](https://docs.cloudfoundry.org/devguide/services/application-binding.html){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") na documentação do Cloud Foundry para obter mais informações sobre aplicativos de ligação usando o comando da CLI `cf bind-service`.

## Visibilidade do serviço
{: #service_visibility}

Os clientes podem controlar quem pode criar novos serviços do IBM Cloud por meio de um ambiente do CFEE.  Eles também
podem controlar quem pode incluir os serviços do IBM Cloud existentes em um espaço do CFEE.  Esse controle é exercido
controlando a visibilidade das ofertas de serviços (e/ou das instâncias dessas ofertas de serviços) para os usuários.

### Controlando a criação das instâncias de serviço
{: #control_servicecreation}

O controle da criação de serviços pode ser realizado controlando a visibilidade das ofertas de serviços
no catálogo do IBM Cloud para todos os usuários na conta do IBM Cloud ou para os usuários em organizações específicas do Cloud Foundry.

#### Controlando a visibilidade para os usuários em uma conta do IBM Cloud para o catálogo do IBM Cloud
{: #creation_accountvisibility}

Os administradores de conta do IBM Cloud (usuários com a função de _administrador_ para todos os
serviços ativados para o IAM) podem evitar que um serviço ou um plano de serviço seja mostrado no catálogo para todos os
usuários em uma determinada **conta do IBM Cloud**.  Os administradores de conta podem evitar que
todos os usuários da conta usem um serviço inserindo um serviço ou um plano de serviço em uma lista de bloqueio para
todos os usuários em uma conta. A inserção de um serviço em uma lista de bloqueio evitará até que os usuários dessa conta
vejam esse serviço (ou plano de serviço) no catálogo do IBM Cloud.  Isso é feito por meio de um comando `ibmcloud catalog
blacklist` que tem a seguinte sintaxe:

  ```
  ibmcloud catalog blacklist [--add service NAME or entry ID] [--remove service NAME or entry ID] [--service-list]
[--output TYPE]
  ```

Em seu formato mais simples, é possível usar o comando `catalog backlist` para tornar uma oferta
de serviços (todos os seus planos) invisível para todos os usuários na conta atual:

  ```
  Ibmcloud catalog blacklist [--add service NAME or entry ID] [--remove service NAME or entry ID] [--service-list] [--output TYPE]

  ```
  {: pre}

É possível inserir a visibilidade em uma lista de bloqueio não apenas de forma geral para a oferta de serviços
inteira, mas para um plano específico em uma determinada região.  Para isso, no entanto, é necessário usar o ID para a
entrada correspondente no catálogo global, não o nome.   Se você deseja evitar que os usuários vejam um plano específico
em uma região específica, é possível emitir o seguinte:

  ```
  Ibmcloud catalog blacklist -add <catalogEntryID>
  ```
  {: pre}
  
 Para remover os serviços da lista de bloqueio:
 ```
  Ibmcloud catalog blacklist -remove <catalogEntryID>
  ```
  {: pre}
 
É possível localizar o ID de uma determinada entrada no catálogo (por exemplo, um plano do serviço e sua região de
implementação) por meio do comando a seguir. O comando retorna todos os planos e suas regiões de implementação para um
determinado serviço:

  ```
  ibmcloud catalog service <thisService>
  ```
  {: pre}

É possível localizar mais detalhes para o `ibmcloud catalog blacklist` emitindo o seguinte:

  ```
  Ibmcloud catalog blacklist -help
  ```
  {: pre}


#### Controlando a visibilidade para os usuários em uma organização do Cloud Foundry
{: #creation_orgvisibility}

É possível usar os comandos `cf enable-service-access` e `cf
disable-service-access` para controlar o acesso aos serviços para as **organizações do Cloud Foundry** específicas.  Aqui, o ponto de controle para o acesso é o Cloud Foundry (não a conta do IBM Cloud).  Observe que este é um comando
`cf` nativo (não um comando `ibmcloud`). 

Considere o seguinte ao configurar a visibilidade do serviço por meio da CLI `cf`:

*  Os comandos `cf` ativam e/ou desativam as ofertas de serviços (opcionalmente, os planos de
oferta de serviços específicos) para todos os usuários de organizações específicas do Cloud Foundry
* A ativação funciona construindo uma "lista de desbloqueio" da organização. Ou seja, uma lista de inclusão de
organizações do Cloud Foundry que têm acesso a um serviço (em oposição à abordagem "lista de bloqueio" no comando
`ibmcloud` descrito anteriormente, que funciona definindo uma lista de serviços excluídos da visibilidade para
os usuários da conta). Isso significa que o controle de acesso a um serviço de catálogo com essa abordagem funciona, não
por meio da definição de quais organizações não podem ver um serviço (lista de bloqueio), mas por meio da
definição de quais organizações podem vê-lo (lista de desbloqueio).
*  Ao conceder a uma organização acesso a um serviço (ou plano de serviços) incluindo essa
organização em uma lista de desbloqueio, você está, de fato, desativando (excluindo) o acesso de todas as outras
organizações a ele.  Ou seja, uma vez que você inclui uma organização em uma lista de desbloqueio, você está desativando
o acesso de todas as outras organizações a esse serviço (ou plano de serviços).
*  Antes de incluir as organizações na lista de "visibilidade permitida", é preciso desativar a visibilidade
para todas as organizações.  Não será possível desativar o acesso a um plano de serviços se o plano estiver atualmente
disponível para todas as organizações.

De acordo com o comportamento geral descrito acima, recomendamos controlar o acesso da organização aos serviços
emitindo os seguintes comandos:

  * **Desativar** o acesso a um plano de serviços para todas as organizações:
  ```
  cf disable-service-access SERVICE [-p PLAN] [-o ORG]
  ```
  {: pre}
  
  O exemplo a seguir desativa o acesso ao plano padrão do serviço do Cloudant para todos os membros de MyOrg:
  ```
  cf disable-service-access cloudant -p standard -o MyOrg
  ```
  {: pre}

  
  * **Ativar** o acesso ao plano de serviços para as organizações do CFEE específicas.  Isso
desativará o plano de serviços para todas as outras organizações não ativadas especificamente no comando. 
  ```
  cf enable-service-access SERVICE [-p PLAN] [-o ORG]
  ```
  {: pre}

<br>  
**Nota:** recomendamos executar os comandos de ativação e desativação de serviço do Cloud
Foundry para o serviço inteiro, não para planos específicos.


### Controlando o acesso às instâncias de serviço existentes
{: #control_serviceaddition}

Os desenvolvedores em um espaço do CFEE podem usar as instâncias de serviço disponíveis existentes na conta do IBM
Cloud.  Dentro de um espaço específico de um CFEE, os usuários podem **Incluir** uma instância de
serviço para esse espaço (consulte [Incluindo instâncias de serviço existentes](https://cloud.ibm.com/docs/cloud-foundry/add-serv-inst.html#workingwith-services#adding-services-inspace) acima). A inclusão de uma instância de serviço em um espaço do CFEE cria um alias (uma referência) para a instância de serviço, o
que permite que um desenvolvedor ligue a instância de serviço a um aplicativo implementado nesse espaço do CFEE como se
ela fosse a instância de serviço real.

Os administradores de conta podem controlar o uso de instâncias de serviço por meio
das [Políticas de acesso do IAM](https://cloud.ibm.com/docs/iam/iamusermanage.html#iamusermanage).  Por meio da IU ou da
CLI ibmcloud, eles podem designar funções para as próprias instâncias de serviço ou para os grupos de recursos nos quais
essas instâncias residiam.  Um desenvolvedor precisaria da função de _visualizador_ ou superior para
uma instância de serviço (ou seu grupo de recursos) antes de poder incluir essa instância em um espaço.

É possível localizar vídeos com discussões e demonstrações aprofundadas sobre os serviços do CFEE na
[Lista de execução de vídeos do CFEE](https://ibm.biz/CFEE-Playlist){: new_window}
![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo").
{:tip}
