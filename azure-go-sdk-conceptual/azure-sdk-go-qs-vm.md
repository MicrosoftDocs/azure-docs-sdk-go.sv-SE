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
ms.openlocfilehash: 7592e8617436a76dd27cac5269971051982425bf
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38067024"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="85baf-103">Snabbstart: Distribuera en virtuell Azure-dator från en mall med Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="85baf-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="85baf-104">Den här snabbstarten fokuserar på att distribuera resurser från en mall med Azure SDK för Go.</span><span class="sxs-lookup"><span data-stu-id="85baf-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="85baf-105">Mallar är ögonblicksbilder av alla resurserna som ingår i en [Azure-resursgrupp](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="85baf-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="85baf-106">Du får du bekanta dig med funktioner och konventioner för detta SDK medan du utför en användbar aktivitet.</span><span class="sxs-lookup"><span data-stu-id="85baf-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="85baf-107">I slutet av den här snabbstarten har du en aktiv virtuell dator som du kan logga in på med ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="85baf-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="85baf-108">Om du använder en lokal installation av Azure CLI så krävs version __2.0.28__ eller senare för den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="85baf-108">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="85baf-109">Kör `az --version` för att kontrollera om din CLI-installation uppfyller det här kravet.</span><span class="sxs-lookup"><span data-stu-id="85baf-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="85baf-110">Om du behöver installera eller uppgradera kan du läsa informationen i [Installera Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="85baf-110">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="85baf-111">Installera Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="85baf-111">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="85baf-112">Skapa ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="85baf-112">Create a service principal</span></span>

<span data-ttu-id="85baf-113">Om du vill logga in icke-interaktivt i Azure med ett program behöver du ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="85baf-113">To sign in non-interactively to Azure with an application, you need a service principal.</span></span> <span data-ttu-id="85baf-114">Tjänstens huvudnamn är en del av rollbaserad åtkomstkontroll (RBAC), som skapar en unik användaridentitet.</span><span class="sxs-lookup"><span data-stu-id="85baf-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="85baf-115">Kör följande kommando för att skapa ett unikt huvudnamn för tjänsten med CLI:</span><span class="sxs-lookup"><span data-stu-id="85baf-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

<span data-ttu-id="85baf-116">Ange att miljövariabeln `AZURE_AUTH_LOCATION` ska vara den fullständiga sökvägen för den här filen.</span><span class="sxs-lookup"><span data-stu-id="85baf-116">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="85baf-117">Sedan hittar och läser SDK autentiseringsuppgifterna direkt från den här filen utan att du behöver göra ändringar eller registrera information från tjänsten huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="85baf-117">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="85baf-118">Hämta koden</span><span class="sxs-lookup"><span data-stu-id="85baf-118">Get the code</span></span>

<span data-ttu-id="85baf-119">Hämta snabbstartskoden och alla dess beroenden med `go get`.</span><span class="sxs-lookup"><span data-stu-id="85baf-119">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="85baf-120">Du behöver inte göra ändringar i källkoden om variabeln `AZURE_AUTH_LOCATION` har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="85baf-120">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="85baf-121">När programmet körs läser det in all nödvändig autentiseringsinformation därifrån.</span><span class="sxs-lookup"><span data-stu-id="85baf-121">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="85baf-122">Köra koden</span><span class="sxs-lookup"><span data-stu-id="85baf-122">Running the code</span></span>

<span data-ttu-id="85baf-123">Kör snabbstarten med kommandot `go run`.</span><span class="sxs-lookup"><span data-stu-id="85baf-123">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="85baf-124">Om ett fel uppstår under distributionen så får du ett meddelande om att ett problem har uppstått, men informationen du får kanske inte är tillräckligt detaljerad.</span><span class="sxs-lookup"><span data-stu-id="85baf-124">If there is a failure in the deployment, you get a message indicating that there was an issue, but it may not include enough detail.</span></span> <span data-ttu-id="85baf-125">Du kan få mer information om distributionsfel med följande kommando med hjälp av Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="85baf-125">Using the Azure CLI, get the full details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="85baf-126">Om distributionen lyckas visas ett meddelande som ger användarnamn, IP-adress och lösenord för att logga in på den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="85baf-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="85baf-127">SSH-anslut till den här datorn för att bekräfta att den är igång.</span><span class="sxs-lookup"><span data-stu-id="85baf-127">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="85baf-128">Rensa</span><span class="sxs-lookup"><span data-stu-id="85baf-128">Cleaning up</span></span>

<span data-ttu-id="85baf-129">Rensa resurserna som du skapade i den här snabbstarten genom att ta bort resursgruppen med CLI.</span><span class="sxs-lookup"><span data-stu-id="85baf-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="85baf-130">Kod på djupet</span><span class="sxs-lookup"><span data-stu-id="85baf-130">Code in depth</span></span>

<span data-ttu-id="85baf-131">Snabbstartskoden är uppdelad i block med variabler och flera små funktioner som alla beskrivs här.</span><span class="sxs-lookup"><span data-stu-id="85baf-131">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="85baf-132">Variabler, konstanter och typer</span><span class="sxs-lookup"><span data-stu-id="85baf-132">Variables, constants, and types</span></span>

<span data-ttu-id="85baf-133">Eftersom snabbstarten är självständig använder den globala konstanter och variabler.</span><span class="sxs-lookup"><span data-stu-id="85baf-133">Since quickstart is self-contained, it uses global constants and variables.</span></span>

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

<span data-ttu-id="85baf-134">Värden deklareras och ger namn på skapade resurser.</span><span class="sxs-lookup"><span data-stu-id="85baf-134">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="85baf-135">Platsen anges också här och du kan ändra för att se hur distributioner fungerar i andra datacenter.</span><span class="sxs-lookup"><span data-stu-id="85baf-135">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="85baf-136">Alla datacenter har inte alla de nödvändiga resurserna tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="85baf-136">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="85baf-137">Typen `clientInfo` har deklarerats innehålla all information som måste läsas in separat från autentiseringsfilen för att konfigurera klienter i SDK och ange lösenordet för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="85baf-137">The `clientInfo` type is declared to encapsulate all of the information that must be independently loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="85baf-138">Konstanterna `templateFile` och `parametersFile` pekar på filerna som behövs för distribution.</span><span class="sxs-lookup"><span data-stu-id="85baf-138">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="85baf-139">`authorizer` konfigureras av Go SDK för autentisering och variabeln `ctx` är en [Go-kontext](https://blog.golang.org/context) för nätverksåtgärderna.</span><span class="sxs-lookup"><span data-stu-id="85baf-139">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="85baf-140">Autentisering och initiering</span><span class="sxs-lookup"><span data-stu-id="85baf-140">Authentication and initialization</span></span>

<span data-ttu-id="85baf-141">Funktionen `init` ställer in autentisering.</span><span class="sxs-lookup"><span data-stu-id="85baf-141">The `init` function sets up authentication.</span></span> <span data-ttu-id="85baf-142">Eftersom autentisering är ett villkor för allt innehåll i snabbstarten kan det vara bra att ha det som en del av initieringen.</span><span class="sxs-lookup"><span data-stu-id="85baf-142">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="85baf-143">Den läser även in information som behövs från autentiseringsfilen för att konfigurera klienter och den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="85baf-143">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

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

<span data-ttu-id="85baf-144">Först anropas [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) för att läsa in autentiseringsinformationen från filen i `AZURE_AUTH_LOCATION`.</span><span class="sxs-lookup"><span data-stu-id="85baf-144">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="85baf-145">Därefter läses den här filen in manuellt genom funktionen `readJSON` (utelämnas här) för att hämta de två värden som behövs för att köra resten av programmet: Klientens prenumerations-ID och hemligheten för tjänstens huvudnamn som också används för den virtuella datorns lösenord.</span><span class="sxs-lookup"><span data-stu-id="85baf-145">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="85baf-146">Lösenordet för tjänstens huvudnamn återanvänds om du vill att snabbstarten ska fortsätta vara enkel.</span><span class="sxs-lookup"><span data-stu-id="85baf-146">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="85baf-147">Var noga med att __aldrig__ återanvända ett lösenord som ger tillgång till Azure-resurser i produktionen.</span><span class="sxs-lookup"><span data-stu-id="85baf-147">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="85baf-148">Åtgärdsflöde i huvud()</span><span class="sxs-lookup"><span data-stu-id="85baf-148">Flow of operations in main()</span></span>

<span data-ttu-id="85baf-149">Funktionen `main` är enkel och indikerar endast åtgärdsflödet samt utför felsökning.</span><span class="sxs-lookup"><span data-stu-id="85baf-149">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="85baf-150">De steg som koden kör igenom är, i följande ordning:</span><span class="sxs-lookup"><span data-stu-id="85baf-150">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="85baf-151">Skapa resursgruppen för att distribuera till (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="85baf-151">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="85baf-152">Skapa distributionen i den här gruppen (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="85baf-152">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="85baf-153">Skaffa och visa inloggningsinformation för den distribuerade virtuella datorn (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="85baf-153">Obtain and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="85baf-154">Skapa resursgruppen</span><span class="sxs-lookup"><span data-stu-id="85baf-154">Creating the resource group</span></span>

<span data-ttu-id="85baf-155">Funktionen `createGroup` skapar resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="85baf-155">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="85baf-156">Titta på anropsflödet och argumenten för att se hur tjänstens interaktioner är strukturerade i SDK.</span><span class="sxs-lookup"><span data-stu-id="85baf-156">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="85baf-157">Det allmänna flödet som interagerar med en Azure-tjänst är:</span><span class="sxs-lookup"><span data-stu-id="85baf-157">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="85baf-158">Skapa klienten med metoden `service.New*Client()` där `*` är resurstypen för `service` som du vill interagera med.</span><span class="sxs-lookup"><span data-stu-id="85baf-158">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="85baf-159">Den här funktionen använder alltid ett prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="85baf-159">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="85baf-160">Ange auktoriseringsmetoden för klienten så att den kan interagera med API:et via en fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="85baf-160">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="85baf-161">Utför metodanropet på klienten som motsvarar API:et via fjärranslutningen.</span><span class="sxs-lookup"><span data-stu-id="85baf-161">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="85baf-162">Tjänstklientmetoder tar vanligtvis namnet på resursen och ett metadataobjekt.</span><span class="sxs-lookup"><span data-stu-id="85baf-162">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="85baf-163">Funktionen [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) används här för att utföra en typkonvertering.</span><span class="sxs-lookup"><span data-stu-id="85baf-163">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="85baf-164">Parametrarna för metoderna i SDK tar nästan uteslutande markörer, så bekväma metoder tillhandahålls för att förenkla typkonverteringar.</span><span class="sxs-lookup"><span data-stu-id="85baf-164">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="85baf-165">Se dokumentationen för modulen [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) för en fullständig lista över bekvämlighetskonverterare och deras beteenden.</span><span class="sxs-lookup"><span data-stu-id="85baf-165">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="85baf-166">Metoden `groupsClient.CreateOrUpdate` returnerar en markör till en datatyp som representerar resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="85baf-166">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="85baf-167">Ett direkt returvärde för den här typen anger ett kortvarig åtgärd som är avsedd att vara synkron.</span><span class="sxs-lookup"><span data-stu-id="85baf-167">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="85baf-168">I nästa avsnitt visas ett exempel på en långvarig åtgärd och hur du kan interagera med den.</span><span class="sxs-lookup"><span data-stu-id="85baf-168">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="85baf-169">Utför distributionen</span><span class="sxs-lookup"><span data-stu-id="85baf-169">Performing the deployment</span></span>

<span data-ttu-id="85baf-170">När du har skapat resursgruppen är det dags att utföra distributionen.</span><span class="sxs-lookup"><span data-stu-id="85baf-170">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="85baf-171">Den här koden har delats upp i mindre delar för att betona olika delar av logiken.</span><span class="sxs-lookup"><span data-stu-id="85baf-171">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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

<span data-ttu-id="85baf-172">Distributionsfilerna läses in av `readJSON`. Information om detta hoppas över här.</span><span class="sxs-lookup"><span data-stu-id="85baf-172">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="85baf-173">Den här funktionen returnerar en `*map[string]interface{}`, typen som används för att konstruera metadata för resursdistributionens anrop.</span><span class="sxs-lookup"><span data-stu-id="85baf-173">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="85baf-174">Lösenordet för den virtuella datorn anges också manuellt med distributionsparametrarna.</span><span class="sxs-lookup"><span data-stu-id="85baf-174">The VM's password is also set manually on the deployment parameters.</span></span>

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

<span data-ttu-id="85baf-175">Den här koden följer samma mönster som vid processen när resursgruppen skapas.</span><span class="sxs-lookup"><span data-stu-id="85baf-175">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="85baf-176">En ny klient skapas, ges möjlighet att autentisera med Azure och sedan anropas en metod.</span><span class="sxs-lookup"><span data-stu-id="85baf-176">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="85baf-177">Metoden har till och med samma namn (`CreateOrUpdate`) som den motsvarande metoden för resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="85baf-177">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="85baf-178">Det här mönstret återkommer om och om igen i SDK.</span><span class="sxs-lookup"><span data-stu-id="85baf-178">This pattern is seen throughout the SDK.</span></span> <span data-ttu-id="85baf-179">Metoder som utför liknande funktioner har normalt samma namn.</span><span class="sxs-lookup"><span data-stu-id="85baf-179">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="85baf-180">Den största skillnaden är i returvärdet för metoden `deploymentsClient.CreateOrUpdate`.</span><span class="sxs-lookup"><span data-stu-id="85baf-180">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="85baf-181">Det här värdet är av typen [Framtida](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) som följer det [framtida designmönstret](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="85baf-181">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="85baf-182">Framtida värden representerar en långvarig åtgärd i Azure som du kan avsöka, avbryta eller blockera vid slutförande.</span><span class="sxs-lookup"><span data-stu-id="85baf-182">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

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

<span data-ttu-id="85baf-183">I det här exemplet är det bäst att vänta tills åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="85baf-183">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="85baf-184">Att vänta på ett framtidsobjekt kräver både ett [kontextobjekt](https://blog.golang.org/context) och klienten som skapade `Future`.</span><span class="sxs-lookup"><span data-stu-id="85baf-184">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="85baf-185">Det finns två möjliga felkällor i det här fallet: Ett fel som orsakats på klientsidan vid försök att anropa metoden eller ett felsvar från servern.</span><span class="sxs-lookup"><span data-stu-id="85baf-185">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="85baf-186">Det senare returneras som en del av anropet `deploymentFuture.Result`.</span><span class="sxs-lookup"><span data-stu-id="85baf-186">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

<span data-ttu-id="85baf-187">När distributionsinformationen hämtas finns en lösning för möjliga buggar där distributionsinformationen kan vara tom med ett manuell anrop till `deploymentsClient.Get` för att se till att data fylls i.</span><span class="sxs-lookup"><span data-stu-id="85baf-187">Once the deployment information is retrieved, there is a workaround for possible bugs where the deployment information may be empty with a manual call to `deploymentsClient.Get` to ensure that the data is populated.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="85baf-188">Hämta den tilldelade IP-adressen</span><span class="sxs-lookup"><span data-stu-id="85baf-188">Obtaining the assigned IP address</span></span>

<span data-ttu-id="85baf-189">Om du vill göra något med den nya virtuella datorn så behöver du den tilldelade IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="85baf-189">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="85baf-190">IP-adresser har egna separata Azure-resurser som är kopplade till nätverkskortresurser (NIC).</span><span class="sxs-lookup"><span data-stu-id="85baf-190">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="85baf-191">Den här metoden är beroende av information som lagras i parameterfilen.</span><span class="sxs-lookup"><span data-stu-id="85baf-191">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="85baf-192">Koden kan fråga den virtuella datorn direkt för att få dess nätverkskort, fråga nätverkskortet för att få dess IP-resurs och sedan fråga IP-resursen direkt.</span><span class="sxs-lookup"><span data-stu-id="85baf-192">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="85baf-193">Det här är en lång kedja av beroenden och åtgärder att lösa, vilket gör att det blir dyrt.</span><span class="sxs-lookup"><span data-stu-id="85baf-193">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="85baf-194">Eftersom JSON-informationen finns lokalt kan den läsas in istället.</span><span class="sxs-lookup"><span data-stu-id="85baf-194">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="85baf-195">Värdet för VM-användaren laddas också från JSON-filen.</span><span class="sxs-lookup"><span data-stu-id="85baf-195">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="85baf-196">VM-lösenordet blev inläst tidigare från autentiseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="85baf-196">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="85baf-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="85baf-197">Next steps</span></span>

<span data-ttu-id="85baf-198">I den här snabbstarten använde du en befintlig mall och distribuerade den via Go.</span><span class="sxs-lookup"><span data-stu-id="85baf-198">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="85baf-199">Sedan anslöt du den till den nyligen skapade virtuella datorn via SSH för att se till att den är aktiv.</span><span class="sxs-lookup"><span data-stu-id="85baf-199">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="85baf-200">Om du vill fortsätta att lära dig om hur du arbetar med virtuella datorer i Azure-miljön med Go så kan du ta en titt på [beräkningsexemplen för Go i Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) eller [exemplen på resurshantering för Go i Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="85baf-200">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="85baf-201">Läs [Autentisering med Azure SDK för Go](azure-sdk-go-authorization.md) för att lära dig mer om de tillgängliga autentiseringsmetoderna i SDK och vilka typer av autentisering de stöder.</span><span class="sxs-lookup"><span data-stu-id="85baf-201">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
