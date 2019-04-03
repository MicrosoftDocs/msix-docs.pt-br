---
title: Notas de versão da ferramenta de empacotamento MSIX
description: Histórico completo das notas de versão da ferramenta de empacotamento MSIX
author: c-don
ms.author: cdon
ms.date: 03/05/2019
ms.topic: article
keywords: Windows 10, uwp, msix, ferramenta de empacotamento msix, programa insider
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: be65c2838d189760707e859bfd97751424af2d35
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900788"
---
# <a name="msix-packaging-tool-release-notes"></a>Notas de versão de ferramenta de empacotamento MSIX 

#### <a name="ver-120194020"></a>Ver 1.2019.402.0

 - Validar o ProgId COM valores de tipo, as entradas de classe COM e remover registros inválidos de COM
 - Atualizar as ferramentas do SDK do Windows que são redistribuídas na ferramenta de empacotamento MSIX 

#### <a name="ver-120193220"></a>Ver 1.2019.322.0

 - Dividir automaticamente argumentos em uma campos separados para manifestada executáveis de com

#### <a name="ver-120193150"></a>Ver 1.2019.315.0

 - Tratamento robusto de argumentos do instalador do PowerShell
 - Serviço de atualização do Windows desabilitando feito uma etapa necessária na fase de preparação de máquina

#### <a name="ver-120193080"></a>Ver 1.2019.308.0

- Adicionada a capacidade para o carimbo de data / hora seu pacote assinado em todos os fluxos de trabalho em que a assinatura está disponível no momento
    - Você pode especificar sua URL de carimbo de data / hora padrão e o tipo de servidor de carimbo de data / hora na página de configurações de ferramenta 
- As pastas vazias criadas no VFS do Editor de pacote persistirão depois de salvar o pacote

#### <a name="ver-120193040"></a>Ver 1.2019.304.0

Novos recursos:

- Usuário pode especificar códigos de saída esperado válido para conversões de CLI
- Pastas vazias, geradas durante uma conversão serão mantido por meio de empacotamento
- Atualizado [lógica de geração de AppID](#appid-generation-logic)e adicionou uma validação adicional para o aplicativo e o nome do pacote 

##### <a name="appid-generation-logic"></a>Lógica de geração de AppID
O procedimento atual para derivar a ID do aplicativo é o seguinte: 
1. Localize o nome do exe/msi e a extensão da faixa
2. Converter em maiusculas
3. Remover todos os caracteres não alfanuméricos
4. Traduzir os numerais a serem correspondentes palavras em inglês
5. Se a cadeia de caracteres resultante contém menos de 3 caracteres, use a cadeia de caracteres "Aplicativo"
6. Se a cadeia de caracteres resultante contém mais de 64 caracteres, truncá-la.
7. Colisões, truncar a cadeia de caracteres, se ele contiver mais de 62 caracteres e acrescentar um valor de dois dígitos, começando com 00.

#### <a name="ver-120192260"></a>Ver 1.2019.226.0
Novos recursos:

- Capacidade de converter em uma máquina remota - [obter mais informações](../remote-conversion-setup.md)
- Melhor experiência de gerenciamento no editor de pacote
- Recomendações de controle de versão automático ao salvar no editor de pacote

Atualizações adicionais

- Adicionada a capacidade de usar "." o campo de versão de progresso
- Correção de validação para o local de instalação
- Manipuladores de associação e as entradas do servidor com o tipo de problemas de conversão fixo de manifesto para o arquivo
- Adicionada a capacidade de obter o status de suas conversões de linha de comando
- Log de aviso COM aprimorado com erros legíveis por humanos

