---
author: mcleanbyron
title: Instalar aplicativos do Windows 10 de uma página da web
description: Nesta seção, analisaremos as etapas que você precisa executar para permitir que os usuários instalem seus aplicativos diretamente a partir da página da Web.
ms.author: mcleans
ms.date: 11/16/2017
ms.topic: article
keywords: windows 10, uwp, instalador de aplicativo, AppInstaller, sideload, conjunto relacionado, pacotes opcionais
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2a2645ae4527342af85d747a2217e576a91c0c2c
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900948"
---
# <a name="installing-windows-10-apps-from-a-web-page"></a>Instalar aplicativos do Windows 10 de uma página da web

Normalmente, um aplicativo precisa estar disponível localmente em um dispositivo antes de poder ser instalado com o Instalador de Aplicativo. Para o cenário da Web, isso significa que o usuário deve baixar o conjunto de aplicativo do servidor Web, após o qual ele pode ser instalado com o Instalador de Aplicativo. Isso é ineficiente e desperdiça espaço de disco, por isso, o Instalador de Aplicativo agora tem recursos integrados para otimizar o processo no disco.

O Instalador de Aplicativo pode instalar um aplicativo diretamente de um servidor Web. Quando o usuário clica em um link de Web do aplicativo hospedado do pacote, o Instalador de Aplicativo é invocado automaticamente. O usuário, em seguida, será levado para a exibição de informações do aplicativo no Instalador de Aplicativo e depois estará a um clique de distância de interagir diretamente com o aplicativo.

A instalação do aplicativo direta só está disponível no Windows 10 Fall Creators Update e versões mais recentes. Versões anteriores do Windows (voltando para a Atualização de Aniversário do Windows 10) terão suporte pela [Experiência de instalação da Web em versões anteriores do Windows 10](#web-install-experience). Essa experiência não é tão fluída como a instalação direta do aplicativo, mas ela fornece melhorias significativas ao procedimento de instalação do aplicativo existente.

> [!NOTE]
> A versão do Instalador de Aplicativo deve ser superior a 1.0.12271.0 para oferecer suporte a esse recurso.

## <a name="protocol-activation-scheme"></a>Esquema de ativação de protocolos
Nesse mecanismo, o Instalador de Aplicativo registra com o sistema operacional para um esquema de ativação de protocolo. Quando o usuário clica em um link da Web, o navegador busca no sistema operacional aplicativos que estão registrados em um link da Web. Se o esquema coincidir com o esquema de ativação de protocolo especificado pelo Instalador de Aplicativo, o instalador é invocado. É importante observar que esse mecanismo é independente do navegador. Isso é benéfico para administradores de sites, por exemplo, que não precisam considerar diferenças entre navegadores da Web ao incorporar isso em uma página da Web.

### <a name="requirements-for-protocol-activation-scheme"></a>Requisitos para o esquema de ativação de protocolo

1. Servidores Web precisam ter suporte para solicitações de intervalo de bytes (HTTP/1.1)
    - Servidores que dão suporte ao protocolo HTTP/1.1 devem ter suporte para solicitações de intervalo de bytes
2. Servidores Web precisam saber sobre os tipos de conteúdo do pacote de aplicativo Windows 10
    - Aqui está como declarar os novos tipos de conteúdo como parte do [o arquivo de configuração da web](web-install-IIS.md#step-7---configure-the-web-app-for-app-package-mime-types)

### <a name="how-to-enable-this-on-a-webpage"></a>Como habilitar isso em uma página da Web
Os desenvolvedores de aplicativos que desejam pacotes do aplicativo host em seus sites precisam seguir essa etapa:

Fixe seus URIs de conjunto de aplicativo com o esquema de ativação `'ms-appinstaller:?source='` onde o Instalador de Aplicativo está registrado para quando for fazer referência a eles em sua página da Web. Consulte o exemplo de **Página da Web MyApp** para obter detalhes.
``` html
<html>
    <body>
        <h1> MyApp Web Page </h1>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubApp.appx"> Install app package </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppBundle.appxbundle"> Install app bundle  </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppSet.appinstaller"> Install related set </a>
    </body>
</html>
```

## <a name="signing-the-app-package"></a>Assinando o pacote de aplicativos
Para que os usuários instalem seu aplicativo, você precisará assinar o pacote do aplicativo com um certificado confiável. Você pode usar um certificado de terceiros pago de uma autoridade de certificação confiável para assinar seu pacote do aplicativo. Se um certificado de terceiros for usado, o usuário precisará ter o dispositivo no modo de sideload ou desenvolvedor para instalar e executar seu aplicativo.

Se você estiver implantando um aplicativo para funcionários em uma empresa, poderá usar uma empresa que emitiu o certificado para assinar o aplicativo. É importante observar que o certificado corporativo deve ser implantado em todos os dispositivos em que o aplicativo será instalado. Para obter mais informações sobre como implantar aplicativos corporativos, consulte [Gerenciamento de aplicativos corporativos](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management).

## Instalar experiência da Web em versões anteriores do Windows 10<a name="web-install-experience"></a>

Invocar o Instalador de Aplicativo do navegador é compatível com todas as versões do Windows 10 onde o Instalador de Aplicativo está disponível (começando com a atualização de aniversário). No entanto, a funcionalidade para instalar diretamente da web sem a necessidade de baixar o pacote primeiro só está disponível no Windows 10 Fall Creators Update.  

Usuários de versões anteriores do Windows 10 (com o instalador do aplicativo disponível) também podem tirar proveito da instalação da web de aplicativos do Windows 10 por meio do instalador do aplicativo, mas terão uma experiência de usuário diferente. Quando os usuários clicarem no link da Web, o Instalador de Aplicativo solicitará para **Baixar** o pacote, em vez de **Instalar**. Após o download, o Instalador de Aplicativo irá iniciar o lançamento do pacote baixado automaticamente. Como o conjunto de aplicativo é baixado da Web, esses arquivos passarão pelo Microsoft SmartScreen para uma verificação de segurança. Depois do usuário fornecer permissão para continuar e, em seguida, mais um clique em **Instalar**, o aplicativo está pronto para uso!

Embora esse fluxo não seja tão perfeito como a instalação direta no Windows 10 Fall Creators Update, os usuários ainda podem interagir rapidamente com o aplicativo. Além disso, com esse fluxo o usuário não precisa se preocupar com arquivos do conjunto de aplicativo desnecessariamente ocupando espaço em unidades. O Instalador de Aplicativo gerencia o espaço com eficiência baixando o pacote em sua pasta de dados de aplicativo e limpando pacotes quando eles não são mais necessários.

Veja uma rápida comparação da versão do Windows 10 Fall Creators Update do Instalador de Aplicativo e a versão anterior do Instalador de Aplicativo:

| Instalador de Aplicativo, Última versão | Instalador de Aplicativo, Versão anterior |
|------------------------------|----------------------------------|
| O Instalador de Aplicativo mostra as informações do aplicativo antes de iniciar o download | Navegador solicita que o usuário optar por baixar  |
| O Instalador de Aplicativo executa o download | Usuário deve iniciar manualmente o lançamento do conjunto de aplicativo |
| Após o download do pacote, o Instalador de Aplicativo inicia automaticamente o conjunto de aplicativo | Usuário deve clicar em **Instalar** e iniciar manualmente o conjunto de aplicativo |
| O Instalador de Aplicativo cuidará da remoção de pacotes baixados | Usuário exclui manualmente os arquivos baixados |

## <a name="web-install-experience-on-previous-versions-of-windows-10"></a>Instalar experiência da Web em versões anteriores do Windows 10
Em versões anteriores a Windows 10 Fall Creators Update, o Instalador de Aplicativo não pode instalar um aplicativo da Web. Nessas versões, o Instalador de Aplicativo só pode instalar pacotes de aplicativos que estão disponíveis localmente. Em vez disso, o Instalador de Aplicativo baixará o pacote e pedirá que o usuário clique duas vezes no pacote baixado para instalar.


## <a name="microsoft-smartscreen-integration"></a>Integração do Microsoft SmartScreen

Microsoft SmartScreen sempre foi parte do processo de instalação para instalar aplicativos por meio do Instalador de Aplicativo. O SmartScreen garante que os usuários estão protegidos contra conteúdo malicioso que pode entrar em seus dispositivos. Com a atualização mais recente do Instalador de Aplicativo, a integração com o SmartScreen é mais perfeita e robusta, fornecendo avisos quando instalar aplicativos desconhecidos e protegendo os dispositivos contra danos.
