---
author: c-don
title: Como agrupar pacotes MSIX
description: Pacotes MSIX de arquiteturas diferentes de agrupamento
ms.author: cdon
ms.date: 10/25/2018
ms.topic: article
keywords: windows 10, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 54e6f063aa05c1a297725e6c1c7a091ebffbb043
ms.sourcegitcommit: 9bbb116d1984082123f694130b4d6cc078fa8510
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983438"
---
# <a name="bundle-msix-packages"></a>Agrupar pacotes de MSIX 

Este artigo descreve o processo de criação de um pacote depois de converter as versões x86 e x64 dos seus instaladores do Windows usando a ferramenta de empacotamento MSIX. 

Reunindo as várias versões de arquitetura do seu instalador para uma entidade, apenas o pacote precisa ser carregado para a Store ou em outro local de distribuição. A plataforma de implantação do Windows 10 está ciente de que o tipo de pacote .msixbundle e simplesmente baixará os arquivos que são aplicáveis a arquitetura do seu dispositivo. Tenha em mente que, se você decidir distribuir um .msixbundle para um determinado aplicativo, não é possível reverter para distribuição apenas um pacote MSIX. 

A seção a seguir apresenta uma abordagem passo a passo para criar um .msixbundle. Ele pressupõe que você já tenha [convertidos em suas versões x86 e x64 existentes](https://docs.microsoft.com/windows/msix/mpt-best-practices) do instalador do Windows para pacotes MSIX. 

### <a name="setup"></a>Configuração
A configuração a seguir para compilar com êxito um pacote de MSIX será necessário:
- [SDK do Windows 10](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) (versão 1809 ou superior)
- Pacotes MSIX convertidos x64 e x86 

## <a name="step-1-find-makeappxexe"></a>Etapa 1: Localizar MakeAppx.exe
[MakeAppx.exe](https://docs.microsoft.com/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) é uma ferramenta disponível no SDK do Windows 10 que permitem empacotamento e agrupamento de pacotes MSIX. Você usará essa ferramenta para agrupar os dois pacotes MSIX. 

MakeAppx.exe pode ser usado para extrair o conteúdo do arquivo de um pacote de aplicativo do Windows 10 ou grupo. Ela também criptografa e descriptografa os pacotes e pacotes de aplicativos.

Depois de instalar o SDK do Windows 10, MakeAppx.exe costuma ser encontrado aqui: 
- [x86]-C:\Program arquivos (x86) \Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe
- [x64]-C:\Program arquivos (x86) \Windows Kits\10\bin\10.0.17763.0\x64\MakeAppx.exe

## <a name="step-2-bundle-the-packages"></a>Etapa 2: Os pacotes de pacote

A maneira mais fácil de agrupar pacotes com MakeApp.exe é adicionar todos os pacotes que você deseja agrupar em uma pasta. O diretório deve estar livre de todo o resto, exceto os pacotes que foram sejam agrupados. 

Mova os pacotes de aplicativo que você deseja agrupar em um diretório, conforme mostrado na seguinte captura de tela.

![Pacotes de pacote em um diretório](images/bundle-pic1.png)

>[!NOTE] 
> MakeAppx.exe agrupa somente pacotes que têm a mesma identidade, o que significa que o AppID, publisher, versão precisa ser o mesmo. Somente a arquitetura do processador de pacote para um pacote de aplicativo pode ser diferente. 

MakeAppx.exe tem a seguinte sintaxe de linha de comando.

' ' C: do Prompt de comando\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe" bundle /d input_directorypath /p <filepath>.msixbundle
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

Aqui está um exemplo de comando.

```
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd SHA256 /a 
/f c:\private-cert.pfx /p aaabbb123 c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

Para obter mais informações sobre a assinatura de pacotes de aplicativos com SignTool.exe, consulte [deste artigo](https://docs.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool). 

Depois de entrar com êxito o pacote, você está pronto para hospedá-lo em um compartilhamento de rede ou em qualquer rede de distribuição de conteúdo para distribuí-lo para seus usuários. 

