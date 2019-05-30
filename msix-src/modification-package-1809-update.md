---
author: dianmsft
title: Pacotes de modificação de MSIX na atualização do Windows 10 1809
description: Nesta seção, analisaremos os pacotes de modificação no Windows 10 1809 Update
ms.author: diahar
ms.date: 01/15/2019
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 7273f3866eb8ed416b8dc60a621498cbddec54bd
ms.sourcegitcommit: 5669d59a0979a9de1dead4949f44d1544fd45988
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795417"
---
# <a name="msix-modification-packages-on-windows-10-version-1809"></a>Pacotes de modificação de MSIX no Windows 10 versão 1809 

Começando com o Windows 10 versão 1809, você pode agora empacotar seus plug-ins baseada no registro e personalização dentro de um [pacote de modificação de MSIX](modification-packages.md). Personalização pode incluir configurações necessárias para ser definida no registro, de modo que o pacote principal será executado conforme o esperado. Que significa que seu aplicativo principal aproveita o registro para ver se existe um plug-in. Quando você implanta o pacote principal e o pacote de modificação, em tempo de execução do aplicativo exibirá o registro virtual (VREG) do pacote principal e o pacote de modificação. 

Observe que o pacote principal pode estar usando o VREG para fazer o seguinte: 
1.  Exibindo onde carregar o arquivo, ou seja, a dll do plug-in. Se esse for o caso, em seguida, certifique-se de que o arquivo faz parte do pacote. Ao fazer isso, o pacote principal é capaz de acessar o arquivo em tempo de execução.  
2.  Exibindo where ver o valor das chaves de VREG. O pacote principal pode estar procurando um valor que existe no VREG. Quando você cria seu pacote de modificação por manualmente ou usando nosso [ferramenta](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf), certifique-se de que o valor está correto. 

## <a name="create-a-modification-package-using-the-msix-packaging-tool"></a>Criar um pacote de modificação usando a ferramenta de empacotamento MSIX

Você pode criar um pacote de modificação com a ferramenta de empacotamento MSIX:
* Especifique o pacote principal. Certifique-se de ter a versão MSIX do seu pacote principal disponível no computador que você está convertendo em. Se não for que pedimos que você fornecer manualmente o publicador e as informações do aplicativo principal. Também alguma personalização exigem que o aplicativo principal é instalado em seu computador.
![Pacote de modificação NTAR](images/MPT-mod-page.png)

* Modifique o pacote depois que ele passou por conversão usando o editor de pacote. Pode haver um caso em que o pacote principal requer o pacote de modificação para ter certos valores no seu VREG. Isso é onde você poderia ir e editar o pacote adequadamente. 

## <a name="create-a-modification-package-using-makeappxexe"></a>Criar um pacote de modificação usando MakeAppx.exe

Você pode criar um pacote de modificação manualmente usando o [MakeAppX.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool) ferramenta que está incluída no SDK do Windows 10:
* No manifesto, especifique o pacote principal. Inclua o publicador e o nome do pacote principal.

    ```xml
    <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="12.0.0.0"/>
      <uap4:MainPackageDependency Name="HeadTrax" Publisher="CN=Contoso Software, O=Contoso Corporation, C=US" />
    </Dependencies>
    ```
- Criar Registry.dat, User. dat e Userclass.dat para criar quaisquer chaves do registro são necessárias para carregar seu pacote de modificação. Isso só será necessária se você precisa que seu aplicativo principal para exibir as chaves do registro personalizada. Lembre-se de que uma vez que tudo está sendo executado dentro de um contêiner em tempo de execução do pacote principal e o pacote de modificação virtual de registro será mesclagem tal pacote principal pode exibir o registro virtual de pacotes de modificação.  

Esse processo também dá suporte ao sistema de arquivo plug-ins e personalizações, desde que o executável do aplicativo principal não está em um sistema de arquivos virtual (VFS). Isso é para garantir que o pacote principal obterá VFS do pacote principal e o pacote de modificação. 

Suporte para o sistema de arquivo plug-ins e personalizações enquanto o executável do aplicativo principal está em um VFS é planejado para a próxima versão principal do Windows. Você pode visualizar esse suporte começando no Windows 10 Insider Preview compilar 18312 ou posterior. Para obter mais informações, consulte [este artigo](modification-package-1903.md). 

