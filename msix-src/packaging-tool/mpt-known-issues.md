---
title: Problemas conhecidos e soluções de problemas
description: Descreve problemas conhecidos e fornece dicas de solução de problemas para a ferramenta de empacotamento MSIX.
author: dianmsft
ms.author: diahar
ms.date: 02/26/2019
ms.topic: article
keywords: ferramenta de empacotamento msix, problemas, solução de problemas conhecidos
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3853af303041fb0375557ec6e1735df9dbd793e7
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900728"
---
# <a name="known-issues-and-troubleshooting"></a>Problemas conhecidos e soluções de problemas

Este artigo descreve problemas conhecidos e fornece dicas de solução de problemas a serem considerados ao converter seus aplicativos em MSIX usando a ferramenta de empacotamento MSIX.

## <a name="known-issues"></a>Problemas conhecidos

### <a name="msix-packaging-tool-driver-considerations"></a>Considerações sobre a ferramenta de empacotamento MSIX driver

Driver da ferramenta de empacotamento MSIX é entregue como um [recurso na demanda FOD ()](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) do pacote do Windows Update e não será instalado se o serviço Windows Update está desabilitado no computador ou as configurações do Windows Insider voo anel não fazem nenhuma correspondência o sistema operacional compilação da máquina.

### <a name="installing-msix-packaging-tool-driver-fod-on-windows-insider-builds"></a>Instalando a ferramenta de empacotamento MSIX compilações de driver FOD no Windows Insider

Se você estiver tendo problemas ao instalar o driver em um build do Windows 10 Insider Preview:

- Navegue até **as configurações** > **atualizações e segurança** > **Windows Insider Program**.
- Se você vir uma mensagem de que as configurações de build do Insider Preview precisam de atenção, clique no **corrigir me** botão fazer logon novamente. Talvez você precise ir para página de atualização do Windows e verifique se há atualização antes da alteração das configurações entra em vigor.
- Tente executar a ferramenta novamente para baixar o driver MSIX ferramenta de empacotamento. Se você ainda estiver enfrentando problemas, tente alterar o anel de voo para Insider rápido, instale as atualizações mais recentes do Windows e tente novamente.

### <a name="installing-msix-packaging-tool-driver-fod-in-wsus"></a>Instalar o driver da ferramenta de empacotamento MSIX FOD no WSUS

As organizações que usam o Windows Server Update Services (WSUS) devem tomar medidas para instalar manualmente o driver:

- [Verifique sua versão do Windows](https://support.microsoft.com/help/13443/windows-which-operating-system). Você deve ter pelo menos Windows 10, versão 1809 (build 17763).
- Baixe o arquivo. cab FOD para [Windows 10, versão 1809](https://download.microsoft.com/download/8/4/3/8436215A-42DB-4FD2-966D-60D436D6EEFC/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab).
- Recurso obtido individualmente em pacotes de demanda FOD () pode ser instalado usando [opções de linha de comando DISM](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options). Em um tipo de janela do PowerShell com privilégios elevados: ```Dism /Online /add-package /packagepath:(path)```

Os administradores de TI também podem criar [(pasta compartilhada) do repositório de recursos lado a lado](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server) para permitir o acesso ao driver de ferramenta de empacotamento MSIX FOD. Você pode encontrar mais detalhes na parte inferior deste [postagem de blog](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Language-pack-acquisition-and-retention-for-enterprise-devices/ba-p/275404).

Caso contrário, se você tiver acesso ao enterprise ou canais OEM pode obter o driver de recursos do Windows 10 na mídia de demanda de uma das seguintes fontes:

- [Volume Licensing Service Center (VLSC)](https://www.microsoft.com/Licensing/servicecenter/default.aspx): É necessário acesso de licença de volume.
- [Portal de OEM](https://www.microsoftoem.com): É necessário acesso de OEM.
- [Download do MSDN](https://my.visualstudio.com/Downloads/Featured): Assinatura do MSDN é necessária.

Recurso obtido individualmente em pacotes de demanda pode ser instalado usando as opções de linha de comando DISM.

## <a name="other-known-issues"></a>Outros problemas conhecidos

- Não há suporte para reiniciar o computador durante a instalação do aplicativo. Ignorar a solicitação de reinicialização se possível ou passar um argumento para o instalador para não exigir uma reinicialização.
- Instaladores podem exigir determinadas estruturas ou os drivers sejam instalados antes da instalação. Para consultar estruturas e drivers no computador que está sendo usado para converter aplicativos, use as seguintes consultas: ```driverquery /v | Out-File``` ou ```driverquery /v | Out-File "path to text file"```

# <a name="troubleshooting"></a>Solução de problemas

## <a name="log-files"></a>Arquivos de log

Se a conversão foi bem-sucedida, os arquivos de log são gerados para cada conversão. Eles podem ser encontrados aqui: %localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\

Códigos de falha são escritos e indicam qualquer ponto de falha durante o processo de conversão. Os códigos de erro devem ser amigáveis.

## <a name="examples-of-failures-during-conversions"></a>Exemplos de falhas durante as conversões de

### <a name="uninstallation-error---exit-code-259"></a>Erro de desinstalação - código de saída 259

Alguns instaladores podem falhar ao converter com código de saída 259. Isso indica que o instalador gerado de um thread e não aguardou a conclusão. Em outras palavras, o thread principal concluir a instalação, mas ele foi encerrado com erro 259 porque ele gerado de um thread que ainda está em execução. É recomendável que você use a opção de instalação apropriado para setup.exe.

### <a name="internal-warning-messages"></a>Mensagens de aviso interna

Você pode encontrar mensagens de aviso, como: **[aviso] W_COM_PUBFAIL_INPROC_SERVER_NOT_SUPPORTED**.
Isso indica que a ferramenta de empacotamento MSIX detectou um registro COM que não têm uma correspondência com MSIX hoje mesmo. Se você converte um aplicativo e ele funciona, provavelmente que eles eram entradas desnecessárias do COM. No entanto, se você vir falta de comportamento, que significa que esse comportamento COM não foi capturado durante a conversão.

## <a name="sending-feedback"></a>Enviando comentários

A melhor maneira de enviar seus comentários é por meio de **Hub de comentários**.
1. Abra **Hub de comentários** ou digite **Windows + F**.
2. Forneça um título e as etapas necessárias para reproduzir o problema.
3. Sob **categoria**, selecione **Apps** e selecione **ferramenta de empacotamento MSIX**.
4. Anexe qualquer [arquivos de log](#log-files) associado à conversão. Você pode encontrar os logs na pasta fornecida acima.
5. Anexe o pacote MSIX convertido (se possível).
6. Clique em **Enviar**.

Você também pode enviar comentários diretamente da ferramenta de empacotamento MSIX indo para o **Feedback** guia sob **configurações**. 

> [!NOTE]
> Pode levar 24 horas para que seus comentários obter a nós. Portanto, se você estiver usando uma VM para converter seu pacote, convém manter sua VM no e no estado atual por 24 horas após a conversão. 
