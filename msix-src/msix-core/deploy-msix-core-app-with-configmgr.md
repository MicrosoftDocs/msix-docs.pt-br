---
title: Implantar aplicativos MSIX core com o Microsoft Endpoint Configuration Manager
description: Descreve como implantar o MSIX core com o Microsoft Endpoint Configuration Manager.
ms.date: 03/02/2020
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 62e1e29d16733496613495b14c45532eaf25e53f
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090284"
---
# <a name="deploy-msix-core-apps-with-microsoft-endpoint-configuration-manager"></a>Implantar aplicativos MSIX core com o Microsoft Endpoint Configuration Manager
A entrega de aplicativos MSIXs usando o Microsoft Endpoint Configuration Manager permite que os profissionais de ti vinculem outros aplicativos como dependências, forçando-os a instalarem antes do. Ao criar uma dependência para o aplicativo MSIX Core, impõem o aplicativo MSIX core a ser instalado somente quando exigido pelo dispositivo. Para obter mais informações sobre dependências de aplicativo no ponto de extremidade Microsoft Configuration Manager consulte: [criar aplicativos: dependências do tipo de implantação](/configmgr/apps/deploy-use/create-applications#bkmk_dt-depend).

## <a name="get-started"></a>Introdução
As etapas a seguir guiarão você pela configuração de uma estratégia de implantação do MSIX Core usando o Microsoft Endpoint Configuration Manager:

1. [Implantar o MSIX Core com o Microsoft Endpoint Configuration Manager](deploy-msix-core-with-configmgr.md)
1. [Atualize seu pacote MSIX existente para dar suporte ao MSIX Core](support-msix-core.md)
1. Implantar aplicativos MSIX core com o Microsoft Endpoint Configuration Manager

## <a name="creating-the-msix-core-microsoft-endpoint-configuration-manager-application"></a>Criando o aplicativo MSIX Core Microsoft Endpoint Configuration Manager
O seguinte guiará você pela criação de um aplicativo de Configuration Manager de ponto de extremidade da Microsoft, com a finalidade de implantar aplicativos MSIX Core em dispositivos cliente.
 
Supondo que você seguiu os guias anteriores (consulte a lista de guias na seção Introdução acima) e tenha recuperado/atualizado/criado um aplicativo otimizado para o MSIX Core. Além disso, copiou o aplicativo para um compartilhamento de arquivos acessível pela ferramenta Microsoft Endpoint Configuration Manager. A próxima etapa é implantar o novo aplicativo em dispositivos cliente em seu ambiente.

### <a name="create-msix-core-dependent-application-in-microsoft-endpoint-configuration-manager"></a>Criar um aplicativo dependente do MSIX Core no Microsoft Endpoint Configuration Manager
1. No console do Microsoft Endpoint Configuration Manager, navegue até: **biblioteca de Software > visão geral/gerenciamento de aplicativos/aplicativos**.
1. Selecione **criar aplicativo** na faixa de opções.
1. Selecione o botão de opção **especificar manualmente as informações do aplicativo** .
1. Selecione o botão **Avançar**.
1. Insira os detalhes do aplicativo nos campos apropriados.
1. Selecione o botão **Avançar** duas vezes.
1. Selecione o botão **Adicionar**.
1. Defina o tipo para **instalador de script**.
1. Selecione o botão **Avançar**.
1. Insira o nome do aplicativo com um sufixo de " **-MSIXCore**" (IE: "Application Y-MSIXCore").
1. Selecione o botão **Avançar**.
1. Selecione o botão **procurar** ao lado de conteúdo local e navegue até o compartilhamento de arquivos que contém a mídia de instalação do aplicativo.
1. Selecione o botão **Selecionar pasta** .
1. Selecione o botão **procurar** ao lado de programa de instalação, defina o tipo de arquivo como **todos os arquivos (*.*)** e selecione a mídia de instalação.
1. Selecione o botão **abrir** .
1. Atualize o campo programa de instalação para: 
```batch
"C:\Program Files\msixmgr\msixmgr.exe -AddPackage [Application.msix] -quietUX"
```
1. Defina o campo programa de desinstalação como: 
```batch
"C:\Program Files\msixmgr\msixmgr.exe" -RemovePackage [Package Family Name] -quietUX
```
1. Substitua [Package Family Name] pelo nome da família de pacotes do aplicativo MSIX.
1. Selecione o botão **Avançar**.
1. Selecione o botão de opção **usar um script personalizado para detectar a presença desse tipo de implantação** .
1. Selecione o botão **Editar** .
1. Verifique se o tipo de script está definido como **PowerShell**
1. Insira o seguinte: 
```PowerShell
Set-Location "C:\Program Files\msixmgr"

IF([Boolean]$(get-item "msixmgr.exe"))
{
    $Result = $(.\msixmgr.exe -FindPackage [Package Family Name]*)

    IF($($Result.GetType().Name) -eq "Object[]")
    {
        Return 1
    }
}
```
1. Atualize [nome da família de pacotes] com o nome da família de pacotes MSIX do aplicativo.
1. Selecione o botão **OK** .
1. Selecione o botão **Avançar**.
1. Defina o comportamento de instalação para **instalar para o usuário**.
1. Defina o tempo de execução máximo permitido (minutos) e o tempo de instalação estimado (minutos) para os valores approriate para este aplicativo.
1. Defina a visibilidade do programa de instalação como **oculta**.
1. Selecione o botão **Avançar**.
1. Selecione o botão **Adicionar**.
1. Verifique se a categoria foi definida como **dispositivo**.
1. Definir a condição como **sistema operacional**
1. Selecione a caixa de seleção do **Windows 7** na lista de sistemas operacionais.
1. Selecione o botão **OK** .
1. Selecione o botão **Avançar**.
1. Selecione o botão **Adicionar**.
1. Defina o nome do grupo de dependência como **MSIX Core**.
1. Selecione o botão **Adicionar**.
1. Selecione **MSIX Core** na lista de aplicativos disponíveis.
1. Selecione as opções de 32 bits e 64 bits na lista tipos de implantação.
1. Selecione o botão **OK** .
1. Selecione o botão **OK** .
1. Selecione o botão **Avançar** duas vezes.
1. Selecione o botão ** Fechar **.

### <a name="add-non-msix-core-dependent-deployment-type"></a>Adicionar tipo de implantação dependente de núcleo não MSIX
1. Selecione o botão **Adicionar**.
1. Verifique se o tipo foi definido como **pacote do aplicativo Windows (*. Appx, *. appxbundle, *. msix, *. msixbundle) * *. 
1. Selecione o botão **procurar...** e navegue até a mídia de instalação do aplicativo MSIX Core enprocessador e selecione o botão **abrir** .
1. Selecione o botão **Avançar** seis vezes.
1. Selecione o botão ** Fechar **.
1. Selecione o botão **Avançar** duas vezes.
1. Selecione o botão ** Fechar **.