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
ms.openlocfilehash: 2799e3a6c637036eeaf7b20adf8aa55a8a4ab400
ms.sourcegitcommit: 4db332f5e43a5b43032ff9017805d5fd5a650d86
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55145540"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="28846-103">Installera Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="28846-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="28846-104">Välkommen till Azure SDK för Go!</span><span class="sxs-lookup"><span data-stu-id="28846-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="28846-105">Med detta SDK kan du hantera och interagera med Azure-tjänster från Go-program.</span><span class="sxs-lookup"><span data-stu-id="28846-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="28846-106">Hämta Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="28846-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="28846-107">Vissa Azure-tjänster har sitt eget Go-SDK och ingår inte i Azure SDK for Go-grundpaketet.</span><span class="sxs-lookup"><span data-stu-id="28846-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="28846-108">Följande tabell innehåller en lista över tjänsterna med egna SDK:er och deras paketnamn.</span><span class="sxs-lookup"><span data-stu-id="28846-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="28846-109">Alla dessa paket anses utgöra en förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="28846-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="28846-110">Tjänst</span><span class="sxs-lookup"><span data-stu-id="28846-110">Service</span></span> | <span data-ttu-id="28846-111">Paket</span><span class="sxs-lookup"><span data-stu-id="28846-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="28846-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="28846-112">Blob Storage</span></span> | [<span data-ttu-id="28846-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="28846-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="28846-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="28846-114">File Storage</span></span> | [<span data-ttu-id="28846-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="28846-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="28846-116">Lagringskö</span><span class="sxs-lookup"><span data-stu-id="28846-116">Storage Queue</span></span> | [<span data-ttu-id="28846-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="28846-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="28846-118">Händelsehubb</span><span class="sxs-lookup"><span data-stu-id="28846-118">Event Hub</span></span> | [<span data-ttu-id="28846-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="28846-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="28846-120">Service Bus</span><span class="sxs-lookup"><span data-stu-id="28846-120">Service Bus</span></span> | [<span data-ttu-id="28846-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="28846-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="28846-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="28846-122">Application Insights</span></span> | [<span data-ttu-id="28846-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="28846-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="28846-124">Vendoring i Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="28846-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="28846-125">Du kan utföra vendoring för Azure SDK för Go via [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="28846-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="28846-126">Vendoring rekommenderas av stabilitetsskäl.</span><span class="sxs-lookup"><span data-stu-id="28846-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="28846-127">Om du vill använda `dep` i ditt projekt lägger du till `github.com/Azure/azure-sdk-for-go` i ett `[[constraint]]`-avsnitt i din `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="28846-127">To use `dep` in your own project, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="28846-128">Om du till exempel vill utföra vendoring för version `14.0.0` lägger du till följande post:</span><span class="sxs-lookup"><span data-stu-id="28846-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="28846-129">Ta med Azure SDK för Go i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="28846-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="28846-130">Om du vill använda Azure-tjänster från din Go-kod importerar du alla tjänster som du interagerar med samt de nödvändiga `autorest`-modulerna.</span><span class="sxs-lookup"><span data-stu-id="28846-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="28846-131">Du får en fullständig lista över tillgängliga moduler från GoDoc för [tillgängliga tjänster](https://godoc.org/github.com/Azure/azure-sdk-for-go) och [AutoRest-paket](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="28846-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="28846-132">De vanligaste paketen som du behöver från `go-autorest` är:</span><span class="sxs-lookup"><span data-stu-id="28846-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="28846-133">Paket</span><span class="sxs-lookup"><span data-stu-id="28846-133">Package</span></span> | <span data-ttu-id="28846-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="28846-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="28846-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="28846-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="28846-136">Objekt för hantering av tjänstklientautentisering</span><span class="sxs-lookup"><span data-stu-id="28846-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="28846-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="28846-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="28846-138">Konstanter för interaktioner med Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="28846-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="28846-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="28846-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="28846-140">Autentiseringsmekanismer för åtkomst till Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="28846-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="28846-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="28846-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="28846-142">Ange kontrollhjälp för att arbeta med Azure SDK-datastrukturer</span><span class="sxs-lookup"><span data-stu-id="28846-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="28846-143">Versioner av Go-paket och Azure-tjänster är oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="28846-143">Go packages and Azure services are versioned independently.</span></span> <span data-ttu-id="28846-144">Tjänstversionerna är en del av importsökvägen för modulen nedanför modulen `services`.</span><span class="sxs-lookup"><span data-stu-id="28846-144">The service versions are part of the module import path, underneath the `services` module.</span></span> <span data-ttu-id="28846-145">Den fullständiga sökvägen för modulen är namnet på tjänsten, följt av versionen i formatet `YYYY-MM-DD`, följt av namnet på tjänsten igen.</span><span class="sxs-lookup"><span data-stu-id="28846-145">The full path for the module is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="28846-146">Så här importerar du till exempel versionen `2017-03-30` av beräkningstjänsten:</span><span class="sxs-lookup"><span data-stu-id="28846-146">For example, to import the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="28846-147">Vi rekommenderar att du använder den senaste versionen av en tjänst när du börjar utveckla och är konsekvent.</span><span class="sxs-lookup"><span data-stu-id="28846-147">It's recommended that you use the latest version of a service when starting development and keep it consistent.</span></span>
<span data-ttu-id="28846-148">Tjänstekrav kan ändras från en version till nästa och det kan bryta din kod, även om det inte förekommer Go SDK-uppdateringar under den tiden.</span><span class="sxs-lookup"><span data-stu-id="28846-148">Service requirements may change between versions that could break your code, even if there are no Go SDK updates during that time.</span></span>

<span data-ttu-id="28846-149">Du kan även välja en enskild profilversion om du behöver en kollektiv ögonblicksbild av tjänsterna.</span><span class="sxs-lookup"><span data-stu-id="28846-149">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="28846-150">Just nu är den enda låsta profilen version `2017-03-09`, som kanske inte har de senaste funktionerna för tjänsterna.</span><span class="sxs-lookup"><span data-stu-id="28846-150">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="28846-151">Profilerna finns under modulen `profiles` med versionerna i formatet `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="28846-151">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="28846-152">Tjänsterna är grupperade under profilversionerna.</span><span class="sxs-lookup"><span data-stu-id="28846-152">Services are grouped under their profile version.</span></span> <span data-ttu-id="28846-153">Så här importerar du till exempel hanteringsmodulen för Azure-resurser från profilen `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="28846-153">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="28846-154">Även profilerna `preview` och `latest` är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="28846-154">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="28846-155">Vi rekommenderar inte att du använder dem.</span><span class="sxs-lookup"><span data-stu-id="28846-155">Using them is not recommended.</span></span> <span data-ttu-id="28846-156">De här profilerna är löpande versioner och tjänstbeteendet kan därför ändras när som helst.</span><span class="sxs-lookup"><span data-stu-id="28846-156">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28846-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28846-157">Next steps</span></span>

<span data-ttu-id="28846-158">Testa att använda en snabbstart om du vill börja använda Azure SDK för Go.</span><span class="sxs-lookup"><span data-stu-id="28846-158">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="28846-159">Distribuera en virtuell dator från en mall</span><span class="sxs-lookup"><span data-stu-id="28846-159">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="28846-160">Överföra objekt till Azure Blob Storage med hjälp av Azure Blob SDK för Go</span><span class="sxs-lookup"><span data-stu-id="28846-160">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="28846-161">Ansluta till Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="28846-161">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="28846-162">Om du vill komma igång med andra tjänster i Go SDK direkt så kan du ta en titt på några av de tillgängliga exempelkoderna.</span><span class="sxs-lookup"><span data-stu-id="28846-162">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="28846-163">Autentisera med Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="28846-163">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [<span data-ttu-id="28846-164">Distribuera nya virtuella datorer med SSH-autentisering</span><span class="sxs-lookup"><span data-stu-id="28846-164">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="28846-165">Distribuera en behållaravbildning till Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="28846-165">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="28846-166">Skapa ett kluster i Azure Kubernetes Service</span><span class="sxs-lookup"><span data-stu-id="28846-166">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute)
* [<span data-ttu-id="28846-167">Arbeta med Azure Storage-tjänster</span><span class="sxs-lookup"><span data-stu-id="28846-167">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="28846-168">Alla exempel för Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="28846-168">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
