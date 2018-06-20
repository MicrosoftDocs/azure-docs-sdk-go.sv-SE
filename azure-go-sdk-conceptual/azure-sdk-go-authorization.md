---
title: Autentisering med Azure SDK för Go
description: Lär dig mer om autentiseringsmetoder som är tillgängliga i Azure SDK för Go och hur de används.
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.service: active-directory
ms.component: authentication
ms.openlocfilehash: 370f5607b89c0044022f7987d06c3a55c9d6f352
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/03/2018
ms.locfileid: "32319891"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Autentiseringsmetoder i Azure SDK för Go

Azure SDK för Go erbjuder en mängd olika typer av autentisering och autentiseringsmetoder som du kan använda i ditt program. Autentiseringsmetoderna som stöds sträcker sig från att hämta information från miljövariabler till interaktiv webbaserad autentisering. Den här artikeln ger dig en introduktion till tillgängliga typer av autentisering i SDK, och olika metoder för att använda dem. Du får också metodtips för att välja vilken typ av autentisering som passar ditt program.

## <a name="available-authentication-types-and-methods"></a>Tillgängliga autentiseringstyper och metoder

Azure SDK för Go erbjuder flera olika typer av autentisering som använder olika uppsättningar autentiseringsuppgifter. Var och en av de här autentiseringstyperna är tillgängliga via olika autentiseringsmetoder. Det är så SDK tar de här autentiseringsuppgifterna som indata. I följande tabell beskrivs de tillgängliga typerna av autentisering och situationer där de rekommenderas att användas av ditt program.

| Autentiseringstyp | Rekommenderas när... |
|---------------------|---------------------|
| Certifikatbaserad autentisering | Du har ett X509-certifikat som har konfigurerats för en användare av Azure Active Directory (AAD) eller tjänstens huvudnamn. Läs [Komma igång med certifikatbaserad autentisering i Azure Active Directory] för att lära dig mer. |
| Klientautentiseringsuppgifter | Du har ett konfigurerat huvudnamn för tjänsten som har konfigurerats för det här programmet eller en klass av program som det tillhör. Läs [Skapa Azure-tjänstens huvudnamn med Azure CLI 2.0] för att lära dig mer. |
| Hanterad tjänstidentitet (MSI) | Programmet körs på en Azure-resurs som har konfigurerats med Hanterad tjänstidentitet (MSI). Läs [Hanterad tjänstidentitet (MSI) för Azure-resurser] för att lära dig mer. |
| Enhetstoken | Programmet är avsett att __endast__ användas interaktivt och har en mängd olika användare, potentiellt från flera AAD-klientorganisationer. Användare har åtkomst till en webbläsare för att logga in. Mer information finns i [Använda autentisering med enhetstoken](#use-device-token-authentication).|
| Användarnamn/lösenord | Du har ett interaktivt program som inte kan använda någon annan autentiseringsmetod. Användarna har inte aktiverat multifaktorautentisering för sin AAD-inloggning. |

> [!IMPORTANT]
> Om du använder en autentiseringstyp som skiljer sig från klientens autentiseringsuppgifter måste programmet registreras i Azure Active Directory. Läs [Integrera program med Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications) för att lära dig hur.

> [!NOTE]
> Om du inte har särskilda krav bör du undvika autentisering med användarnamn/lösenord. I situationer där användarbaserad inloggning är lämpligt används vanligtvis autentisering med enhetstoken i stället.

[Komma igång med certifikatbaserad autentisering i Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Skapa Azure-tjänstens huvudnamn med Azure CLI 2.0]: /cli/azure/create-an-azure-service-principal-azure-cli
[Hanterad tjänstidentitet (MSI) för Azure-resurser]: /azure/active-directory/managed-service-identity/overview

De här typerna av autentisering är tillgängliga via olika metoder. [_Miljöbaserad autentisering_](#use-environment-based-authentication) läser autentiseringsuppgifter direkt från programmets miljö. [_Filbaserad autentisering_](#use-file-based-authentication) läser in en fil som innehåller autentiseringsuppgifter för tjänstens huvudnamn. [_Klientbaserad autentisering_](#use-an-authentication-client) använder ett objekt i Go-kod och gör att du ansvarar för att tillhandahålla autentiseringsuppgifterna när programmet körs. Slutligen finns även [_Autentisering med enhetstoken_](#use-device-token-authentication) som kräver att användarna loggar in interaktivt via en webbläsare med en token och som inte kan användas med miljö- eller filbaserad autentisering.

Alla autentiseringsfunktioner och -typer är tillgängliga i paketet `github.com/Azure/go-autorest/autorest/azure/auth`.

> [!NOTE]
> Om du inte har särskilda krav bör du undvika klientbaserad autentisering. Den här metoden för autentisering uppmanar användarna att använda dåliga metoder. I synnerhet gör klientbaserad autentisering det lockande att hårdkoda autentiseringsuppgifter. Att skriva anpassad kod för autentisering kan också sluta fungera efter att kommande SDK-versioner har släppts om de ändrar kraven för autentisering.

## <a name="use-environment-based-authentication"></a>Använda miljö för autentisering

Om du kör programmet i en strikt kontrollerad miljö som i en behållare är miljöbaserad autentisering är det naturliga valet. Du kan konfigurera gränssnittsmiljön innan du kör ditt program och Go SDK läser dessa miljövariabler vid körning för att autentisera med Azure. 

Miljöbaserad autentisering har stöd för alla autentiseringsmetoder förutom enhetstoken och utvärderas i följande ordning: Klientens autentiseringsuppgifter, certifikat, användarnamn/lösenord och hanterad tjänstidentitet (MSI). Om en miljövariabel som krävs är odefinierad eller om SDK blir nekad av autentiseringstjänsten används nästa autentiseringstyp. Om SDK inte kan autentisera från miljön returneras ett fel.

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
| __MSI__ | | MSI kräver inte att några autentiseringsuppgifter anges. Programmet måste köras på en Azure-resurs konfigurerad för att använda MSI. Mer information finns i [Hanterad tjänstidentitet (MSI) för Azure-resurser]. |

Om du behöver ansluta till ett moln eller en hanteringsslutpunkt annat än det offentliga Azure-moln som är standard kan du även ange följande miljövariabler. De vanligaste orsakerna att ange dem är om du använder Azure Stack, ett moln i en annan geografisk region eller Azures klassiska distributionsmodell.

| Miljövariabel | Beskrivning  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | Namnet på molnmiljön som användaren ska ansluta till. |
| `AZURE_AD_RESOURCE` | Resurs-ID för Active Directory som ska användas vid anslutning. Det här bör vara en URI som pekar på hanteringsslutpunkten. |

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

Mer information om hur du använder Azure SDK för Go på Azure Stack finns i [Use API version profiles with Go in Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-version-profiles-go) (Använda API-versionsprofiler med Go i Azure Stack


## <a name="use-file-based-authentication"></a>Använd filbaserad autentisering

Filbaserad autentisering fungerar endast med klientens autentiseringsuppgifter när de lagras i ett lokal filformat som genererats av [Azure CLI 2.0](/cli/azure). Du kan enkelt skapa den här filen när du skapar en ny tjänstens huvudnamn med parametern `--sdk-auth`. Se till att det här argumentet anges när du skapar ett huvudnamn för tjänsten om du tänker använda filbaserad autentisering. Eftersom CLI skriver utdata till `stdout` kan du dirigera om utdata till en fil.

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

Ange miljövariabeln `AZURE_AUTH_LOCATION` till platsen där auktoriseringsfilen lagras. Den här miljövariabeln läses av programmet och autentiseringsuppgifterna i den parsas. Om du måste välja auktoriseringsfilen vid körning kan du ändra programmets miljö med hjälp av funktionen [os.Setenv](https://golang.org/pkg/os/#Setenv).

För att läsa in autentiseringsinformationen anropar du funktionen [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile). Till skillnad från miljöbaserad auktorisering kräver filbaserad auktorisering en resursslutpunkt.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

Läs mer om att använda tjänstens huvudnamn och att hanteras dess åtkomstbehörigheter i [Skapa Azure-tjänstens huvudnamn med Azure CLI 2.0].

## <a name="use-device-token-authentication"></a>Använda autentisering med enhetstoken

Om du vill att användarna ska logga in interaktivt är det bästa sättet att erbjuda den funktionen via autentisering med enhetstoken. Med det här autentiseringsflödet skickas en token till användaren som sedan klistrar in den i en Microsoft-inloggningswebbplats där de sedan loggar in med ett Azure Active Directory (AAD)-konto. Den här autentiseringsmetoden har stöd för konton som har multifaktorautentisering aktiverat, till skillnad från vanlig autentisering med användarnamn/lösenord.

Skapa en [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig)-authorizer med funktionen [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) för att använda autentisering med enhetstoken. Anropa [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) på det resulterande objektet för att starta autentiseringsprocessen. Enhetens flödesautentisering blockerar programkörningen tills hela autentiseringsflödet är klart.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>Använd en autentiseringsklient

Om du måste använda en viss typ av autentisering och kan göra så att programmet läser in autentiseringsuppgifter från användaren kan du använda alla klienter som överensstämmer med gränssnittet [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig). Använd en typ som implementerar det här gränssnittet om du vill använda ett interaktivt program, använda specialiserade konfigurationsfiler eller om du har ett krav som förhindrar dig från att använda en annan autentiseringsmetod.

> [!WARNING]
> Hårdkoda aldrig Azure-autentiseringsuppgifter i ett program. Om du lägger in hemligheter i ett programs binärfiler blir det enklare för en angripare att extrahera dem, oavsett om programmet körs eller inte. Alla Azure-resurser som autentiseringsuppgifterna har behörighet för utsätts då för risk!

I följande tabell visas typerna i SDK som överensstämmer med gränssnittet `AuthorizerConfig`.

| Autentiseringstyp | Authorizer-typ |
|---------------------|-----------------------|
| Certifikatbaserad autentisering | [ClientCertificateConfig] |
| Klientautentiseringsuppgifter | [ClientCredentialsConfig] |
| Hanterad tjänstidentitet (MSI) | [MSIConfig] |
| Användarnamn/lösenord | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

Skapa en autentiserare med dess associerade `New`-funktion och anropa sedan `Authorize` på det resulterande objektet för att utföra autentisering. För att till exempel använda certifikatbaserad autentisering:

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
