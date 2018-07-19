---
title: Verktyg för Go-utvecklare
description: Verktyg för att arbeta med Azure SDK för Go och Azure-tjänster
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 07/13/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: dfa3912ac13e6f6d52d607f9dcc150f3a5b57602
ms.sourcegitcommit: d1790b317a8fcb4d672c654dac2a925a976589d4
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39039513"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="fd27b-103">Verktyg för utvecklare som använder Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="fd27b-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="fd27b-104">Här är några rekommenderade verktyg för att effektivt skriva kod i Go och få det att fungera sömlöst med Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="fd27b-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="fd27b-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fd27b-105">Azure CLI</span></span>

<span data-ttu-id="fd27b-106">Azure CLI tillhandahåller ett kommandoradsgränssnitt för att skapa och konfigurera Azure-resurser i dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="fd27b-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="fd27b-107">CLI kan hjälpa dig att komma igång med att snabbt skapa vanliga och delade Azure-resurser så att du kan fokusera på mer avancerad användning av tjänster.</span><span class="sxs-lookup"><span data-stu-id="fd27b-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="fd27b-108">CLI har funktioner för frågor och filtrering som gör att du kan skicka utdata direkt till dina favoritkommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="fd27b-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="fd27b-109">CLI är tillgängligt för installation på din lokala dator, som en Docker-avbildning eller via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="fd27b-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fd27b-110">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fd27b-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="fd27b-111">Visual Studio-koden</span><span class="sxs-lookup"><span data-stu-id="fd27b-111">Visual Studio Code</span></span>

<span data-ttu-id="fd27b-112">Visual Studio Code är ett enkelt redigeringsprogram som har omfattande stöd för språket Go med hjälp av tillägg.</span><span class="sxs-lookup"><span data-stu-id="fd27b-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="fd27b-113">De här tilläggen har stöd för funktioner som automatisk komplettering, `impl`-mallar, refaktorisering och felsökning.</span><span class="sxs-lookup"><span data-stu-id="fd27b-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="fd27b-114">Visual Studio Code erbjuder även många tillägg för vanliga utvecklingsverktyg som till exempel källkodskontroll och erbjuder även tillägg för direkt interaktion med Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="fd27b-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="fd27b-115">Microsoft har ett officiellt metatillägg som innefattar de här Azure-tilläggen och även ett interaktivt gränssnitt för Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="fd27b-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="fd27b-116">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fd27b-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="fd27b-117">Hämta Go-tillägget för Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fd27b-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="fd27b-118">Hämta tillägget för Azure-verktyg</span><span class="sxs-lookup"><span data-stu-id="fd27b-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="fd27b-119">CI/CD med Azure DevOps Projects</span><span class="sxs-lookup"><span data-stu-id="fd27b-119">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="fd27b-120">Med en Azure DevOps Projects-pipeline kan du konfigurera en kontinuerlig version och distribuera den till dina Go-program.</span><span class="sxs-lookup"><span data-stu-id="fd27b-120">With the Azure DevOps Project pipeline, you can set up a continuous build and deploy for your Go applications.</span></span> <span data-ttu-id="fd27b-121">Allt du behöver är en tillgänglig Git-lagringsplats, och du kan konfigurera för att distribuera och testa direkt på dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="fd27b-121">All you need is an available git repo, and you can set up to deploy and test directly on your Azure resources.</span></span> <span data-ttu-id="fd27b-122">Det är enkelt att skapa och hantera en konfigurationspipeline, och eftersom den etableras direkt i Azure du kan styra den på samma sätt som du hanterar andra Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="fd27b-122">The configuration pipeline is easy to create and manage, and since it's provisioned directly on Azure, you can control it in the same way that you handle your other Azure resources.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fd27b-123">Lär dig att skapa en CI/CD-pipeline med Azure DevOps Projects</span><span class="sxs-lookup"><span data-stu-id="fd27b-123">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="fd27b-124">Beroendehantering med dep</span><span class="sxs-lookup"><span data-stu-id="fd27b-124">Dependency management with dep</span></span>

<span data-ttu-id="fd27b-125">Det finns många sätt att hantera dina paketberoenden och utföra vendoring med Go eftersom det inte finns någon officiell lösning ännu.</span><span class="sxs-lookup"><span data-stu-id="fd27b-125">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="fd27b-126">Det rekommenderade sättet att utföra den här hanteringen är med beroendehanteraren `dep`.</span><span class="sxs-lookup"><span data-stu-id="fd27b-126">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="fd27b-127">Azure SDK för Go använder dep för vendoring och hämtar beroenden korrekt för alla andra projekt med hjälp av dep.</span><span class="sxs-lookup"><span data-stu-id="fd27b-127">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fd27b-128">Hämta dep-beroendehanteraren</span><span class="sxs-lookup"><span data-stu-id="fd27b-128">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
