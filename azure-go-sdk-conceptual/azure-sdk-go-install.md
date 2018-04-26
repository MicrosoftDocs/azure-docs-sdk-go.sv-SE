---
title: Installera Azure SDK för Go
description: Anvisningar om hur du installerar, utför vendoring och konfigurerar Azure SDK för Go.
author: sptramer
ms.author: sttramer
ms.date: 03/14/2018
ms.topic: article
ms.devlang: go
manager: carmonm
ms.openlocfilehash: a6a92e080aea1a92f47a9d7083f133ca05a47541
ms.sourcegitcommit: 26520a8c6e812facb5b9432d68c370fa23c99888
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/16/2018
---
# <a name="install-the-azure-sdk-for-go"></a>Installera Azure SDK för Go

Välkommen till Azure SDK för Go! Med detta SDK kan du hantera och interagera med Azure-tjänster från Go-program.

## <a name="get-the-azure-sdk-for-go"></a>Hämta Azure SDK för Go

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

Om du vill arbeta med Azure Storage Blobs krävs ett separat SDK.

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendor-the-azure-sdk-for-go"></a>Vendoring i Azure SDK för Go

Du kan utföra vendoring för Azure SDK för Go via [dep](https://github.com/golang/dep). Vendoring rekommenderas av stabilitetsskäl. Om du vill använda stöd för `dep` lägger du till `github.com/Azure/azure-sdk-for-go` i ett `[[constraint]]`-avsnitt i din `Gopkg.toml`. Om du till exempel vill utföra vendoring för version `14.0.0` lägger du till följande post:

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a>Ta med Azure SDK för Go i ditt projekt

Om du vill använda Azure-tjänster från din Go-kod importerar du alla tjänster som du interagerar med samt de nödvändiga `autorest`-modulerna.
Du får en fullständig lista över tillgängliga moduler från GoDoc för [tillgängliga tjänster](https://godoc.org/github.com/Azure/azure-sdk-for-go) och [AutoRest-paket](https://godoc.org/github.com/Azure/go-autorest). De vanligaste paketen som du behöver från `go-autorest` är:

| Paket | Beskrivning |
|---------|-------------|
| [github.com/Azure/go-autorest/autorest][autorest] | Objekt för hantering av tjänstklientautentisering |
| [github.com/Azure/go-autorest/autorest/azure][autorest/azure] | Konstanter för interaktioner med Azure-tjänster |
| [github.com/Azure/go-autorest/autorest/adal][autorest/adal] | Autentiseringsmekanismer för åtkomst till Azure-tjänster |
| [github.com/Azure/go-autorest/autorest/to][autorest/to] | Ange kontrollhjälp för att arbeta med Azure SDK-datastrukturer |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

Versioner av moduler för Azure-tjänster är oberoende av deras SDK-API:er. De här versionerna är en del av importsökvägen för modulen och kommer antingen från en _tjänstversion_ eller en _profil_. För närvarande rekommenderar vi att du använder en specifik tjänstversion för både utveckling och utgivning. Tjänster finns under modulen `services`. Den fullständiga sökvägen för importen är namnet på tjänsten, följt av versionen i formatet `YYYY-MM-DD`, följt av namnet på tjänsten igen. Så här inkluderar du till exempel versionen `2017-03-30` av beräkningstjänsten:

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

Just nu rekommenderar vi att du använder den senaste versionen av en tjänst om du inte har en bra anledning att inte göra det.

Du kan även välja en enskild profilversion om du behöver en kollektiv ögonblicksbild av tjänsterna. Just nu är den enda låsta profilen version `2017-03-30`, som kanske inte har de senaste funktionerna för tjänsterna. Profilerna finns under modulen `profiles` med versionerna i formatet `YYYY-MM-DD`. Tjänsterna är grupperade under profilversionerna. Så här importerar du till exempel hanteringsmodulen för Azure-resurser från profilen `2017-03-09`:

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> Även profilerna `preview` och `latest` är tillgängliga. Vi rekommenderar inte att du använder dem. De här profilerna är löpande versioner och tjänstbeteendet kan därför ändras när som helst.

## <a name="next-steps"></a>Nästa steg

Testa att använda en snabbstart om du vill börja använda Azure SDK för Go.

* [Distribuera en virtuell dator från en mall](azure-sdk-go-qs-vm.md)
* [Överföra objekt till Azure Blob Storage med hjälp av Azure Blob SDK för Go](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [Ansluta till Azure Database for PostgreSQL](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

Om du vill komma igång med andra tjänster i Go SDK direkt så kan du ta en titt på några av de tillgängliga exempelkoderna.

* [Autentisera med Azure-tjänster](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [Distribuera nya virtuella datorer med SSH-autentisering](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Distribuera en behållaravbildning till Azure Container Instances](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [Skapa ett kluster i Azure Kubernetes Service](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [Arbeta med Azure Storage-tjänster](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [Alla exempel för Azure SDK för Go](https://github.com/azure-samples/azure-sdk-for-go-samples)
