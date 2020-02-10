---
title: Contêiner MSIX
description: Este artigo descreve o contêiner MSIX
ms.date: 02/04/2020
ms.topic: article
author: dianmsft
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 760491c0cbfe31bb47e5deb201e43d885101b631
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073482"
---
# <a name="msix-container"></a>Contêiner MSIX

Aplicativos que são empacotados usando MSIX são executados em um contêiner de aplicativo leve. O processo do aplicativo MSIX e seus processos filho são executados dentro do contêiner e são isolados usando a virtualização do registro e do sistema de arquivos. Todos os aplicativos MSIX podem ler o registro global. Um aplicativo MSIX grava em seu próprio registro virtual e pasta de dados de aplicativo, e esses dados serão excluídos quando o aplicativo for desinstalado ou redefinido. Outros aplicativos não têm acesso ao registro virtual ou ao sistema de arquivos virtual de um aplicativo MSIX.

Para obter mais informações, visite [noções básicas sobre como os pacotes do MSIX são executados no Windows](desktop/desktop-to-uwp-behind-the-scenes.md). 
