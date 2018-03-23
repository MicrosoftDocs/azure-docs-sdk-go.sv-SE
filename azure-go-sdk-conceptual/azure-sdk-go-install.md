---
title: Installera Azure SDK för Go
description: Anvisningar om hur du installerar, utför vendoring och konfigurerar Azure SDK för Go.
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 580daf4f2e91eabf97e3acd21bda183c559b57da
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/23/2018
---
# <a name="installing-the-azure-sdk-for-go"></a><span data-ttu-id="98934-103">Installera Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="98934-103">Installing the Azure SDK for Go</span></span>

<span data-ttu-id="98934-104">Välkommen till Azure SDK för Go!</span><span class="sxs-lookup"><span data-stu-id="98934-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="98934-105">Med detta SDK kan du hantera och interagera med Azure-tjänster från Go-program.</span><span class="sxs-lookup"><span data-stu-id="98934-105">This SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="98934-106">Hämta Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="98934-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="98934-107">Om du vill arbeta med Azure Storage Blobs krävs ett separat SDK.</span><span class="sxs-lookup"><span data-stu-id="98934-107">Working with Azure Storage Blobs requires a separate SDK.</span></span>

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendoring-the-azure-sdk-for-go"></a><span data-ttu-id="98934-108">Utföra vendoring i Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="98934-108">Vendoring the Azure SDK for Go</span></span>

<span data-ttu-id="98934-109">Du kan utföra vendoring för Azure SDK för Go via [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="98934-109">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="98934-110">Vendoring rekommenderas av stabilitetsskäl.</span><span class="sxs-lookup"><span data-stu-id="98934-110">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="98934-111">Om du vill använda stöd för `dep` lägger du till `github.com/Azure/azure-sdk-for-go` i ett `[[constraint]]`-avsnitt i din `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="98934-111">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="98934-112">Om du till exempel vill utföra vendoring för version `14.0.0` lägger du till följande post:</span><span class="sxs-lookup"><span data-stu-id="98934-112">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="including-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="98934-113">Ta med Azure SDK för Go i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="98934-113">Including the Azure SDK for Go in your project</span></span>

<span data-ttu-id="98934-114">Om du vill använda Azure-tjänster från din Go-kod importerar du alla tjänster som du interagerar med samt de nödvändiga `autorest`-modulerna.</span><span class="sxs-lookup"><span data-stu-id="98934-114">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="98934-115">Du får en fullständig lista över tillgängliga moduler från GoDoc för [tillgängliga tjänster](https://godoc.org/github.com/Azure/azure-sdk-for-go) och [AutoRest-paket](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="98934-115">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="98934-116">De vanligaste paketen som du behöver från `go-autorest` är:</span><span class="sxs-lookup"><span data-stu-id="98934-116">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="98934-117">Paket</span><span class="sxs-lookup"><span data-stu-id="98934-117">Package</span></span> | <span data-ttu-id="98934-118">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="98934-118">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="98934-119">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="98934-119">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="98934-120">Objekt för hantering av tjänstklientautentisering</span><span class="sxs-lookup"><span data-stu-id="98934-120">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="98934-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="98934-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="98934-122">Konstanter för interaktioner med Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="98934-122">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="98934-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="98934-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="98934-124">Autentiseringsmekanismer för åtkomst till Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="98934-124">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="98934-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="98934-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="98934-126">Ange kontrollhjälp för att arbeta med Azure SDK-datastrukturer</span><span class="sxs-lookup"><span data-stu-id="98934-126">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="98934-127">Versioner av moduler för Azure-tjänster är oberoende av deras SDK-API:er.</span><span class="sxs-lookup"><span data-stu-id="98934-127">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="98934-128">De här versionerna är en del av importsökvägen för modulen och kommer antingen från en _tjänstversion_ eller en _profil_.</span><span class="sxs-lookup"><span data-stu-id="98934-128">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="98934-129">För närvarande rekommenderar vi att du använder en specifik tjänstversion för både utveckling och utgivning.</span><span class="sxs-lookup"><span data-stu-id="98934-129">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="98934-130">Tjänster finns under modulen `services`.</span><span class="sxs-lookup"><span data-stu-id="98934-130">Services are located under the `services` module.</span></span> <span data-ttu-id="98934-131">Den fullständiga sökvägen för importen är namnet på tjänsten, följt av versionen i formatet `YYYY-MM-DD`, följt av namnet på tjänsten igen.</span><span class="sxs-lookup"><span data-stu-id="98934-131">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="98934-132">Så här inkluderar du till exempel versionen `2017-03-30` av beräkningstjänsten:</span><span class="sxs-lookup"><span data-stu-id="98934-132">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="98934-133">Just nu rekommenderar vi att du använder den senaste versionen av en tjänst om du inte har en bra anledning att inte göra det.</span><span class="sxs-lookup"><span data-stu-id="98934-133">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="98934-134">Du kan även välja en enskild profilversion om du behöver en kollektiv ögonblicksbild av tjänsterna.</span><span class="sxs-lookup"><span data-stu-id="98934-134">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="98934-135">Just nu är den enda låsta profilen version `2017-03-30`, som kanske inte har de senaste funktionerna för tjänsterna.</span><span class="sxs-lookup"><span data-stu-id="98934-135">Right now, the only locked profile is version `2017-03-30`, which may not have the latest features of services.</span></span> <span data-ttu-id="98934-136">Profilerna finns under modulen `profiles` med versionerna i formatet `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="98934-136">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="98934-137">Tjänsterna är grupperade under profilversionerna.</span><span class="sxs-lookup"><span data-stu-id="98934-137">Services are grouped under their profile version.</span></span> <span data-ttu-id="98934-138">Så här importerar du till exempel hanteringsmodulen för Azure-resurser från profilen `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="98934-138">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="98934-139">Även profilerna `preview` och `latest` är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="98934-139">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="98934-140">Vi rekommenderar inte att du använder dem.</span><span class="sxs-lookup"><span data-stu-id="98934-140">Using them is not recommended.</span></span> <span data-ttu-id="98934-141">De här profilerna är löpande versioner och tjänstbeteendet kan därför ändras när som helst.</span><span class="sxs-lookup"><span data-stu-id="98934-141">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="98934-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="98934-142">Next steps</span></span>

<span data-ttu-id="98934-143">Testa att använda en snabbstart om du vill börja använda Azure SDK för Go.</span><span class="sxs-lookup"><span data-stu-id="98934-143">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="98934-144">Distribuera en virtuell dator från en mall</span><span class="sxs-lookup"><span data-stu-id="98934-144">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="98934-145">Överföra objekt till Azure Blob Storage med hjälp av Azure Blob SDK för Go</span><span class="sxs-lookup"><span data-stu-id="98934-145">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="98934-146">Ansluta till Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="98934-146">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="98934-147">Om du vill komma igång med andra tjänster i Go SDK direkt så kan du ta en titt på några av de tillgängliga exempelkoderna.</span><span class="sxs-lookup"><span data-stu-id="98934-147">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="98934-148">Autentisera med Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="98934-148">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="98934-149">Distribuera nya virtuella datorer med SSH-autentisering</span><span class="sxs-lookup"><span data-stu-id="98934-149">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="98934-150">Distribuera en behållaravbildning till Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="98934-150">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="98934-151">Skapa ett kluster i Azure Kubernetes Service</span><span class="sxs-lookup"><span data-stu-id="98934-151">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="98934-152">Arbeta med Azure Storage-tjänster</span><span class="sxs-lookup"><span data-stu-id="98934-152">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="98934-153">Alla exempel för Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="98934-153">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
