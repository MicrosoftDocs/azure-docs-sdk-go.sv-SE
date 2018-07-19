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
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Verktyg för utvecklare som använder Azure SDK för Go

Här är några rekommenderade verktyg för att effektivt skriva kod i Go och få det att fungera sömlöst med Azure-tjänster.

## <a name="azure-cli"></a>Azure CLI

Azure CLI tillhandahåller ett kommandoradsgränssnitt för att skapa och konfigurera Azure-resurser i dina prenumerationer. CLI kan hjälpa dig att komma igång med att snabbt skapa vanliga och delade Azure-resurser så att du kan fokusera på mer avancerad användning av tjänster. CLI har funktioner för frågor och filtrering som gör att du kan skicka utdata direkt till dina favoritkommandoradsverktyg. CLI är tillgängligt för installation på din lokala dator, som en Docker-avbildning eller via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Installera Azure CLI](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio-koden

Visual Studio Code är ett enkelt redigeringsprogram som har omfattande stöd för språket Go med hjälp av tillägg. De här tilläggen har stöd för funktioner som automatisk komplettering, `impl`-mallar, refaktorisering och felsökning. Visual Studio Code erbjuder även många tillägg för vanliga utvecklingsverktyg som till exempel källkodskontroll och erbjuder även tillägg för direkt interaktion med Azure-tjänster. Microsoft har ett officiellt metatillägg som innefattar de här Azure-tilläggen och även ett interaktivt gränssnitt för Azure CLI.

* [Installera Visual Studio Code](https://code.visualstudio.com/Download)
* [Hämta Go-tillägget för Visual Studio Code](https://code.visualstudio.com/docs/languages/go)
* [Hämta tillägget för Azure-verktyg](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>CI/CD med Azure DevOps Projects

Med en Azure DevOps Projects-pipeline kan du konfigurera en kontinuerlig version och distribuera den till dina Go-program. Allt du behöver är en tillgänglig Git-lagringsplats, och du kan konfigurera för att distribuera och testa direkt på dina Azure-resurser. Det är enkelt att skapa och hantera en konfigurationspipeline, och eftersom den etableras direkt i Azure du kan styra den på samma sätt som du hanterar andra Azure-resurser.

> [!div class="nextstepaction"]
> [Lär dig att skapa en CI/CD-pipeline med Azure DevOps Projects](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Beroendehantering med dep

Det finns många sätt att hantera dina paketberoenden och utföra vendoring med Go eftersom det inte finns någon officiell lösning ännu. Det rekommenderade sättet att utföra den här hanteringen är med beroendehanteraren `dep`. Azure SDK för Go använder dep för vendoring och hämtar beroenden korrekt för alla andra projekt med hjälp av dep.

> [!div class="nextstepaction"]
> [Hämta dep-beroendehanteraren](https://github.com/golang/dep)
