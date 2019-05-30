---
author: mcleanbyron
title: Instalar aplicativos do Windows 10 com o instalador de aplicativo
description: Esta seção contém ou é ligada a artigos sobre o Instalador de Aplicativo e como usar seus recursos.
ms.author: mcleans
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, instalador de aplicativo, AppInstaller, sideload, conjunto relacionado, pacotes opcionais
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 74da9d3f2897947aecc1f63284777ff5c16b3c57
ms.sourcegitcommit: 5669d59a0979a9de1dead4949f44d1544fd45988
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795239"
---
# <a name="install-windows-10-apps-with-app-installer"></a>Instalar aplicativos do Windows 10 com o instalador de aplicativo

## <a name="purpose"></a>Finalidade
Esta seção contém ou é ligada a artigos sobre o Instalador de Aplicativo e como usar seus recursos.

O Instalador de Aplicativo permite que os aplicativos do Windows 10 sejam instalados clicando duas vezes no pacote do aplicativo. Isso significa que os usuários não precisam usar o PowerShell ou outras ferramentas de desenvolvedor para implantar aplicativos do Windows 10. O Instalador de Aplicativo também pode instalar um aplicativo da Web, pacotes opcionais e conjuntos relacionados. 

O instalador de aplicativo podem ser baixado para uso offline na empresa da Microsoft Store para empresas [portal da web](https://businessstore.microsoft.com/en-us/store/details/app-installer/9NBLGGH4NNS1). Você pode aprender mais sobre a distribuição off-line [aqui](https://docs.microsoft.com/en-us/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app).

Para saber como usar o Instalador de Aplicativo para instalar seu aplicativo, consulte os tópicos na tabela.

| Tópico | Descrição |
|-------|-------------|
| [Visão geral de arquivo do instalador de aplicativo](app-installer-file-overview.md) | Saiba mais sobre o conteúdo de arquivos do instalador do aplicativo e como eles funcionam. |
| [Criar um arquivo do instalador do aplicativo no Visual Studio](create-appinstallerfile-vs.md)| Saiba como usar o Visual Studio para permitir atualizações automáticas usando o arquivo .appinstaller. |
| [Criar um arquivo do instalador do aplicativo manualmente](how-to-create-appinstaller-file.md)| Saiba como criar um arquivo appinstaller manualmente. Isso é particularmente útil para a instalação de um conjunto relacionado que contém um pacote principal e pacotes opcionais. |
| [Definir configurações de atualização no arquivo de instalador de aplicativo](update-settings.md)  |  Saiba como configurar atualizações de aplicativo usando o arquivo do instalador do aplicativo. |
| [Instalar um aplicativo do Windows 10 da web](installing-windows10-apps-web.md) | Nesta seção, analisaremos as etapas que você precisa executar para permitir que os usuários instalem seus aplicativos diretamente a partir da página da Web. |
| [Pacotes opcionais e conjuntos relacionados](install-related-set.md) | Saiba mais sobre conjuntos relacionados que contêm um pacote principal e os pacotes opcionais relacionados.  |
| [Solucionar problemas de instalação com o arquivo do instalador do aplicativo](troubleshoot-appinstaller-issues.md) | Problemas e soluções comuns ao fazer sideload de aplicativos com o arquivo do Instalador de Aplicativo. |
| [Documentação relacionada](app-installer-documentation.md) | Fornece links para documentação relacionada, incluindo APIs que você pode usar para modificar pacotes por meio de arquivos do instalador do aplicativo ou para recuperar informações sobre os aplicativos com uma associação de instalador de aplicativo.  |
| [Referência de arquivo (appinstaller) de instalador de aplicativo](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file?context=/windows/msix/render) | Exibir o esquema XML completo para o arquivo do Instalador de Aplicativo. |

## <a name="tutorials"></a>Tutoriais

Siga estes tutoriais e saiba como hospedar e instalar um aplicativo do Windows 10 em várias plataformas de distribuição. Estes tutoriais são úteis para empresas e desenvolvedores que não desejam ou precisam publicar seus aplicativos na Store, mas ainda querem aproveitar a plataforma de empacotamento e implantação do Windows 10.

| Tutorial | Descrição |
|----------|-------------|
| [Instalar um aplicativo do Windows 10 de um aplicativo Web do Azure](web-install-azure.md) | Criar um aplicativo Web do Azure e usá-lo para hospedar e distribuir seu pacote de aplicativos do Windows 10. |
| [Instalar um aplicativo do Windows 10 de um servidor IIS](web-install-IIS.md) | Configure um servidor IIS, verifique se o aplicativo Web pode hospedar pacotes de aplicativos e use o Instalador de aplicativo de maneira eficaz. |
| [Instalam de pacotes de aplicativos no AWS para web hospedando o Windows 10](web-install-aws.md) | Saiba como configurar a Amazon Simple Storage Service para hospedar seu pacote de aplicativos do Windows 10 de um site da web. |
