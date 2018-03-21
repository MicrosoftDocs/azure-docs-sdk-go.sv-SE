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
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="9be99-104">Snabbstart: Distribuera en virtuell Azure-dator från en mall med Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="9be99-104">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="9be99-105">Den här snabbstarten fokuserar på att distribuera resurser från en mall med Azure SDK för Go.</span><span class="sxs-lookup"><span data-stu-id="9be99-105">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="9be99-106">Mallar är ögonblicksbilder av alla resurserna som ingår i en [Azure-resursgrupp](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="9be99-106">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="9be99-107">Du får du bekanta dig med funktioner och konventioner för detta SDK medan du utför en användbar aktivitet.</span><span class="sxs-lookup"><span data-stu-id="9be99-107">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="9be99-108">I slutet av den här snabbstarten har du en aktiv virtuell dator som du kan logga in på med ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="9be99-108">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="9be99-109">Om du använder en lokal installation av Azure CLI så krävs version 2.0.24 eller senare för den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="9be99-109">If you use a local install of the Azure CLI, this quickstart requires CLI version 2.0.24 or later.</span></span> <span data-ttu-id="9be99-110">Kör `az --version` för att kontrollera om din CLI-installation uppfyller det här kravet.</span><span class="sxs-lookup"><span data-stu-id="9be99-110">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="9be99-111">Om du behöver installera eller uppgradera kan du läsa informationen i [Installera Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9be99-111">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="9be99-112">Installera Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="9be99-112">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="9be99-113">Skapa ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="9be99-113">Create a service principal</span></span>

<span data-ttu-id="9be99-114">Om du vill logga in icke-interaktivt med ett program behöver du ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9be99-114">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="9be99-115">Tjänstens huvudnamn är en del av rollbaserad åtkomstkontroll (RBAC), som skapar en unik användaridentitet.</span><span class="sxs-lookup"><span data-stu-id="9be99-115">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="9be99-116">Kör följande kommando för att skapa ett unikt huvudnamn för tjänsten med CLI:</span><span class="sxs-lookup"><span data-stu-id="9be99-116">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

<span data-ttu-id="9be99-117">__Se till__ att spara värdena `appId`, `password`, och `tenant` i dina utdata.</span><span class="sxs-lookup"><span data-stu-id="9be99-117">__Make sure__ to record the `appId`, `password`, and `tenant` values in the output.</span></span> <span data-ttu-id="9be99-118">De här värdena används av programmet för att autentisera med Azure.</span><span class="sxs-lookup"><span data-stu-id="9be99-118">These values are used by the application to authenticate with Azure.</span></span>

<span data-ttu-id="9be99-119">Läs mer om att skapa och hantera tjänstens huvudnamn med Azure CLI 2.0 i [Skapa Azure-tjänstens huvudnamn med Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9be99-119">For more information on creating and managing service principals with the Azure CLI 2.0, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="9be99-120">Hämta koden</span><span class="sxs-lookup"><span data-stu-id="9be99-120">Get the code</span></span>

<span data-ttu-id="9be99-121">Hämta snabbstartskoden och alla dess beroenden med `go get`.</span><span class="sxs-lookup"><span data-stu-id="9be99-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

<span data-ttu-id="9be99-122">Den här koden kompilerar, men den körs inte korrekt förrän du anger information om ditt Azure-konto och den skapade tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="9be99-122">This code compiles, but doesn't run correctly until you provide it information about your Azure account and the created service principal.</span></span> <span data-ttu-id="9be99-123">I `main.go` finns en variabel, `config`, som innehåller en `authInfo`-struct-datatyp.</span><span class="sxs-lookup"><span data-stu-id="9be99-123">In `main.go` there is a variable, `config`, which contains an `authInfo` struct.</span></span> <span data-ttu-id="9be99-124">Fältvärdena i den här struct-datatypen måste ersättas för att kunna autentisera korrekt.</span><span class="sxs-lookup"><span data-stu-id="9be99-124">This struct needs to have its field values replaced in order to authenticate correctly.</span></span>

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* <span data-ttu-id="9be99-125">`SubscriptionID`: Ditt prenumerations-ID som kan hämtas från CLI-kommandot</span><span class="sxs-lookup"><span data-stu-id="9be99-125">`SubscriptionID`: Your subscription ID, which can be obtained from the CLI command</span></span>

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* <span data-ttu-id="9be99-126">`TenantID`: Ditt klient-ID i värdet `tenant` som sparas när du skapar tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="9be99-126">`TenantID`: Your tenant ID, the `tenant` value recorded when creating the service principal</span></span>
* <span data-ttu-id="9be99-127">`ServicePrincipalID`: Värdet `appId` som sparas när du skapar tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="9be99-127">`ServicePrincipalID`: The `appId` value recorded when creating the service principal</span></span>
* <span data-ttu-id="9be99-128">`ServicePrincipalSecret`: Värdet `password` som sparas när du skapar tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="9be99-128">`ServicePrincipalSecret`: The `password` value recorded when creating the service principal</span></span>

<span data-ttu-id="9be99-129">Du måste även redigera ett värde i filen `vm-quickstart-params.json`.</span><span class="sxs-lookup"><span data-stu-id="9be99-129">You also need to edit a value in the `vm-quickstart-params.json` file.</span></span>

```json
    "vm_password": {
        "value": "_"
    }
```

* <span data-ttu-id="9be99-130">`vm_password`: Lösenordet för VM-användarkontot.</span><span class="sxs-lookup"><span data-stu-id="9be99-130">`vm_password`: The password for the VM user account.</span></span> <span data-ttu-id="9be99-131">Det måste vara 12–72 tecken långt och innehålla tre av följande tecken:</span><span class="sxs-lookup"><span data-stu-id="9be99-131">It must be 12-72 characters in length and contain 3 of the following characters:</span></span>
  * <span data-ttu-id="9be99-132">En gemen bokstav</span><span class="sxs-lookup"><span data-stu-id="9be99-132">A lowercase letter</span></span>
  * <span data-ttu-id="9be99-133">En versal bokstav</span><span class="sxs-lookup"><span data-stu-id="9be99-133">An uppercase letter</span></span>
  * <span data-ttu-id="9be99-134">En siffra</span><span class="sxs-lookup"><span data-stu-id="9be99-134">A number</span></span>
  * <span data-ttu-id="9be99-135">En symbol</span><span class="sxs-lookup"><span data-stu-id="9be99-135">A symbol</span></span>

## <a name="running-the-code"></a><span data-ttu-id="9be99-136">Köra koden</span><span class="sxs-lookup"><span data-stu-id="9be99-136">Running the code</span></span>

<span data-ttu-id="9be99-137">Kör snabbstarten med kommandot `go run`.</span><span class="sxs-lookup"><span data-stu-id="9be99-137">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

<span data-ttu-id="9be99-138">Om ett fel uppstår under distributionen så får du ett meddelande om att ett problem har uppstått men utan specifik information.</span><span class="sxs-lookup"><span data-stu-id="9be99-138">If there is a failure in the deployment, you get a message indicating that there was an issue, but without any specific details.</span></span> <span data-ttu-id="9be99-139">Du kan få information om distributionsfel med följande kommando med hjälp av Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="9be99-139">Using the Azure CLI, get the details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="9be99-140">Om distributionen lyckas visas ett meddelande som ger användarnamn, IP-adress och lösenord för att logga in på den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9be99-140">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="9be99-141">SSH-anslut till den här datorn för att bekräfta att den är igång.</span><span class="sxs-lookup"><span data-stu-id="9be99-141">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="9be99-142">Rensa</span><span class="sxs-lookup"><span data-stu-id="9be99-142">Cleaning up</span></span>

<span data-ttu-id="9be99-143">Rensa resurserna som du skapade i den här snabbstarten genom att ta bort resursgruppen med CLI.</span><span class="sxs-lookup"><span data-stu-id="9be99-143">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="9be99-144">Kod på djupet</span><span class="sxs-lookup"><span data-stu-id="9be99-144">Code in depth</span></span>

<span data-ttu-id="9be99-145">Snabbstartskoden är uppdelad i block med variabler och flera små funktioner som alla beskrivs här.</span><span class="sxs-lookup"><span data-stu-id="9be99-145">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variable-assignments-and-structs"></a><span data-ttu-id="9be99-146">Variabeltilldelning och struct-datatyper</span><span class="sxs-lookup"><span data-stu-id="9be99-146">Variable assignments and structs</span></span>

<span data-ttu-id="9be99-147">Eftersom snabbstarten är fristående används globala variabler istället för kommandoradsalternativ eller miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="9be99-147">Since quickstart is self-contained, it uses global variables rather than command-line options or environment variables.</span></span>

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

<span data-ttu-id="9be99-148">Struct-datatypen `authInfo` deklareras för att kapsla in all information som behövs för auktorisering med Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="9be99-148">The `authInfo` struct is declared to encapsulate all of the information needed for authorization with Azure services.</span></span>

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

<span data-ttu-id="9be99-149">Värden deklareras och ger namn på skapade resurser.</span><span class="sxs-lookup"><span data-stu-id="9be99-149">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="9be99-150">Platsen anges också här och du kan ändra för att se hur distributioner fungerar i andra datacenter.</span><span class="sxs-lookup"><span data-stu-id="9be99-150">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="9be99-151">Alla datacenter har inte alla de nödvändiga resurserna tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="9be99-151">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="9be99-152">Konstanterna `templateFile` och `parametersFile` pekar på filerna som behövs för distribution.</span><span class="sxs-lookup"><span data-stu-id="9be99-152">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="9be99-153">Token för tjänstens huvudnamn beskrivs senare och variabeln `ctx` är en [Go-kontext](https://blog.golang.org/context) för nätverksåtgärder.</span><span class="sxs-lookup"><span data-stu-id="9be99-153">The service principal token is covered later, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="init-and-authorization"></a><span data-ttu-id="9be99-154">init() och auktorisering</span><span class="sxs-lookup"><span data-stu-id="9be99-154">init() and authorization</span></span>

<span data-ttu-id="9be99-155">Metoden `init()` för koden konfigurerar auktorisering.</span><span class="sxs-lookup"><span data-stu-id="9be99-155">The `init()` method for the code sets up authorization.</span></span> <span data-ttu-id="9be99-156">Eftersom auktorisering är ett villkor för allt innehåll i snabbstarten kan det vara bra att ha det som en del av initieringen.</span><span class="sxs-lookup"><span data-stu-id="9be99-156">Since authorization is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> 

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

<span data-ttu-id="9be99-157">Den här koden slutför två steg för auktorisering:</span><span class="sxs-lookup"><span data-stu-id="9be99-157">This code completes two steps for authorization:</span></span>

* <span data-ttu-id="9be99-158">OAuth-konfigurationsinformation för `TenantID` hämtas genom gränssnittet med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9be99-158">OAuth configuration information for the `TenantID` is retrieved by interfacing with Azure Active Directory.</span></span> <span data-ttu-id="9be99-159">Objekten [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) innehåller slutpunkter som används i standardkonfigurationen för Azure.</span><span class="sxs-lookup"><span data-stu-id="9be99-159">The [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) object contains endpoints used in the standard Azure configuration.</span></span>
* <span data-ttu-id="9be99-160">Funktionen [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) anropas.</span><span class="sxs-lookup"><span data-stu-id="9be99-160">The [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) function is called.</span></span> <span data-ttu-id="9be99-161">Den här funktionen tar OAuth-informationen och inloggningsinformation med tjänstens huvudnamn, samt information om vilken typ av Azure-hantering som används.</span><span class="sxs-lookup"><span data-stu-id="9be99-161">This function takes the OAuth information along with the service principal login, as well as which style of Azure management is being used.</span></span> <span data-ttu-id="9be99-162">Förutsatt att du inte har särskilda krav eller vet vad du gör bör det här värdet alltid vara `.ResourceManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="9be99-162">Unless you have specific requirements and know what you're doing, this value should always be `.ResourceManagerEndpoint`.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="9be99-163">Åtgärdsflöde i huvud()</span><span class="sxs-lookup"><span data-stu-id="9be99-163">Flow of operations in main()</span></span>

<span data-ttu-id="9be99-164">Funktionen `main()` är enkel och indikerar endast åtgärdsflödet samt utför felsökning.</span><span class="sxs-lookup"><span data-stu-id="9be99-164">The `main()` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="9be99-165">De steg som koden kör igenom är, i följande ordning:</span><span class="sxs-lookup"><span data-stu-id="9be99-165">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="9be99-166">Skapa resursgruppen för att distribuera till (`createGroup()`)</span><span class="sxs-lookup"><span data-stu-id="9be99-166">Create the resource group to deploy to (`createGroup()`)</span></span>
* <span data-ttu-id="9be99-167">Skapa distributionen i den här gruppen (`createDeployment()`)</span><span class="sxs-lookup"><span data-stu-id="9be99-167">Create the deployment within this group (`createDeployment()`)</span></span>
* <span data-ttu-id="9be99-168">Skaffa och visa inloggningsinformation för den distribuerade virtuella datorn (`getLogin()`)</span><span class="sxs-lookup"><span data-stu-id="9be99-168">Obtain and display login information for the deployed VM (`getLogin()`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="9be99-169">Skapa resursgruppen</span><span class="sxs-lookup"><span data-stu-id="9be99-169">Creating the resource group</span></span>

<span data-ttu-id="9be99-170">Funktionen `createGroup()` skapar resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9be99-170">The `createGroup()` function creates the resource group.</span></span> <span data-ttu-id="9be99-171">Titta på anropsflödet och argumenten för att se hur tjänstens interaktioner är strukturerade i SDK.</span><span class="sxs-lookup"><span data-stu-id="9be99-171">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="9be99-172">Det allmänna flödet som interagerar med en Azure-tjänst är:</span><span class="sxs-lookup"><span data-stu-id="9be99-172">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="9be99-173">Skapa klienten med metoden `service.NewXClient()` där `X` är resurstypen för `service` som du vill interagera med.</span><span class="sxs-lookup"><span data-stu-id="9be99-173">Create the client using the `service.NewXClient()` method, where `X` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="9be99-174">Den här funktionen använder alltid ett prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="9be99-174">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="9be99-175">Ange auktoriseringsmetoden för klienten så att den kan interagera med API:et via en fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="9be99-175">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="9be99-176">Utför metodanropet på klienten som motsvarar API:et via fjärranslutningen.</span><span class="sxs-lookup"><span data-stu-id="9be99-176">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="9be99-177">Tjänstklientmetoder tar vanligtvis namnet på resursen och ett metadataobjekt.</span><span class="sxs-lookup"><span data-stu-id="9be99-177">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="9be99-178">Funktionen [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) används här för att utföra en typkonvertering.</span><span class="sxs-lookup"><span data-stu-id="9be99-178">The [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="9be99-179">Struct-datatyperna för parametrarna för metoderna i SDK tar nästan uteslutande markörer så att de här metoderna tillhandahålls för att förenkla typkonverteringar.</span><span class="sxs-lookup"><span data-stu-id="9be99-179">The parameters structs for methods of the SDK almost exclusively take pointers, so these methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="9be99-180">Se dokumentationen för modulen [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) för en fullständig lista och beteende för bekvämlighetskonverterare.</span><span class="sxs-lookup"><span data-stu-id="9be99-180">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list and behavior of convenience converters.</span></span>

<span data-ttu-id="9be99-181">Åtgärden `groupsClient.CreateOrUpdate()` returnerar en markör till en struct-datatyp som representerar resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9be99-181">The `groupsClient.CreateOrUpdate()` operation returns a pointer to a data struct representing the resource group.</span></span> <span data-ttu-id="9be99-182">Ett direkt returvärde för den här typen anger ett kortvarig åtgärd som är avsedd att vara synkron.</span><span class="sxs-lookup"><span data-stu-id="9be99-182">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="9be99-183">I nästa avsnitt visas ett exempel på en långvarig åtgärd och hur du kan interagera med den.</span><span class="sxs-lookup"><span data-stu-id="9be99-183">In the next section, you'll see an example of a long-running operation and how to interact with them.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="9be99-184">Utför distributionen</span><span class="sxs-lookup"><span data-stu-id="9be99-184">Performing the deployment</span></span>

<span data-ttu-id="9be99-185">När du har skapat gruppen som ska innehålla distributionens resurser är det dags att utföra distributionen.</span><span class="sxs-lookup"><span data-stu-id="9be99-185">Once the group to contain its resources is created, it's time to run the deployment.</span></span> <span data-ttu-id="9be99-186">Den här koden har delats upp i mindre delar för att betona olika delar av logiken.</span><span class="sxs-lookup"><span data-stu-id="9be99-186">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>


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

<span data-ttu-id="9be99-187">Distributionsfilerna läses in av `readJSON`. Information om detta hoppas över här.</span><span class="sxs-lookup"><span data-stu-id="9be99-187">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="9be99-188">Den här funktionen returnerar en `*map[string]interface{}`, typen som används för att konstruera metadata för resursdistributionens anrop.</span><span class="sxs-lookup"><span data-stu-id="9be99-188">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span>

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

<span data-ttu-id="9be99-189">Den här koden följer samma mönster som vid processen när resursgruppen skapas.</span><span class="sxs-lookup"><span data-stu-id="9be99-189">This code follows the same pattern as with creating the resource group.</span></span> <span data-ttu-id="9be99-190">En ny klient skapas, ges möjlighet att autentisera med Azure och sedan anropas en metod.</span><span class="sxs-lookup"><span data-stu-id="9be99-190">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="9be99-191">Metoden har till och med samma namn (`CreateOrUpdate`) som den motsvarande metoden för resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="9be99-191">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="9be99-192">Det här mönstret återkommer om och om igen i SDK.</span><span class="sxs-lookup"><span data-stu-id="9be99-192">This pattern is seen again and again in the SDK.</span></span> <span data-ttu-id="9be99-193">Metoder som utför liknande funktioner har normalt samma namn.</span><span class="sxs-lookup"><span data-stu-id="9be99-193">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="9be99-194">Den största skillnaden är i returvärdet för metoden `deploymentsClient.CreateOrUpdate()`.</span><span class="sxs-lookup"><span data-stu-id="9be99-194">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate()` method.</span></span> <span data-ttu-id="9be99-195">Det här värdet är ett `Future`-objekt som följer det [framtida designmönstret](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="9be99-195">This value is a `Future` object, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="9be99-196">Leveransplaner representerar en långvarig åtgärd i Azure som du ibland kan vilja avsöka när du utför annat arbete.</span><span class="sxs-lookup"><span data-stu-id="9be99-196">Futures represent a long-running operation in Azure that you may want to occasionally poll while performing other work.</span></span>

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="9be99-197">I det här exemplet är det bäst att vänta tills åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="9be99-197">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="9be99-198">Att vänta på ett framtidsobjekt kräver både ett [kontextobjekt](https://blog.golang.org/context) och klienten som skapade framtidsobjektet.</span><span class="sxs-lookup"><span data-stu-id="9be99-198">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the Future object.</span></span> <span data-ttu-id="9be99-199">Det finns två möjliga felkällor i det här fallet: Ett fel som orsakats på klientsidan vid försök att anropa metoden eller ett felsvar från servern.</span><span class="sxs-lookup"><span data-stu-id="9be99-199">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="9be99-200">Det senare returneras som en del av anropet `deploymentFuture.Result()`.</span><span class="sxs-lookup"><span data-stu-id="9be99-200">The latter is returned as part of the `deploymentFuture.Result()` call.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="9be99-201">Hämta den tilldelade IP-adressen</span><span class="sxs-lookup"><span data-stu-id="9be99-201">Obtaining the assigned IP address</span></span>

<span data-ttu-id="9be99-202">Om du vill göra något med den nya virtuella datorn så behöver du den tilldelade IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="9be99-202">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="9be99-203">IP-adresser har egna separata Azure-resurser som är kopplade till nätverkskortresurser (NIC).</span><span class="sxs-lookup"><span data-stu-id="9be99-203">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="9be99-204">Den här metoden är beroende av information som lagras i parameterfilen.</span><span class="sxs-lookup"><span data-stu-id="9be99-204">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="9be99-205">Koden kan fråga den virtuella datorn direkt för att få dess nätverkskort, fråga nätverkskortet för att få dess IP-resurs och sedan fråga IP-resursen direkt.</span><span class="sxs-lookup"><span data-stu-id="9be99-205">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="9be99-206">Det här är en lång kedja av beroenden och åtgärder att lösa, vilket gör att det blir dyrt.</span><span class="sxs-lookup"><span data-stu-id="9be99-206">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="9be99-207">Eftersom JSON-informationen finns lokalt kan den läsas in istället.</span><span class="sxs-lookup"><span data-stu-id="9be99-207">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="9be99-208">Värdena för VM-användaren och lösenordet kan också läsas in från JSON-filen.</span><span class="sxs-lookup"><span data-stu-id="9be99-208">The values for the VM user and password are likewise loaded from the JSON.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9be99-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9be99-209">Next steps</span></span>

<span data-ttu-id="9be99-210">I den här snabbstarten använde du en befintlig mall och distribuerade den via Go.</span><span class="sxs-lookup"><span data-stu-id="9be99-210">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="9be99-211">Sedan anslöt du den till den nyligen skapade virtuella datorn via SSH för att se till att den är aktiv.</span><span class="sxs-lookup"><span data-stu-id="9be99-211">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="9be99-212">Om du vill fortsätta att lära dig om hur du arbetar med virtuella datorer i Azure-miljön med Go så kan du ta en titt på [beräkningsexemplen för Go i Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) eller [exemplen på resurshantering för Go i Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="9be99-212">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>
