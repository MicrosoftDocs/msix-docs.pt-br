---
title: Como a Política de Grupo funciona com aplicativos empacotados como MSIX
description: Descreve como a Política de Grupo funciona com aplicativos convertidos em MSIX.
ms.date: 04/12/2019
ms.topic: article
keywords: MSIX
ms.localizationpriority: medium
ms.openlocfilehash: eb8e7e88438645ae094f026935f7a4c20208079d
ms.sourcegitcommit: d749fa662214bddaa6854f1ee95761c547db8dae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2019
ms.locfileid: "75008036"
---
# <a name="group-policy-and-msix-packaged-apps"></a>Política de Grupo e aplicativos empacotados como MSIX

Os desenvolvedores que usam o MSIX podem aproveitar a Política de Grupo de modo semelhante a como se beneficiam de outros tipos de instaladores.

Se você empacotar o aplicativo Win32 em um MSIX (ou se criou o aplicativo usando a Ponte de Desktop), o aplicativo terá a funcionalidade de confiança total habilitada. Isso permite que você faça a leitura das chaves do Registro da Política de Grupo. Durante o tempo de execução, seu aplicativo terá a mesma exibição do Registro da Política de Grupo que teria se tivesse sido instalado usando um método diferente. Do Windows 10 1809 em diante, se o aplicativo for um aplicativo da UWP (Plataforma Universal do Windows), ele poderá acessar as mesmas chaves da Política de Grupo. Para obter mais informações sobre como criar a Política de Grupo, consulte [este artigo](https://docs.microsoft.com/openspecs/windows_protocols/ms-gpreg/834da877-264f-4589-9b80-b6b012c8edc3).

Se você estiver convertendo um instalador existente em MSIX usando a [Ferramenta de Empacotamento MSIX](mpt-overview.md), não precisará fazer nenhuma alteração para que o aplicativo dê suporte à Política de Grupo. Continue gerenciando as Políticas de Grupo como normalmente faria para o instalador original. Os aplicativos convertidos em MSIX ainda poderão fazer a leitura das chaves por meio do Registro da Política de Grupo existente. 

A Política de Grupo não tem suporte nativo para a instalação de aplicativos MSIX. 
