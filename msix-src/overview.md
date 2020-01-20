---
title: O que é MSIX?
description: Este artigo apresenta os conceitos básicos do formato de empacotamento MSIX, uma experiência moderna para todos os aplicativos do Windows.
ms.date: 12/02/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 620d71b0e92fa9e22700d51c9c8a118accf6d207
ms.sourcegitcommit: f095305b47b5ba14f96413ee520777be45090e3b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75877105"
---
# <a name="what-is-msix"></a>O que é MSIX?

O MSIX é um formato de pacote de aplicativo do Windows que oferece uma experiência de empacotamento moderna para todos os aplicativos do Windows. O formato de pacote MSIX preserva a funcionalidade de pacotes de aplicativo existentes e/ou de arquivos de instalação, além de habilitar recursos de empacotamento e implantação novos e modernos para aplicativos Win32, WPF e Windows Forms.

> [!TIP]
> Visite a página da [Comunidade de Tecnologia do MSIX](https://aka.ms/msixcommunity) para as discussões e informações mais recentes.

Aqui estão alguns destaques breves do MSIX:

* **Empacotar aplicativos existentes do Windows.** Use a [Ferramenta de Empacotamento de MSIX](packaging-tool/mpt-overview.md) para criar um pacote MSIX para qualquer aplicativo do Windows, antigo ou novo. A Ferramenta de Empacotamento MSIX simplifica a experiência de empacotamento, oferecendo uma interface do usuário interativa ou uma linha de comando para converter e empacotar aplicativos do Windows.
* **Instalar pacotes de aplicativo do MSIX.** Use o [Instalador de Aplicativo](app-installer/app-installer-root.md) para instalar ou atualizar qualquer pacote de aplicativo MSIX que esteja localmente disponível ou em qualquer rede de distribuição de conteúdo.
* **Aplicar correções de tempo de execução para aplicativos empacotados.** A [Estrutura de Suporte do Pacote](psf/package-support-framework-overview.md) é um kit de software livre que ajuda a aplicar correções a seu aplicativo da área de trabalho existente quando não há acesso ao código-fonte, para que ele possa ser executado em um contêiner do MSIX.
* **Usar o MSIX em qualquer lugar.** Com o [SDK do MSIX](msix-sdk/sdk-overview.md) de software livre, os pacotes MSIX são mais versáteis e independentes de plataforma. O SDK fornece todas as APIs necessárias para verificar, validar e desempacotar um pacote de aplicativo em qualquer plataforma, incluindo plataformas do Windows 10 e plataformas que não são do Windows 10.

Este vídeo apresenta as principais maneiras pelas quais o empacotamento MSIX pode ajudar você a simplificar e melhorar os fluxos de trabalho de implantação e instalação de seu aplicativo.

<br/>

> [!VIDEO https://www.youtube.com/embed/phrD081sMWc]