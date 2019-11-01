---
title: Problemas conhecidos da ferramenta de empacotamento MSIX e dicas de solução de problemas
description: Descreve problemas conhecidos e fornece dicas de solução de problemas a serem consideradas ao converter seus aplicativos para MSIX usando a ferramenta de empacotamento MSIX.
ms.date: 10/04/2019
ms.topic: article
keywords: Ferramenta de Empacotamento MSIX, problemas conhecidos, solução de problemas
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6267957f49cd7a0ca01d9f4e4095bb500d33af69
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328370"
---
# <a name="known-issues-and-troubleshooting-tips-for-the-msix-packaging-tool"></a>Problemas conhecidos e dicas de solução de problemas para a ferramenta de empacotamento MSIX

Este artigo descreve os problemas conhecidos e fornece dicas de solução de problemas a serem considerados ao converter seus aplicativos em MSIX usando a Ferramenta de Empacotamento MSIX.

## <a name="known-issues"></a>Problemas conhecidos

### <a name="getting-the-latest-insider-preview-build-of-the-msix-packaging-tool"></a>Obtendo a mais recente versão prévia do insider preview da ferramenta de empacotamento MSIX

Se você tiver optado pelo nosso [programa Insider](insider-program.md), verifique se você tem a versão correta da ferramenta de empacotamento MSIX:
- Vá para a seção **sobre** na ferramenta de empacotamento MSIX para exibir em qual versão você está.

- Acesse [aqui](insider-program.md#current-insider-preview-build) para determinar a versão mais recente do insider Preview e confirme se você tem essa versão da ferramenta de empacotamento MSIX instalada. Verifique se o MSA que se inscreveu para o vôo é a conta que está conectada à Microsoft Store. 
- Atualize manualmente a ferramenta de empacotamento MSIX por meio do Microsoft Store em seu computador. Se essa opção estiver disponível para você, abra a loja, vá para **downloads e atualizações**e clique em **obter atualizações**.
- Para instalar a ferramenta de empacotamento MSIX para uso offline, siga [estas instruções](#getting-the-msix-packaging-tool-for-offline-use) para garantir que você obtenha o aplicativo mais recente por meio do nosso processo offline.

Se você estiver interessado em ingressar em nosso programa Insider, clique [aqui](https://aka.ms/MSIXPackagingPreviewProgram).

### <a name="msix-packaging-tool-driver-considerations"></a>Considerações sobre o driver da Ferramenta de Empacotamento MSIX

O driver da ferramenta de empacotamento MSIX é fornecido como um pacote de [fod (recurso sob demanda)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) do Windows Update e não será instalado se o serviço de Windows Update estiver desabilitado no computador ou as configurações do Windows Insider Flight Ring não corresponderem à compilação do sistema operacional do Ele.

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

- [VLSC (centro de serviços de licenciamento por volume)](https://www.microsoft.com/Licensing/servicecenter/default.aspx): o acesso à licença por volume é necessário.
- [Portal OEM](https://www.microsoftoem.com): é necessário acesso ao OEM.
- [Download do MSDN](https://my.visualstudio.com/Downloads/Featured): a assinatura do MSDN é necessária.

Os pacotes de Recurso sob Demanda obtidos individualmente podem ser instalados usando opções de linha de comando do DISM.

### <a name="getting-the-msix-packaging-tool-for-offline-use"></a>Como obter a Ferramenta de Empacotamento MSIX para uso offline

A Ferramenta de Empacotamento MSIX pode ser baixada para uso offline na empresa no [portal da Web](https://businessstore.microsoft.com/store) do Microsoft Store para Empresas. Saiba mais sobre a distribuição offline [aqui](https://docs.microsoft.com/microsoft-store/distribute-offline-apps). Caso tenha problemas com a cópia offline da ferramenta de empacotamento, verifique se você tem a [cópia offline da licença](https://docs.microsoft.com/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app) para a ferramenta. 

Após obter a versão offline do aplicativo é possível usar o [PowerShell](https://docs.microsoft.com/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps) para adicionar o pacote do aplicativo e a licença ao seu computador.

### <a name="frameworks-and-drivers"></a>Estruturas e drivers

Se o aplicativo exigir uma estrutura, verifique se a estrutura está instalada durante a fase de monitoramento da conversão. Percorra os logs para garantir que isso esteja acontecendo. Se seu aplicativo exigir que um driver seja instalado, você precisará avaliar se isso é necessário para que seu aplicativo seja executado corretamente. Atualmente, o MSIX não dá suporte à instalação do driver.

### <a name="remote-machine"></a>Computador remoto
Se você estiver executando problemas com o uso de uma VM remota para suas conversões, consulte [instruções de instalação para conversões de computador remoto](remote-conversion-setup.md).

### <a name="issues-during-conversion"></a>Problemas durante a conversão
- Durante a conversão, os instaladores podem executar os serviços. Os serviços não são capturados durante a conversão. Como resultado, seu aplicativo pode ser instalado, mas pode ser executado com problemas.
- Alguns instaladores podem falhar ao fazer a conversão com o código de saída 259. Isso indica que o instalador gerou um thread e não aguardou a conclusão dele. Em outras palavras, o thread principal concluiu a instalação, mas ele foi encerrado com o erro 259 porque gerou um thread que ainda está em execução. Recomendamos que você use a opção de instalação apropriada para setup.exe.

### <a name="issues-during-signing"></a>Problemas durante a assinatura

#### <a name="bad-pe-certificate-0x800700c1"></a>Certificado PE inadequado (0x800700C1)

Esse problema ocorre quando o pacote contém um arquivo binário que tem um certificado corrompido. Para resolver esse problema, use o comando `dumpbin.exe /headers` para despejar os cabeçalhos de arquivo e inspecionar elementos inválidos. Reescreva manualmente os cabeçalhos para corrigir o problema. Em geral, a ferramenta de empacotamento MSIX detecta automaticamente cabeçalhos inválidos. Se esse problema persistir, os comentários do arquivo. Mais informações podem ser encontradas [aqui](../desktop/desktop-to-uwp-known-issues.md#bad-pe-certificate-0x800700c1).

#### <a name="device-guard-signing"></a>Autenticação do Device Guard

Certifique-se de seguir [estas etapas](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing) e que você está atribuindo as funções apropriadas no Microsoft Store para negócios.

#### <a name="expired-certificate"></a>Certificado expirado

- Use um carimbo de data/hora ao assinar seu pacote.
- Você pode desistir com um certificado de assinatura ou carimbo de data/hora válido.

Você pode desistir de seu aplicativo usando o [script de conversão em lote](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts).

## <a name="troubleshooting"></a>Painel de controle da

### <a name="log-files"></a>Arquivos de log

Independentemente de a conversão ter sido bem-sucedida ou não, os arquivos de log são gerados para cada conversão. Eles podem ser encontrados aqui:

`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\`

Os códigos de falha são escritos e indicam qualquer ponto de falha durante o processo de conversão. Os códigos de erro destinam-se a ser amigáveis.

#### <a name="log-files-from-remote-devices-or-vms"></a>Arquivos de log de dispositivos remotos ou VMs

Se a conversão for executada em um dispositivo remoto ou uma VM, recomendaremos que você copie os arquivos de log desse dispositivo e anexe-os como parte do item de comentário. Isso nos ajudará a diagnosticar e resolver problemas com mais eficiência.

Você encontrará os logs das conversões remotas aqui: `%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\<Logs_#>\RemoteServer\Log.txt`

Seria ainda mais útil se você pudesse compartilhar toda a pasta Logs que incluirá as operações ocorridas no cliente local, bem como no servidor remoto.

### <a name="common-problems"></a>Problemas comuns

#### <a name="makeprimanifest-translation-errors"></a>Erros de tradução de MakePri/manifesto

Esse erro ocorre quando há um problema com o manifesto do pacote. Para identificar o problema, vá para o editor de pacotes e abra o manifesto. Ao abrir o manifesto, você pode identificar o problema e fornecer a correção correta.

#### <a name="file-not-found"></a>Arquivo não encontrado

O arquivo pode estar aberto ou não existente. Para resolver esse problema, adicione o arquivo apropriado ou feche o arquivo que está em uso no momento. Observe que você não receberá um erro `File not Found` se ele estiver aberto. Em vez disso, você obterá um erro de `Access Denied` ou `File in Use`.

#### <a name="file-type-associations"></a>Associações de tipo de arquivo

Os problemas relacionados a FTAS (associações de tipo de arquivo) variam de pacote para pacote. A ferramenta de empacotamento MSIX dá suporte a associações de arquivo para instalações de clique duplo. Por exemplo, se seu aplicativo tiver um menu de contexto, ele não será adicionado automaticamente, portanto, será necessário adicioná-lo manualmente ao manifesto. Consulte o elemento manifesto [desktop4: FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus) para obter um exemplo.

#### <a name="shortcuts-with-arguments"></a>Atalhos com argumentos

Atualmente, não há suporte para atalhos com argumentos com MSIX. Se detectarmos que o instalador inclui isso, o MSIX criará um bloco sem argumentos.

#### <a name="install-directory"></a>Diretório de instalação

Isso é mais comum para aqueles que usam uma unidade secundária para executar conversões de aplicativo. Se você optar por alterar o local de instalação, ele alterará a raiz de onde todos os arquivos vão. Isso significa que a ferramenta de empacotamento MSIX precisará saber onde todos esses arquivos vão e serão capturados durante a conversão. 

## <a name="sending-feedback"></a>Como enviar comentários

A melhor maneira de enviar seus comentários é por meio do [**Hub de comentários**](https://www.microsoft.com/p/feedback-hub/9nblggh4r32n?activetab=pivot:overviewtab).

1. Abra o **Hub de Feedback** ou digite **Windows + F**.
2. Forneça um título e as etapas necessárias para reproduzir o problema.
3. Em **Categoria**, selecione **Aplicativos** e selecione **Ferramenta de Empacotamento MSIX**.
4. Anexe os [arquivos de log](#log-files) associados à conversão. Encontre os logs na pasta fornecida acima.
5. Anexe o pacote MSIX convertido (se possível).
6. Clique em **Enviar**.

Envie-nos também comentários diretamente da Ferramenta de Empacotamento MSIX acessando a guia **Comentários** em **Configurações**. 

> [!NOTE]
> Poderá levar 24 horas até recebermos seus comentários. Portanto, se você estiver usando uma VM para converter o pacote, o ideal será manter a VM ligada e em seu estado atual durante 24 horas após a conversão. Além disso, você pode anexar manualmente os logs de conversão aos comentários.
