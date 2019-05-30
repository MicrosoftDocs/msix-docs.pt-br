# <a name="support-for-desktop-installers-that-require-device-restart"></a>Suporte para instaladores da área de trabalho que exigem a reinicialização do dispositivo

 > [!NOTE] 
 > Reinicialização do dispositivo é um novo recurso que só está disponível no build de versão prévia do insider.
 > Você pode ingressar no programa de visualização MSIX empacotamento ferramenta Insider para obter acesso a esse recurso.

Agora, damos suporte a capacidade de reiniciar durante o processo de conversão usando a ferramenta de empacotamento MSIX! 

Reinicializações têm suporte por meio de nossa interface do usuário interativo e por meio de nossa Interface de linha de comando. Além disso, por meio de nossa linha de comando, você pode também especificar que você deseja para logon automático após a reinicialização tem uma conversão direta de mãos livres. 

Você pode especificar quais códigos de saída indicam uma reinicialização na **página de configurações (códigos de saída do instalador)** . 

Você pode disparar uma reinicialização por meio da interface do usuário durante o fluxo de trabalho de conversão ou por meio do menu Iniciar do computador de conversão. Após a reinicialização tenha ocorrido, você retornará para o mesmo local em que estavam na ferramenta para continuar com sua conversão.

Tenha em mente que esse recurso ainda está em desenvolvimento e pode haver alguns bugs. Se você encontrar algum, não se esqueça [arquivo comentários](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/insider-program#share-your-feedback) para que possamos corrigi-lo!

