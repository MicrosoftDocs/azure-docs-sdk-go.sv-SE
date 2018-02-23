<span data-ttu-id="5bc84-101">Azure SDK för Go är kompatibel med Go-versionerna 1.8 och senare.</span><span class="sxs-lookup"><span data-stu-id="5bc84-101">The Azure SDK for Go is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="5bc84-102">För miljöer med [Azure Stack-profiler](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles) är Go-versionen 1.9 minimikravet.</span><span class="sxs-lookup"><span data-stu-id="5bc84-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span> <span data-ttu-id="5bc84-103">Om du inte har Go tillgängligt på datorn följer du [installationsanvisningarna för Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="5bc84-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="5bc84-104">Du kan hämta Azure SDK för Go och dess beroenden via `go get`.</span><span class="sxs-lookup"><span data-stu-id="5bc84-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="5bc84-105">Kontrollera att du använder versaler för `Azure` i webbadressen.</span><span class="sxs-lookup"><span data-stu-id="5bc84-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="5bc84-106">Om du inte gör det kan det orsaka importproblem när du arbetar med SDK.</span><span class="sxs-lookup"><span data-stu-id="5bc84-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="5bc84-107">Du måste även använda versaler för `Azure` i importinstruktioner.</span><span class="sxs-lookup"><span data-stu-id="5bc84-107">You also need to capitalize `Azure` in your import statements.</span></span>

