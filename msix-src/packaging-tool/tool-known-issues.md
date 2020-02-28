---
title: Problemas conhecidos da ferramenta de empacotamento MSIX e dicas de solução de problemas
description: Descreve problemas conhecidos e fornece dicas de solução de problemas a serem consideradas ao converter seus aplicativos para MSIX usando a ferramenta de empacotamento MSIX.
ms.date: 02/03/2020
ms.topic: article
keywords: Ferramenta de Empacotamento MSIX, problemas conhecidos, solução de problemas
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a8a5c6071b0b507a77fa6c5f590ac40f7bd0ecd0
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073812"
---
# <a name="known-issues-and-troubleshooting-tips-for-the-msix-packaging-tool"></a>Problemas conhecidos e dicas de solução de problemas para a ferramenta de empacotamento MSIX

Este artigo descreve os problemas conhecidos e fornece dicas de solução de problemas a serem considerados ao converter seus aplicativos em MSIX usando a Ferramenta de Empacotamento MSIX. Confira nossos outros documentos se precisar adquirir a ferramenta de empacotamento MSIX ou o driver em [ambientes desconectados](disconnected-environment.md).

## <a name="known-issues"></a>Problemas conhecidos

### <a name="getting-the-latest-insider-preview-build-of-the-msix-packaging-tool"></a>Obtendo a mais recente versão prévia do insider preview da ferramenta de empacotamento MSIX

Se você tiver optado pelo nosso [programa Insider](insider-program.md), verifique se você tem a versão correta da ferramenta de empacotamento MSIX:
- Vá para a seção **sobre** na ferramenta de empacotamento MSIX para exibir em qual versão você está.
- Acesse [aqui](insider-program.md#current-insider-preview-build) para determinar a versão mais recente do insider Preview e confirme se você tem essa versão da ferramenta de empacotamento MSIX instalada. Verifique se o MSA que se inscreveu para o vôo é a conta que está conectada à Microsoft Store. 
- Atualize manualmente a ferramenta de empacotamento MSIX por meio do Microsoft Store em seu computador. Se essa opção estiver disponível para você, abra a loja, vá para **downloads e atualizações**e clique em **obter atualizações**.
- Para instalar a ferramenta de empacotamento MSIX para uso offline, siga [estas instruções](disconnected-environment.md#get-the-msix-packaging-tool) para garantir que você obtenha o aplicativo mais recente por meio do nosso processo offline.

Se você estiver interessado em ingressar em nosso programa Insider, clique [aqui](https://aka.ms/MSIXPackagingPreviewProgram).

### <a name="minimum-version"></a>Versão mínima

Se você converter o instalador existente usando uma versão da [ferramenta de empacotamento MSIX](tool-overview.md) anterior à **1.2019.701.0**, a ferramenta importou Microsoft Store requisitos de controle de versão ou usou outra ferramenta para criar o pacote que não definiu a versão mínima para 10.0.16299.0 (Windows 10, versão 1709). Isso causará uma mensagem de erro ao implantar seu aplicativo no Windows 10, versão 1709 ou uma versão posterior.

Para corrigir esse problema, abra a **ferramenta de empacotamento MSIX** e edite seu aplicativo por meio do **Editor de pacotes**. Abra o manifesto e defina o atributo `MinVersion` do elemento `TargetDeviceFamily` como "10.0.16299.0".

```xml
<Dependencies>
    <TargetDeviceFamily> Name="Windows.Desktop" MinVersion="10.0.16299.0" MaxVersionTested = "10.0.17763.0" />
</Dependencies>
```

### <a name="frameworks-and-drivers"></a>Estruturas e drivers

Se o aplicativo exigir uma estrutura, verifique se a estrutura está instalada durante a fase de monitoramento da conversão. Percorra os logs para garantir que isso esteja acontecendo. Se seu aplicativo exigir que um driver seja instalado, você precisará avaliar se isso é necessário para que seu aplicativo seja executado corretamente. Atualmente, o MSIX não dá suporte à instalação do driver.

### <a name="remote-machine"></a>{1&gt;{2&gt;Computador Remoto&lt;2}&lt;1}
Se você estiver executando problemas com o uso de uma VM remota para suas conversões, consulte [instruções de instalação para conversões de computador remoto](remote-conversion-setup.md).

### <a name="issues-during-conversion"></a>Problemas durante a conversão
- Alguns instaladores podem falhar ao fazer a conversão com o código de saída 259. Isso indica que o instalador gerou um thread e não aguardou a conclusão dele. Em outras palavras, o thread principal concluiu a instalação, mas ele foi encerrado com o erro 259 porque gerou um thread que ainda está em execução. Recomendamos que você use a opção de instalação apropriada para setup.exe.

### <a name="issues-during-signing"></a>Problemas durante a assinatura

#### <a name="bad-pe-certificate-0x800700c1"></a>Certificado PE inadequado (0x800700C1)

Esse problema ocorre quando o pacote contém um arquivo binário que tem um certificado corrompido. Para resolver esse problema, use o comando `dumpbin.exe /headers` para despejar os cabeçalhos de arquivo e inspecionar elementos inválidos. Reescreva manualmente os cabeçalhos para corrigir o problema. Em geral, a ferramenta de empacotamento MSIX detecta automaticamente cabeçalhos inválidos. Se esse problema persistir, os comentários do arquivo. Mais informações podem ser encontradas [aqui](../desktop/desktop-to-uwp-known-issues.md#bad-pe-certificate-0x800700c1).

#### <a name="device-guard-signing"></a>Autenticação do Device Guard

Certifique-se de seguir [estas etapas](../package/signing-package-device-guard-signing.md) e que você está atribuindo as funções apropriadas no Microsoft Store para negócios.

#### <a name="expired-certificate"></a>Certificado expirado

- Use um carimbo de data/hora ao assinar seu pacote.
- Você pode desistir com um certificado de assinatura ou carimbo de data/hora válido.

Você pode desistir de seu aplicativo usando o [script de conversão em lote](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts).

## <a name="troubleshooting"></a>Solução de problemas

### <a name="log-files"></a>Arquivos de log

Independentemente de a conversão ter sido bem-sucedida ou não, os arquivos de log são gerados para cada conversão. Eles podem ser encontrados aqui:

`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\`

Os códigos de falha são escritos e indicam qualquer ponto de falha durante o processo de conversão. Os códigos de erro destinam-se a ser amigáveis.

#### <a name="log-files-from-remote-devices-or-vms"></a>Arquivos de log de dispositivos remotos ou VMs

Se a conversão for executada em um dispositivo remoto ou uma VM, recomendaremos que você copie os arquivos de log desse dispositivo e anexe-os como parte do item de comentário. Isso nos ajudará a diagnosticar e resolver problemas com mais eficiência.

Você encontrará os logs das conversões remotas aqui: `%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\<Logs_#>\RemoteServer\Log.txt`

Seria ainda mais benéfico se você puder compartilhar a pasta de logs inteira que incluirá as operações que ocorrem no cliente local, bem como no servidor remoto.

### <a name="common-problems"></a>Problemas comuns

#### <a name="makeprimanifest-translation-errors"></a>Erros de tradução de MakePri/manifesto

Esse erro ocorre quando há um problema com o manifesto do pacote. Para identificar o problema, vá para o editor de pacotes e abra o manifesto. Ao abrir o manifesto, você pode identificar o problema e fornecer a correção correta.

#### <a name="file-not-found"></a>Arquivo não encontrado

O arquivo pode estar aberto ou não existente. Para resolver esse problema, adicione o arquivo apropriado ou feche o arquivo que está em uso no momento. Observe que você não receberá um erro `File not Found` se ele estiver aberto. Em vez disso, você obterá um erro de `Access Denied` ou `File in Use`.

#### <a name="file-type-associations"></a>File Type Associations

Os problemas relacionados a FTAS (associações de tipo de arquivo) variam de pacote para pacote. A ferramenta de empacotamento MSIX dá suporte a associações de arquivo para instalações de clique duplo. Por exemplo, se seu aplicativo tiver um menu de contexto, ele não será adicionado automaticamente, portanto, será necessário adicioná-lo manualmente ao manifesto. Consulte o elemento manifesto [desktop4: FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus) para obter um exemplo.

#### <a name="shortcuts-with-arguments"></a>Atalhos com argumentos

Atualmente, não há suporte para atalhos com argumentos com MSIX. Se detectarmos que o instalador inclui isso, o MSIX criará um bloco sem argumentos.

#### <a name="install-directory"></a>Diretório de instalação

Isso é mais comum para aqueles que usam uma unidade secundária para executar conversões de aplicativo. Se você optar por alterar o local de instalação, ele alterará a raiz de onde todos os arquivos vão. Isso significa que a ferramenta de empacotamento MSIX precisará saber onde todos esses arquivos vão e serão capturados durante a conversão. 

Você pode corrigir isso usando a gravação da estrutura de suporte do pacote para instalar a correção do diretório. Adicionamos isso como um recurso por padrão na ferramenta MSIX, que permite isso até 1809. Se seu aplicativo não estiver funcionando no 1709 e estiver em 1809, isso provavelmente será o problema.

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