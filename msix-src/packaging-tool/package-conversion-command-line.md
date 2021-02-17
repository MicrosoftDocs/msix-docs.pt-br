---
title: Criar um pacote usando a interface de linha de comando
description: Este artigo descreve como criar um pacote MSIX usando a interface de linha de comando para a ferramenta de empacotamento MSIX.
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d9b9d7f62e1f3a0238341f9ad7337dc3ee35ace8
ms.sourcegitcommit: 1c4e671172104bba39eebd513d849cfbbb689539
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100630867"
---
# <a name="conversion-with-the-command-line"></a>Conversão com a linha de comando

## <a name="automate-conversion-of-windows-installers-to-msix-packages-using-scripts"></a>Automatizar a conversão de instaladores do Windows em pacotes do MSIX usando scripts

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a Ferramenta de Empacotamento MSIX</a></p></div>

A ferramenta de empacotamento MSIX dá suporte a uma interface de linha de comando para criar pacotes de aplicativos MSIX. Isso permite que você automatize o processo de reempacotamento de instaladores de aplicativos e execute conversões em massa.

Para os scripts PowerShell e Bash de exemplo que demonstram como automatizar o processo de empacotamento, assinatura, gerenciamento e distribuição de pacotes MSIX, consulte a pasta [scripts](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts) do Kit de Ferramentas MSIX.

## <a name="use-the-command-line-with-the-command-prompt"></a>Usar a linha de comando com o prompt de comando

Para criar um novo pacote MSIX para seu aplicativo, execute o `MsixPackagingTool.exe create-package` comando em uma janela de prompt de comando do administrador.

Aqui estão os parâmetros que podem ser passados como argumentos de linha de comando (diferencia maiúsculas de minúsculas):

|**Parâmetro** |    **Descrição**|
|---------|---------|
|-? --help  |Mostra informações da Ajuda|
|--template | [obrigatório] Caminho para o arquivo XML do modelo de conversão que contém informações do pacote e configurações para essa conversão|
|--virtualMachinePassword   | [opcional] A senha para a Máquina Virtual ser usada para o ambiente de conversão. Observação: o arquivo de modelo deve conter um elemento VirtualMachine e o atributo Settings:: AllowPromptForPassword não deve ser definido como true.|
|--machinePassword  |adicional A senha do computador remoto a ser usada para o ambiente de conversão. Observação: o arquivo de modelo deve conter um elemento RemoteMachine ou VirtualMachine e o atributo Settings:: AllowPromptForPassword não deve ser definido como true.|
|--retomar   |adicional Usado para retomar o fluxo de conversão após uma reinicialização.|
|-v --verbose   |[opcional] Imprime os logs detalhados no console.|

Exemplos:

```console

    MsixPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml -v

    MSIXPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml --virtualMachinePassword pswd112893
    
```

> [!NOTE]
> Atualmente, a conversão do App-V 5.x tem suporte para ser convertida por meio da linha de comando. Isso inclui recursos.

Você pode [gerar um arquivo de modelo de linha de comando](generate-template-file.md) por meio da ferramenta de EMPACOTAmento MSIX passando o processo de conversão com um aplicativo ou pode criar um de nosso modelo de exemplo.
