---
title: Como agrupar pacotes MSIX
description: Este artigo descreve o processo de criação de um pacote após a conversão de versões x86 e x64 de seus instaladores de aplicativo usando a ferramenta de empacotamento MSIX.
ms.date: 10/25/2018
ms.topic: article
keywords: Windows 10, MSIX
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: d7eaa493577c4a83739829c1bee51acbb41640a8
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77072926"
---
# <a name="bundle-msix-packages"></a>Agrupar pacotes de MSIX

Este artigo descreve o processo de criação de um pacote depois de converter as versões x86 e x64 dos instaladores do Windows usando a Ferramenta de Empacotamento MSIX. 

Ao agrupar várias versões de arquitetura do instalador em uma só entidade, apenas o pacote precisa ser carregado na Store ou em outro local de distribuição. A plataforma de implantação do Windows 10 reconhece o tipo de pacote .msixbundle e só baixará os arquivos aplicáveis à arquitetura do dispositivo. Tenha em mente que, se você decidir distribuir um .msixbundle para determinado aplicativo, não será possível reverter para a distribuição de apenas um pacote MSIX. 

A seção a seguir apresenta uma abordagem passo a passo para a criação de um .msixbundle. Ela pressupõe que você já tenha [convertido as versões x86 e x64 existentes](https://docs.microsoft.com/windows/msix/tool-best-practices) do instalador do Windows em pacotes MSIX. 

### <a name="setup"></a>Instalação

Você precisará da seguinte configuração para compilar com êxito um pacote MSIX:

- [SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk) (versão 1809 ou superior)
- Pacotes MSIX x64 e x86 convertidos

## <a name="step-1-find-makeappxexe"></a>Etapa 1: localizar MakeAppx. exe

O [MakeAppx.exe](https://docs.microsoft.com/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) é uma ferramenta disponível no SDK do Windows 10 que permite o empacotamento e o agrupamento de pacotes MSIX. Você usará essa ferramenta para agrupar os dois pacotes MSIX.

O MakeAppx.exe pode ser usado para extrair o conteúdo do arquivo de um pacote ou um pacote do aplicativo do Windows 10. Ele também criptografa e descriptografa pacotes e pacotes de aplicativos.

Após a instalação do SDK do Windows 10, MakeAppx.exe é geralmente encontrado aqui:

- [x86] – C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe
- [x64] – C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x64\MakeAppx.exe

## <a name="step-2-bundle-the-packages"></a>Etapa 2: agrupar os pacotes

A maneira mais fácil de agrupar pacotes com o MakeApp.exe é adicionar todos os pacotes que você deseja agrupar a uma pasta. O diretório precisa estar livre de todo o resto, exceto os pacotes que devem ser agrupados.

Mova os pacotes de aplicativos que deseja agrupar para um diretório, conforme mostrado na captura de tela a seguir.

![Agrupar pacotes em um diretório](images/bundle-pic1.png)

>[!NOTE]
> O MakeAppx.exe só agrupa os pacotes que têm a mesma identidade, o que significa que a AppID, o fornecedor e a versão precisam ser iguais. Somente a arquitetura do processador de pacote de um pacote do aplicativo pode ser diferente.

O MakeAppx.exe tem a sintaxe de linha de comando a seguir.

```Command Prompt
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe" bundle /d input_directorypath 
/p <filepath>.msixbundle
```

Veja a seguir um comando de exemplo.

```
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe" bundle /d c:\AppPackages\ 
/p c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

Após executar o comando, um .msixbundle não assinado será criado no caminho especificado. Os pacotes não necessitam ser assinados antes do agrupamento.  

## <a name="step-3-sign-the-bundle"></a>Etapa 3: assinar o pacote

Após criar o lote, você deve assinar o pacote antes de distribuir o aplicativo para os usuários ou instalá-lo. 

Para assinar um pacote, você precisará de um certificado de assinatura de código geral e usar o SignTool.exe do SDK do Windows 10. 

É altamente recomendável que você use um certificado confiável da autoridade de certificação, pois isso permite que o pacote seja distribuído e implantado nos dispositivos dos usuários finais diretamente. Após ter acesso ao certificado privado (arquivo. pfx), você pode assinar o pacote conforme mostrado abaixo.

>[!NOTE]
> O SignTool. exe está disponível no mesmo diretório que o MakeAppx. exe no SDK do Windows 10. 

O MakeAppx.exe tem a sintaxe de linha de comando a seguir.

```Command Prompt
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd <Hash Algorithm> /a 
/f <Path to Certificate>.pfx /p <Your Password> <File path>.msixbundle
```

Veja a seguir um comando de exemplo.

```
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd SHA256 /a 
/f c:\private-cert.pfx /p aaabbb123 c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

Para obter mais informações sobre como assinar pacotes de aplicativos com SignTool.exe, confira [este artigo](../package/sign-app-package-using-signtool.md). 

Depois de assinar o pacote com êxito, você estará pronto para hospedá-lo em um compartilhamento de rede ou em qualquer rede de distribuição de conteúdo para distribuí-lo para seus usuários. 

