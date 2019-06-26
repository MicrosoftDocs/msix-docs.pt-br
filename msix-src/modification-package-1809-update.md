---
author: dianmsft
title: Pacotes de modificação MSIX no Windows 10 atualização 1809
description: Nesta seção, examinaremos os pacotes de modificação no Windows 10 Atualização 1809
ms.author: diahar
ms.date: 01/15/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 7273f3866eb8ed416b8dc60a621498cbddec54bd
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2019
ms.locfileid: "65795417"
---
# <a name="msix-modification-packages-on-windows-10-version-1809"></a>Pacotes de modificação MSIX no Windows 10 versão 1809 

No Windows 10 versão 1809 em diante, agora você pode empacotar os plug-ins baseados no Registro e a personalização em um [pacote de modificação MSIX](modification-packages.md). A personalização pode incluir configurações necessárias para serem definidas no Registro, de tal forma que o pacote principal seja executado conforme o esperado. Isso significa que o aplicativo principal utiliza o Registro para ver se um plug-in existe. Quando você implantar o pacote principal e o pacote de modificação, em tempo de execução, o aplicativo exibirá o VREG (Registro virtual) do pacote principal e do pacote de modificação. 

Observe que o pacote principal pode estar usando o VREG para fazer o seguinte: 
1.  Exibir em que local o arquivo, ou seja, a DLL do plug-in, será carregado. Se esse for o caso, verifique se o arquivo faz parte do pacote. Ao fazer isso, o pacote principal pode acessar o arquivo em tempo de execução.  
2.  Exibir em que local o valor das chaves do VREG pode ser visto. O pacote principal pode estar procurando um valor que existe no VREG. Ao criar o pacote de modificação manualmente ou usando nossa [ferramenta](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf), verifique se o valor está correto. 

## <a name="create-a-modification-package-using-the-msix-packaging-tool"></a>Criar um pacote de modificação usando a Ferramenta de Empacotamento MSIX

Crie um pacote de modificação com a Ferramenta de Empacotamento MSIX:
* Especifique o pacote principal. Verifique se você tem a versão do MSIX do pacote principal disponível no computador no qual está sendo feita a conversão. Caos contrário, solicitamos que você forneça manualmente as informações do fornecedor e do aplicativo principal. Além disso, uma personalização exige a instalação do aplicativo principal no computador.
![MPT do pacote de modificação](images/MPT-mod-page.png)

* Modifique o pacote depois que ele passar pela conversão usando o editor de pacote. Poderá haver casos em que o pacote principal exija que o pacote de modificação tenha certos valores no VREG. É nesse momento que você pode acessar e editar o pacote de forma adequada. 

## <a name="create-a-modification-package-using-makeappxexe"></a>Criar um pacote de modificação usando o MakeAppx.exe

Crie um pacote de modificação manualmente usando a ferramenta [MakeAppX.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool) que está incluída no SDK do Windows 10:
* No manifesto, especifique o pacote principal. Inclua o fornecedor e o nome do pacote principal.

    ```xml
    <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="12.0.0.0"/>
      <uap4:MainPackageDependency Name="HeadTrax" Publisher="CN=Contoso Software, O=Contoso Corporation, C=US" />
    </Dependencies>
    ```
- Crie Registry.dat, User.dat e Userclass.dat para criar as chaves do Registro necessárias para carregar o pacote de modificação. Isso só será necessário se você precisar que o aplicativo principal exiba chaves do Registro personalizadas. Lembre-se de que, como tudo está sendo executado dentro de um contêiner, em tempo de execução, o Registro virtual do pacote principal e do pacote de modificação será mesclado, de modo que o pacote principal possa exibir o Registro virtual dos pacotes de modificação.  

Esse processo também dá suporte aos plug-ins e às personalizações do sistema de arquivos, desde que o executável do aplicativo principal não esteja em um VFS (sistema de arquivos virtual). Isso serve para garantir que o pacote principal obtenha todo o VFS do pacote principal e do pacote de modificação. 

O suporte para os plug-ins e as personalizações do sistema de arquivos enquanto o executável do aplicativo principal está em um VFS está planejado para a próxima versão principal do Windows. Visualize esse suporte no Build do Windows 10 Insider Preview 18312 ou posterior. Para obter mais informações, consulte [este artigo](modification-package-1903.md). 

