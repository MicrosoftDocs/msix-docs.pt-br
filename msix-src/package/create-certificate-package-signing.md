---
title: Criar um certificado para a assinatura de pacote
description: Criar e exportar um certificado para a assinatura de pacote de apps com as ferramentas do PowerShell.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 7bc2006f-fc5a-4ff6-b573-60933882caf8
ms.localizationpriority: medium
ms.openlocfilehash: b251930550aa1a5c6d5d42c117256ca579504fde
ms.sourcegitcommit: d749fa662214bddaa6854f1ee95761c547db8dae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2019
ms.locfileid: "75008106"
---
# <a name="create-a-certificate-for-package-signing"></a>Criar um certificado para a assinatura de pacote

Este artigo explica como criar e exportar um certificado para a assinatura de pacote de apps usando as ferramentas do PowerShell. É recomendável usar o Visual Studio para [empacotar aplicativos UWP](packaging-uwp-apps.md) e [empacotar aplicativos de área de trabalho](../desktop/desktop-to-uwp-packaging-dot-net.md), mas você ainda poderá empacotar um aplicativo manualmente se não tiver usado o Visual Studio para desenvolver seu aplicativo.

## <a name="prerequisites"></a>Pré-requisitos

- **Um aplicativo empacotado ou desempacotado**  
Um app que contém um arquivo AppxManifest.xml. Você precisará fazer referência ao arquivo de manifesto ao criar o certificado que será usado para assinar o pacote do app final. Para obter detalhes sobre como empacotar manualmente um app, consulte [Criar um pacote de apps com a ferramenta MakeAppx.exe](create-app-package-with-makeappx-tool.md).

- **Cmdlets de PKI (infraestrutura de chave pública)**  
Você precisa de cmdlets de PKI para criar e exportar o certificado de assinatura. Para obter mais informações, consulte [Cmdlets da Infraestrutura de Chave Pública](https://docs.microsoft.com/powershell/module/pkiclient/).

## <a name="create-a-self-signed-certificate"></a>Criar um certificado autoassinado

Um certificado autoassinado é útil para testar seu aplicativo antes que você esteja pronto para publicá-lo na loja. Siga as etapas descritas nesta seção para criar um certificado autoassinado.

> [!NOTE]
> Quando você cria e usa um certificado autoassinado, somente os usuários que instalam e confiam em seu certificado podem executar seu aplicativo. Isso é fácil de implementar para teste, mas pode impedir que usuários adicionais instalem seu aplicativo. Quando você estiver pronto para publicar seu aplicativo, recomendamos que você use um certificado emitido por uma fonte confiável. Esse sistema de confiança centralizada ajuda a garantir que o ecossistema de aplicativos tenha níveis de verificação para proteger os usuários contra atores mal-intencionados.

### <a name="determine-the-subject-of-your-packaged-app"></a>Determine o assunto do seu app empacotado  

Usar um certificado para assinar o pacote de apps, o "Assunto" no certificado **deve** corresponder à seção "Fornecedor" no manifesto do app.

Por exemplo, a seção "Identidade" no arquivo de AppxManifest.xml do seu app deve ser algo parecido com isso:

```xml
  <Identity Name="Contoso.AssetTracker" 
    Version="1.0.0.0" 
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"/>
```

O "Fornecedor" nesse caso é "CN=Contoso Software, O=Contoso Corporation, C=US" que precisa ser usado para criar o certificado.

### <a name="use-new-selfsignedcertificate-to-create-a-certificate"></a>Use **New-SelfSignedCertificate** para criar um certificado

Use o cmdlet **New-SelfSignedCertificate** do PowerShell para criar um certificado autoassinado. **New-SelfSignedCertificate** tem vários parâmetros para personalização, mas para fins deste artigo, vamos nos concentrar sobre como criar um certificado simples que funcionará com **SignTool**. Para obter mais exemplos e usos deste cmdlet, consulte [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/New-SelfSignedCertificate).

Com base no arquivo AppxManifest.xml do exemplo anterior, você deve usar a sintaxe a seguir para criar um certificado. Em um prompt do PowerShell elevado:

```powershell
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName "Your friendly name goes here" -CertStoreLocation "Cert:\CurrentUser\My" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")
```

Observe os seguintes detalhes sobre alguns dos parâmetros:

- **KeyUsage**: esse parâmetro define para que o certificado pode ser usado. Para um certificado de assinatura automática, esse parâmetro deve ser definido como **DigitalSignature**.

- **Textextension**: esse parâmetro inclui configurações para as seguintes extensões:

  - EKU (uso estendido de chave): essa extensão indica finalidades adicionais para as quais a chave pública certificada pode ser usada. Para um certificado de assinatura automática, esse parâmetro deve incluir a cadeia de caracteres de extensão **"2.5.29.37 = {Text} 1.3.6.1.5.5.7.3.3"** , que indica que o certificado deve ser usado para a assinatura de código.

  - Restrições básicas: essa extensão indica se o certificado é uma autoridade de certificação (CA) ou não. Para um certificado de assinatura automática, esse parâmetro deve incluir a cadeia de caracteres de extensão **"2.5.29.19 = {Text}"** , que indica que o certificado é uma entidade final (não uma CA).

Depois de executar esse comando, o certificado será adicionado ao repositório de certificados local, conforme especificado no parâmetro "-CertStoreLocation". O resultado do comando também produzirá a impressão digital do certificado.  

Você pode exibir o certificado em uma janela do PowerShell usando os seguintes comandos:

```powershell
Set-Location Cert:\CurrentUser\My
Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint
```

Isso exibirá todos os certificados no repositório local.

## <a name="export-a-certificate"></a>Exportar um certificado 

Para exportar o certificado no repositório local para um arquivo de troca de informações pessoais (PFX), use o cmdlet **Export-PfxCertificate**.

Ao usar **Export-PfxCertificate**, você deve criar e usar uma senha ou usar o parâmetro "-ProtectTo" para especificar quais usuários ou grupos podem acessar o arquivo sem uma senha. Observe que um erro será exibido se você não usar o parâmetro "-Password" ou "-ProtectTo".

### <a name="password-usage"></a>Uso de senhas

```powershell
$password = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText 
Export-PfxCertificate -cert "Cert:\CurrentUser\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $password
```

### <a name="protectto-usage"></a>Uso de ProtectTo

```powershell
Export-PfxCertificate -cert Cert:\CurrentUser\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Username or group name>
```

Depois de criar e exportar o certificado, você está pronto para assinar seu pacote de apps com **SignTool**. Para a próxima etapa no processo de empacotamento manual, consulte [Assinar um pacote de apps usando a SignTool](sign-app-package-using-signtool.md).

## <a name="security-considerations"></a>Considerações sobre a segurança

Ao adicionar um certificado em [repositórios de certificados do computador local](https://docs.microsoft.com/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores), você afetará a confiança do certificado de todos os usuários no computador. É recomendável que você remova esses certificados quando eles não são mais necessários para evitar que eles sejam usados para comprometer a confiança do sistema.
