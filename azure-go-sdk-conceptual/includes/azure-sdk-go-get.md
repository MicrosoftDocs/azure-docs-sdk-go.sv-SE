---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 0febc2cc42ad95a1b7a5032c7987e37cc82f374e
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38067041"
---
<span data-ttu-id="1126a-101">[Azure SDK för Go](https://github.com/Azure/azure-sdk-for-go) kan användas med Go-version 1.8 och senare.</span><span class="sxs-lookup"><span data-stu-id="1126a-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="1126a-102">För miljöer med [Azure Stack-profiler](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles) är Go-versionen 1.9 minimikravet.</span><span class="sxs-lookup"><span data-stu-id="1126a-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="1126a-103">Om du inte har Go tillgängligt på datorn följer du [installationsanvisningarna för Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="1126a-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="1126a-104">Du kan hämta Azure SDK för Go och dess beroenden via `go get`.</span><span class="sxs-lookup"><span data-stu-id="1126a-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="1126a-105">Kontrollera att du använder versaler för `Azure` i webbadressen.</span><span class="sxs-lookup"><span data-stu-id="1126a-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="1126a-106">Om du inte gör det kan det orsaka importproblem när du arbetar med SDK.</span><span class="sxs-lookup"><span data-stu-id="1126a-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="1126a-107">Du måste även använda versaler för `Azure` i importinstruktioner.</span><span class="sxs-lookup"><span data-stu-id="1126a-107">You also need to capitalize `Azure` in your import statements.</span></span>

