---
description: Este artigo descreve como assinar um pacote MSIX no Visual Studio.
title: Assinar um pacote MSIX no Visual Studio
ms.date: 07/12/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.openlocfilehash: 7904f6de962804a9776338860485c98d04360f02
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073672"
---
# <a name="sign-an-msix-package-in-visual-studio"></a>Assinar um pacote MSIX no Visual Studio

O projeto de **projeto de empacotamento de aplicativos do Windows** no Visual Studio inclui um arquivo de formato PFX (troca de informações pessoais) protegido por senha que você provavelmente deseja substituir pelo seu próprio. Se sua empresa não fornecer um certificado de assinatura de código, você poderá comprar um de uma autoridade confiável ou criar um certificado autoassinado. Há uma opção "criar certificado de teste" e um assistente de importação no Visual Studio, que você encontrará se abrir o arquivo Package. appxmanifest no designer de manifesto de aplicativo padrão e examinar a guia empacotamento. Se você não estiver em assistentes e caixas de diálogo, poderá usar o cmdlet New-SelfSignedCertificate do PowerShell para criar um certificado:

```
 > New-SelfSignedCertificate -Type CodeSigningCert -Subject "CN=MyCompany,
  O=MyCompany, L=Stockholm, S=N/A, C=Sweden" -KeyUsage DigitalSignature
    -FriendlyName MyCertificate -CertStoreLocation "Cert:\LocalMachine\My"
      -TextExtension @('2.5.29.37={text}1.3.6.1.5.5.7.3.3',
        '2.5.29.19={text}Subject Type:End Entity')
```

O cmdlet gera uma impressão digital (como a A27... D9F aqui) que você pode passar para outro cmdlet, move-item, para mover o certificado para o repositório de certificação raiz confiável:

```
>Move-Item Cert:\LocalMachine\My\A27A5DBF5C874016E1A0DEBF38A97061F6625D9F
  -Destination Cert:\LocalMachine\Root
```
Novamente, você precisa instalar o certificado nesse armazenamento em todos os computadores em que pretende instalar e executar o aplicativo empacotado. Você também precisa habilitar o Sideload de aplicativos nesses dispositivos. Em um computador não gerenciado, isso pode ser feito em atualizar & segurança | Para desenvolvedores no aplicativo configurações. Em um dispositivo gerenciado por uma organização, você pode ativar o Sideload enviando uma política com um provedor de MDM (gerenciamento de dispositivo móvel).

A impressão digital também pode ser usada para exportar o certificado para um novo arquivo PFX usando o cmdlet Export-PfxCertificate:

```
>$pwd = ConvertTo-SecureString -String secret -Force -AsPlainText
>Export-PfxCertificate -cert
  "Cert:\LocalMachine\Root\A27A5DBF5C874016E1A0DEBF38A97061F6625D9F"
    -FilePath "c:/<SolutionFolder>/Msix/certificate.pfx" -Password $pwd
```
Lembre-se de instruir o Visual Studio a usar o arquivo PFX gerado para assinar o pacote MSIX selecionando-o na guia empacotamento no designer ou editando manualmente o arquivo de projeto. wapproj e substituindo os valores dos elementos <PackageCertificateKeyFile> e <PackageCertificateThumbprint>.
