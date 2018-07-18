---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: d021dd8ef4744b7c50b296b231bf63481f92411a
ms.sourcegitcommit: 2a3bd491e087a1d0e7d269bed896c029357d62a6
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988084"
---
[Azure SDK för Go](https://github.com/Azure/azure-sdk-for-go) kan användas med Go-version 1.8 och senare. För miljöer med [Azure Stack-profiler](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles) är Go-versionen 1.9 minimikravet.
Om du inte har Go tillgängligt på datorn följer du [installationsanvisningarna för Go](https://golang.org/doc/install).

Du kan hämta Azure SDK för Go och dess beroenden via `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Kontrollera att du använder versaler för `Azure` i webbadressen. Om du inte gör det kan det orsaka importproblem när du arbetar med SDK. Du måste även använda versaler för `Azure` i importinstruktioner.
