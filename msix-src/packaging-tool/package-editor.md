---
title: Editar um pacote usando o editor de pacote
description: Este artigo descreve como usar o editor de pacote na ferramenta de pacote MSIX para editar informações de pacote, como as propriedades no manifesto.
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b71f94871b9cce020cf0a20f86c36ca32b0c0bf1
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090874"
---
# <a name="edit-a-package-using-package-editor"></a>Editar um pacote usando o editor de pacote

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a Ferramenta de Empacotamento MSIX</a></p></div>

Para fazer alterações em um pacote MSIX existente, como editar as propriedades no manifesto ou o conteúdo do pacote sem precisar empacotar o instalador novamente, você pode usar o editor de **pacote** na ferramenta de empacotamento MSIX.

Na página inicial da ferramenta de empacotamento MSIX, selecione o ícone do **Editor de pacote** , procure o pacote MSIX e selecione **Abrir pacote**. Você também pode clicar com o botão direito do mouse em um pacote MSIX e selecionar **Editar com a ferramenta de empacotamento MSIX** (disponível nas versões 1.2019.1018.0 e posterior).

Você pode desempacotar o pacote MSIX do editor de pacote por meio do botão ' desempacotar ' na parte inferior. Em seguida, você pode selecionar o local onde deseja desempacotar o pacote MSIX. 

## <a name="package-information-page"></a>Página Informações do pacote

Na página **Informações do pacote**, você pode alterar as informações do pacote por meio dos campos na interface do usuário ou optar por abrir o arquivo de manifesto MSIX manualmente no editor de sua escolha para fazer alterações nos campos do manifesto. Enquanto você estiver editando o manifesto, a página do editor de pacote não será editável. Depois que você salvar o manifesto, a interface do usuário será atualizada.

Você pode navegar para outras seções do editor de pacote para editar seus recursos, registro virtual ou arquivos de pacote. Quando terminar de editar o pacote, assine o pacote e atualize sua versão antes de salvar as alterações.

![packageeditorpkginfo1](images/PackageEditorPkgInfo1.png)

![packageeditorpkginfo2](images/PackageEditorPkgInfo2.png)

## <a name="capabilities-page"></a>Página Funcionalidades

Na página [Funcionalidades](/uwp/schemas/appxpackage/uapmanifestschema/element-capability), você pode adicionar ou remover **funcionalidades** do pacote. Se uma funcionalidade estiver presente no pacote, a caixa de seleção ficará marcada. Se você marcar ou desmarcar uma funcionalidade, ela atualizará o manifesto. Isso é convertido no elemento <capability> no manifesto MSIX.

![pic11](images/pic11.png)

## <a name="virtual-registry-page"></a>Página Registro virtual

A página **Registro virtual** mostra todas as entradas do Registro virtual empacotadas para o aplicativo.

Clique com o botão direito do mouse em um nó na janela esquerda para executar essas operações:

- Expandir/recolher: para expandir ou recolher todas as chaves do Registro no hive.
- Chave: permite que o usuário renomeie, exclua ou crie uma chave.
- Valor: permite que o usuário adicione um valor de chave como cadeia de caracteres, binário ou DWORD.

Clique com o botão direito do mouse em qualquer lugar na janela direita para executar essas operações:

- Excluir: para excluir uma chave.
- Adicionar Cadeia de Caracteres: para adicionar um valor de cadeia de caracteres a uma chave.
- Adicionar binário: para adicionar um valor binário a uma chave.
- Adicionar DWORD: para adicionar um valor DWORD a uma chave.

![pic12](images/pic12.png)

## <a name="package-files-page"></a>Página Arquivos de pacote

Na página **Arquivos de pacote**, clique duas vezes para expandir o sistema de arquivos do conteúdo do pacote. Por exemplo, você pode usar essa página para [editar ativos e ícones do aplicativo](edit-icons-and-assets.md).

Clique com o botão direito do mouse em uma pasta para executar essas operações:

- Adicionar arquivo: Adiciona um arquivo à pasta selecionada.
- Nova pasta: Crie uma nova pasta vazia.
- Adicionar pasta: Navegue para adicionar uma pasta existente.
- Excluir: exclui a pasta selecionada.
- Mover: renomear ou mover a pasta para um novo local.

Clique com o botão direito do mouse em um arquivo para executar essas operações:

- Excluir: exclui o arquivo selecionado.
- Mover: renomear ou mover o arquivo para um novo local.

![pic13](images/pic13.png)