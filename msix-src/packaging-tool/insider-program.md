---
title: Programa de insider de ferramenta de empacotamento MSIX
description: Saiba mais sobre o programa de insider de ferramenta de empacotamento e como ingressar
author: c-don
ms.author: cdon
ms.date: 02/14/2019
ms.topic: article
keywords: Windows 10, uwp, MSIX, ferramenta de empacotamento MSIX
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b19c7158f6323d867b3b6610d9827396fb6f91d8
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900658"
---
# <a name="msix-packaging-tool-insider-program"></a>Programa de Insider de ferramenta de empacotamento MSIX

O programa de Insider de ferramenta de empacotamento MSIX fornece acesso antecipado a profissionais de TI e desenvolvedores que estejam interessados na conversão de aplicativos da área de trabalho existentes em pacotes MSIX. O programa oferece uma oportunidade de avaliar os novos recursos antes das versões da ferramenta de empacotamento MSIX gerais. Além disso, você pode fornecer comentários para ajudar a moldar a ferramenta para atender às suas necessidades comerciais específicas. 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://aka.ms/MSIXPackagingPreviewProgram" data-linktype="external">Clique aqui para ingressar</a></p></div>

## <a name="prerequisites"></a>Pré-requisitos
- Windows 10, versão 1809 (ou posterior).
- Um alias de conta válido do Microsoft para acessar o aplicativo da Microsoft Store.
- Privilégios de administrador no seu computador para executar a ferramenta.

## <a name="install"></a>Instalar

Depois de ingressar no programa, você receberá um email confirmando o registro. 

Instalar a ferramenta de empacotamento MSIX da Microsoft Store [aqui](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf). Verifique que você está conectado com a conta da Microsoft que você usou para inscrever-se o programa de Insider MSIX ferramenta de empacotamento. Em seguida, vá para a página de descrição de produto e clique em **instalar** para iniciar a instalação.

Se a ferramenta já estiver instalada no seu computador, verifique a versão instalada. Execute a ferramenta de empacotamento MSIX, clique no ícone de engrenagem no canto superior direito e, em seguida, clique no **sobre** guia ver a versão. A versão do aplicativo deve corresponder a compilação atual do Insider Preview da seção [abaixo](#current-insider-preview-build). 

### <a name="current-insider-preview-build"></a>Compilação atual do Insider Preview 

#### <a name="ver-120194020"></a>Ver 1.2019.402.0

Novos recursos:

- Capacidade de converter em uma máquina remota - [obter mais informações](remote-conversion-setup.md)
- Melhor experiência de gerenciamento no editor de pacote
    - Recomendações de controle de versão automático ao salvar no editor de pacote
    - Agora dá suporte à adição de pasta existente para o pacote no VFS
- Usuário pode especificar códigos de saída de válidos conhecidos para conversões de CLI
- Adicionada a capacidade para o carimbo de data / hora seu pacote assinado em todos os fluxos de trabalho em que a assinatura está disponível no momento 
    - Você pode especificar sua URL de carimbo de data / hora padrão e o tipo de servidor de carimbo de data / hora na página de configurações de ferramenta
- Atualizado [lógica de geração de AppID](release-notes/history.md#appid-generation-logic)e adicionou uma validação adicional para o aplicativo e o nome do pacote 

Você pode encontrar o histórico completo das notas de versão da ferramenta de empacotamento MSIX [aqui](release-notes/history.md).

## <a name="share-your-feedback"></a>Compartilhe seus comentários 

Se você tiver um problema ao usar o aplicativo, pressione **tecla Windows + F** para inicializar **Hub de comentários**. Forneça o máximo de detalhes possível sobre o problema para nos ajudar a diagnosticar e resolver o problema. 

Você também pode compartilhar comentários diretamente de dentro do aplicativo. Clique no Settings(gear icon) na tela inicial e escolha **comentários** guia e selecione o botão que melhor representa seu problema. Isso iniciará diretamente **Hub de comentários** e preencha as informações de categoria necessárias em seu nome. 

**Hub de comentários** também é uma ótima maneira de compartilhar ideias e sugestões de novos recursos que você gostaria de ver no aplicativo.  

## <a name="faqs"></a>Perguntas frequentes

1. Não recebi um email confirmando o meu registro no programa Insider. 
    - Por favor [ingressar no programa](https://aka.ms/MSIXPackagingPreviewProgram) novamente.  

2. Eu estou inscrito no programa Insider, mas não tenho a versão do Insider Preview da ferramenta de empacotamento MSIX no meu computador. 
    - Atualizações a partir do Microsoft Store são empurradas gradualmente para usuários em todo o mundo, e pode haver um atraso na obtenção de atualizações automáticas. Você pode disparar uma verificação de atualização indo para o [página ferramenta](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf) na Store. 
3. Eu gostaria recusar o programa de Insider MSIX ferramenta de empacotamento. 
    - Lamentamos vê-lo a deixar o programa. Preencha o formulário [aqui](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR-NSOqDz219PqoOqk5qxQEZUMlEwNVNKMDhNUVlKOVpTRTlVWFhMMThLQy4u) para cancelar a assinatura do programa. 
