---
title: Criar um aplicativo com um pacote de modificação
description: Este artigo descreve como os pacotes de modificação vão funcionar com o aplicativo quando ele é empacotado como um MSIX.
ms.date: 02/04/2020
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 91eacb13777804c63c62a0b3eedd9c39d1e67530
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073982"
---
# <a name="building-an-app-with-a-modification-package"></a>Criar um aplicativo com um pacote de modificação 
Os pacotes de modificação permitem aos profissionais de TI personalizar os aplicativos MSIX sem precisar reempacotá-los. Isso significa que, quando o profissional de TI implanta o aplicativo principal e o pacote de modificação e o usuário executa o aplicativo, a personalização contida no pacote de modificação se sobrepõe ao aplicativo principal de modo a parecer um único aplicativo para o usuário. Isso acontece porque os pacotes de modificação funcionam dentro do contêiner MSIX do aplicativo principal e podem ser acessados apenas pelo aplicativo principal. Exemplos de pacotes de modificação são plug-ins/complementos, arquivos e chaves do Registro. Isso é algo a ter em mente durante o desenvolvimento do aplicativo, especialmente para empresas. 

Como desenvolvedor com o modelo de aplicativo principal e pacote de modificação, o cliente da empresa receberá o aplicativo mais recente e melhor de você, sem a necessidade de reempacotar. Isso acontece porque a personalização é tudo que fica armazenado no pacote de modificação que pode ser implantado e mantido de forma independente. 

