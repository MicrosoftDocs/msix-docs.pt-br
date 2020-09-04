---
description: Uma visão geral dos tópicos sobre criação de um pacote do MSIX com base em código-fonte
title: Criar um pacote do MSIX com base em seu código
ms.date: 01/27/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6fb34afdbe3b3b5f74c6a19997e6c7a29994b471
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090614"
---
# <a name="extend-your-packaged-applications"></a>Estenda os aplicativos empacotados

O MSIX facilita a extensão do aplicativo usando extensões de aplicativos e pacotes opcionais. As extensões de aplicativo proporcionam funcionalidade semelhante ao que plug-ins, suplementos e complementos fazem em outras plataformas. Você pode tornar o aplicativo um host de extensão para permitir que ele consuma conteúdo e eventos de implantação de uma extensão empacotada. Extensões de aplicativo foram apresentadas na Edição de Aniversário do Windows 10 (versão 1607, build 10.0.14393).

Os pacotes opcionais são úteis para dividir um aplicativo grande ou complexo ou para adicionar novos componentes a um aplicativo que já foi publicado. Com o Visual Studio 2017, versão 15.7 e .NET Native 2.1, é possível carregar código executável de pacotes opcionais C++ e C#.

As extensões de aplicativo são um ecossistema aberto e destinam-se a qualquer pessoa para aprimorar o aplicativo. Não há retenção ou controle sobre quem pode fazer uma extensão de aplicativo. Os pacotes opcionais são um ecossistema fechado no qual o editor decide quem tem permissão de fazer um pacote opcional para o pacote principal.

As extensões de aplicativo também são pacotes independentes. Elas podem ser aplicativos independentes e não podem ter uma dependência de implantação com outro aplicativo.  Os pacotes opcionais precisam do pacote principal e não podem funcionar sem ele.

|Tópico| Descrição |
|:---|:---|
|[Criação e hospedagem de uma extensão de aplicativo](/windows/uwp/launch-resume/how-to-create-an-extension?context=%252fwindows%252fmsix%252frender)|Esta seção aborda como criar e hospedar uma extensão de aplicativo no pacote MSIX. |
[Propriedades personalizadas de extensões de aplicativo](custom-props-app-extensions.md)|Esta seção discute como usar as propriedades personalizadas de extensões de aplicativo. |
|[Extensão do aplicativo usando pacotes opcionais](../package/optional-packages-with-executable-code.md)| Esta seção aborda como aproveitar o modelo de pacote opcional para carregar conteúdo no pacote principal. |
