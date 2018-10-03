---
title: Autentisering med Azure SDK för Go
description: Lär dig mer om autentiseringsmetoder som är tillgängliga i Azure SDK för Go och hur de används.
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.technology: azure-sdk-go
ms.devlang: go
ms.component: authentication
ms.openlocfilehash: f5c2c56e43828f0bedad0b5781dc71991ce1fd3e
ms.sourcegitcommit: 172f81dd6e4c6a275dc8031815aa87cdb488cbf0
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47231683"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Autentiseringsmetoder i Azure SDK för Go

Med Azure SDK för Go får du flera olika sätt att autentisera med Azure. De här _typerna_ av autentisering kan anropas genom olika _autentiseringsmetoder_. I den här artikeln beskrivs tillgängliga typer, metoder och hur du väljer vad som är bäst för ditt program.

## <a name="available-authentication-types-and-methods"></a>Tillgängliga autentiseringstyper och metoder

Azure SDK för Go erbjuder flera olika typer av autentisering som använder olika uppsättningar autentiseringsuppgifter. Var och en av de här autentiseringstyperna är tillgängliga via olika autentiseringsmetoder. Det är så SDK tar de här autentiseringsuppgifterna som indata. I följande tabell beskrivs de tillgängliga typerna av autentisering och situationer där de rekommenderas att användas av ditt program.

| Autentiseringstyp | Rekommenderas när... |
|---------------------|---------------------|
| Certifikatbaserad autentisering | Du har ett X509-certifikat som har konfigurerats för en användare av Azure Active Directory (AAD) eller tjänstens huvudnamn. Läs [Komma igång med certifikatbaserad autentisering i Azure Active Directory] för att lära dig mer. |
| Klientautentiseringsuppgifter | Du har ett konfigurerat huvudnamn för tjänsten som har konfigurerats för det här programmet eller en klass av program som det tillhör. Läs [Skapa ett huvudnamn för tjänsten med Azure CLI] för att lära dig mer. |
| Hanterade identiteter för Azure-resurser | Programmet körs på en Azure-resurs som har konfigurerats med en hanterad identitet. Mer information finns i [Hanterade identiteter för Azure-resurser]. |
| Enhetstoken | Programmet är avsett att __endast__ användas interaktivt. Användare kan ha aktiverat multifaktorautentisering. Användare har åtkomst till en webbläsare för att logga in. Mer information finns i [Använda autentisering med enhetstoken](#use-device-token-authentication).|
| Användarnamn/lösenord | Du har ett interaktivt program som inte kan använda någon annan autentiseringsmetod. Användarna har inte aktiverat multifaktorautentisering för sin AAD-inloggning. |

> [!IMPORTANT]
> Om du använder en autentiseringstyp som skiljer sig från klientens autentiseringsuppgifter måste programmet registreras i Azure Active Directory. Läs [Integrera program med Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications) för att lära dig hur.
>
> [!NOTE]
> Om du inte har särskilda krav bör du undvika autentisering med användarnamn/lösenord. I situationer där användarbaserad inloggning är lämpligt används vanligtvis autentisering med enhetstoken i stället.

[Komma igång med certifikatbaserad autentisering i Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Skapa ett huvudnamn för tjänsten med Azure CLI]: /cli/azure/create-an-azure-service-principal-azure-cli
[Hanterade identiteter för Azure-resurser]: /azure/active-directory/managed-identities-azure-resources/overview

De här typerna av autentisering är tillgängliga via olika metoder.

* [_Miljöbaserad autentisering_](#use-environment-based-authentication) läser autentiseringsuppgifter direkt från programmets miljö.
* [_Filbaserad autentisering_](#use-file-based-authentication) läser in en fil som innehåller autentiseringsuppgifter för tjänstens huvudnamn.
* [_Klientbaserad autentisering_](#use-an-authentication-client) använder ett objekt i kod och gör att du ansvarar för att tillhandahålla autentiseringsuppgifterna när programmet körs.
* [_Autentisering med enhetstoken_ ](#use-device-token-authentication) kräver att användare loggar in interaktivt via en webbläsare med en token.

Alla autentiseringsfunktioner och -typer är tillgängliga i paketet `github.com/Azure/go-autorest/autorest/azure/auth`.

> [!NOTE]
> Om du inte har särskilda krav bör du undvika klientbaserad autentisering. Den här metoden för autentisering uppmanar användarna att använda dåliga metoder. I synnerhet gör klientbaserad autentisering det lockande att hårdkoda autentiseringsuppgifter. Att skriva anpassad kod för autentisering kan också sluta fungera efter att kommande SDK-versioner har släppts om de ändrar kraven för autentisering.

## <a name="use-environment-based-authentication"></a>Använda miljö för autentisering

Om du kör programmet i en kontrollerad miljö är miljöbaserad autentisering det naturliga valet. Med den här autentiseringsmetoden kan du konfigurera gränssnittsmiljön innan du kör programmet. Vid körning läser Go-SDK dessa miljövariabler för att autentisera med Azure.

Miljöbaserad autentisering har stöd för alla autentiseringsmetoder förutom enhetstoken och utvärderas i följande ordning:

* Klientautentiseringsuppgifter
* X509-certifikat
* Användarnamn/lösenord
* Hanterade identiteter för Azure-resurser

Om en autentiseringstyp har odefinierade värden eller nekas försöker SDK automatiskt nästa autentiseringstyp. SDK returnerar ett fel när det inte finns fler typer.

I följande tabell beskrivs miljövariabler som måste anges för varje autentiseringstyp som stöds av miljöbaserad autentisering.

| Autentiseringstyp | Miljövariabel | Beskrivning |
| ------------------- | -------------------- | ----------- |
| __Klientautentiseringsuppgifter__ | `AZURE_TENANT_ID` | ID för Active Directory-klientorganisationen som tjänstens huvudnamn tillhör. |
| | `AZURE_CLIENT_ID` | Namn eller ID för tjänstens huvudnamn. |
| | `AZURE_CLIENT_SECRET` | Hemligheten som är associerad med tjänstens huvudnamn. |
| __Certifikat__ | `AZURE_TENANT_ID` | ID för Active Directory-klientorganisationen som certifikatet är registrerat i. |
| | `AZURE_CLIENT_ID` | Programmets klient-ID som är associerat med certifikatet. |
| | `AZURE_CERTIFICATE_PATH` | Sökvägen till klientcertifikatfilen. |
| | `AZURE_CERTIFICATE_PASSWORD` | Lösenordet för klientcertifikatet. |
| __Användarnamn/lösenord__ | `AZURE_TENANT_ID` | ID för Active Directory-klientorganisationen som användaren tillhör. |
| | `AZURE_CLIENT_ID` | Programmets klients-ID. |
| | `AZURE_USERNAME` | Användarnamnet som används för att logga in. |
| | `AZURE_PASSWORD` | Lösenordet som används för att logga in. |
| __Hanterad identitet__ | | Autentiseringsuppgifter behövs inte för autentisering av hanterad identitet. Programmet måste köras på en Azure-resurs som har konfigurerats för att använda hanterade identiteter. Information finns i [Hanterade identiteter för Azure-resurser]. |

Om du behöver ansluta till ett moln eller en hanteringsslutpunkt annat än det offentliga Azure-moln som är standard kan du ange följande miljövariabler. De vanligaste orsakerna är att du använder Azure Stack, ett moln i en annan geografisk region eller den klassiska distributionsmodellen.

| Miljövariabel | Beskrivning  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | Namnet på molnmiljön som användaren ska ansluta till. |
| `AZURE_AD_RESOURCE` | Resurs-ID för Active Directory som ska användas vid anslutning, som en URI till hanteringsslutpunkten. |

När du använder miljöbaserad autentisering anropar du funktionen [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) för att hämta ditt authorizer-objekt. Det här objektet konfigureras i egenskapen `Authorizer` för klienter för att ge dem åtkomst till Azure.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a>Autentisering på Azure Stack

Om du vill autentisera på Azure Stack måste du ange följande variabler:

| Miljövariabel | Beskrivning  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | Azure Active Directory-slutpunkten. |
| `AZURE_AD_RESOURCE` | Resurs-ID:t för Azure Active Directory. |

Dessa variabler kan hämtas från Azure Stack-metadatainformationen. Om du vill hämta metadata öppnar du en webbläsare i Azure Stack-miljön och använder URL:en: `(ResourceManagerURL)/metadata/endpoints?api-version=1.0`

`ResourceManagerURL` varierar beroende på regionsnamnet, datornamnet och det fullständiga domännamnet för Azure Stack-distributionen:

| Miljö | ResourceManagerURL |
|----------------------|--------------|
| Development Kit | `https://management.local.azurestack.external/` |
| Integrerade system | `https://management.(region).ext-(machine-name).(FQDN)` |

Mer information om hur du använder Azure SDK för Go på Azure Stack finns i [Use API version profiles with Go in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go) (Använda API-versionsprofiler med Go i Azure Stack)

## <a name="use-file-based-authentication"></a>Använd filbaserad autentisering

Filbaserad autentisering använder ett filformat som genererats av [Azure CLI](/cli/azure). Du kan enkelt skapa den här filen när du skapar en ny tjänstens huvudnamn med parametern `--sdk-auth`. Se till att det här argumentet anges när du skapar ett huvudnamn för tjänsten om du tänker använda filbaserad autentisering. Eftersom CLI skriver utdata till `stdout` kan du dirigera om utdata till en fil.

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

Ange miljövariabeln `AZURE_AUTH_LOCATION` till platsen där auktoriseringsfilen lagras. Den här miljövariabeln läses av programmet och autentiseringsuppgifterna i den parsas. Om du måste välja auktoriseringsfilen vid körning kan du ändra programmets miljö med hjälp av funktionen [os.Setenv](https://golang.org/pkg/os/#Setenv).

För att läsa in autentiseringsinformationen anropar du funktionen [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile). Till skillnad från miljöbaserad auktorisering kräver filbaserad auktorisering en resursslutpunkt.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

Mer information om hur du använder huvudnamn för tjänster och hanterar deras åtkomstbehörigheter finns i [Skapa ett huvudnamn för tjänsten med Azure CLI].

## <a name="use-device-token-authentication"></a>Använda autentisering med enhetstoken

Om du vill att användarna ska logga in interaktivt är det bästa sättet via autentisering med enhetstoken. Med det här autentiseringsflödet skickas en token till användaren som sedan klistrar in den i en Microsoft-inloggningswebbplats där de sedan autentiserar med ett Azure Active Directory (AAD)-konto. Den här autentiseringsmetoden har stöd för konton som har multifaktorautentisering aktiverat, till skillnad från vanlig autentisering med användarnamn/lösenord.

Skapa en [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig)-authorizer med funktionen [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) för att använda autentisering med enhetstoken. Anropa [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) på det resulterande objektet för att starta autentiseringsprocessen. Enhetens flödesautentisering blockerar programkörningen tills hela autentiseringsflödet är klart.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>Använd en autentiseringsklient

Om du måste använda en viss typ av autentisering och kan göra så att programmet läser in autentiseringsuppgifter från användaren kan du använda alla klienter som överensstämmer med gränssnittet [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig). Använd en typ som implementerar det här gränssnittet när du:

* Skriver ett interaktivt program.
* Använder specialiserade konfigurationsfiler.
* Har ett krav som förhindrar användning av en inbyggd autentiseringsmetod.

> [!WARNING]
> Hårdkoda aldrig Azure-autentiseringsuppgifter i ett program. Om du lägger in hemligheter i ett programs binärfiler blir det enklare för en angripare att extrahera dem, oavsett om programmet körs eller inte. Alla Azure-resurser som autentiseringsuppgifterna har behörighet för utsätts då för risk!

I följande tabell visas typerna i SDK som överensstämmer med gränssnittet `AuthorizerConfig`.

| Autentiseringstyp | Authorizer-typ |
|---------------------|-----------------------|
| Certifikatbaserad autentisering | [ClientCertificateConfig] |
| Klientautentiseringsuppgifter | [ClientCredentialsConfig] |
| Hanterade identiteter för Azure-resurser | [MSIConfig] |
| Användarnamn/lösenord | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

Skapa en autentiserare med dess associerade `New`-funktion och anropa sedan `Authorize` på det resulterande objektet för att autentisera. För att till exempel använda certifikatbaserad autentisering:

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
