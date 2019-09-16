---
title: Problemas conhecidos e soluções de problemas
description: Descreve os problemas conhecidos e fornece dicas de solução de problemas para a Ferramenta de Empacotamento MSIX.
ms.date: 02/26/2019
ms.topic: article
keywords: Ferramenta de Empacotamento MSIX, problemas conhecidos, solução de problemas
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b38231f6cf918ecbcd598c01e17c68e4e072cfd5
ms.sourcegitcommit: 9cb3d2cdbe03b300bef60ed949e5e4d3b24d35ba
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70863999"
---
# <a name="known-issues-and-troubleshooting"></a>Problemas conhecidos e soluções de problemas

Este artigo descreve os problemas conhecidos e fornece dicas de solução de problemas a serem considerados ao converter seus aplicativos em MSIX usando a Ferramenta de Empacotamento MSIX.

## <a name="known-issues"></a>Problemas conhecidos

### <a name="msix-packaging-tool-driver-considerations"></a>Considerações sobre o driver da Ferramenta de Empacotamento MSIX

O driver da Ferramenta de Empacotamento MSIX é entregue como um pacote [FOD (Recurso sob Demanda)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) do Windows Update e não será instalado se o serviço Windows Update estiver desabilitado no computador ou se as configurações do anel da versão de pré-lançamento do Usuário do Windows Insider não corresponderem ao build de sistema operacional do computador.

### <a name="installing-msix-packaging-tool-driver-fod-on-windows-insider-builds"></a>Como instalar o FOD do driver da Ferramenta de Empacotamento MSIX em builds do Usuário do Windows Insider

Se você estiver tendo problemas ao instalar o driver em um build do Windows 10 Insider Preview:

- Procure **Configurações** > **Atualizações e Segurança** > **Programa Windows Insider**.
- Se uma mensagem for exibida informando que as configurações de build do Insider Preview precisam de atenção, clique no botão **Corrigir-me** para fazer logon novamente. Talvez você precise acessar a página do Windows Update e verificar se há alguma atualização antes que a alteração das configurações entre em vigor.
- Tente executar a ferramenta novamente para baixar o driver da Ferramenta de Empacotamento MSIX. Caso ainda esteja tendo problemas, tente alterar o anel da versão de pré-lançamento para Participante do Programa Windows Insider – Modo Rápido, instale as atualizações mais recentes do Windows e tente novamente.

### <a name="installing-msix-packaging-tool-driver-fod-in-wsus"></a>Como instalar o FOD do driver da Ferramenta de Empacotamento MSIX no WSUS

As organizações que usam o WSUS (Windows Server Update Services) precisam executar ações para instalar o driver manualmente:

- [Verifique sua versão do Windows](https://support.microsoft.com/help/13443/windows-which-operating-system). É necessário ter, pelo menos, o Windows 10, versão 1809 (build 17763).
- Baixe o arquivo .cab FOD para o [Windows 10, versão 1809](https://download.microsoft.com/download/8/4/3/8436215A-42DB-4FD2-966D-60D436D6EEFC/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab).
- Os pacotes FOD (Recurso sob Demanda) obtidos individualmente podem ser instalados usando [opções de linha de comando do DISM](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options). Em uma janela do PowerShell com privilégios elevados, digite: ```Dism /Online /add-package /packagepath:(path)```

Os administradores de TI também podem criar um [Repositório de recursos lado a lado (pasta compartilhada)](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server) para permitir o acesso ao FOD do driver da Ferramenta de Empacotamento MSIX. Encontre mais detalhes ao final desta [postagem no blog](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Language-pack-acquisition-and-retention-for-enterprise-devices/ba-p/275404).

Caso contrário, se você tiver acesso aos canais empresariais ou do OEM, poderá obter o driver da mídia de Recursos sob Demanda do Windows 10 em uma das seguintes fontes:

- [VLSC (Centro de Serviços de Licenciamento por Volume)](https://www.microsoft.com/Licensing/servicecenter/default.aspx): É necessário ter acesso à licença de volume.
- [Portal do OEM](https://www.microsoftoem.com): É necessário ter acesso do OEM.
- [Download do MSDN](https://my.visualstudio.com/Downloads/Featured): É necessário ter uma assinatura do MSDN.

Os pacotes de Recurso sob Demanda obtidos individualmente podem ser instalados usando opções de linha de comando do DISM.

### <a name="getting-the-msix-packaging-tool-for-offline-use"></a>Como obter a Ferramenta de Empacotamento MSIX para uso offline

A Ferramenta de Empacotamento MSIX pode ser baixada para uso offline na empresa no [portal da Web](https://businessstore.microsoft.com/store) do Microsoft Store para Empresas. Saiba mais sobre a distribuição offline [aqui](https://docs.microsoft.com/microsoft-store/distribute-offline-apps). Caso tenha problemas com a cópia offline da ferramenta de empacotamento, verifique se você tem a [cópia offline da licença](https://docs.microsoft.com/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app) para a ferramenta. 

Após obter a versão offline do aplicativo é possível usar o [PowerShell](https://docs.microsoft.com/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps) para adicionar o pacote do aplicativo e a licença ao seu computador.


## <a name="other-known-issues"></a>Outros problemas conhecidos

- Os instaladores podem exigir que estruturas ou drivers específicos sejam instalados antes da instalação. Para pesquisar as estruturas e os drivers no computador que estão sendo usados para converter aplicativos, use as seguintes consultas: ```driverquery /v | Out-File``` ou ```driverquery /v | Out-File "path to text file"```
- Durante a conversão, os instaladores podem executar os serviços. Os serviços não são capturados durante a conversão. Como resultado, o aplicativo pode ser instalado, mas ser executado com problemas.

# <a name="troubleshooting"></a>Solução de problemas

## <a name="log-files"></a>Arquivos de log

Independentemente de a conversão ter sido bem-sucedida ou não, os arquivos de log são gerados para cada conversão. Eles podem ser encontrados aqui: 

`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\`

Os códigos de falha são escritos e indicam qualquer ponto de falha durante o processo de conversão. Os códigos de erro destinam-se a ser amigáveis.

### <a name="log-files-from-remote-devices-or-vms"></a>Arquivos de log de dispositivos remotos ou VMs

Se a conversão for executada em um dispositivo remoto ou uma VM, recomendaremos que você copie os arquivos de log desse dispositivo e anexe-os como parte do item de comentário. Isso nos ajudará a diagnosticar e resolver problemas com mais eficiência. 

Você encontrará os logs das conversões remotas aqui: `%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\<Logs_#>\RemoteServer\Log.txt`

Seria ainda mais útil se você pudesse compartilhar toda a pasta Logs que incluirá as operações ocorridas no cliente local, bem como no servidor remoto.

## <a name="examples-of-failures-during-conversions"></a>Exemplos de falhas durante conversões

### <a name="uninstallation-error---exit-code-259"></a>Erro de desinstalação – código de saída 259

Alguns instaladores podem falhar ao fazer a conversão com o código de saída 259. Isso indica que o instalador gerou um thread e não aguardou a conclusão dele. Em outras palavras, o thread principal concluiu a instalação, mas ele foi encerrado com o erro 259 porque gerou um thread que ainda está em execução. Recomendamos que você use a opção de instalação apropriada para setup.exe.

### <a name="internal-warning-messages"></a>Mensagens de aviso internas

Você pode encontrar mensagens de aviso como: **[Aviso] W_COM_PUBFAIL_INPROC_SERVER_NOT_SUPPORTED**.
Isso indica que a Ferramenta de Empacotamento MSIX detectou um registro COM que, atualmente, não têm uma correspondência com o MSIX. Se você converter um aplicativo e ele funcionar, provavelmente essas serão as entradas COM desnecessárias. No entanto, se você observar um comportamento ausente, isso significará que esse comportamento COM não foi capturado durante a conversão.

## <a name="sending-feedback"></a>Como enviar comentários

A melhor maneira de enviar seus comentários é por meio do **Hub de Feedback**.
1. Abra o **Hub de Feedback** ou digite **Windows + F**.
2. Forneça um título e as etapas necessárias para reproduzir o problema.
3. Em **Categoria**, selecione **Aplicativos** e selecione **Ferramenta de Empacotamento MSIX**.
4. Anexe os [arquivos de log](#log-files) associados à conversão. Encontre os logs na pasta fornecida acima.
5. Anexe o pacote MSIX convertido (se possível).
6. Clique em **Enviar**.

Envie-nos também comentários diretamente da Ferramenta de Empacotamento MSIX acessando a guia **Comentários** em **Configurações**. 

> [!NOTE]
> Poderá levar 24 horas até recebermos seus comentários. Portanto, se você estiver usando uma VM para converter o pacote, o ideal será manter a VM ligada e em seu estado atual durante 24 horas após a conversão. 
