---
title: Installera Azure SDK för Go
description: Anvisningar om hur du installerar, utför vendoring och konfigurerar Azure SDK för Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 013a771345d96f0fa8dbece3218a01650744f70b
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059194"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="49e2f-103">Installera Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="49e2f-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="49e2f-104">Välkommen till Azure SDK för Go!</span><span class="sxs-lookup"><span data-stu-id="49e2f-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="49e2f-105">Med detta SDK kan du hantera och interagera med Azure-tjänster från Go-program.</span><span class="sxs-lookup"><span data-stu-id="49e2f-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="49e2f-106">Hämta Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="49e2f-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="49e2f-107">Vissa Azure-tjänster har sitt eget Go-SDK och ingår inte i Azure SDK for Go-grundpaketet.</span><span class="sxs-lookup"><span data-stu-id="49e2f-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="49e2f-108">Följande tabell innehåller en lista över tjänsterna med egna SDK:er och deras paketnamn.</span><span class="sxs-lookup"><span data-stu-id="49e2f-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="49e2f-109">Alla dessa paket anses utgöra en förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="49e2f-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="49e2f-110">Tjänst</span><span class="sxs-lookup"><span data-stu-id="49e2f-110">Service</span></span> | <span data-ttu-id="49e2f-111">Paket</span><span class="sxs-lookup"><span data-stu-id="49e2f-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="49e2f-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="49e2f-112">Blob Storage</span></span> | [<span data-ttu-id="49e2f-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="49e2f-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="49e2f-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="49e2f-114">File Storage</span></span> | [<span data-ttu-id="49e2f-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="49e2f-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="49e2f-116">Lagringskö</span><span class="sxs-lookup"><span data-stu-id="49e2f-116">Storage Queue</span></span> | [<span data-ttu-id="49e2f-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="49e2f-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="49e2f-118">Händelsehubb</span><span class="sxs-lookup"><span data-stu-id="49e2f-118">Event Hub</span></span> | [<span data-ttu-id="49e2f-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="49e2f-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="49e2f-120">Service Bus</span><span class="sxs-lookup"><span data-stu-id="49e2f-120">Service Bus</span></span> | [<span data-ttu-id="49e2f-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="49e2f-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="49e2f-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="49e2f-122">Application Insights</span></span> | [<span data-ttu-id="49e2f-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="49e2f-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="49e2f-124">Vendoring i Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="49e2f-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="49e2f-125">Du kan utföra vendoring för Azure SDK för Go via [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="49e2f-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="49e2f-126">Vendoring rekommenderas av stabilitetsskäl.</span><span class="sxs-lookup"><span data-stu-id="49e2f-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="49e2f-127">Om du vill använda `dep` i ditt projekt lägger du till `github.com/Azure/azure-sdk-for-go` i ett `[[constraint]]`-avsnitt i din `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="49e2f-127">To use `dep` in your own project, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="49e2f-128">Om du till exempel vill utföra vendoring för version `14.0.0` lägger du till följande post:</span><span class="sxs-lookup"><span data-stu-id="49e2f-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="49e2f-129">Ta med Azure SDK för Go i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="49e2f-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="49e2f-130">Om du vill använda Azure-tjänster från din Go-kod importerar du alla tjänster som du interagerar med samt de nödvändiga `autorest`-modulerna.</span><span class="sxs-lookup"><span data-stu-id="49e2f-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="49e2f-131">Du får en fullständig lista över tillgängliga moduler från GoDoc för [tillgängliga tjänster](https://godoc.org/github.com/Azure/azure-sdk-for-go) och [AutoRest-paket](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="49e2f-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="49e2f-132">De vanligaste paketen som du behöver från `go-autorest` är:</span><span class="sxs-lookup"><span data-stu-id="49e2f-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="49e2f-133">Paket</span><span class="sxs-lookup"><span data-stu-id="49e2f-133">Package</span></span> | <span data-ttu-id="49e2f-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="49e2f-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="49e2f-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="49e2f-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="49e2f-136">Objekt för hantering av tjänstklientautentisering</span><span class="sxs-lookup"><span data-stu-id="49e2f-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="49e2f-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="49e2f-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="49e2f-138">Konstanter för interaktioner med Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="49e2f-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="49e2f-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="49e2f-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="49e2f-140">Autentiseringsmekanismer för åtkomst till Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="49e2f-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="49e2f-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="49e2f-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="49e2f-142">Ange kontrollhjälp för att arbeta med Azure SDK-datastrukturer</span><span class="sxs-lookup"><span data-stu-id="49e2f-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="49e2f-143">Versioner av Go-paket och Azure-tjänster är oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="49e2f-143">Go packages and Azure services are versioned independently.</span></span> <span data-ttu-id="49e2f-144">Tjänstversionerna är en del av importsökvägen för modulen nedanför modulen `services`.</span><span class="sxs-lookup"><span data-stu-id="49e2f-144">The service versions are part of the module import path, underneath the `services` module.</span></span> <span data-ttu-id="49e2f-145">Den fullständiga sökvägen för modulen är namnet på tjänsten, följt av versionen i formatet `YYYY-MM-DD`, följt av namnet på tjänsten igen.</span><span class="sxs-lookup"><span data-stu-id="49e2f-145">The full path for the module is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="49e2f-146">Så här importerar du till exempel versionen `2017-03-30` av beräkningstjänsten:</span><span class="sxs-lookup"><span data-stu-id="49e2f-146">For example, to import the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="49e2f-147">Vi rekommenderar att du använder den senaste versionen av en tjänst när du börjar utveckla och är konsekvent.</span><span class="sxs-lookup"><span data-stu-id="49e2f-147">It's recommended that you use the latest version of a service when starting development and keep it consistent.</span></span>
<span data-ttu-id="49e2f-148">Tjänstekrav kan ändras från en version till nästa och det kan bryta din kod, även om det inte förekommer Go SDK-uppdateringar under den tiden.</span><span class="sxs-lookup"><span data-stu-id="49e2f-148">Service requirements may change between versions that could break your code, even if there are no Go SDK updates during that time.</span></span>

<span data-ttu-id="49e2f-149">Du kan även välja en enskild profilversion om du behöver en kollektiv ögonblicksbild av tjänsterna.</span><span class="sxs-lookup"><span data-stu-id="49e2f-149">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="49e2f-150">Just nu är den enda låsta profilen version `2017-03-09`, som kanske inte har de senaste funktionerna för tjänsterna.</span><span class="sxs-lookup"><span data-stu-id="49e2f-150">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="49e2f-151">Profilerna finns under modulen `profiles` med versionerna i formatet `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="49e2f-151">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="49e2f-152">Tjänsterna är grupperade under profilversionerna.</span><span class="sxs-lookup"><span data-stu-id="49e2f-152">Services are grouped under their profile version.</span></span> <span data-ttu-id="49e2f-153">Så här importerar du till exempel hanteringsmodulen för Azure-resurser från profilen `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="49e2f-153">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="49e2f-154">Även profilerna `preview` och `latest` är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="49e2f-154">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="49e2f-155">Vi rekommenderar inte att du använder dem.</span><span class="sxs-lookup"><span data-stu-id="49e2f-155">Using them is not recommended.</span></span> <span data-ttu-id="49e2f-156">De här profilerna är löpande versioner och tjänstbeteendet kan därför ändras när som helst.</span><span class="sxs-lookup"><span data-stu-id="49e2f-156">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="49e2f-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49e2f-157">Next steps</span></span>

<span data-ttu-id="49e2f-158">Testa att använda en snabbstart om du vill börja använda Azure SDK för Go.</span><span class="sxs-lookup"><span data-stu-id="49e2f-158">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="49e2f-159">Distribuera en virtuell dator från en mall</span><span class="sxs-lookup"><span data-stu-id="49e2f-159">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="49e2f-160">Överföra objekt till Azure Blob Storage med hjälp av Azure Blob SDK för Go</span><span class="sxs-lookup"><span data-stu-id="49e2f-160">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="49e2f-161">Ansluta till Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="49e2f-161">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="49e2f-162">Om du vill komma igång med andra tjänster i Go SDK direkt så kan du ta en titt på några av de tillgängliga exempelkoderna.</span><span class="sxs-lookup"><span data-stu-id="49e2f-162">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="49e2f-163">Autentisera med Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="49e2f-163">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="49e2f-164">Distribuera nya virtuella datorer med SSH-autentisering</span><span class="sxs-lookup"><span data-stu-id="49e2f-164">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="49e2f-165">Distribuera en behållaravbildning till Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="49e2f-165">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="49e2f-166">Skapa ett kluster i Azure Kubernetes Service</span><span class="sxs-lookup"><span data-stu-id="49e2f-166">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="49e2f-167">Arbeta med Azure Storage-tjänster</span><span class="sxs-lookup"><span data-stu-id="49e2f-167">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="49e2f-168">Alla exempel för Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="49e2f-168">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
