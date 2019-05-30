---
title: Como a diretiva de grupo funcionam com MSIX aplicativos empacotados
description: Descreve como a diretiva de grupo funciona com aplicativos convertido para MSIX
author: dianmsft
ms.author: diahar
ms.date: 04/12/2019
ms.topic: article
keywords: msix
ms.localizationpriority: medium
ms.openlocfilehash: d85f37e08d0748b1e8cab5cfc288def793c92d02
ms.sourcegitcommit: 958d9e8177036c8485fb79b49f35cb2deb8d553c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308443"
---
# <a name="group-policy-and-msix-packaged-apps"></a>Aplicativos empacotados de política de grupo e MSIX

Os desenvolvedores que usam MSIX podem aproveitar a diretiva de grupo, semelhante a outros tipos de instalador.

Se você empacotou o aplicativo Win32 em um MSIX (ou se você criou seu aplicativo usando a ponte de Desktop), seu aplicativo tem a capacidade de confiança total habilitada. Isso permite que você leia as chaves de registro de diretiva de grupo. Em tempo de execução, seu aplicativo terá a mesma exibição do registro de diretiva de grupo, como faria com qualquer outro aplicativo. Se seu aplicativo é um aplicativo de plataforma Universal do Windows (UWP), começando no Windows 10 1809 seu aplicativo pode acessar as mesmas chaves de política de grupo. Para obter mais informações sobre a criação de política de grupo, consulte [deste artigo](https://docs.microsoft.com/openspecs/windows_protocols/ms-gpreg/834da877-264f-4589-9b80-b6b012c8edc3).

Se você estiver convertendo um instalador existente em MSIX usando o [ferramenta de empacotamento MSIX](mpt-overview.md), não há nenhum novo trabalho necessário para seu aplicativo dar suporte à diretiva de grupo. Continue a gerenciar políticas de grupo, como faria normalmente para o instalador original. Os aplicativos convertidos em MSIX ainda poderão ler de chaves do registro de diretiva de grupo existente. 

Diretiva de grupo não tem suporte nativo para instalar aplicativos MSIX. 
