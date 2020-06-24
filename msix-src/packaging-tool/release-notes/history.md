---
title: Notas de versão da ferramenta de empacotamento MSIX
description: Este artigo fornece o histórico completo de notas de versão para diferentes versões da ferramenta de empacotamento MSIX.
ms.date: 03/25/2020
ms.topic: article
keywords: Windows 10, UWP, msix, ferramenta de empacotamento msix, programa Insider
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 6aa47ea616a28694a9e22cb68a2a89a100e86629
ms.sourcegitcommit: 6c517bd2f6354db1a2c51217a208e1d2cfd466da
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85295573"
---
# <a name="release-notes-for-the-msix-packaging-tool"></a>Notas sobre a versão da Ferramenta de Empacotamento MSIX

## <a name="version-120206180"></a>1.2020.618.0 da versão
- Adicionada uma sessão de tempo limite mais longa para conversões de linha de comando remotas
- Melhores seleções do so MSIX Core para reduzir conflitos e confusão

## <a name="version-120206030"></a>1.2020.603.0 da versão
- Corrigido um problema com os valores do registro do App-V durante a conversão

## <a name="version-120205280"></a>1.2020.528.0 da versão
- Capacidade de adicionar vários arquivos ao editor de pacotes
- Capacidade de importar vários arquivos. reg para o editor de pacote
- Suporte aprimorado para converter qualquer tipo de instalador
- Correções de bugs:
    - Desabilite a seleção da máquina VM local quando o Hyper-V não estiver presente
    - A caixa de seleção impor requisitos de repositório está marcada, impedir a criação de ícones não aceitos pela loja

## <a name="version-120204230"></a>1.2020.423.0 da versão
- Capacidade de adicionar itens do editor de pacote à lista de exclusões
- Usar Ctrl para opções de seleção múltipla no editor de pacote
- Adicionou um prompt ao substituir arquivos ao mover ou adicionar

## <a name="version-120204020---public-release"></a>Versão 1.2020.402.0 – versão pública
- A configuração de integridade do pacote está ativada por padrão
- Capacidade de adicionar automaticamente o [suporte do MSIX Core](../../msix-core/msixcore.md) a um MSIX
- Capacidade de importar ou exportar arquivos de registro (. reg) no editor de pacote
- A página Criar pacote agora mostra o local de salvamento padrão
- Adicionar extensão InstalledLocationVirtualization
- Qualidade aprimorada de ícones extraídos
- Extração de ícones aprimorados de erros corrigidos de atalhos:
- Validar o formato do manifesto após a edição 
- Tornar a mensagem quando a tarefa de inicialização inicial falhar mais claro 
- Proibir caminhos relativos para desempacotamento 
- Filtros de arquivo atualizados para mostrar quais formatos são válidos (por exemplo, para instaladores que ele costumava dizer *.*   e agora `*.msi, *.exe, ...` ) 
- Corrigido quando o Unpack converteria espaços em caminhos em "%20"
- correções de bugs gerais

## <a name="version-1201912200---public-release"></a>Versão 1.2019.1220.0 – versão pública
- O suporte para a conversão de um instalador existente com serviços agora está disponível
  - Veja todos os serviços detectados e modifique-os na nova [página do Relatório de serviços](../convert-an-installer-with-services.md)
- Nova configuração para especificar o ambiente de conversão padrão
- Nova configuração para especificar o local de procura do instalador padrão
- Nova configuração para adicionar integridade de pacote a aplicativos
- Adicionar botões executar e remover à página iniciar tarefas primeiro
- Adicionada a capacidade de arrastar e soltar arquivos para movê-los no editor de pacotes
- Correção adicionada para um problema com a tradução de caminhos de servidor COM que o estavam causando inválidos
