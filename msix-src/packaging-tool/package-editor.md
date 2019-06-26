---
title: Editor de Pacote | Microsoft Docs
description: Use o Editor de Pacote para modificar as informações do pacote
author: mcleanbyron
ms.author: mcleans
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 79aa7202570c91d3e4d631ff8f3b1413e5f9a79e
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2019
ms.locfileid: "59566510"
---
# <a name="package-editor"></a>Editor de pacote

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a Ferramenta de Empacotamento MSIX</a></p></div>
      
Para fazer alterações em um pacote MSIX existente, como modificar as propriedades no manifesto ou o conteúdo do pacote sem a necessidade de empacotar o instalador novamente, use o editor de pacote na Ferramenta de Empacotamento MSIX. 

Na tela de boas-vindas, selecione o ícone do Editor de pacote, procure o pacote MSIX e selecione Abrir pacote.

Na página Informações do pacote, você pode alterar as informações do pacote por meio dos campos na interface do usuário ou optar por abrir o arquivo de manifesto MSIX manualmente no editor de sua escolha para fazer alterações nos campos do manifesto. Enquanto você estiver editando o manifesto, a página do editor de pacote não será editável. Depois que você salvar o manifesto, a interface do usuário será atualizada.

Navegue até outras seções do editor de pacote para modificar suas funcionalidades, o Registro virtual ou os arquivos de pacote. Quando terminar de editar o pacote, assine o pacote e atualize sua versão antes de salvar as alterações. 

![pic10](images/pic10.png)

Na página Funcionalidades, adicione ou remova [funcionalidades](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/uapmanifestschema/element-capability) do pacote. Se uma funcionalidade estiver presente no pacote, a caixa de seleção ficará marcada. Se você marcar ou desmarcar uma funcionalidade, ela atualizará o manifesto. Isso é convertido no elemento <capability> no manifesto MSIX.

![pic11](images/pic11.png)

A página Registro virtual mostra todas as entradas do Registro virtual empacotadas para o aplicativo. 

Clicar com o botão direito do mouse em um nó na janela à esquerda:
- Expandir/recolher: para expandir ou recolher todas as chaves do Registro no hive
- Chave: permite que o usuário renomeie, exclua ou crie uma chave
- Valor: permite que o usuário adicione um valor de chave como cadeia de caracteres, binário ou DWORD

Clicar com o botão direito do mouse em qualquer lugar na janela à direita:
- Excluir: para excluir uma chave
- Adicionar Cadeia de Caracteres: para adicionar um valor de cadeia de caracteres a uma chave
- Adicionar binário: para adicionar um valor binário a uma chave
- Adicionar DWORD: para adicionar um valor DWORD a uma chave

![pic12](images/pic12.png)

Na página Arquivos de pacote, clique duas vezes para expandir o sistema de arquivos. 

Clicar com o botão direito do mouse em uma pasta:
- Adicionar arquivo: Adicionar um arquivo à pasta selecionada
- Nova pasta: Criar uma pasta vazia
- Adicionar pasta: Procurar para adicionar uma pasta existente
- Excluir: Excluir a pasta selecionada
- Mover: Renomear ou mover a pasta para uma nova localização

Clicar com o botão direito do mouse em um arquivo:
- Excluir: Excluir o arquivo selecionado
- Mover: Renomear ou mover o arquivo para uma nova localização

![pic13](images/pic13.png)

