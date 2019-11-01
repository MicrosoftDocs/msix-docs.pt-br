---
title: Editar ícones e ativos usando a Ferramenta de Empacotamento MSIX
description: Descreve como editar ícones e ativos durante o processo de conversão do aplicativo ao usar a ferramenta de empacotamento MSIX.
ms.date: 06/27/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 54bc62d90ee3fef134668c562655d4b4bef39801
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328723"
---
# <a name="edit-icons-and-assets-using-the-msix-packaging-tool"></a>Editar ícones e ativos usando a Ferramenta de Empacotamento MSIX

Há várias maneiras de modificar os ativos de pacote do aplicativo durante o processo de conversão ao usar a Ferramenta de Empacotamento MSIX:

* É possível modificar os ativos de pacote do aplicativo durante o processo de conversão.
* Depois que seu pacote for criado, será possível modificar os ativos de pacote do aplicativo por meio do [Editor de pacote](package-editor.md).

Confira as [diretrizes para ícone e ativos](https://docs.microsoft.com/windows/uwp/design/style/app-icons-and-logos) enquanto faz suas modificações.

## <a name="modify-assets-during-the-conversion-process"></a>Modificar ativos durante o processo de conversão

Durante a fase de instalação monitorada do fluxo de trabalho da Ferramenta de Empacotamento MSIX, essa ferramenta capturará as alterações que você fizer em arquivos ou pastas do seu aplicativo. Se você alterar seus ativos durante o monitoramento, essas alterações serão capturadas em seu pacote final.

## <a name="modify-assets-after-your-package-has-been-created"></a>Modificar ativos após a criação do seu pacote

Para modificar os ativos do pacote do aplicativo após criar seu pacote MSIX, abra o pacote MSIX no [Editor de pacotes](package-editor.md) e, em seguida, abra a página **Arquivos de pacote**. É possível excluir os ativos existentes ou adicionar novos ícones e ativos.

- Para adicionar um novo arquivo de ativo, clique com o botão direito do mouse na pasta dos ativos e selecione **Adicionar arquivo** ou **Adicionar pasta**.
- Para excluir um arquivo de ativo existente, clique com o botão direito do mouse no arquivo e selecione **Excluir**.

Para verificar as alterações do ativo, acesse a página **Informações do pacote** e abra o arquivo de manifesto. Confirme se os ativos adicionados ou removidos estão representados no nó [uap:VisualElements](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-visualelements). A referência do manifesto precisa ser específica, mas os arquivos do ativo podem ser nomeados da maneira como você desejar. 
