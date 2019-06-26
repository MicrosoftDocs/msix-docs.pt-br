---
author: c-don
title: Como agrupar pacotes MSIX
description: Como agrupar pacotes MSIX de arquiteturas diferentes
ms.author: cdon
ms.date: 10/25/2018
ms.topic: article
keywords: Windows 10, MSIX
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 54e6f063aa05c1a297725e6c1c7a091ebffbb043
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2019
ms.locfileid: "59983438"
---
# <a name="bundle-msix-packages"></a>Agrupar pacotes de MSIX 

Este artigo descreve o processo de criação de um pacote depois de converter as versões x86 e x64 dos instaladores do Windows usando a Ferramenta de Empacotamento MSIX. 

Ao agrupar várias versões de arquitetura do instalador em uma só entidade, apenas o pacote precisa ser carregado na Store ou em outro local de distribuição. A plataforma de implantação do Windows 10 reconhece o tipo de pacote .msixbundle e só baixará os arquivos aplicáveis à arquitetura do dispositivo. Tenha em mente que, se você decidir distribuir um .msixbundle para determinado aplicativo, não será possível reverter para a distribuição de apenas um pacote MSIX. 

A seção a seguir apresenta uma abordagem passo a passo para a criação de um .msixbundle. Ela pressupõe que você já tenha [convertido as versões x86 e x64 existentes](https://docs.microsoft.com/windows/msix/mpt-best-practices) do instalador do Windows em pacotes MSIX. 

### <a name="setup"></a>Configuração
Você precisará da seguinte configuração para compilar com êxito um pacote MSIX:
- [SDK do Windows 10](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) (versão 1809 ou superior)
- Pacotes MSIX x64 e x86 convertidos 

## <a name="step-1-find-makeappxexe"></a>Etapa 1: Localizar o MakeAppx.exe
O [MakeAppx.exe](https://docs.microsoft.com/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) é uma ferramenta disponível no SDK do Windows 10 que permite o empacotamento e o agrupamento de pacotes MSIX. Você usará essa ferramenta para agrupar os dois pacotes MSIX. 

O MakeAppx.exe pode ser usado para extrair o conteúdo do arquivo de um pacote ou um pacote do aplicativo do Windows 10. Ele também criptografa e descriptografa pacotes e pacotes de aplicativos.

Após a instalação do SDK do Windows 10, MakeAppx.exe é geralmente encontrado aqui: 
- [x86] – C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe
- [x64] – C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x64\MakeAppx.exe

## <a name="step-2-bundle-the-packages"></a>Etapa 2: Agrupar os pacotes

A maneira mais fácil de agrupar pacotes com o MakeApp.exe é adicionar todos os pacotes que você deseja agrupar a uma pasta. O diretório precisa estar livre de todo o resto, exceto os pacotes que devem ser agrupados. 

Mova os pacotes de aplicativos que deseja agrupar para um diretório, conforme mostrado na captura de tela a seguir.

![Agrupar pacotes em um diretório](images/bundle-pic1.png)

>[!NOTE] 
> O MakeAppx.exe só agrupa os pacotes que têm a mesma identidade, o que significa que a AppID, o fornecedor e a versão precisam ser iguais. Somente a arquitetura do processador de pacote de um pacote do aplicativo pode ser diferente. 

O MakeAppx.exe tem a sintaxe de linha de comando a seguir.

```Prompt de Comando C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe" bundle /d input_directorypath /p <filepath>.msixbundle
```

Here is an example command.

```
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe" bundle /d c:\AppPackages\ /p c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

After running the command, an unsigned .msixbundle will be created in the path specified. Packages do not need to be signed before bundling.  

## Step 3: Sign the bundle

After you create the bundle, you must sign the package before you can distribute the app to your users or install it. 

To sign a package, you will need a general code signing certificate and use SignTool.exe from the Windows 10 SDK. 

We strongly recommend that you use a trusted cert from certificate authority as that allows for the package to be distributed and deployed on your end users devices seamlessly. Once you have access to the private certificate (.pfx file), you can sign the package as shown below.

>[!NOTE]
> SignTool.exe is available in the same directory as MakeAppx.exe in the Windows 10 SDK. 

SignTool.exe has the following command line syntax.

```Command Prompt
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd <Hash Algorithm> /a 
/f <Path to Certificate>.pfx /p <Your Password> <File path>.msixbundle
```

Veja a seguir um comando de exemplo.

```
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd SHA256 /a 
/f c:\private-cert.pfx /p aaabbb123 c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

Para obter mais informações sobre como assinar pacotes de aplicativos com SignTool.exe, confira [este artigo](https://docs.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool). 

Depois de assinar o pacote com êxito, você estará pronto para hospedá-lo em um compartilhamento de rede ou em qualquer rede de distribuição de conteúdo para distribuí-lo para seus usuários. 

