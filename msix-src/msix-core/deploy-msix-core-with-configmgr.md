---
title: Implantar o MSIX core com o Microsoft Endpoint Configuration Manager
description: Descreve como implantar o MSIX core com o Microsoft Endpoint Configuration Manager.
ms.date: 03/02/2020
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: faa5930763d1bb554f3c21a8f390e67996b54cf4
ms.sourcegitcommit: fa41875f6c2b79db3d7dde29b10c0f24765532bc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79111308"
---
# <a name="deploy-msix-core-with-microsoft-endpoint-configuration-manager"></a>Implantar o MSIX core com o Microsoft Endpoint Configuration Manager
O MSIX Core permite que uma empresa padronize o formato de empacotamento MSIX para seus aplicativos. Os profissionais de ti podem facilmente implantar o instalador do MSIX Core em seus dispositivos e permite: instalar, consultar e remover aplicativos empacotados do MSIX (que já funcionam nessas versões do Windows).

O MSIX core fornece um subconjunto de recursos do MSIX nativo, funcionando de forma semelhante aos tipos existentes do Win32 Installer. O MSIX Core não fornece os benefícios do contêiner do MSIX nativo, nem permite que um aplicativo que usa recursos específicos do Windows 10 funcione em versões anteriores do Windows.

## <a name="get-started"></a>Introdução
As etapas a seguir guiarão você pela configuração de uma estratégia de implantação do MSIX Core usando o Microsoft Endpoint Configuration Manager:

1. Implantar o MSIX core com o Microsoft Endpoint Configuration Manager
1. [Atualize seu pacote MSIX existente para dar suporte ao MSIX Core](support-msix-core.md)
1. [Implantar aplicativos MSIX core com o Microsoft Endpoint Configuration Manager](deploy-msix-core-app-with-configmgr.md)

> [!Note]
> Opcionalmente, depois de seguir este guia, você pode optar por enviar por push a instalação do aplicativo MSIX Core para todos os dispositivos necessários em seu ambiente antes de instalar qualquer aplicativo otimizado para o núcleo do MSIX.

## <a name="creating-the-msix-core-application"></a>Criando o aplicativo MSIX Core
O seguinte guiará você pela criação de um aplicativo de Configuration Manager de ponto de extremidade da Microsoft, com a finalidade de implantar o MSIX Core em dispositivos Windows 7. Se você pretende enviar isso por push para outras versões do sistema operacional Windows, altere os requisitos do aplicativo para direcionar a essas versões de sistema operacional selecionadas.
 
### <a name="download-msix-core"></a>Baixar o MSIX Core
1. Baixe a versão mais recente do [MSIX Core](https://github.com/microsoft/msix-packaging/releases).
1. Mova a mídia de instalação do MSIX Core (x86/x64) baixada para um compartilhamento de arquivos que é acessível pelo seu ambiente de Configuration Manager de ponto de extremidade da Microsoft.

### <a name="create-microsoft-endpoint-configuration-manager-application---msix-core"></a>Criar Microsoft Endpoint Configuration Manager Application-MSIX Core
1. Abra o console do Microsoft Endpoint Configuration Manager. Navegue até **biblioteca de Software > visão geral \ gerenciamento de aplicativos \ aplicativos.**
1. Selecione **criar aplicativo** na faixa de opções.
1. Verifique se o **tipo** foi definido como **Windows Installer (arquivo *. msi)** . 
1. Selecione o botão **procurar...** e navegue até a mídia de instalação do MSIX Core (x64) baixada e, em seguida, selecione o botão **abrir** .
1. Selecione o botão **Avançar** duas vezes.
1. Examine as informações listadas e atualize todos os campos em branco ou ausentes (opcional).
1. Selecione o botão **Avançar** duas vezes.
1. Selecione o botão **fechar** .
1. Clique com o botão direito do mouse em **MSIX Core** de dentro da lista de aplicativos e escolha **Propriedades** no menu suspenso.
1. Selecione a guia **tipos de implantação** na parte superior da janela aberta recentemente.
1. Selecione **MSIX core – Windows Installer (arquivo *. msi)** na lista de tipos de implantação e selecione o botão **Editar** .
1. Defina o **nome** como "**MSIX core-Windows Installer (arquivo *. msi)-64-bit**".
1. Selecione a guia **requisitos** na parte superior da janela Propriedades abertas recentemente.
1. Selecione o botão **Adicionar** .
1. Defina a categoria como **dispositivo**e defina a condição como **sistema operacional**.
1. Na lista de sistemas operacionais, selecione a caixa de seleção **Windows 7 > todos os Windows 7 (64 bits)** .
1. Selecione o botão **OK** duas vezes.
1. Selecione o botão **Adicionar** .
1. Verifique se o **tipo** foi definido como **Windows Installer (arquivo *. msi)** .
1. Selecione o botão **procurar...** e navegue até a mídia de instalação do MSIX Core (x86) baixada e, em seguida, selecione o botão **abrir** .
1. Selecione o botão **Avançar** duas vezes.
1. Examine as informações listadas e atualize todos os campos em branco ou ausentes (opcional).
1. Selecione o botão **Avançar** duas vezes.
1. Selecione o botão **fechar** .
1.  Selecione **MSIX core – Windows Installer (arquivo *. msi)** na lista de tipos de implantação e selecione o botão **Editar** .
1. Defina o **nome** como "**MSIX core-Windows Installer (arquivo *. msi)-32-bit**".
1. Selecione a guia **requisitos** na parte superior da janela Propriedades abertas recentemente.
1. Selecione o botão **Adicionar** .
1. Defina a categoria como **dispositivo**e defina a condição como **sistema operacional**.
1. Na lista de sistemas operacionais, selecione a caixa de seleção **Windows 7 > todos os Windows 7 (32 bits)** .
1. Selecione o botão **OK** três vezes.

## <a name="distribute-msix-core-application-to-distribution-points"></a>Distribuir o aplicativo MSIX Core para pontos de distribuição
O seguinte guiará você pela distribuição da mídia de instalação do MSIX Core para todos os pontos de distribuição-supondo que o grupo de pontos de distribuição "todos os pontos de distribuição" exista em seu ambiente. Selecione o destino apropriado (ponto de distribuição, grupo de pontos de distribuição ou coleção) para o seu ambiente.

1. De dentro do console do Microsoft Endpoint Configuration Manager: **biblioteca de Software > visão geral/gerenciamento de aplicativos/aplicativos** 
1. Clique com o botão direito do mouse em **MSIX Core** na lista de aplicativos.
1. Selecione **distribuir conteúdo** no menu suspenso.
1. Selecione o botão **Avançar** duas vezes.
1. Selecione o botão **Adicionar** e escolha **grupo de pontos de distribuição** no menu suspenso.
1. Marque a caixa de seleção **todos os pontos de distribuição** e selecione o botão **OK** .
1. Selecione o botão **Avançar** duas vezes.
1. Selecione o botão **fechar** .
