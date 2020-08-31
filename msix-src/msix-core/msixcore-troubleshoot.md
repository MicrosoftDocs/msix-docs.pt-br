---
title: Dicas de solução de problemas
description: Este artigo lista os códigos de erro e os problemas que os clientes podem enfrentar ao trabalhar com o MSIX Core
ms.date: 11/15/2019
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 2a18add1641e56cf3117ab8ad281a9696538e95e
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090264"
---
# <a name="troubleshooting-issues-for-msix-core"></a>Solucionando problemas do MSIX Core

Este artigo descreve os códigos de erro que você pode encontrar ao instalar pacotes do MSIX com o MSIX Core e dicas de solução de problemas.

## <a name="error-codes"></a>Códigos do Erro

Aqui estão mensagens de erro comuns que você pode encontrar.

| Código do Erro |Descrição |
|------------|------------|
| 0x8BAD0042 | Isso normalmente significa que o certificado com o qual o aplicativo foi assinado não está instalado. Para resolver isso, instale o certificado e tente novamente| 
| 0x80070032 | O pacote contém comentários que o MSIX Core não dá suporte. Por exemplo, não há suporte para algumas funcionalidades da estrutura de suporte do pacote. Essas são a estrutura de suporte a pacotes que chama um script que é executado no final da instalação, os scripts que são definidos para execução são iguais a falsas ou a estruturas de suporte a pacotes formatados incorretamente. | 
|0x8BAD0071 | Esse erro significa que você está tentando instalar um pacote. O MSIX Core atualmente não dá suporte a pacotes.|

Os erros a seguir ocorrem quando há um problema com o formato do pacote.

| Código do Erro |Descrição |
|------------|:------------|
| 0x8BAD0031 | MissingAppxSignatureP7X|
| 0x8BAD0032 | MissingContentTypesXML|
| 0x8BAD0033 | MissingAppxBlockMapXML|
| 0x8BAD0034 | MissingAppxManifestXML|
| 0x8BAD0035 | DuplicateFootprintFile |
| 0x8BAD0036 | UnknownFileNameEncoding |
| 0x8BAD0037 | Duplicatafile |

Os erros a seguir estão relacionados a problemas de arquivo

 | Código do Erro |Descrição |
|------------|:------------|
| 0x8BAD0001 | FileOpen|
| 0x8BAD0002 | FileSeek|
| 0x8BAD0003 | Fileread|
| 0x8BAD0003 | Filewrite|
| 0x8BAD0004 | FileCreateDirectory  |
| 0x8BAD0005 | FileSeekOutOfRange  |

Os erros a seguir ocorrem quando há um problema com o certificado com o qual o pacote foi assinado. 

| Código do Erro |Descrição |
|------------|:------------|
| 0x8BAD0041 | SignatureInvalid| 
| 0x8BAD0042 | CertNotTrusted|
| 0x8BAD0043 | PublisherMismatch|

Outros problemas que você pode encontrar

| Código do Erro |Descrição |
|------------|:------------|
| 0x8BAD0011 | ZipCentralDirectoryHeader|
| 0x8BAD0012 | ZipLocalFileHeader|
| 0x8BAD0013 | Zip64EOCDRecord|
| 0x8BAD0014 | Zip64EOCDLocator |
| 0x8BAD0015 | ZipEOCDRecord |
| 0x8BAD0016 | ZipHiddenData |
| 0x8BAD0017 | ZipBadExtendedData | 

| Código do Erro |Descrição |
|------------|:------------|
| 0x8BAD0051 | BlockMapSemanticError|
| 0x8BAD0052 | BlockMapInvalidData|

| Código do Erro |Descrição |
|------------|:------------|
| 0x8BAD0061 | AppxManifestSemanticError |
| 0x8BAD0082 | DeflateInitialize |
| 0x8BAD0081 | DeflateWrite |
| 0x8BAD0083 | DeflateRead  |

| Código do Erro |Descrição |
|------------|:------------|
| 0x8BAD1001 | Xmlwarning  |
| 0x8BAD1002 | Xmlerror|
| 0x8BAD1003 | Xmlfatal |
| 0x8BAD1004 | XmlInvalidData |

Para procurar outros códigos de erro, acesse [aqui](/windows/win32/debug/system-error-codes).

Para obter uma lista completa, visite a página de [código de erro do MSIX Core](https://github.com/microsoft/msix-packaging/blob/master/src/inc/public/MsixErrors.hpp) . 

## <a name="msix-tracing-powershell-script"></a>Script do PowerShell de rastreamento de MSIX

Vá para nossa [página de lançamento](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-1.1-release) e baixe **msixtrace.ps1**. Este é o script do PowerShell de rastreamento MSIX que gerará logs para ajudar se você estiver se estendo em um problema com a instalação do MSIX.

Use os comandos a seguir

```PowerShell
msixtrace.ps1 -wait
``` 

Siga o prompt que o script apresenta para gerar os logs. Ou use os comandos a seguir.

```PowerShell
msixtrace.ps1 -start
```

Instale o pacote MSIX. Ao concluir, conclua com o comando a seguir.

```PowerShell
msixtrace.ps1 -stop
```