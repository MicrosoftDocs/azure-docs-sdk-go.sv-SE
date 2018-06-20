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
ms.openlocfilehash: 3388359bba791c87025b6ffd0e6b476f95589f73
ms.sourcegitcommit: 81e97407e6139375bf7357045e818c87a17dcde1
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36262996"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="3e2b8-103">Installera Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="3e2b8-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="3e2b8-104">Välkommen till Azure SDK för Go!</span><span class="sxs-lookup"><span data-stu-id="3e2b8-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="3e2b8-105">Med detta SDK kan du hantera och interagera med Azure-tjänster från Go-program.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="3e2b8-106">Hämta Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="3e2b8-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="3e2b8-107">Vissa Azure-tjänster har sitt eget Go-SDK och ingår inte i Azure SDK for Go-grundpaketet.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="3e2b8-108">Följande tabell innehåller en lista över tjänsterna med egna SDK:er och deras paketnamn.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="3e2b8-109">Alla dessa paket anses utgöra en förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="3e2b8-110">Tjänst</span><span class="sxs-lookup"><span data-stu-id="3e2b8-110">Service</span></span> | <span data-ttu-id="3e2b8-111">Paket</span><span class="sxs-lookup"><span data-stu-id="3e2b8-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="3e2b8-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3e2b8-112">Blob Storage</span></span> | [<span data-ttu-id="3e2b8-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="3e2b8-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="3e2b8-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="3e2b8-114">File Storage</span></span> | [<span data-ttu-id="3e2b8-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="3e2b8-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="3e2b8-116">Lagringskö</span><span class="sxs-lookup"><span data-stu-id="3e2b8-116">Storage Queue</span></span> | [<span data-ttu-id="3e2b8-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="3e2b8-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="3e2b8-118">Händelsehubb</span><span class="sxs-lookup"><span data-stu-id="3e2b8-118">Event Hub</span></span> | [<span data-ttu-id="3e2b8-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="3e2b8-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="3e2b8-120">Service Bus</span><span class="sxs-lookup"><span data-stu-id="3e2b8-120">Service Bus</span></span> | [<span data-ttu-id="3e2b8-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="3e2b8-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="3e2b8-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="3e2b8-122">Application Insights</span></span> | [<span data-ttu-id="3e2b8-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="3e2b8-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="3e2b8-124">Vendoring i Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="3e2b8-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="3e2b8-125">Du kan utföra vendoring för Azure SDK för Go via [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="3e2b8-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="3e2b8-126">Vendoring rekommenderas av stabilitetsskäl.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="3e2b8-127">Om du vill använda stöd för `dep` lägger du till `github.com/Azure/azure-sdk-for-go` i ett `[[constraint]]`-avsnitt i din `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-127">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="3e2b8-128">Om du till exempel vill utföra vendoring för version `14.0.0` lägger du till följande post:</span><span class="sxs-lookup"><span data-stu-id="3e2b8-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="3e2b8-129">Ta med Azure SDK för Go i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="3e2b8-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="3e2b8-130">Om du vill använda Azure-tjänster från din Go-kod importerar du alla tjänster som du interagerar med samt de nödvändiga `autorest`-modulerna.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="3e2b8-131">Du får en fullständig lista över tillgängliga moduler från GoDoc för [tillgängliga tjänster](https://godoc.org/github.com/Azure/azure-sdk-for-go) och [AutoRest-paket](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="3e2b8-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="3e2b8-132">De vanligaste paketen som du behöver från `go-autorest` är:</span><span class="sxs-lookup"><span data-stu-id="3e2b8-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="3e2b8-133">Paket</span><span class="sxs-lookup"><span data-stu-id="3e2b8-133">Package</span></span> | <span data-ttu-id="3e2b8-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3e2b8-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="3e2b8-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="3e2b8-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="3e2b8-136">Objekt för hantering av tjänstklientautentisering</span><span class="sxs-lookup"><span data-stu-id="3e2b8-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="3e2b8-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="3e2b8-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="3e2b8-138">Konstanter för interaktioner med Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="3e2b8-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="3e2b8-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="3e2b8-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="3e2b8-140">Autentiseringsmekanismer för åtkomst till Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="3e2b8-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="3e2b8-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="3e2b8-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="3e2b8-142">Ange kontrollhjälp för att arbeta med Azure SDK-datastrukturer</span><span class="sxs-lookup"><span data-stu-id="3e2b8-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="3e2b8-143">Versioner av moduler för Azure-tjänster är oberoende av deras SDK-API:er.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-143">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="3e2b8-144">De här versionerna är en del av importsökvägen för modulen och kommer antingen från en _tjänstversion_ eller en _profil_.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-144">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="3e2b8-145">För närvarande rekommenderar vi att du använder en specifik tjänstversion för både utveckling och utgivning.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-145">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="3e2b8-146">Tjänster finns under modulen `services`.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-146">Services are located under the `services` module.</span></span> <span data-ttu-id="3e2b8-147">Den fullständiga sökvägen för importen är namnet på tjänsten, följt av versionen i formatet `YYYY-MM-DD`, följt av namnet på tjänsten igen.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-147">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="3e2b8-148">Så här inkluderar du till exempel versionen `2017-03-30` av beräkningstjänsten:</span><span class="sxs-lookup"><span data-stu-id="3e2b8-148">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="3e2b8-149">Just nu rekommenderar vi att du använder den senaste versionen av en tjänst om du inte har en bra anledning att inte göra det.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-149">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="3e2b8-150">Du kan även välja en enskild profilversion om du behöver en kollektiv ögonblicksbild av tjänsterna.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-150">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="3e2b8-151">Just nu är den enda låsta profilen version `2017-03-09`, som kanske inte har de senaste funktionerna för tjänsterna.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-151">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="3e2b8-152">Profilerna finns under modulen `profiles` med versionerna i formatet `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-152">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="3e2b8-153">Tjänsterna är grupperade under profilversionerna.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-153">Services are grouped under their profile version.</span></span> <span data-ttu-id="3e2b8-154">Så här importerar du till exempel hanteringsmodulen för Azure-resurser från profilen `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="3e2b8-154">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="3e2b8-155">Även profilerna `preview` och `latest` är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-155">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="3e2b8-156">Vi rekommenderar inte att du använder dem.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-156">Using them is not recommended.</span></span> <span data-ttu-id="3e2b8-157">De här profilerna är löpande versioner och tjänstbeteendet kan därför ändras när som helst.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-157">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e2b8-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e2b8-158">Next steps</span></span>

<span data-ttu-id="3e2b8-159">Testa att använda en snabbstart om du vill börja använda Azure SDK för Go.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-159">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="3e2b8-160">Distribuera en virtuell dator från en mall</span><span class="sxs-lookup"><span data-stu-id="3e2b8-160">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="3e2b8-161">Överföra objekt till Azure Blob Storage med hjälp av Azure Blob SDK för Go</span><span class="sxs-lookup"><span data-stu-id="3e2b8-161">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="3e2b8-162">Ansluta till Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="3e2b8-162">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="3e2b8-163">Om du vill komma igång med andra tjänster i Go SDK direkt så kan du ta en titt på några av de tillgängliga exempelkoderna.</span><span class="sxs-lookup"><span data-stu-id="3e2b8-163">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="3e2b8-164">Autentisera med Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="3e2b8-164">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="3e2b8-165">Distribuera nya virtuella datorer med SSH-autentisering</span><span class="sxs-lookup"><span data-stu-id="3e2b8-165">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="3e2b8-166">Distribuera en behållaravbildning till Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="3e2b8-166">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="3e2b8-167">Skapa ett kluster i Azure Kubernetes Service</span><span class="sxs-lookup"><span data-stu-id="3e2b8-167">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="3e2b8-168">Arbeta med Azure Storage-tjänster</span><span class="sxs-lookup"><span data-stu-id="3e2b8-168">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="3e2b8-169">Alla exempel för Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="3e2b8-169">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
