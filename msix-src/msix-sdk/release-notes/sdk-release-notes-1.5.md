---
title: Notas de versão MSIX SDK versão 1.5
description: Notas de versão do SDK MSIX versão 1.5
author: c-don
ms.author: cdon
ms.date: 02/06/2018
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: f6b6350c6d55b0d330d73c4588d4c9c50fb151e1
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900488"
---
# <a name="msix-sdk-15-update"></a>Atualização MSIX SDK 1.5

Com o lançamento do SDK (1.5), nosso investimento principal era reduzir o tamanho do binário do SDK. Um dos principais comentários sobre nosso SDK era o tamanho e especialmente para aplicativos móveis com uma dependência binária que custará cerca de 5MB não é satisfatória. 

Para resolver esse problema, substituímos as dependências de binárias estáticas para análise de XML com os binários de caixa de entrada no Android e iOS/MacOS. Como parte da redução do tamanho binário, também adicionamos opções para as diferentes funcionalidades nos scripts de compilação que está disponível com o SDK. Adicionando apenas recursos que seu aplicativo precisa no binário, o tamanho do tamanho do binário MSIX SDK pode ser ainda mais reduzido. 

Você pode obter o SDK mais recente no GitHub. 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging/tree/release_v1.5" data-linktype="external">MSIX SDK no GitHub</a></p></div>

