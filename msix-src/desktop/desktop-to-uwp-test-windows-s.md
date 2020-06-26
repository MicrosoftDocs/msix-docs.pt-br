---
description: Este artigo descreve como testar seu aplicativo do Windows para verificar se ele funcionará corretamente em dispositivos que executam o Windows 10 no modo S.
title: Testar seu aplicativo do Windows para Windows 10 S
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10 S, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: d6b19d6664bb8e2bc95b8f3e85b34541e2fc68e3
ms.sourcegitcommit: e3a06eccd3322053b8b498cb6343fb6f711a7a0b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84724520"
---
# <a name="test-your-windows-app-for-windows-10-in-s-mode"></a>Testar seu aplicativo do Windows para Windows 10 no modo S

Teste seu aplicativo do Windows para verificar se ele funcionará corretamente em dispositivos que executam o Windows 10 no modo S. Na verdade, se você pretende publicar seu aplicativo na Microsoft Store, faça isso, pois esse é um requisito da loja. Para testar o aplicativo, você pode aplicar uma política do WDAC (Controle de Aplicativos do Windows Defender) a um dispositivo que esteja executando o Windows 10 Pro.

> [!NOTE]
> Teste seu aplicativo do Windows para verificar se ele funcionará corretamente em dispositivos que executam o Windows 10 no modo S. Na verdade, se você pretende publicar seu aplicativo na Microsoft Store, faça isso, pois esse é um requisito da loja. Para testar o aplicativo, você pode aplicar uma política do WDAC a um dispositivo que esteja executando o Windows 10 Pro.


Essa política do WDAC impõe as regras com as quais os aplicativos devem estar em conformidade para serem executados no Windows 10 S.


> [!IMPORTANT]
>Recomendamos que você aplique essas políticas a uma máquina virtual, mas se você deseja aplicá-las ao computador local, examine nossas diretrizes de melhores práticas na seção "Próximo, instalar a política e reiniciar o sistema" deste tópico antes de aplicar uma política.

<a id="choose-policy"></a>

## <a name="first-download-the-policies-and-then-choose-one"></a>Primeiro, baixe as políticas e, depois, escolha uma delas

Baixe as políticas do WDAC [aqui](https://go.microsoft.com/fwlink/?linkid=849018).

Em seguida, escolha a que faz mais sentido para você. Veja abaixo um resumo de cada política.

|Política |Imposição |Certificado de autenticação |Nome do Arquivo |
|--|--|--|--|
|Política de modo de auditoria |Registra problemas em log/não faz bloqueios |Repositório |SiPolicy_Audit.p7b |
|Política de modo de produção |Sim |Repositório |SiPolicy_Enforced.p7b |
|Política de modo de produto com aplicativos autoassinados |Sim |Certificado de teste do AppX  |SiPolicy_DevModeEx_Enforced.p7b |

Recomendamos que você comece com a política de modo de auditoria. Examine os logs de eventos de integridade do código e use essas informações para ajudar você a fazer ajustes no seu aplicativo. Em seguida, aplique a política de modo de produção quando estiver pronto para o teste final.

Veja a seguir mais informações sobre cada política.

### <a name="audit-mode-policy"></a>Política de modo de auditoria
Com esse modo, o aplicativo é executado, mesmo que ele execute tarefas não compatíveis com o Windows 10 S. O Windows registra em log todos os executáveis que teriam sido bloqueados nos logs de eventos de integridade do código.

Encontre esses logs abrindo o **Visualizador de Eventos** e navegue até esta localização: Logs de Aplicativos e Serviços -> Microsoft -> Windows -> Integridade de Código -> Operacional.

![code-integrity-event-logs](images/code-integrity-logs.png)

Esse modo é seguro e não impedirá que o sistema seja inicializado.

#### <a name="optional-find-specific-failure-points-in-the-call-stack"></a>(Opcional) Encontrar pontos de falha específicos na pilha de chamadas
Para encontrar pontos específicos na pilha de chamadas em que ocorrem problemas de bloqueio, adicione esta chave do Registro e depois [configure um ambiente de depuração do modo kernel](https://docs.microsoft.com/windows-hardware/drivers/debugger/getting-started-with-windbg--kernel-mode-#span-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanset-up-a-kernel-mode-debugging).

|Chave|Nome|Tipo|Valor|
|--|---|--|--|
|HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\CI| DebugFlags |REG_DWORD | 1 |


![reg-setting](images/ci-debug-setting.png)

### <a name="production-mode-policy"></a>Política de modo de produção
Essa política impõe as regras de integridade do código que combinam o Windows 10 S para que você possa simular a execução no Windows 10 S. Essa é a política mais rigorosa e é excelente para o teste de produção final. Neste modo, seu aplicativo está sujeito às mesmas restrições que estarão sujeitas no dispositivo de um usuário. Para usar esse modo, o aplicativo precisa ser assinado pela Microsoft Store.

### <a name="production-mode-policy-with-self-signed-apps"></a>Política de modo de produção com aplicativos autoassinados
Esse modo é semelhante à política de modo de produção, mas também permite a execução de itens que sejam assinados com o certificado de teste incluído no arquivo zip. Instale o arquivo .pfx incluído na pasta **AppxTestRootAgency** desse arquivo zip. Em seguida, assine o aplicativo nele. Dessa forma, você pode iterar rapidamente sem precisar da assinatura da Store.

Como o nome do fornecedor do certificado precisa corresponder ao nome do fornecedor do aplicativo, você precisará alterar temporariamente o valor do atributo **Publisher** do elemento **Identity** para "CN=Appx Test Root Agency Ex". Você pode alterar esse atributo novamente para o valor original depois de concluir os testes.

## <a name="next-install-the-policy-and-restart-your-system"></a>Em seguida, instale a política e reinicie o sistema

Recomendamos que você aplique essas políticas a uma máquina virtual, porque essas políticas podem levar a falhas de inicialização. Isso ocorre porque essas políticas bloqueiam a execução do código que não foi assinado pela Microsoft Store, incluindo drivers.

Se você deseja aplicar essas políticas ao computador local, é melhor começar com a política de modo de auditoria. Com essa política, você pode examinar os logs de eventos de integridade do código para verificar se nada crítico será bloqueado em uma política imposta.

Quando estiver pronto para aplicar uma política, encontre o arquivo .P7B da política escolhida, renomeie-o para **SIPolicy.P7B** e salve esse arquivo nesta localização do sistema: **C:\Windows\System32\CodeIntegrity\\** .

Em seguida, reinicie o sistema.

>[!NOTE]
>Para remover uma política do sistema, exclua o arquivo .P7B e reinicie o sistema.

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas dúvidas**

Tem dúvidas? Pergunte-nos no Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Examinar um artigo de blog detalhado postado pela nossa Equipe de Consultoria de Aplicativos**

Confira [Como portar e testar seus aplicativos clássicos da área de trabalho no Windows 10 S com a Ponte de Desktop](https://blogs.msdn.microsoft.com/appconsult/2017/06/15/porting-and-testing-your-classic-desktop-applications-on-windows-10-s-with-the-desktop-bridge/).

**Saiba mais sobre as ferramentas que facilitam os testes do Windows no modo S**

Confira [Desempacotar, modificar, reempacotar e assinar um appx](https://blogs.msdn.microsoft.com/appconsult/2017/08/07/unpack-modify-repack-sign-appx/).
