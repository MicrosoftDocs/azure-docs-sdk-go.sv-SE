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
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Verktyg för utvecklare som använder Azure SDK för Go

Här är några rekommenderade verktyg för att effektivt skriva kod i Go och få det att fungera sömlöst med Azure-tjänster.

## <a name="azure-cli"></a>Azure CLI

Azure CLI tillhandahåller ett kommandoradsgränssnitt för att skapa och konfigurera Azure-resurser i dina prenumerationer. CLI kan hjälpa dig att komma igång med att snabbt skapa vanliga och delade Azure-resurser så att du kan fokusera på mer avancerad användning av tjänster. CLI har funktioner för frågor och filtrering som gör att du kan skicka utdata direkt till dina favoritkommandoradsverktyg. CLI är tillgängligt för installation på din lokala dator, som en Docker-avbildning eller via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Installera Azure CLI](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio-koden

Visual Studio Code är ett enkelt redigeringsprogram med stöd för Go. Det här tillägget har stöd för funktioner som automatisk komplettering, `impl`-mallar, refaktorisering och felsökning. Visual Studio Code har även stöd för åtkomst till källkontroll i redigeraren och tillägg för att arbeta med Azure-tjänster.

* [Installera Visual Studio Code](https://code.visualstudio.com/Download)
* [Hämta Go-tillägget för Visual Studio Code](https://code.visualstudio.com/docs/languages/go)
* [Hämta Azure Tools-tillägget för Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>CI/CD med Azure DevOps Projects

Med en Azure DevOps Project-pipeline kan du konfigurera ett system för kontinuerlig integrering för dina Go-program. Allt som behövs är en Git-lagringsplats för att distribuera och testa direkt på Azure.

> [!div class="nextstepaction"]
> [Lär dig att skapa en CI/CD-pipeline med Azure DevOps Projects](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Beroendehantering med dep

Azure SDK för Go hanterar beroenden med hjälp av dep. Med kommandot dep kan du hämta och utföra vendoring för dina Go-programkrav, undvika versionskonflikter och se till att ditt projekt fungerar korrekt.

> [!div class="nextstepaction"]
> [Hämta dep-beroendehanteraren](https://github.com/golang/dep)
