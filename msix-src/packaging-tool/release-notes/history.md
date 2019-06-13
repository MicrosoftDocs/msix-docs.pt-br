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
ms.openlocfilehash: 4d3826450ade25d5e6fe8a682f03435bc1f1f4d3
ms.sourcegitcommit: 23d6e24cd3197204868091bca2f21cfd9b1bf0da
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67027932"
---
# <a name="msix-packaging-tool-release-notes"></a>Notas de versão de ferramenta de empacotamento MSIX

## <a name="version-120196110"></a>Versão 1.2019.611.0

- Restaurar as configurações padrão e itens de exclusão agora também limpa os códigos de senha e sair do certificado de assinatura
- Corrigido um problema em que as primeiras tarefas de inicialização não foram obtendo excluídas corretamente
- Irá ignorar os atalhos para arquivos excluídos durante a criação do pacote

## <a name="version-120196040"></a>Versão 1.2019.604.0

- O padrão é Assinando um pacote, se um certificado de assinatura padrão é especificado nas configurações de
- Permitir códigos negativo instalador seja especificado nas configurações do
- Respeitar os códigos de saída do instalador do PowerShell
- Informa ao usuário quando eles precisarem de uma reinicialização do driver

## <a name="version-120195220"></a>Versão 1.2019.522.0

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

## <a name="version-120194020---public-release"></a>Versão 1.2019.402.0 - lançamento público

 - Capacidade de converter em uma máquina remota - [obter mais informações](../remote-conversion-setup.md)
 - Validar o ProgId COM valores de tipo, as entradas de classe COM e remover registros inválidos de COM
 - Atualizar as ferramentas do SDK do Windows que são redistribuídas na ferramenta de empacotamento MSIX 
 - Dividir automaticamente argumentos em uma campos separados para manifestada executáveis de com
 - Tratamento robusto de argumentos do instalador do PowerShell
 - Serviço de atualização do Windows desabilitando feito uma etapa necessária na fase de preparação de máquina
- Adicionada a capacidade para o carimbo de data / hora seu pacote assinado em todos os fluxos de trabalho em que a assinatura está disponível no momento
    - Você pode especificar sua URL de carimbo de data / hora padrão e o tipo de servidor de carimbo de data / hora na página de configurações de ferramenta 
- As pastas vazias criadas no VFS do Editor de pacote persistirão depois de salvar o pacote
- Usuário pode especificar códigos de saída esperado válido para conversões de CLI
- Pastas vazias, geradas durante uma conversão serão mantido por meio de empacotamento
- Lógica de geração de AppID atualizada e validação adicional adicionada para o aplicativo e o nome do pacote 
- Melhor experiência de gerenciamento no editor de pacote
- Recomendações de controle de versão automático ao salvar no editor de pacote
- Adicionada a capacidade de usar "." o campo de versão de progresso
- Correção de validação para o local de instalação
- Manipuladores de associação e as entradas do servidor com o tipo de problemas de conversão fixo de manifesto para o arquivo
- Adicionada a capacidade de obter o status de suas conversões de linha de comando
- Log de aviso COM aprimorado com erros legíveis por humanos
