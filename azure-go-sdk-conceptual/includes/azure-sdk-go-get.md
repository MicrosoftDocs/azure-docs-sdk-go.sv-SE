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
<span data-ttu-id="2d0aa-101">[Azure SDK för Go](https://github.com/Azure/azure-sdk-for-go) är kompatibel med Go-version 1.8 och senare.</span><span class="sxs-lookup"><span data-stu-id="2d0aa-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and higher.</span></span> <span data-ttu-id="2d0aa-102">För miljöer med [Azure Stack-profiler](/azure/azure-stack/user/azure-stack-version-profiles-go) är Go-versionen 1.9 minimikravet.</span><span class="sxs-lookup"><span data-stu-id="2d0aa-102">For environments using [Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="2d0aa-103">Följ [installationsanvisningarna](https://golang.org/doc/install) om du behöver installera Go.</span><span class="sxs-lookup"><span data-stu-id="2d0aa-103">If you need to install Go, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="2d0aa-104">Du kan hämta Azure SDK för Go och dess beroenden via `go get`.</span><span class="sxs-lookup"><span data-stu-id="2d0aa-104">You can download the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="2d0aa-105">Kontrollera att du använder versaler för `Azure` i webbadressen.</span><span class="sxs-lookup"><span data-stu-id="2d0aa-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="2d0aa-106">Om du inte gör det kan det orsaka importproblem när du arbetar med SDK.</span><span class="sxs-lookup"><span data-stu-id="2d0aa-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="2d0aa-107">Du måste även använda versaler för `Azure` i importinstruktioner.</span><span class="sxs-lookup"><span data-stu-id="2d0aa-107">You also need to capitalize `Azure` in your import statements.</span></span>
