---
title: Verktyg för Go-utvecklare
description: Verktyg för att arbeta med Azure SDK för Go och Azure-tjänster
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 006d140bffb66fdd769a14511232d4ea5081811d
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38066990"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="bce20-103">Verktyg för utvecklare som använder Azure SDK för Go</span><span class="sxs-lookup"><span data-stu-id="bce20-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="bce20-104">Här är några rekommenderade verktyg för att effektivt skriva kod i Go och få det att fungera sömlöst med Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="bce20-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="bce20-105">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bce20-105">Azure CLI 2.0</span></span>

<span data-ttu-id="bce20-106">Azure CLI 2.0 tillhandahåller ett kommandoradsgränssnitt för att skapa och konfigurera Azure-resurser i dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="bce20-106">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="bce20-107">CLI kan hjälpa dig att komma igång med att snabbt skapa vanliga och delade Azure-resurser så att du kan fokusera på mer avancerad användning av tjänster.</span><span class="sxs-lookup"><span data-stu-id="bce20-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="bce20-108">CLI har funktioner för frågor och filtrering som gör att du kan skicka utdata direkt till dina favoritkommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="bce20-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="bce20-109">CLI är tillgängligt för installation på din lokala dator, som en Docker-avbildning eller via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="bce20-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bce20-110">Installera Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bce20-110">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="bce20-111">Visual Studio-koden</span><span class="sxs-lookup"><span data-stu-id="bce20-111">Visual Studio Code</span></span>

<span data-ttu-id="bce20-112">Visual Studio Code är ett enkelt redigeringsprogram som har omfattande stöd för språket Go med hjälp av tillägg.</span><span class="sxs-lookup"><span data-stu-id="bce20-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="bce20-113">De här tilläggen har stöd för funktioner som automatisk komplettering, `impl`-mallar, refaktorisering och felsökning.</span><span class="sxs-lookup"><span data-stu-id="bce20-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="bce20-114">Visual Studio Code erbjuder även många tillägg för vanliga utvecklingsverktyg som till exempel källkodskontroll och erbjuder även tillägg för direkt interaktion med Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="bce20-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="bce20-115">Microsoft har ett officiellt metatillägg som innefattar de här Azure-tilläggen och även ett interaktivt gränssnitt för Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="bce20-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="bce20-116">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bce20-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="bce20-117">Hämta Go-tillägget för Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bce20-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="bce20-118">Hämta tillägget för Azure-verktyg</span><span class="sxs-lookup"><span data-stu-id="bce20-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="bce20-119">Beroendehantering med dep</span><span class="sxs-lookup"><span data-stu-id="bce20-119">Dependency management with dep</span></span>

<span data-ttu-id="bce20-120">Det finns många sätt att hantera dina paketberoenden och utföra vendoring med Go eftersom det inte finns någon officiell lösning ännu.</span><span class="sxs-lookup"><span data-stu-id="bce20-120">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="bce20-121">Det rekommenderade sättet att utföra den här hanteringen är med beroendehanteraren `dep`.</span><span class="sxs-lookup"><span data-stu-id="bce20-121">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="bce20-122">Azure SDK för Go använder dep för vendoring och hämtar beroenden korrekt för alla andra projekt med hjälp av dep.</span><span class="sxs-lookup"><span data-stu-id="bce20-122">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bce20-123">Hämta dep-beroendehanteraren</span><span class="sxs-lookup"><span data-stu-id="bce20-123">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
