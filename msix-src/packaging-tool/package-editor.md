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
ms.openlocfilehash: ba85a6f8c55774af070be53819dc60417ca86236
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900368"
---
# <a name="package-editor"></a>Editor de pacote

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a ferramenta de empacotamento MSIX</a></p></div>
      
Para fazer alterações em um pacote existente de MSIX para alterar as propriedades no manifesto ou o conteúdo do pacote sem a necessidade de empacotar o instalador novamente, um pode usar o editor de pacote na ferramenta de empacotamento de MSIX. 

Na tela de boas-vinda, selecione a opção de editor de pacote:

![pic1](images/pic1.PNG)

Em seguida, atingem o Procurar para localizar o pacote MSIX que precisa ser editado e selecione Abrir pacote:

![pic10](images/pic10.png)

Editor de pacote agora mostra o conteúdo e as propriedades do pacote.

![pic11](images/pic11.png)

Primeira página mostra as informações do pacote, usuário pode alterar as informações de pacote na interface do usuário ou optar por abrir o arquivo de manifesto MSIX manualmente em seu editor de escolha para fazer alterações aos campos de manifesto. Enquanto eles estão editando o manifesto a página do editor de pacote não é editável. Uma vez o salvar que o manifesto, a interface do usuário será atualizado.

Usuário pode selecionar o botão Salvar quando terminar de editar o pacote ou pressione Cancelar ou x para descartar as alterações. Caso contrário, eles poderão navegar para outras páginas clicando nas páginas à esquerda.

![pic12](images/pic12.png)

Nesta página, eles podem adicionar ou remover recursos para o pacote. Se um recurso está presente no pacote, a caixa de seleção será verificada.
- Isso se traduz no <capability> elemento no manifesto MSIX.

![pic13](images/pic13.png)

Esta página mostra todas as entradas do registro virtual empacotados para o aplicativo. Há várias opções de menu de contexto nesta página:

1. Clique com botão direito em um nó na janela esquerda:
    - Expandir/Recolher: para expandir ou recolher todos os registro chaves no hive
    - Chave: permite que o usuário renomear, excluir ou criar uma nova chave
    - Valor: permite que o usuário adicione um valor de chave como cadeia de caracteres, binária ou DWORD


2. Clique com botão direito em qualquer lugar na janela à direita:
 
    - Excluir: excluir uma chave
    - Adicione a cadeia de caracteres: adicionar um valor de cadeia de caracteres a uma chave
    - Adicionar binário: adicionar um valor binário em uma chave
    - Adicione um DWORD: adicionar um valor DWORD para uma chave

Esta página mostra os arquivos de pacote, usuário pode adicionar ou excluir arquivos com o botão direito clicando em um arquivo e selecionando Adicionar arquivo ou excluir.


