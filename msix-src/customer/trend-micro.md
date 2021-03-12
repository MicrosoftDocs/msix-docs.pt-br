---
title: Trend Micro
description: TREND micro descreve sua experiência com o MSIX
author: dianmsft
ms.date: 03/10/2021
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 93505f2f7ce207dc61200206d2412a43d067facf
ms.sourcegitcommit: 08c68ac2dd42098d734baeea215dfbdffda750a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/11/2021
ms.locfileid: "102783247"
---
# <a name="trend-micro"></a>Trend Micro

![Logotipo da Trend Micro](../images/Trend_Micro-lightmode.png)

A Trend Micro Incorporated., um líder global em segurança cibernética, ajuda a tornar o mundo seguro para a troca de informações digitais. Em um mundo cada vez mais conectado, nossas soluções inovadoras para empresas, governos e consumidores fornecem segurança em camadas para data centers, ambientes de nuvem, redes e pontos de extremidade.

Além do setor de segurança, também estamos procurando novas oportunidades em outros domínios, como a manutenção e a otimização do sistema. Por exemplo, estamos desenvolvendo uma versão mais limpa, um aplicativo inovador para ajudar os usuários a obter mais espaço livre em disco (removendo lixos, arquivos grandes, arquivos duplicados, etc.) e otimizam o desempenho do computador. No momento, o mais limpo tem dois canais de distribuição, Microsoft Store e online.

Durante nosso desenvolvimento, enfrentamos alguns desafios e, por fim, os resolvemos usando novas tecnologias de desenvolvimento do Windows. 

Anteriormente, uma versão de armazenamento mais limpa foi desenvolvida com base no aplicativo universal do Windows (UWP), a versão online era um aplicativo de desktop que adota o Win32 Tech. Era difícil manter duas ramificações de código diferentes. Para unificar as duas ramificações, escolhemos e aplicamos o empacotamento de áreas de trabalho e do Windows (ponte de desktop) e funcionou bem na prática. Além disso, aproveitando o C++/WinRT, implementamos com êxito as APIs "notificação do sistema do Windows" e "tarefa de inicialização" do Windows 10 na versão unificada.  

No limpador, o sistema de redes não tem o mecanismo de Chromium cujo tamanho de pacote é grande, o que torna o download e a atualização de todo o pacote difícil, especialmente quando há problemas de conexão de rede. Como MSIX é um método de empacotamento moderno no Windows e dá suporte à atualização incremental bem, com a ajuda do MS Windows AppConsult, começamos a implementar o empacotamento do MSIX, o que ajuda muito não apenas na atualização incremental, mas também na simplificação do CI/CD em nosso pipeline DevOps. Agora, o empacotamento moderno do Windows é executado sem problemas em nosso ambiente. Enquanto isso, nossa versão online do pacote do produto pode até mesmo se beneficiar do MSIX.

Com essas tecnologias, ajudamos nossos usuários e aprimoramos nossas aquisições também. 
-   Ao aproveitar o empacotamento do Windows, unificamos nossas ramificações de código da versão de armazenamento e da versão online.
-   Ao integrar a API "notificação do sistema do Windows", fornecemos uma experiência de usuário melhor e mais consistente com menos interferência.
-   Ao integrar a API de "tarefa de inicialização", fornecemos aos usuários a opção de habilitar ou desabilitar o limpador. Nós usamos para obter muitas preocupações com o usuário sobre a capacidade de controlar a inicialização automática do aplicativo.
-   Usando o MSIX, podemos tornar nosso produto modernizado na implantação, melhorar a experiência de atualização para os usuários e simplificar o pipeline do DevOps corretamente.

*"MSIX e WinRT são técnicas empolgantes para nós. O MSIX unifica o formato da nossa versão de armazenamento e da versão online, torna o empacotamento e a implantação mais fáceis para os desenvolvedores.  Espero que possamos resumir ainda mais MSIX e usá-lo para capacitar nosso processo de implantação. Em comparação com a API do Win32, o C++/WinRT é orientado a objeto, poderoso e ainda mais fácil de entender. O mais importante é que ele não só dá suporte a aplicativos UWP, mas também nos dá a oportunidade de usar as mais recentes tecnologias Windows 10 em aplicativos tradicionais do Windows. "* -Líder do desenvolvedor, Trend Micro
