---
title: MSIX Core
description: Este artigo fornece uma visão geral do MSIX Core, que oferece suporte MSIX para o Windows 7 SP1, Windows 8.1, atualmente com suporte no Windows Server (com experiência desktop) e versões do Windows 10 anteriores à 1709 (atualização de aniversário de outono).
ms.date: 12/19/2019
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: f12efb71fb004f7354ef50193ef2127173e82ded
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090254"
---
# <a name="msix-core"></a>MSIX Core

O MSIX Core oferece suporte ao MSIX para versões do Windows anteriores ao Windows 10, versão 1709. O MSIX Core é um [projeto](https://github.com/Microsoft/msix-packaging/tree/master/MsixCore) de software livre no GitHub que permite que essas versões anteriores do Windows instalem pacotes MSIX. Você pode começar baixando a [versão mais recente](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-1.1-release) ou as [compilações de visualização](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-preview).

Com o MSIX Core, os desenvolvedores e os profissionais de ti que precisam dar suporte a usuários nessas versões anteriores do Windows agora podem adotar e aproveitar os benefícios do MSIX.

## <a name="what-is-msix-core"></a>O que é o MSIX Core?

O MSIX Core permite a instalação de aplicativos MSIX em versões anteriores do Windows, desde que os aplicativos já tenham sido criados para funcionar nessas versões do Windows. O MSIX Core foi criado para as seguintes versões do Windows que atualmente não dão suporte nativo a MSIX:

* Windows 7 SP1
* Windows 8.1
* Windows Server com suporte no momento (com experiência desktop)
* Versões do Windows 10 anteriores a 1709

O MSIX Core foi projetado para desenvolvedores e profissionais de ti. Os desenvolvedores podem usar a biblioteca do MSIX Core para permitir que os instaladores existentes instalem seus aplicativos empacotados do MSIX em versões anteriores do Windows, para que eles possam produzir apenas um pacote MSIX para direcionar todos os usuários do Windows. Os profissionais de ti podem baixar o instalador do MSIX Core.  O instalador do MSIX Core permite a instalação de linha de comando do MSIX junto com a capacidade de os usuários instalarem pacotes MSIX simplesmente clicando neles duas vezes.

## <a name="considerations-of-msix-core"></a>Considerações do MSIX Core

O objetivo do MSIX Core é habilitar a instalação, a consulta e a remoção de aplicativos empacotados do MSIX (que já funcionam nessas versões do Windows) e fornecer o máximo possível de uma desinstalação. O MSIX core fornece um subconjunto de recursos do MSIX nativo, funcionando de forma semelhante aos tipos existentes do Win32 Installer.

* O MSIX Core não fornece os benefícios do contêiner do MSIX nativo, nem permite que um aplicativo que usa recursos específicos do Windows 10 funcione em versões anteriores do Windows.
* Ao usar o MSIX Core em um sistema operacional de nível inferior, OS [aliases de execução de aplicativo](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-your-application-by-using-an-alias) só funcionarão do **Win + R** e não do prompt de comando nem do PowerShell.
* O MSIX Core não dá suporte à integração Microsoft Store. Os desenvolvedores que desejam publicar seus aplicativos na loja podem seguir a documentação [aqui](/windows/uwp/publish/).

## <a name="get-started"></a>Introdução

Para implantar o pacote MSIX com o MSIX Core, você deve primeiro [atualizar o manifesto existente do MSIX](support-msix-core.md). Em seguida, você pode [implantar seu pacote MSIX com o MSIX Core](deploy-with-msix-core.md) (se você tiver apenas o pacote) ou pode [criar um pacote do MSIX com o MSIX Core do código-fonte](msixcore-clickonce-solution.md) (se você tiver o código-fonte).