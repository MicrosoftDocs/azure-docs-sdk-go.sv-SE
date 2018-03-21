---
title: "Distribuera en virtuell dator från Go"
description: "Distribuera en virtuell dator med hjälp av Azure SDK för Go."
keywords: azure, virtual machine, vm, go, golang, azure sdk
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: routlaw
ms.openlocfilehash: ae460dbf21b13c40f3d564274f8b790afe005aae
ms.sourcegitcommit: af3473779cd7c2978f290fbdc51ee15eb1130840
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/15/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Snabbstart: Distribuera en virtuell Azure-dator från en mall med Azure SDK för Go

Den här snabbstarten fokuserar på att distribuera resurser från en mall med Azure SDK för Go. Mallar är ögonblicksbilder av alla resurserna som ingår i en [Azure-resursgrupp](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). Du får du bekanta dig med funktioner och konventioner för detta SDK medan du utför en användbar aktivitet.

I slutet av den här snabbstarten har du en aktiv virtuell dator som du kan logga in på med ett användarnamn och lösenord.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Om du använder en lokal installation av Azure CLI så krävs version 2.0.24 eller senare för den här snabbstarten. Kör `az --version` för att kontrollera om din CLI-installation uppfyller det här kravet. Om du behöver installera eller uppgradera kan du läsa informationen i [Installera Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Installera Azure SDK för Go 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Skapa ett huvudnamn för tjänsten

Om du vill logga in icke-interaktivt med ett program behöver du ett huvudnamn för tjänsten. Tjänstens huvudnamn är en del av rollbaserad åtkomstkontroll (RBAC), som skapar en unik användaridentitet. Kör följande kommando för att skapa ett unikt huvudnamn för tjänsten med CLI:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

__Se till__ att spara värdena `appId`, `password`, och `tenant` i dina utdata. De här värdena används av programmet för att autentisera med Azure.

Läs mer om att skapa och hantera tjänstens huvudnamn med Azure CLI 2.0 i [Skapa Azure-tjänstens huvudnamn med Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).

## <a name="get-the-code"></a>Hämta koden

Hämta snabbstartskoden och alla dess beroenden med `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

Den här koden kompilerar, men den körs inte korrekt förrän du anger information om ditt Azure-konto och den skapade tjänstens huvudnamn. I `main.go` finns en variabel, `config`, som innehåller en `authInfo`-struct-datatyp. Fältvärdena i den här struct-datatypen måste ersättas för att kunna autentisera korrekt.

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* `SubscriptionID`: Ditt prenumerations-ID som kan hämtas från CLI-kommandot

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* `TenantID`: Ditt klient-ID i värdet `tenant` som sparas när du skapar tjänstens huvudnamn
* `ServicePrincipalID`: Värdet `appId` som sparas när du skapar tjänstens huvudnamn
* `ServicePrincipalSecret`: Värdet `password` som sparas när du skapar tjänstens huvudnamn

Du måste även redigera ett värde i filen `vm-quickstart-params.json`.

```json
    "vm_password": {
        "value": "_"
    }
```

* `vm_password`: Lösenordet för VM-användarkontot. Det måste vara 12–72 tecken långt och innehålla tre av följande tecken:
  * En gemen bokstav
  * En versal bokstav
  * En siffra
  * En symbol

## <a name="running-the-code"></a>Köra koden

Kör snabbstarten med kommandot `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

Om ett fel uppstår under distributionen så får du ett meddelande om att ett problem har uppstått men utan specifik information. Du kan få information om distributionsfel med följande kommando med hjälp av Azure CLI:

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

Om distributionen lyckas visas ett meddelande som ger användarnamn, IP-adress och lösenord för att logga in på den nya virtuella datorn. SSH-anslut till den här datorn för att bekräfta att den är igång.

## <a name="cleaning-up"></a>Rensa

Rensa resurserna som du skapade i den här snabbstarten genom att ta bort resursgruppen med CLI.

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a>Kod på djupet

Snabbstartskoden är uppdelad i block med variabler och flera små funktioner som alla beskrivs här.

### <a name="variable-assignments-and-structs"></a>Variabeltilldelning och struct-datatyper

Eftersom snabbstarten är fristående används globala variabler istället för kommandoradsalternativ eller miljövariabler.

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

Struct-datatypen `authInfo` deklareras för att kapsla in all information som behövs för auktorisering med Azure-tjänster.

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

var (
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }

    ctx = context.Background()

    token *adal.ServicePrincipalToken
)
```

Värden deklareras och ger namn på skapade resurser. Platsen anges också här och du kan ändra för att se hur distributioner fungerar i andra datacenter. Alla datacenter har inte alla de nödvändiga resurserna tillgängliga.

Konstanterna `templateFile` och `parametersFile` pekar på filerna som behövs för distribution. Token för tjänstens huvudnamn beskrivs senare och variabeln `ctx` är en [Go-kontext](https://blog.golang.org/context) för nätverksåtgärder.

### <a name="init-and-authorization"></a>init() och auktorisering

Metoden `init()` för koden konfigurerar auktorisering. Eftersom auktorisering är ett villkor för allt innehåll i snabbstarten kan det vara bra att ha det som en del av initieringen. 

```go
// Authenticate with the Azure services over OAuth, using a service principal.
func init() {
    oauthConfig, err := adal.NewOAuthConfig(azure.PublicCloud.ActiveDirectoryEndpoint, config.TenantID)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v\n", err)
    }
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        config.ServicePrincipalID,
        config.ServicePrincipalSecret,
        azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("faled to get token: %v\n", err)
    }
}
```

Den här koden slutför två steg för auktorisering:

* OAuth-konfigurationsinformation för `TenantID` hämtas genom gränssnittet med Azure Active Directory. Objekten [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) innehåller slutpunkter som används i standardkonfigurationen för Azure.
* Funktionen [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) anropas. Den här funktionen tar OAuth-informationen och inloggningsinformation med tjänstens huvudnamn, samt information om vilken typ av Azure-hantering som används. Förutsatt att du inte har särskilda krav eller vet vad du gör bör det här värdet alltid vara `.ResourceManagerEndpoint`.

### <a name="flow-of-operations-in-main"></a>Åtgärdsflöde i huvud()

Funktionen `main()` är enkel och indikerar endast åtgärdsflödet samt utför felsökning.

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("created group: %v\n", *group.Name)

    log.Println("starting deployment")
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy correctly: %v", err)
    }
    log.Printf("Completed deployment: %v", *result.Name)
    getLogin()
}
```

De steg som koden kör igenom är, i följande ordning:

* Skapa resursgruppen för att distribuera till (`createGroup()`)
* Skapa distributionen i den här gruppen (`createDeployment()`)
* Skaffa och visa inloggningsinformation för den distribuerade virtuella datorn (`getLogin()`)

### <a name="creating-the-resource-group"></a>Skapa resursgruppen

Funktionen `createGroup()` skapar resursgruppen. Titta på anropsflödet och argumenten för att se hur tjänstens interaktioner är strukturerade i SDK.

```go
func createGroup() (group resources.Group, err error) {
        groupsClient := resources.NewGroupsClient(config.SubscriptionID)
        groupsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

Det allmänna flödet som interagerar med en Azure-tjänst är:

* Skapa klienten med metoden `service.NewXClient()` där `X` är resurstypen för `service` som du vill interagera med. Den här funktionen använder alltid ett prenumerations-ID.
* Ange auktoriseringsmetoden för klienten så att den kan interagera med API:et via en fjärranslutning.
* Utför metodanropet på klienten som motsvarar API:et via fjärranslutningen. Tjänstklientmetoder tar vanligtvis namnet på resursen och ett metadataobjekt.

Funktionen [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) används här för att utföra en typkonvertering. Struct-datatyperna för parametrarna för metoderna i SDK tar nästan uteslutande markörer så att de här metoderna tillhandahålls för att förenkla typkonverteringar. Se dokumentationen för modulen [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) för en fullständig lista och beteende för bekvämlighetskonverterare.

Åtgärden `groupsClient.CreateOrUpdate()` returnerar en markör till en struct-datatyp som representerar resursgruppen. Ett direkt returvärde för den här typen anger ett kortvarig åtgärd som är avsedd att vara synkron. I nästa avsnitt visas ett exempel på en långvarig åtgärd och hur du kan interagera med den.

### <a name="performing-the-deployment"></a>Utför distributionen

När du har skapat gruppen som ska innehålla distributionens resurser är det dags att utföra distributionen. Den här koden har delats upp i mindre delar för att betona olika delar av logiken.


```go
func createDeployment() (deployment resources.DeploymentExtended, err error) {
    template, err := readJSON(templateFile)
    if err != nil {
        return
    }
    params, err := readJSON(parametersFile)
    if err != nil {
        return
    }

        // ...
```

Distributionsfilerna läses in av `readJSON`. Information om detta hoppas över här. Den här funktionen returnerar en `*map[string]interface{}`, typen som används för att konstruera metadata för resursdistributionens anrop.

```go
        // ...
        
        deploymentsClient := resources.NewDeploymentsClient(config.SubscriptionID)
        deploymentsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        deploymentFuture, err := deploymentsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                deploymentName,
                resources.Deployment{
                        Properties: &resources.DeploymentProperties{
                                Template:   template,
                                Parameters: params,
                                Mode:       resources.Incremental,
                        },
                },
        )
        if err != nil {
                log.Fatalf("Failed to create deployment: %v", err)
        }
        //...
```

Den här koden följer samma mönster som vid processen när resursgruppen skapas. En ny klient skapas, ges möjlighet att autentisera med Azure och sedan anropas en metod. Metoden har till och med samma namn (`CreateOrUpdate`) som den motsvarande metoden för resursgrupper. Det här mönstret återkommer om och om igen i SDK. Metoder som utför liknande funktioner har normalt samma namn.

Den största skillnaden är i returvärdet för metoden `deploymentsClient.CreateOrUpdate()`. Det här värdet är ett `Future`-objekt som följer det [framtida designmönstret](https://en.wikipedia.org/wiki/Futures_and_promises). Leveransplaner representerar en långvarig åtgärd i Azure som du ibland kan vilja avsöka när du utför annat arbete.

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

I det här exemplet är det bäst att vänta tills åtgärden har slutförts. Att vänta på ett framtidsobjekt kräver både ett [kontextobjekt](https://blog.golang.org/context) och klienten som skapade framtidsobjektet. Det finns två möjliga felkällor i det här fallet: Ett fel som orsakats på klientsidan vid försök att anropa metoden eller ett felsvar från servern. Det senare returneras som en del av anropet `deploymentFuture.Result()`.

### <a name="obtaining-the-assigned-ip-address"></a>Hämta den tilldelade IP-adressen

Om du vill göra något med den nya virtuella datorn så behöver du den tilldelade IP-adressen. IP-adresser har egna separata Azure-resurser som är kopplade till nätverkskortresurser (NIC).

```go
func getLogin() {
        params, err := readJSON(parametersFile)
        if err != nil {
                log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
        }

        addressClient := network.NewPublicIPAddressesClient(config.SubscriptionID)
        addressClient.Authorizer = autorest.NewBearerAuthorizer(token)
        ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
        ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
        if err != nil {
                log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
        }

        vmUser := (*params)["vm_user"].(map[string]interface{})
        vmPass := (*params)["vm_password"].(map[string]interface{})

        log.Printf("Log in with ssh: %s@%s, password: %s",
                vmUser["value"].(string),
                *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
                vmPass["value"].(string))
}
```

Den här metoden är beroende av information som lagras i parameterfilen. Koden kan fråga den virtuella datorn direkt för att få dess nätverkskort, fråga nätverkskortet för att få dess IP-resurs och sedan fråga IP-resursen direkt. Det här är en lång kedja av beroenden och åtgärder att lösa, vilket gör att det blir dyrt. Eftersom JSON-informationen finns lokalt kan den läsas in istället.

Värdena för VM-användaren och lösenordet kan också läsas in från JSON-filen.

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten använde du en befintlig mall och distribuerade den via Go. Sedan anslöt du den till den nyligen skapade virtuella datorn via SSH för att se till att den är aktiv.

Om du vill fortsätta att lära dig om hur du arbetar med virtuella datorer i Azure-miljön med Go så kan du ta en titt på [beräkningsexemplen för Go i Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) eller [exemplen på resurshantering för Go i Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).
