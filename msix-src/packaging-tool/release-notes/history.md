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
ms.openlocfilehash: b5d3f7742195105d2e1890a614070173277f19fe
ms.sourcegitcommit: b3564e47328d21916cdeb4c84d638ac12be0a461
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66186155"
---
# <a name="msix-packaging-tool-release-notes"></a>Notas de versão de ferramenta de empacotamento MSIX 

#### <a name="ver-120195220"></a>Ver 1.2019.522.0

Novos recursos:

- Suporte para instaladores da área de trabalho que exigem reinicialização - [Saiba mais](../support-restart.md)
    - Opção de logon automático para reinicialização 
- Novas opções nas configurações do aplicativo
    - Especifique um certificado padrão para assinar os pacotes com 
    - Especifique os códigos de saída para instaladores que exigem reinicialização
    
Problemas conhecidos:

- Códigos de saída de reinicialização negativos não têm suporte atualmente
- Se o certificado padrão for especificado, cada fluxo de trabalho de conversão será necessário selecionar 'usar cert'
- Durante a VM reiniciar ou remoto, pode haver um prompt de logon adicional 
- Restaurar padrões botão não remove os códigos de saída de senha ou o instalador do certificado
- Há alguns incongruencies da interface do usuário

### <a name="ver-120194020---public-release"></a>**Ver 1.2019.402.0 - lançamento público**

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

 ### <a name="ver-120191100---public-release"></a>**Ver 1.2019.110.0 - lançamento público**
  
Novos recursos:

- Tempos de empacotamento aprimorado 
- Lista de exclusão de arquivo padrão atualizado
- Incorporado a ferramenta reporting MSIExec logs de erros
- Logs atualizados para adicionar mais clareza e etapas de solução de problemas
- Suporte adicionado para capturar a instalação do ISE do PowerShell durante o empacotamento manual
- Suporte adicionado para declarar os scripts do PowerShell como um argumento na interface do usuário e o arquivo de modelo de linha de comando do instalador
- Adicionado um sinalizador de registro em log detalhado (-verbose | - v) para a interface de linha de comando
- Corrigido um problema em que os caminhos de rede na VM, às vezes, eram inacessíveis
- Corrigido um problema em que a validação de requisito de controle de versão do Store falhava ao usar a interface de linha de comando
- Corrigido um problema em que os caminhos de arquivo em cotações não foram sendo aceitas
- Corrigido um problema em que a VM não foi sejam limpos corretamente após a conversão
- Corrigido um problema no qual adicionar arquivos aos pacotes no editor de pacote não estava funcionando corretamente
- Limpeza de interface do usuário 
