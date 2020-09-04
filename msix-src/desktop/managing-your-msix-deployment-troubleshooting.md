---
Description: Este artigo tem todos os detalhes de que você precisa para gerenciar a implantação de aplicativos MSIX em um ambiente empresarial.  Este artigo destina-se a profissionais de TI e corporativos.
title: Gerenciar a implantação do MSIX
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, implantação, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 2ecd18c4bf7cc48bca8f9cea39dedd4a042e41fc
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090454"
---
# <a name="msix-validation-and-troubleshooting"></a>Validação e solução de problemas do MSIX
Embora o MSIX tenha uma taxa de 99% de instalações bem-sucedidas, às vezes, você pode precisar resolver problemas em uma instalação.

## <a name="know-the-application"></a>Conheça o aplicativo
Conhecer o aplicativo que está sendo instalado e como ele funciona pode ser uma excelente forma de solucionar problemas de experiência do usuário.  Por exemplo, o aplicativo tem determinadas limitações que podem estar causando os problemas?  O usuário tem acesso aos recursos necessários para o aplicativo?  Alguma dependência do aplicativo não foi atendida pelo sistema operacional atual?

## <a name="test-your-application"></a>Testar o aplicativo
Antes de implantar o aplicativo, assegure-se de testá-lo.  O SDK do Windows fornece uma ferramenta, o Kit de Certificação de Aplicativos Windows, que pode identificar problemas comuns antes da publicação.  
Para instalar o SDK mais recente do Windows, [acesse aqui.](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
Para saber mais sobre o Kit de Certificação de Aplicativos Windows, consulte [Kit de Certificação de Aplicativos Windows](/windows/uwp/debug-test-perf/windows-app-certification-kit)

## <a name="flight-your-application"></a>Libere uma versão de pré-lançamento do aplicativo
Outra ótima maneira de detectar problemas com antecedência é usar uma versão de pré-lançamento dos aplicativos.  Se estiver implantando pela Windows Store ou pela Microsoft Store para Empresas, será possível usar versões de pré-lançamento para implantar o aplicativo em um subconjunto de indivíduos para obter testes reais adicionais.  
Para saber mais sobre os testes, consulte [Liberação de versões de pré-lançamento do pacote.](/windows/uwp/publish/package-flights?context=%252fwindows%252fmsix%252frender)

## <a name="device-portal-and-debugging"></a>Portal de Dispositivos e Depuração
Às vezes, é necessário interagir com o aplicativo no ambiente do usuário para entender adequadamente o problema.  O Windows fornece uma ferramenta poderosa, o [Portal de Dispositivos de Área de Trabalho](/windows/uwp/debug-test-perf/device-portal-desktop), que permite que você se conecte ao dispositivo e interaja com o aplicativo remotamente.  
Para saber mais sobre o Portal de Dispositivos, consulte [Visão geral do Portal de Dispositivos](/windows/uwp/debug-test-perf/device-portal-desktop).
Para saber mais sobre a Depuração de pacotes MSIX, consulte [Executar, depurar e testar um aplicativo de área de trabalho empacotado.](./desktop-to-uwp-debug.md)

## <a name="installation-issues"></a>Problemas de instalação
Quando há problemas com a instalação, é possível investigar os artefatos fornecidos pelo AppInstaller.  Primeiro, o AppInstaller fornece falhas de código de erro.  Se o código de erro de qualquer falha não for suficiente para determinar o que está errado, o AppInstaller também registrará todas as interações no Visualizador de Eventos.  Você encontrará os logs aqui: Logs de Aplicativos e Serviços->Microsoft->Windows->AppxDeployment-Server.

Você pode usar o Visualizador de Eventos ou o PowerShell para acessar esses eventos. Para saber mais sobre como exibir os eventos, veja [Solução de problemas de empacotamento, implantação e consulta de aplicativos do Windows.](/windows/win32/appxpkg/troubleshooting)

Para saber mais sobre a solução de problemas do AppInstaller, veja [Solucionar problemas do AppInstaller.](../app-installer/troubleshoot-appinstaller-issues.md)


## <a name="next-steps"></a>Próximas etapas

Tem dúvidas? Pergunte-nos no Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums//home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

