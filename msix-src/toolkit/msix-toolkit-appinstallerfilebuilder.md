---
title: Ferramenta do construtor de arquivos AppInstaller
description: Este artigo fornece detalhes sobre a ferramenta AppInstaller File Builder no kit de ferramentas do MSIX.
ms.date: 1/23/2020
ms.topic: article
keywords: Windows 10, msix, msix Toolkit
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: eaf2234423ccc20cb2bb5f1024a7f8144722f1c1
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073552"
---
# <a name="appinstaller-file-builder-tool"></a>Ferramenta do construtor de arquivos AppInstaller

A [ferramenta AppInstaller File Builder](https://github.com/microsoft/MSIX-Toolkit/tree/master/AppInstallerFileBuilder) no kit de ferramentas do MSIX pode ser usada para configurar um aplicativo MSIX empacotado para fazer check-in de uma fonte central para atualizações. Essa ferramenta ajuda na construção de um arquivo AppInstaller (. AppInstaller). O arquivo AppInstaller pode ser usado com [Opções de implantação](../desktop/managing-your-msix-deployment-overview.md)diferentes.

## <a name="create-an-appinstaller-file"></a>Criar um arquivo AppInstaller

1. Baixar o [AppInstaller File Builder](https://github.com/microsoft/MSIX-Toolkit/releases/download/1.3.3/AppInstallerFileBuilder_1.2019.1001.0.msix)
2. Instale e inicie o aplicativo **AppInstaller File Builder** .
3. Selecione o botão **obter informações do pacote** e navegue até o aplicativo MSIX que você deseja instalar usando o arquivo AppInstaller.
4. Especifique as opções de atualização que você deseja que o pacote de aplicativo tenha.
    > [!Note]
    > As opções podem variar com base na versão de sistema operacional mínima selecionada. **Verificar se há atualizações na inicialização do aplicativo** deve ser habilitado dentro da ferramenta para ver ou configurar esses valores.
5. Continue com o assistente, adicionando [pacotes opcionais](../package/optional-packages.md), [pacotes de modificação](//modification-packages.md), [pacotes relacionados](../package/optional-packages.md) e dependências, conforme necessário.
6. Selecione o botão **gerar** depois de revisar os detalhes do resumo para criar o arquivo AppInstaller.
