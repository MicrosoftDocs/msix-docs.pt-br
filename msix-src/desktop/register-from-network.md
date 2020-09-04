---
description: Este artigo oferece orientações sobre como registrar um layout de pacote de um compartilhamento de rede
title: Registrar um layout de pacote de um compartilhamento de rede
ms.date: 02/06/2020
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: ca179a49a282d2bee6217a142ef94409e2161c8d
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091224"
---
# <a name="registering-a-package-layout-from-a-network-share"></a>Registrar um layout de pacote de um compartilhamento de rede

O desenvolvimento colaborativo e para vários dispositivos pode ser demorado devido à necessidade de copiar repositórios de pacotes, pastas de build etc. Caso esteja desenvolvendo no Windows 10 versão 1709 e posteriores é possível aproveitar um recurso para criar seu layout de pacote em um compartilhamento de rede e registrar o layout em um dispositivo remoto diretamente pela rede.

Várias pessoas podem contribuir para um único layout de pacote de aplicativo em um compartilhamento de rede e as alterações estarão visíveis para outros colaboradores e usuários que registraram o aplicativo. Se estiver criando um aplicativo para vários dispositivos, poderá aproveitar o recurso e evitar ter que copiar o aplicativo para cada dispositivo para fins de teste.

## <a name="prerequisites"></a>Pré-requisitos

1. Seus dispositivos devem estar na Atualização do Windows 10 para Criadores Insider Build 14965 ou posterior.

2. Será necessário habilitar o modo desenvolvedor e a detecção de dispositivo em todos os dispositivos.

3. Os colaboradores precisam de acesso de leitura e gravação à pasta do build.

4. Os usuários precisarão apenas de acesso de leitura à pasta do build.

## <a name="in-visual-studio"></a>No Visual Studio

Se estiver desenvolvendo no Visual Studio, será possível seguir as etapas descritas [aqui](/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#advanced-remote-deployment-options).

## <a name="from-the-command-line"></a>Da linha de comando

Caso não esteja desenvolvendo no Visual Studio e usando ferramentas de linha de comando, será possível usar [WinDeployAppCmd](/windows/uwp/packaging/install-universal-windows-apps-with-the-winappdeploycmd-tool). Abaixo está um exemplo de como fazer isso por uma janela de linha de comando:

```
WinAppDeployCmd.exe registerfiles -remotedeploydir <network path> -ip <IP Address> -pin <target machine PIN>
```
- Caminho de Rede – o caminho para os arquivos soltos do aplicativo;

- Endereço IP – insira o Endereço IP do computador de destino aqui;

- PIN - Um PIN, se necessário para estabelecer uma conexão com o dispositivo de destino. (Você deverá tentar novamente usando a opção -pin caso a autenticação seja necessária.) Clique aqui para saber como obter um PIN.
 

Você também pode criar um aplicativo totalmente empacotado que acessa arquivos em um compartilhamento de rede durante o teste e a validação. Veja este [exemplo](https://github.com/AppInstaller/Windows-appsample-marble-maze).