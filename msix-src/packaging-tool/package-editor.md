---
title: Editor de pacote | Microsoft Docs
description: Use o Editor de pacote para modificar as informações de pacote
author: mcleanbyron
ms.author: mcleans
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 79aa7202570c91d3e4d631ff8f3b1413e5f9a79e
ms.sourcegitcommit: 67e56f5414857671c47334c65d636d531632b8f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59566510"
---
# <a name="package-editor"></a>Editor de pacote

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a ferramenta de empacotamento MSIX</a></p></div>
      
Para fazer alterações em um pacote existente de MSIX como modificar as propriedades no manifesto ou o conteúdo do pacote sem a necessidade de empacotar o instalador novamente, você pode usar o editor de pacote na ferramenta de empacotamento MSIX. 

Na tela de boas-vinda, selecione o ícone do editor de pacote, navegue para seu pacote MSIX e selecione Abrir pacote.

Na página de informações do pacote, você pode alterar as informações do pacote por meio dos campos na interface do usuário ou optar por abrir o arquivo de manifesto MSIX manualmente no editor de sua escolha para fazer alterações aos campos de manifesto. Enquanto você estiver editando o manifesto a página do editor de pacote não é editável. Depois de salvar o manifesto, a interface do usuário será atualizado.

Você pode navegar para outras seções do editor de pacote para modificar sua capablities, registro virtual ou arquivos de pacote. Quando você terminar de editar seu pacote, certifique-se de assinar seu pacote e atualizar sua versão antes de salvar suas alterações. 

![pic10](images/pic10.png)

Na página de recursos, você pode adicionar ou remover [recursos](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/uapmanifestschema/element-capability) para o pacote. Se um recurso está presente no pacote, a caixa de seleção será verificada. Se você selecionar ou não selecione um recurso, ele atualizará o manifesto. Isso se traduz no <capability> elemento no manifesto MSIX.

![pic11](images/pic11.png)

A página de registro Virtual mostra todas as entradas do registro virtual empacotados para o aplicativo. 

Clique com botão direito em um nó na janela esquerda:
- Expandir/Recolher: para expandir ou recolher todos os registro chaves no hive
- Chave: permite que o usuário renomear, excluir ou criar uma nova chave
- Valor: permite que o usuário adicione um valor de chave como cadeia de caracteres, binária ou DWORD

Clique com botão direito em qualquer lugar na janela à direita:
- Excluir: excluir uma chave
- Adicione a cadeia de caracteres: adicionar um valor de cadeia de caracteres a uma chave
- Adicionar binário: adicionar um valor binário em uma chave
- Adicione um DWORD: adicionar um valor DWORD para uma chave

![pic12](images/pic12.png)

Na página de arquivos do pacote, você pode clicar duas vezes para expandir o sistema de arquivos. 

Clique com botão direito em uma pasta:
- Adicione arquivo: Adicionar um arquivo para a pasta selecionada
- Nova pasta: Criar uma nova pasta vazia
- Adicione pasta: Procurar para adicionar uma pasta existente
- Exclua: Excluir a pasta selecionada
- Mova: Renomear ou mover a pasta para um novo local

Clique com botão direito em um arquivo:
- Exclua: Excluir o arquivo selecionado
- Mova: Renomear ou mover o arquivo para um novo local

![pic13](images/pic13.png)

