---
title: Como a Política de Grupo funciona com aplicativos empacotados como MSIX
description: Descreve como a Política de Grupo funciona com aplicativos convertidos em MSIX
author: dianmsft
ms.author: diahar
ms.date: 04/12/2019
ms.topic: article
keywords: MSIX
ms.localizationpriority: medium
ms.openlocfilehash: d85f37e08d0748b1e8cab5cfc288def793c92d02
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2019
ms.locfileid: "66308443"
---
# <a name="group-policy-and-msix-packaged-apps"></a>Aplicativos de política de grupo e do MSIX empacotados

Os desenvolvedores que usam o MSIX podem aproveitar a Política de Grupo, de forma semelhante a outros tipos de instaladores.

Se você empacotar o aplicativo Win32 em um MSIX (ou se criou o aplicativo usando a Ponte de Desktop), o aplicativo terá a funcionalidade de confiança total habilitada. Isso permite que você faça a leitura das chaves do Registro da Política de Grupo. Em tempo de execução, seu aplicativo terá a mesma exibição do Registro da Política de Grupo como qualquer outro aplicativo. Se o aplicativo for um aplicativo UWP (Plataforma Universal do Windows), no Windows 10 1809 em diante, o aplicativo poderá acessar as mesmas chaves da Política de Grupo. Para obter mais informações sobre como criar a Política de Grupo, confira [este artigo](https://docs.microsoft.com/openspecs/windows_protocols/ms-gpreg/834da877-264f-4589-9b80-b6b012c8edc3).

Se você estiver convertendo um instalador existente em MSIX usando a [Ferramenta de Empacotamento MSIX](mpt-overview.md), não haverá nenhum novo trabalho necessário para que o aplicativo dê suporte à Política de Grupo. Continue gerenciando as políticas de grupo como normalmente faria para o instalador original. Os aplicativos convertidos em MSIX ainda poderão fazer a leitura das chaves do Registro da Política de Grupo existente. 

A Política de Grupo não tem suporte nativo para a instalação de aplicativos MSIX. 
