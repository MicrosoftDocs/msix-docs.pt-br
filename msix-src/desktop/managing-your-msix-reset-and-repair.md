---
description: Este artigo fornece detalhes sobre a redefinição e o reparo de aplicativos MSIX que foram implantados em um dispositivo.
title: Redefinir ou reparar Aplicativos MSIX
ms.date: 11/10/2020
ms.topic: article
keywords: windows 10, implantação, msix, redefinir, reparar
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: f2a0047242e300a4e10c0afcf5db89d41d3e2ae4
ms.sourcegitcommit: de0c711ce28851ef6976a71dd1317291f278b4d1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96615511"
---
# <a name="reset-or-repair-msix-apps"></a>Redefinir ou reparar aplicativos MSIX

## <a name="overview"></a>Visão geral

Os aplicativos do Windows (*.msix, *.msixbundle, *.appx e *.appxbundle) oferecem uma funcionalidade conhecida como Redefinir e Reparar Aplicativo. Que pode ser executada pelo usuário do dispositivo (sem elevação) para reinstalar ou atualizar a instalação do Aplicativo do Windows em uma tentativa de resolver uma falha com um Aplicativo do Windows.

## <a name="repair"></a>Reparar

Você pode reparar aplicativos do Windows que foram instalados em uma imagem online do Windows. Quando você usa as Configurações do Windows para reparar um Aplicativo do Windows, ele retém os dados do aplicativo enquanto tenta recarregá-lo.

### <a name="repairing-a-windows-app"></a>Como reparar um Aplicativo do Windows

Como reparar um aplicativo do Windows usando as configurações do Windows:
    1. Abra o Aplicativo Configurações no menu iniciar.
    1. Selecione o item de menu **Aplicativos**.
    1. Selecione o item de menu **Aplicativos & Recursos**, na navegação do lado esquerdo.
    1. Selecione o Aplicativo que precisa ser redefinido.
    1. Selecione o link **Opções avançadas**.
    1. Selecione o botão **Reparar**.


## <a name="reset"></a>Redefinir

Você pode redefinir aplicativos do Windows que foram instalados em uma imagem online do Windows. Quando você usar os cmdlets do PowerShell, ou o aplicativo de Configurações do Windows para redefinir um Aplicativo do Windows, ele excluirá permanentemente os dados de aplicativos e reinstalará o aplicativo novo. O Aplicativo do Windows perderá as preferências e/ou os detalhes de entrada etc....


### <a name="resetting-a-windows-app"></a>Como redefinir um Aplicativo do Windows

Como redefinir um Aplicativo do Windows usando o Windows PowerShell: 
    1. Abra uma janela administrativa do PowerShell e digite:
        ```PowerShell
        Get-AppxPackage -Name "MyEmployees" | Reset-AppxPackage
        ```

Como redefinir um aplicativo do Windows usando as configurações do Windows:
    1. Abra o Aplicativo Configurações no menu iniciar.
    1. Selecione o item de menu **Aplicativos**.
    1. Selecione o item de menu **Aplicativos & Recursos**, na navegação do lado esquerdo.
    1. Selecione o Aplicativo que precisa ser redefinido.
    1. Selecione o link **Opções avançadas**.
    1. Selecione o botão **Redefinir**.
    1. No prompt de confirmação, selecione o botão **Redefinir**.
