---
title: Assinar um pacote do aplicativo usando a SignTool
description: Este artigo descreve como usar SignTool para assinar manualmente um pacote de aplicativo ou um pacote com um certificado.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 171f332d-2a54-4c68-8aa0-52975d975fb1
ms.localizationpriority: medium
ms.openlocfilehash: 58fa5477c8b1503a061e3c72a4342b25acc2387f
ms.sourcegitcommit: cb145c63d700446e6aec1be8f9e6d2ae7481fb6b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2020
ms.locfileid: "92011578"
---
# <a name="sign-an-app-package-using-signtool"></a>Assinar um pacote do aplicativo usando a SignTool

**SignTool** é uma ferramenta de linha de comando usada para assinar digitalmente um pacote ou lote de aplicativos com um certificado. O certificado pode ser criado pelo usuário (para fins de teste) ou emitido por uma empresa (para distribuição). Assinar um pacote do aplicativo fornece ao usuário uma verificação de que os dados do aplicativo não foram modificados depois que ele foi assinado enquanto também confirma a identidade do usuário ou empresa que assinou. **SignTool** pode assinar pacotes e lotes de aplicativos criptografados ou não criptografados.

> [!IMPORTANT] 
> Se você usou o Visual Studio para desenvolver seu aplicativo, é recomendável que você use o Assistente do Visual Studio para criar e assinar seu pacote de aplicativo. Para obter mais informações, consulte [empacotar um aplicativo UWP com o Visual Studio](packaging-uwp-apps.md) e [empacotar um aplicativo de desktop do código-fonte usando o Visual Studio](../desktop/desktop-to-uwp-packaging-dot-net.md).

Para obter mais informações sobre assinatura de código e certificados em geral, consulte [Introdução à Assinatura de Código](/windows/desktop/SecCrypto/cryptography-tools).

## <a name="prerequisites"></a>Pré-requisitos

- **Um aplicativo empacotado**  
    Para saber mais sobre como criar manualmente um pacote de aplicativos, consulte [Criar um pacote do aplicativo com a ferramenta MakeAppx.exe](create-app-package-with-makeappx-tool.md).

- **Um certificado de assinatura válido**  
    Para obter mais informações sobre como criar ou importar um certificado válido de assinatura, consulte [Criar ou importar um certificado de assinatura de pacote](create-certificate-package-signing.md).

- **SignTool.exe**  
    Dependendo do seu caminho de instalação do SDK, a **SignTool** é instalada no computador Windows 10 nos seguintes locais:
    - x86: C:\Arquivos de programas (x86) \Windows Kits\10\bin \\ &lt; sdk versão &gt;\x86\SignTool.exe
    - x64: C:\Arquivos de programas (x86) \Windows Kits\10\bin \\ &lt; sdk versão &gt;\x64\SignTool.exe

## <a name="using-signtool"></a>Usando o SignTool

**SignTool** pode ser usado para assinar arquivos, verificar assinaturas ou carimbos de data e hora, remover assinaturas e muito mais. Para a finalidade de assinar um pacote de aplicativos, destacaremos o comando **sign**. Para obter informações completas sobre o **SignTool**, consulte a página de referência do [SignTool](/windows/desktop/SecCrypto/signtool).

### <a name="determine-the-hash-algorithm"></a>Determinar o algoritmo de hash

Ao usar o **SignTool** para assinar seu pacote ou lote de aplicativos, o algoritmo de hash usado em **SignTool** deve ser o mesmo algoritmo usado para empacotar seu aplicativo. Por exemplo, se você usou o **MakeAppx.exe** para criar o pacote de aplicativos com as configurações padrão, você deve especificar SHA256 ao usar o **SignTool** já que é o algoritmo padrão usado pelo **MakeAppx.exe**.

Para descobrir qual algoritmo de hash foi usado durante o empacotamento de seu aplicativo, extraia o conteúdo do pacote de aplicativos e inspecione o arquivo AppxBlockMap.xml. Para saber como descompactar/extrair um pacote de aplicativos, consulte [Extrair arquivos de um pacote ou lote](create-app-package-with-makeappx-tool.md). O método de hash está no elemento BlockMap e tem este formato:

```xml
<BlockMap xmlns="http://schemas.microsoft.com/appx/2010/blockmap"
HashMethod="http://www.w3.org/2001/04/xmlenc#sha256">
```

Esta tabela mostra cada valor de HashMethod e seu algoritmo de hash correspondente:


| Valor de HashMethod                              | Algoritmo de hash |
|-----------------------------------------------|----------------|
| http://www.w3.org/2001/04/xmlenc#sha256       | SHA256         |
| http://www.w3.org/2001/04/xmldsig-more#sha384 | SHA384         |
| http://www.w3.org/2001/04/xmlenc#sha512       | SHA512         |

> [!NOTE]
> Como o algoritmo padrão do **SignTool** é SHA1 (não disponível no **MakeAppx.exe**), você deve sempre especificar um algoritmo de hash ao usar o **SignTool**.

### <a name="sign-the-app-package"></a>Assinar o pacote de aplicativos

Depois que você tiver todos os pré-requisitos e determinar qual algoritmo de hash foi usado para empacotar seu aplicativo, você estará pronto para assiná-lo. 

A sintaxe de linha de comando geral para a assinatura de pacote do **SignTool** é:

```syntax
SignTool sign [options] <filename(s)>
```

O certificado usado para assinar seu aplicativo deve ser um arquivo .pfx ou ser instalado em um repositório de certificados.

Para assinar o pacote de aplicativos com um certificado de um arquivo .pfx, use a seguinte sintaxe:

```syntax
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.appx
```

```syntax
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.msix
```

Observe que a opção `/a` permite que o **SignTool** escolha o melhor certificado automaticamente.

Se o certificado não for um arquivo .pfx, use a seguinte sintaxe:

```syntax
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.appx
```

```syntax
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.msix
```

Como alternativa, você pode especificar o hash SHA1 do certificado desejado, em vez do &lt;Nome do Certificado&gt; usando esta sintaxe:

```syntax
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.appx
```

```syntax
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.msix
```

Para obter mais exemplos, consulte [usando Signtool para assinar um arquivo](/windows/win32/seccrypto/using-signtool-to-sign-a-file)

Observe que alguns certificados não usam uma senha. Se o certificado não tiver uma senha, omita "/p &lt;Sua Senha&gt;" dos comandos de amostra.

Depois que o pacote de aplicativos é assinado com um certificado válido, você está pronto para carregar o pacote para a Loja. Para obter mais orientações sobre como carregar e enviar aplicativos para a Loja, consulte [Envios de aplicativos](/windows/uwp/publish/app-submissions).