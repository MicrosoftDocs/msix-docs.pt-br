---
title: Criar um pacote usando a interface de linha de comando
description: Este artigo descreve como criar um pacote MSIX usando a interface de linha de comando para a ferramenta de empacotamento MSIX.
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 726d743106932f7febac35ad70256b867146101d
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073712"
---
# <a name="conversion-with-the-command-line"></a>Conversão com a linha de comando

## <a name="automate-conversion-of-windows-installers-to-msix-packages-using-scripts"></a>Automatizar a conversão de instaladores do Windows em pacotes do MSIX usando scripts

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a Ferramenta de Empacotamento MSIX</a></p></div>

A ferramenta de empacotamento MSIX dá suporte a uma interface de linha de comando para criar pacotes de aplicativos MSIX. Isso permite que você automatize o processo de reempacotamento de instaladores de aplicativos e execute conversões em massa.

Para os scripts PowerShell e Bash de exemplo que demonstram como automatizar o processo de empacotamento, assinatura, gerenciamento e distribuição de pacotes MSIX, consulte a pasta [scripts](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts) do Kit de Ferramentas MSIX.

## <a name="use-the-command-line-with-the-command-prompt"></a>Usar a linha de comando com o prompt de comando

Para criar um novo pacote MSIX para seu aplicativo, execute o comando `MsixPackagingTool.exe create-package` em uma janela de prompt de comando do administrador.

Estes são os parâmetros que podem ser passados como argumentos de linha de comando:

|**Parâmetro** |    **Descrição**|
|---------|---------|
|-? --help  |Mostra informações da Ajuda|
|--template | [obrigatório] Caminho para o arquivo XML do modelo de conversão que contém informações do pacote e configurações para essa conversão|
|--virtualMachinePassword   | [opcional] A senha para a Máquina Virtual ser usada para o ambiente de conversão. Observações: o arquivo de modelo deve conter um elemento VirtualMachine e o atributo Settings:: AllowPromptForPassword não deve ser definido como true.|
|-v --verbose   |[opcional] Imprime os logs detalhados no console.|

Exemplos:

```console

    MsixPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml -v

    MSIXPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml --virtualMachinePassword pswd112893
    
```

> [!NOTE]
> Atualmente, a conversão do App-V 5.x tem suporte para ser convertida por meio da linha de comando. Isso inclui recursos.

Você pode [gerar um arquivo de modelo de linha de comando](generate-template-file.md) por meio da ferramenta de EMPACOTAmento MSIX passando o processo de conversão com um aplicativo ou pode criar um de nosso modelo de exemplo.
