---
title: Distribuera en virtuell dator från Go
description: Distribuera en virtuell dator med hjälp av Azure SDK för Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: quickstart
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 1fbcc54df2a2aebce56c5a5800361f3d3aed1ccc
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/03/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Snabbstart: Distribuera en virtuell Azure-dator från en mall med Azure SDK för Go

Den här snabbstarten fokuserar på att distribuera resurser från en mall med Azure SDK för Go. Mallar är ögonblicksbilder av alla resurserna som ingår i en [Azure-resursgrupp](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). Du får du bekanta dig med funktioner och konventioner för detta SDK medan du utför en användbar aktivitet.

I slutet av den här snabbstarten har du en aktiv virtuell dator som du kan logga in på med ett användarnamn och lösenord.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Om du använder en lokal installation av Azure CLI så krävs version __2.0.28__ eller senare för den här snabbstarten. Kör `az --version` för att kontrollera om din CLI-installation uppfyller det här kravet. Om du behöver installera eller uppgradera kan du läsa informationen i [Installera Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Installera Azure SDK för Go 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Skapa ett huvudnamn för tjänsten


Om du vill logga in icke-interaktivt med ett program behöver du ett huvudnamn för tjänsten. Tjänstens huvudnamn är en del av rollbaserad åtkomstkontroll (RBAC), som skapar en unik användaridentitet. Kör följande kommando för att skapa ett unikt huvudnamn för tjänsten med CLI:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

Ange att miljövariabeln `AZURE_AUTH_LOCATION` ska vara den fullständiga sökvägen för den här filen. Sedan hittar och läser SDK autentiseringsuppgifterna direkt från den här filen utan att du behöver göra ändringar eller registrera information från tjänsten huvudnamn.

## <a name="get-the-code"></a>Hämta koden

Hämta snabbstartskoden och alla dess beroenden med `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

Du behöver inte göra ändringar i källkoden om variabeln `AZURE_AUTH_LOCATION` har konfigurerats korrekt. När programmet körs läser det in all nödvändig autentiseringsinformation därifrån.

## <a name="running-the-code"></a>Köra koden

Kör snabbstarten med kommandot `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

Om ett fel uppstår under distributionen så får du ett meddelande om att ett problem har uppstått, men informationen du får kanske inte är tillräckligt detaljerad. Du kan få mer information om distributionsfel med följande kommando med hjälp av Azure CLI:

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

### <a name="variables-constants-and-types"></a>Variabler, konstanter och typer

Eftersom snabbstarten är självständig använder den globala konstanter och variabler.

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

// Information loaded from the authorization file to identify the client
type clientInfo struct {
    SubscriptionID string
    VMPassword     string
}

var (
    ctx        = context.Background()
    clientData clientInfo
    authorizer autorest.Authorizer
)
```

Värden deklareras och ger namn på skapade resurser. Platsen anges också här och du kan ändra för att se hur distributioner fungerar i andra datacenter. Alla datacenter har inte alla de nödvändiga resurserna tillgängliga.

Typen `clientInfo` har deklarerats innehålla all information som måste läsas in separat från autentiseringsfilen för att konfigurera klienter i SDK och ange lösenordet för den virtuella datorn.

Konstanterna `templateFile` och `parametersFile` pekar på filerna som behövs för distribution. `authorizer` konfigureras av Go SDK för autentisering och variabeln `ctx` är en [Go-kontext](https://blog.golang.org/context) för nätverksåtgärderna.

### <a name="authentication-and-initialization"></a>Autentisering och initiering

Funktionen `init` ställer in autentisering. Eftersom autentisering är ett villkor för allt innehåll i snabbstarten kan det vara bra att ha det som en del av initieringen. Den läser även in information som behövs från autentiseringsfilen för att konfigurera klienter och den virtuella datorn.

```go
func init() {
    var err error
    authorizer, err = auth.NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v", err)
    }

    authInfo, err := readJSON(os.Getenv("AZURE_AUTH_LOCATION"))
    clientData.SubscriptionID = (*authInfo)["subscriptionId"].(string)
    clientData.VMPassword = (*authInfo)["clientSecret"].(string)
}
```

Först anropas [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) för att läsa in autentiseringsinformationen från filen i `AZURE_AUTH_LOCATION`. Därefter läses den här filen in manuellt genom funktionen `readJSON` (utelämnas här) för att hämta de två värden som behövs för att köra resten av programmet: Klientens prenumerations-ID och hemligheten för tjänstens huvudnamn som också används för den virtuella datorns lösenord.

> [!WARNING]
> Lösenordet för tjänstens huvudnamn återanvänds om du vill att snabbstarten ska fortsätta vara enkel. Var noga med att __aldrig__ återanvända ett lösenord som ger tillgång till Azure-resurser i produktionen.

### <a name="flow-of-operations-in-main"></a>Åtgärdsflöde i huvud()

Funktionen `main` är enkel och indikerar endast åtgärdsflödet samt utför felsökning.

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("Created group: %v", *group.Name)

    log.Printf("Starting deployment: %s", deploymentName)
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy: %v", err)
    }
    if result.Name != nil {
        log.Printf("Completed deployment %v: %v", deploymentName, *result.Properties.ProvisioningState)
    } else {
        log.Printf("Completed deployment %v (no data returned to SDK)", deploymentName)
    }
    getLogin()
}
```

De steg som koden kör igenom är, i följande ordning:

* Skapa resursgruppen för att distribuera till (`createGroup`)
* Skapa distributionen i den här gruppen (`createDeployment`)
* Skaffa och visa inloggningsinformation för den distribuerade virtuella datorn (`getLogin`)

### <a name="creating-the-resource-group"></a>Skapa resursgruppen

Funktionen `createGroup` skapar resursgruppen. Titta på anropsflödet och argumenten för att se hur tjänstens interaktioner är strukturerade i SDK.

```go
func createGroup() (group resources.Group, err error) {
    groupsClient := resources.NewGroupsClient(clientData.SubscriptionID)
    groupsClient.Authorizer = authorizer

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

Det allmänna flödet som interagerar med en Azure-tjänst är:

* Skapa klienten med metoden `service.New*Client()` där `*` är resurstypen för `service` som du vill interagera med. Den här funktionen använder alltid ett prenumerations-ID.
* Ange auktoriseringsmetoden för klienten så att den kan interagera med API:et via en fjärranslutning.
* Utför metodanropet på klienten som motsvarar API:et via fjärranslutningen. Tjänstklientmetoder tar vanligtvis namnet på resursen och ett metadataobjekt.

Funktionen [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) används här för att utföra en typkonvertering. Parametrarna för metoderna i SDK tar nästan uteslutande markörer, så bekväma metoder tillhandahålls för att förenkla typkonverteringar. Se dokumentationen för modulen [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) för en fullständig lista över bekvämlighetskonverterare och deras beteenden.

Metoden `groupsClient.CreateOrUpdate` returnerar en markör till en datatyp som representerar resursgruppen. Ett direkt returvärde för den här typen anger ett kortvarig åtgärd som är avsedd att vara synkron. I nästa avsnitt visas ett exempel på en långvarig åtgärd och hur du kan interagera med den.

### <a name="performing-the-deployment"></a>Utför distributionen

När du har skapat resursgruppen är det dags att utföra distributionen. Den här koden har delats upp i mindre delar för att betona olika delar av logiken.

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
    (*params)["vm_password"] = map[string]string{
        "value": clientData.VMPassword,
    }
        // ...
```

Distributionsfilerna läses in av `readJSON`. Information om detta hoppas över här. Den här funktionen returnerar en `*map[string]interface{}`, typen som används för att konstruera metadata för resursdistributionens anrop. Lösenordet för den virtuella datorn anges också manuellt med distributionsparametrarna.

```go
        // ...

    deploymentsClient := resources.NewDeploymentsClient(clientData.SubscriptionID)
    deploymentsClient.Authorizer = authorizer

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
        return
    }
```

Den här koden följer samma mönster som vid processen när resursgruppen skapas. En ny klient skapas, ges möjlighet att autentisera med Azure och sedan anropas en metod. Metoden har till och med samma namn (`CreateOrUpdate`) som den motsvarande metoden för resursgrupper. Det här mönstret återkommer om och om igen i SDK. Metoder som utför liknande funktioner har normalt samma namn.

Den största skillnaden är i returvärdet för metoden `deploymentsClient.CreateOrUpdate`. Det här värdet är av typen [Framtida](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) som följer det [framtida designmönstret](https://en.wikipedia.org/wiki/Futures_and_promises). Framtida värden representerar en långvarig åtgärd i Azure som du kan avsöka, avbryta eller blockera vid slutförande.

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    deployment, err = deploymentFuture.Result(deploymentsClient)

    // Work around possible bugs or late-stage failures
    if deployment.Name == nil || err != nil {
        deployment, _ = deploymentsClient.Get(ctx, resourceGroupName, deploymentName)
    }
    return
```

I det här exemplet är det bäst att vänta tills åtgärden har slutförts. Att vänta på ett framtidsobjekt kräver både ett [kontextobjekt](https://blog.golang.org/context) och klienten som skapade `Future`. Det finns två möjliga felkällor i det här fallet: Ett fel som orsakats på klientsidan vid försök att anropa metoden eller ett felsvar från servern. Det senare returneras som en del av anropet `deploymentFuture.Result`.

När distributionsinformationen hämtas finns en lösning för möjliga buggar där distributionsinformationen kan vara tom med ett manuell anrop till `deploymentsClient.Get` för att se till att data fylls i.

### <a name="obtaining-the-assigned-ip-address"></a>Hämta den tilldelade IP-adressen

Om du vill göra något med den nya virtuella datorn så behöver du den tilldelade IP-adressen. IP-adresser har egna separata Azure-resurser som är kopplade till nätverkskortresurser (NIC).

```go
func getLogin() {
    params, err := readJSON(parametersFile)
    if err != nil {
        log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
    }

    addressClient := network.NewPublicIPAddressesClient(clientData.SubscriptionID)
    addressClient.Authorizer = authorizer
    ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
    ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
    if err != nil {
        log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
    }

    vmUser := (*params)["vm_user"].(map[string]interface{})

    log.Printf("Log in with ssh: %s@%s, password: %s",
        vmUser["value"].(string),
        *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
        clientData.VMPassword)
}
```

Den här metoden är beroende av information som lagras i parameterfilen. Koden kan fråga den virtuella datorn direkt för att få dess nätverkskort, fråga nätverkskortet för att få dess IP-resurs och sedan fråga IP-resursen direkt. Det här är en lång kedja av beroenden och åtgärder att lösa, vilket gör att det blir dyrt. Eftersom JSON-informationen finns lokalt kan den läsas in istället.

Värdet för VM-användaren laddas också från JSON-filen. VM-lösenordet blev inläst tidigare från autentiseringsfilen.

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten använde du en befintlig mall och distribuerade den via Go. Sedan anslöt du den till den nyligen skapade virtuella datorn via SSH för att se till att den är aktiv.

Om du vill fortsätta att lära dig om hur du arbetar med virtuella datorer i Azure-miljön med Go så kan du ta en titt på [beräkningsexemplen för Go i Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) eller [exemplen på resurshantering för Go i Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).

Läs [Autentisering med Azure SDK för Go](azure-sdk-go-authorization.md) för att lära dig mer om de tillgängliga autentiseringsmetoderna i SDK och vilka typer av autentisering de stöder.
