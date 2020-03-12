---
title: Modificar script do editor de pacote
description: Este artigo fornece detalhes sobre o script de editor de pacote de modificação no TOOLIT MSIX.
ms.date: 1/23/2020
ms.topic: article
keywords: Windows 10, msix, msix Toolkit
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b8033cebdaf046164a857abe074b211171c17906
ms.sourcegitcommit: fa41875f6c2b79db3d7dde29b10c0f24765532bc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79097870"
---
# <a name="modify-package-publisher-script"></a>Modificar script do editor de pacote

O [script de editor de pacote de modificação](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts/ModifyPackagePublisher) no kit de ferramentas do MSIX pode ser usado para atualizar o Publicador no manifesto antes de assinar novamente o pacote com base em um novo certificado. Esse script está atualmente limitado a aplicativos MSIX e não a pacotes MSIX.

## <a name="syntax"></a>Sintaxe

```powershell
.\modify-package-publisher.ps1 -directory <String> -redist <String> -certPath <String> [[-pfxPath] <String>] [[-Password] <String>] [[-forceContinue]<Switch>]
```

## <a name="examples"></a>Exemplos

### <a name="update-the-publisher-based-on-the-certificate"></a>Atualizar o Publicador com base no certificado

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer"
```

Esse comando pesquisa recursivamente o conteúdo de C:\MSIX para todos os pacotes do MSIX e atualiza o editor do aplicativo MSIX para corresponder ao Publicador do certificado localizado em C:\cert\mycert.cer. Não há suporte para a assinatura de um aplicativo de formato de pacote MSIX com um certificado SHA1.

### <a name="update-the-publisher-and-sign-the-msix-app"></a>Atualizar o Publicador e assinar o aplicativo MSIX

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx"
```

Esse comando pesquisa recursivamente o conteúdo de C:\MSIX para todos os pacotes do MSIX e atualiza o editor do aplicativo MSIX para corresponder ao Publicador do certificado localizado em C:\cert\mycert.cer. Em seguida, o comando assina novamente os pacotes MSIX identificados usando o certificado localizado em C:\cert\CertKey.pfx. Não há suporte para a assinatura do aplicativo formato de pacote MSIX com um certificado SHA1.

### <a name="update-the-publisher-and-sign-the-msix-app-with-a-password-protected-pfx-certificate"></a>Atualizar o Publicador e assinar o aplicativo MSIX com um certificado PFX protegido por senha

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx" -password "aaabbbccc"
```

Esse comando pesquisa recursivamente o conteúdo de C:\MSIX para todos os pacotes do MSIX e atualiza o editor do aplicativo MSIX para corresponder ao Publicador do certificado localizado em C:\cert\mycert.cer. Em seguida, o comando assina novamente os pacotes MSIX identificados usando o certificado localizado em C:\cert\CertKey.pfx usando a senha *aaabbbccc* para desbloquear o certificado protegido por senha. Não há suporte para a assinatura do aplicativo formato de pacote MSIX com um certificado SHA1.

### <a name="update-the-publisher-sign-the-msix-app-and-force-continue-to-next-msix-app"></a>Atualize o Publicador, assine o aplicativo MSIX e Force continuar para o próximo aplicativo MSIX

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx" -forceContinue -pfxPath "C:\cert\CertKey.pfx"
```

Esse comando pesquisa recursivamente o conteúdo de C:\MSIX para todos os pacotes do MSIX e atualiza o editor do aplicativo MSIX para corresponder ao Publicador do certificado localizado em C:\cert\mycert.cer. Em seguida, o comando assina novamente os pacotes MSIX identificados usando o certificado localizado em C:\cert\CertKey.pfx. Se ocorrerem erros durante o processamento de um pacote MSIX, o script continuará atualizando o Publicador e assinará novamente os pacotes MSIX identificados. Não há suporte para a assinatura do aplicativo formato de pacote MSIX com um certificado SHA1.

## <a name="parameters"></a>Parâmetros

### <a name="-directory"></a>-diretório

Fornece o diretório raiz que contém aplicativos MSIX. Esse diretório é pesquisado recursivamente por todos os pacotes MSIX.

* **Tipo:** Strings
* **Necessário:** Ok
* **Posição:** Nomeado
* **Valor padrão:** None

### <a name="-certpath"></a>-certPath

Fornece o caminho completo para o arquivo de certificado (*. cer) usado para identificar as informações do Publicador do aplicativo novo ou atualizado.

* **Tipo:** Strings
* **Necessário:** Ok
* **Posição:** Nomeado
* **Valor padrão:** None

### <a name="-redist"></a>-Redist

O caminho para o arquivo redistribuível recuperado de dentro do [Kit de ferramentas do MSIX](https://aka.ms/msixtoolkit). Esse arquivo é usado para reempacotar o aplicativo no formato de pacote MSIX. Deve apontar para a arquitetura de 32 bits ou de 64 bits redistribuível.

* **Tipo:** Strings
* **Necessário:** Ok
* **Posição:** Nomeado
* **Valor padrão:** None

### <a name="-pfxpath"></a>-pfxPath

O caminho para o certificado de assinatura de código (*. pfx) que será usado para assinar o pacote MSIX após a atualização do editor do aplicativo.

* **Tipo:** Strings
* **Necessário:** Não
* **Posição:** Nomeado
* **Valor padrão:** None

### <a name="-password"></a>-password

A senha exigida pelo certificado de assinatura de código (*. pfx).

* **Tipo:** Strings
* **Necessário:** Não
* **Posição:** Nomeado
* **Valor padrão:** None

### <a name="-forcecontinue"></a>-forceContinue

Se especificado, o script irá ignorar erros e tentar atualizar as informações do Publicador de todos os aplicativos.

* **Tipo:** Strings
* **Necessário:** Não
* **Posição:** Nomeado
* **Valor padrão:** None
