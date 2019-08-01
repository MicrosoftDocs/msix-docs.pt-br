---
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: Instalação de streaming de aplicativo
description: A instalação do streaming de aplicativos permite que você especifique quais partes do seu aplicativo você deseja que o Microsoft Store Baixe primeiro. Quando os arquivos essenciais do aplicativo são baixados primeiro, o usuário pode iniciar e interagir com o aplicativo enquanto o resto termina de fazer o download em segundo plano.
ms.date: 07/02/2019
author: dianmsft
ms.author: diahar
ms.topic: article
keywords: Windows 10, msix, UWP, instalação de streaming
ms.localizationpriority: medium
ms.openlocfilehash: 95c4301b21ccbe7775afbac873565d778ba64e32
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68690005"
---
# <a name="app-streaming-install"></a>Instalação de streaming de aplicativo

A instalação do streaming de aplicativos permite que você especifique quais partes do seu aplicativo você deseja que o Microsoft Store Baixe primeiro. Quando os arquivos essenciais do aplicativo são baixados primeiro, o usuário pode iniciar e interagir com o aplicativo enquanto o resto termina de fazer o download em segundo plano.

Para usar a instalação de streaming de aplicativo, você precisará dividir os arquivos do aplicativo em seções. Para fazer isso, você criará um mapa de grupo de conteúdo, que é um arquivo XML que está empacotado com seu aplicativo, permitindo que você defina prioridade e ordem de download. Veja o tópico abaixo relacionado para obter mais informações.

Para obter um guia completo sobre como adicionar a instalação de streaming ao seu aplicativo, Confira esta [série de Blogs](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).

| Tópico | Descrição |
|-------|-------------|
| [Criar e converter um mapa de grupo de conteúdo de origem](create-cgm.md) | Para preparar seu aplicativo para a instalação de streaming, você precisará criar um mapa de grupo de conteúdo. Este artigo irá ajudá-lo com as especificidades de criar e converter um mapa de grupo de conteúdo, fornecendo algumas dicas e truques ao longo do caminho. |