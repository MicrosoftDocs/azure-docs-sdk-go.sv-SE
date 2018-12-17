---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059279"
---
[Azure SDK för Go](https://github.com/Azure/azure-sdk-for-go) är kompatibel med Go-version 1.8 och senare. För miljöer med [Azure Stack-profiler](/azure/azure-stack/user/azure-stack-version-profiles-go) är Go-versionen 1.9 minimikravet.
Följ [installationsanvisningarna](https://golang.org/doc/install) om du behöver installera Go.

Du kan hämta Azure SDK för Go och dess beroenden via `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Kontrollera att du använder versaler för `Azure` i webbadressen. Om du inte gör det kan det orsaka importproblem när du arbetar med SDK. Du måste även använda versaler för `Azure` i importinstruktioner.
