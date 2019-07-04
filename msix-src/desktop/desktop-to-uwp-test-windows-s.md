---
Description: Teste seu aplicativo para Windows 10 no modo S.
title: Testar seu aplicativo do Windows para o Windows 10 S
ms.date: 05/11/2017
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: Windows 10 S, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: 63803681601e03392273a1606f77c49d17fe11e8
ms.sourcegitcommit: 52010495873758d9bfe7a9fb0b240108b25b3d3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555596"
---
# <a name="test-your-windows-app-for-windows-10-in-s-mode"></a>Testar seu aplicativo do Windows para o Windows 10 no modo S

Você pode testar seu aplicativo do Windows para garantir que ele funcione corretamente em dispositivos que executam o Windows 10 no modo S. Na verdade, se planeja publicar o aplicativo na Microsoft Store, você deve fazer isso, pois é um requisito da Store. Para testar seu aplicativo, você pode aplicar uma política de integridade de código do Device Guard em um dispositivo que estiver executando o Windows 10 Pro.

> [!NOTE]
> O dispositivo no qual você aplica a política de integridade de código do Device Guard deve estar executando o Windows 10 Creators Edition (10.0; compilação 15063) ou posterior.

A política de integridade de código do Device Guard impõe as regras com as quais os aplicativos devem estar de acordo para serem executados no Windows 10 S.

> [!IMPORTANT]
>Recomendamos que você aplique essas políticas a uma máquina virtual, mas se você quiser aplicá-las à sua máquina local, certifique-se de revisar nossa orientação sobre as melhores práticas na seção "Próximo, instalar a política e reiniciar o sistema" deste tópico antes de aplicar uma política.

<a id="choose-policy" />

## <a name="first-download-the-policies-and-then-choose-one"></a>Primeiro, baixe as políticas e escolha uma

Baixe as políticas de integridade de código do Device Guard [aqui](https://go.microsoft.com/fwlink/?linkid=849018).

Em seguida, escolha o que faz mais sentido para você. Aqui está o resumo de cada política.

|Política |Imposição |Certificado de autenticação |Nome do Arquivo |
|--|--|--|--|
|Política do modo de auditoria |Registra problemas / não bloqueia |Loja |SiPolicy_Audit.p7b |
|Política de modo de produção |Sim |Loja |SiPolicy_Enforced.p7b |
|Política de modo de produto com aplicativos auto-assinados |Sim |Certificado de teste do AppX  |SiPolicy_DevModeEx_Enforced.p7b |

Recomendamos que você comece com a política de modo de auditoria. Você pode revisar os registros de eventos de integridade do código e usar essas informações para ajudá-lo a fazer ajustes em seu aplicativo. Em seguida, aplique a política de modo de produção quando estiver pronto para testes finais.

Aqui está um pouco mais informações sobre cada política.

### <a name="audit-mode-policy"></a>Política do modo de auditoria
Com este modo, seu aplicativo é executado, mesmo que ele execute tarefas que não são suportadas no Windows 10 S. O Windows registra todos os executáveis que teriam sido bloqueados nos Registros de eventos de integridade do código.

Você pode encontrar esses logs, abrindo o **Visualizador de eventos**e, em seguida, navegando até este local: Logs de aplicativos e serviços -> Microsoft -> Windows -> CodeIntegrity -> operacionais.

![code-integrity-event-logs](images/code-integrity-logs.png)

Esse modo é seguro e não impede que o sistema seja inicializado.

#### <a name="optional-find-specific-failure-points-in-the-call-stack"></a>(Opcional) Encontrar pontos de falha específicos na pilha de chamadas
Para encontrar pontos específicos na pilha de chamadas onde ocorrem problemas de bloqueio, adicione esta chave do Registro e depois [configure um ambiente de depuração do modo kernel](https://docs.microsoft.com/windows-hardware/drivers/debugger/getting-started-with-windbg--kernel-mode-#span-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanset-up-a-kernel-mode-debugging).

|Chave|Nome|type|Valor|
|--|---|--|--|
|HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\CI| DebugFlags |REG_DWORD | 1 |


![reg-setting](images/ci-debug-setting.png)

### <a name="production-mode-policy"></a>Política de modo de produção
Esta política impõe as regras de integridade do código que combinam o Windows 10 S para que você possa simular a execução no Windows 10 S. Essa é a política mais rigorosa e é excelente para o teste de produção final. Neste modo, seu aplicativo está sujeito às mesmas restrições que estarão sujeitas no dispositivo de um usuário. Para usar este modo, seu aplicativo deve ser assinado pela Microsoft Store.

### <a name="production-mode-policy-with-self-signed-apps"></a>Política de modo de produção com aplicativos autoassinados
Este modo é semelhante à política de modo de produção, mas também permite que as coisas sejam executadas que são assinadas com o certificado de teste incluído no arquivo zip. Instale o arquivo PFX incluído na pasta **AppxTestRootAgency** desse arquivo zip. Em seguida, assine seu app com ele. Dessa forma, você pode iterar rapidamente sem exigir a assinatura da loja.

Como o nome do publicador do seu certificado deve corresponder ao nome do fornecedor do seu app, você precisará alterar temporariamente o valor do atributo **Publisher** do elemento **Identity** para "CN=Appx Test Root Agency Ex". Você pode alterar esse atributo para seu valor original depois de concluir os testes.

## <a name="next-install-the-policy-and-restart-your-system"></a>Em seguida, instale a política e reinicie o sistema

Recomendamos que você aplique essas políticas a uma máquina virtual porque essas políticas podem levar a falhas de inicialização. Isso ocorre porque essas políticas bloqueiam a execução do código que não foi assinado pela Microsoft Store, incluindo drivers.

Se você deseja aplicar essas políticas à sua máquina local, é melhor começar com a política de Modo de auditoria. Com esta política, você pode revisar os registros de eventos de integridade do código para garantir que nada crítico seja bloqueado em uma política forçada.

Quando você estiver pronto para aplicar uma política, localize o. Arquivo p7b para a política que você escolheu, renomeie-o para **SIPolicy.P7B**e, em seguida, salve esse arquivo para esse local em seu sistema: **C:\Windows\System32\CodeIntegrity\\** .

Em seguida, reinicie o sistema.

>[!NOTE]
>Para remover uma política do seu sistema, exclua o arquivo .P7B e, em seguida, reinicie o sistema.

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Examine um artigo de blog detalhadas que foi lançado pela nossa equipe de aplicativo, consulte**

Consulte [Como portar e testar seus aplicativos da área de trabalho clássicos no Windows 10 S com a Ponte de Desktop](https://blogs.msdn.microsoft.com/appconsult/2017/06/15/porting-and-testing-your-classic-desktop-applications-on-windows-10-s-with-the-desktop-bridge/).

**Saiba mais sobre as ferramentas que facilitam o teste para Windows no modo S**

Consulte [Descompactar, modificar, reempacotar, assinar um APPX](https://blogs.msdn.microsoft.com/appconsult/2017/08/07/unpack-modify-repack-sign-appx/).
