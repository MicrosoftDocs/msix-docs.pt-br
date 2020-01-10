---
title: Convertendo um instalador com serviços
description: Este artigo explica como converter um instalador existente com serviços para MSIX usando a ferramenta de empacotamento MSIX
ms.date: 12/19/2019
ms.topic: article
keywords: Windows 10, MSIX, MSIX Packaging Tool, serviços
ms.localizationpriority: medium
ms.openlocfilehash: 9cbc748311222d27cc6da946bc88add7744915e9
ms.sourcegitcommit: 71c49de79d061909fb1ab632ec7550227d2287bd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75754865"
---
# <a name="convert-an-installer-that-includes-services"></a>Converter um instalador que inclui serviços

O Windows 10, versão 2004, apresenta suporte à execução de um pacote MSIX que inclui serviços. Você pode usar a ferramenta de empacotamento MSIX para pegar um instalador existente com serviços e convertê-lo em MSIX. Esse suporte estará disponível na versão de janeiro de 2020 da ferramenta de empacotamento MSIX, mas você pode experimentá-lo agora em uma versão de visualização como parte do nosso [programa de visualização do MSIX Insider](insider-program.md).

## <a name="instructions"></a>Instruções

Para converter um instalador que inclui serviços, use a ferramenta de empacotamento MSIX como faria com qualquer [pacote de aplicativo](create-app-package-msi-vm.md). Selecione um instalador que tenha serviços e você verá a página de relatório de **Serviços** antes da etapa final para criar o pacote MSIX.

A página de relatório de **Serviços** lista os serviços que foram detectados no instalador durante a conversão. Os serviços que têm todas as informações de que precisam e que têm suporte serão mostrados na tabela **incluída** . Os serviços que precisam de informações adicionais, precisam de uma correção ou não têm suporte serão mostrados na tabela **excluída** .

Para corrigir um serviço ou ver dados adicionais sobre o serviço, clique duas vezes na entrada de serviço na tabela para exibir um pop-up com mais informações sobre o serviço. Você pode editar algumas dessas informações se precisar.

- **Nome da chave:** O nome do serviço. Isso não é editável.
- **Descrição:** A descrição da entrada de serviço.
- **Nome para exibição:** O nome de exibição do serviço.
- **Caminho da imagem:** Local do executável do serviço. Isso não é editável.
- **Conta inicial:** A conta inicial do serviço.
- **Tipo de inicialização:** Tipo de inicialização para o serviço. Dá suporte a **automático**, **manual**e **desabilitado**.
- **Argumentos:** Argumentos a serem executados quando o serviço for iniciado.
- **Dependências:** Dependências para o serviço.

Depois que um serviço for corrigido, você poderá movê-lo para a tabela **incluída** ou pode optar por deixá-lo na tabela **excluída** se não quiser em seu pacote final. Em seguida, você pode continuar na etapa final para criar seu pacote MSIX.

## <a name="known-limitations"></a>Limitações conhecidas

O caminho do executável dos serviços (também chamado de caminho da imagem) não é editável no momento. Para corrigir problemas com o caminho, você deve editar manualmente o caminho do executável do serviço antes de converter o instalador. Como alternativa, após a conversão, você pode editar o manifesto manualmente usando o **Editor de pacote** na ferramenta de empacotamento MSIX.

O relatório de **Serviços** não está disponível no **Editor de pacotes**no momento. Você deve editar manualmente o manifesto para fazer alterações nos serviços incluídos no pacote MSIX.

No momento, não há suporte para serviços com dependências fora do pacote.
