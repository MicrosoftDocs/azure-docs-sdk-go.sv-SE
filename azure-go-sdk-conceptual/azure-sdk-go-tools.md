---
title: Verktyg för utvecklare som använder Azure SDK för Go
description: Verktyg för att arbeta med Azure SDK för Go och Azure-tjänster
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 70cf7d645f47df29e8e42599a0acd75858144783
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059211"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="b903a-103">Verktyg för utvecklare som använder Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="b903a-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="b903a-104">Här är några rekommenderade verktyg för att effektivt skriva kod i Go och få det att fungera sömlöst med Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="b903a-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="b903a-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b903a-105">Azure CLI</span></span>

<span data-ttu-id="b903a-106">Azure CLI tillhandahåller ett kommandoradsgränssnitt för att skapa och konfigurera Azure-resurser i dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="b903a-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="b903a-107">CLI kan hjälpa dig att komma igång med att snabbt skapa vanliga och delade Azure-resurser så att du kan fokusera på mer avancerad användning av tjänster.</span><span class="sxs-lookup"><span data-stu-id="b903a-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="b903a-108">CLI har funktioner för frågor och filtrering som gör att du kan skicka utdata direkt till dina favoritkommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="b903a-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="b903a-109">CLI är tillgängligt för installation på din lokala dator, som en Docker-avbildning eller via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="b903a-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b903a-110">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b903a-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="b903a-111">Visual Studio-koden</span><span class="sxs-lookup"><span data-stu-id="b903a-111">Visual Studio Code</span></span>

<span data-ttu-id="b903a-112">Visual Studio Code är ett enkelt redigeringsprogram med stöd för Go.</span><span class="sxs-lookup"><span data-stu-id="b903a-112">Visual Studio Code is a lightweight editor that offers Go support.</span></span> <span data-ttu-id="b903a-113">Det här tillägget har stöd för funktioner som automatisk komplettering, `impl`-mallar, refaktorisering och felsökning.</span><span class="sxs-lookup"><span data-stu-id="b903a-113">This extension offers features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="b903a-114">Visual Studio Code har även stöd för åtkomst till källkontroll i redigeraren och tillägg för att arbeta med Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="b903a-114">Visual Studio Code also offers support for in-editor access to source control, and extensions for working with Azure services.</span></span>

* [<span data-ttu-id="b903a-115">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b903a-115">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="b903a-116">Hämta Go-tillägget för Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b903a-116">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="b903a-117">Hämta Azure Tools-tillägget för Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b903a-117">Get the Visual Studio Code Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="b903a-118">CI/CD med Azure DevOps Projects</span><span class="sxs-lookup"><span data-stu-id="b903a-118">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="b903a-119">Med en Azure DevOps Project-pipeline kan du konfigurera ett system för kontinuerlig integrering för dina Go-program.</span><span class="sxs-lookup"><span data-stu-id="b903a-119">Azure DevOps Project pipelines allow you to set up a continuous integration system for your Go applications.</span></span> <span data-ttu-id="b903a-120">Allt som behövs är en Git-lagringsplats för att distribuera och testa direkt på Azure.</span><span class="sxs-lookup"><span data-stu-id="b903a-120">All it takes is a git repo, and you can deploy and test directly on Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b903a-121">Lär dig att skapa en CI/CD-pipeline med Azure DevOps Projects</span><span class="sxs-lookup"><span data-stu-id="b903a-121">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="b903a-122">Beroendehantering med dep</span><span class="sxs-lookup"><span data-stu-id="b903a-122">Dependency management with dep</span></span>

<span data-ttu-id="b903a-123">Azure SDK för Go hanterar beroenden med hjälp av dep.</span><span class="sxs-lookup"><span data-stu-id="b903a-123">The Azure SDK for Go uses dep for dependency management.</span></span> <span data-ttu-id="b903a-124">Med kommandot dep kan du hämta och utföra vendoring för dina Go-programkrav, undvika versionskonflikter och se till att ditt projekt fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="b903a-124">The dep command allows you to pull and vendor requirements for your Go application, avoiding version conflicts and ensuring that your project works correctly.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b903a-125">Hämta dep-beroendehanteraren</span><span class="sxs-lookup"><span data-stu-id="b903a-125">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
