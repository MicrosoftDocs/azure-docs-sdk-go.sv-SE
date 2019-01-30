---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 3d26b464f196f8675c636ae792637d4c882fb92c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/14/2018
ms.locfileid: "55145557"
---
<span data-ttu-id="effe5-101">[Azure SDK för Go](https://github.com/Azure/azure-sdk-for-go) är kompatibel med Go-version 1.8 och senare.</span><span class="sxs-lookup"><span data-stu-id="effe5-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and higher.</span></span> <span data-ttu-id="effe5-102">För miljöer med [Azure Stack-profiler](/azure/azure-stack/user/azure-stack-version-profiles-go) är Go-versionen 1.9 minimikravet.</span><span class="sxs-lookup"><span data-stu-id="effe5-102">For environments using [Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="effe5-103">Följ [installationsanvisningarna](https://golang.org/doc/install) om du behöver installera Go.</span><span class="sxs-lookup"><span data-stu-id="effe5-103">If you need to install Go, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="effe5-104">Du kan hämta Azure SDK för Go och dess beroenden via `go get`.</span><span class="sxs-lookup"><span data-stu-id="effe5-104">You can download the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="effe5-105">Kontrollera att du använder versaler för `Azure` i webbadressen.</span><span class="sxs-lookup"><span data-stu-id="effe5-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="effe5-106">Om du inte gör det kan det orsaka importproblem när du arbetar med SDK.</span><span class="sxs-lookup"><span data-stu-id="effe5-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="effe5-107">Du måste även använda versaler för `Azure` i importinstruktioner.</span><span class="sxs-lookup"><span data-stu-id="effe5-107">You also need to capitalize `Azure` in your import statements.</span></span>
