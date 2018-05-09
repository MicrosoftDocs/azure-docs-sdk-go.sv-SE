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
ms.openlocfilehash: 2ea44fb8a4fdd6098bb44d3b5092cfbc352b424d
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/03/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Verktyg för utvecklare som använder Azure SDK för Go

Här är några rekommenderade verktyg för att effektivt skriva kod i Go och få det att fungera sömlöst med Azure-tjänster.

## <a name="azure-cli-20"></a>Azure CLI 2.0

Azure CLI 2.0 tillhandahåller ett kommandoradsgränssnitt för att skapa och konfigurera Azure-resurser i dina prenumerationer. CLI kan hjälpa dig att komma igång med att snabbt skapa vanliga och delade Azure-resurser så att du kan fokusera på mer avancerad användning av tjänster. CLI har funktioner för frågor och filtrering som gör att du kan skicka utdata direkt till dina favoritkommandoradsverktyg. CLI är tillgängligt för installation på din lokala dator, som en Docker-avbildning eller via [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Installera Azure CLI 2.0](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio-koden

Visual Studio Code är ett enkelt redigeringsprogram som har omfattande stöd för språket Go med hjälp av tillägg. De här tilläggen har stöd för funktioner som automatisk komplettering, `impl`-mallar, refaktorisering och felsökning. Visual Studio Code erbjuder även många tillägg för vanliga utvecklingsverktyg som till exempel källkodskontroll och erbjuder även tillägg för direkt interaktion med Azure-tjänster. Microsoft har ett officiellt metatillägg som innefattar de här Azure-tilläggen och även ett interaktivt gränssnitt för Azure CLI.

* [Installera Visual Studio Code](https://code.visualstudio.com/Download)
* [Hämta Go-tillägget för Visual Studio Code](https://code.visualstudio.com/docs/languages/go)
* [Hämta tillägget för Azure-verktyg](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a>Beroendehantering med dep

Det finns många sätt att hantera dina paketberoenden och utföra vendoring med Go eftersom det inte finns någon officiell lösning ännu. Det rekommenderade sättet att utföra den här hanteringen är med beroendehanteraren `dep`. Azure SDK för Go använder dep för vendoring och hämtar beroenden korrekt för alla andra projekt med hjälp av dep.

> [!div class="nextstepaction"]
> [Hämta dep-beroendehanteraren](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a>Telemetri med Application Insights

[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) är en analysprodukt som gör att du enkelt kan samla in telemetri från dina program och kan integrera dem med Azure-ekosystemet, Visual Studio Team Services och GitHub. Det kan användas i många program och Microsoft erbjuder en Go-SDK för att arbeta med Application Insights.

> [!div class="nextstepaction"]
> [Hämta Application Insights för Go-SDK](https://github.com/Microsoft/ApplicationInsights-Go) 
