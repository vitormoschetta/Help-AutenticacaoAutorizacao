# Conceitos de Autenticação e Autorização

### JWT vs Cookies (Correto: JWT vs Session) - História

Diferente do _LocalStorage_ e _SessionStorage_, _Cookies_ podem ser armazenados por tempo indeterminado, permanecendo salvo mesmo ao fechar o browser. Graças aos Cookies podemos salvar nossas crendencias no navegador, e abrir nossas redes sociais no dia seguinte sem precisar informar o login novamente.

Como podemos observar, **Cookies nao estão diretamente ligados à autenticação**, mas na persistência de informações no browser. Acontece que essa capacidade de persistência duradoura do Cookie o associou à **autenticação baseada em Session**. 

Neste formato de autenticação os **dados do usuário NÃO estão no Cookie, mas em uma Session na memória do Server**. Esta session é criada no servidor com um ID de identificação, e é justamente este ID que é passado para o _Client_ persistir no _Cookie/browser_. Logo **o Cookie possui apenas o ID da Session que contém os dados do Usuário autenticado.**

Obs: Um _Cookie_ pode ser definido no _Client_ ou no _Server_, e quando definido no _Server_ é utilizado uma segurança adicional (_HttpOnly_) que os torna visíveis apenas para servidores, e não para o javascript do lado do cliente. Este é mais voltado para autenticação.

Perceba que na autenticação com _Cookie/Session_ é necessário manter um estado na memória do Servidor. Por isso usamos o termo **autenticação com estado**.

Dentro do escopo de autenticação, a função do Cookie é similar ao JWT, porém, possuem casos de uso diferente.

Hoje em dia temos requisitos diferentes como aplicativos híbridos, SPA e Api's. Que podem depender de vários back-ends (divididos em servidores de autenticação de micro-services, bancos de dados, servidores de processamento de imagem, etc.). Nestes tipos de cenários mais elaborados, o cookie vai ser uma má decisão, pois a sessão que obtemos de um servidor não corresponde a do outro servidor.

Por este motivo foi necessário o desenvolvimento de um outro método de autenticação, **sem estado**, onde os dados do usuário pudessem trafegar em cada solicitação ao invés de ser mantida na memória do servidor, afinal, essa aplicação precisará "conversar" com diversos servidores.

A grande questão aqui é: Como saber que este Token que trafega com os dados do Usuário via HTTP é válido? Como os Servidores poderiam validar esta solicitação? 

É aí que entra o **Secret**, a Chave. O JWT é montado a partir das informações/claims do Usuário (_Payload_) + sequência de caracteres conhecidos como Secret/Chave (_Signature_). Qualquer um pode decifrar _Paylod_ (corpo) de um JWT, que é a parte  que contém as informações do usuário. Mas, somente quem possui a Chave pode validar a assintura desse JWT. 

Dá-se o início do  **OpenId Connect**, que é a capacidade de permitir que os usuários sejam autenticados por sites cooperantes (conhecidos como partes confiáveis ou RP) usando um serviço de autorização de terceiros.  

Outro conceito que passou a ser muito usado foi o **Federation Gateway**, que é exatamente a capacidade de se efetuar o login através de 
um provedor externo, como por exemplo, logar com suas redes sociais ou conta do Google.

Mas é aí também que entramos em outra discussão de segurança: Se para validar um JWT precisamos da _Chave/Segredo_, isso quer dizer que precisamos compartilhar um Segredo de Servidor com vários Clients. Como podemos garantir que esses clientes estão armazenando a chave de forma segura? 

Em resposta a essa questão foi criado um segundo algoritimo de autenticação  para JWT onde existem duas chaves: uma chave Privada que fica no servidor de autenticação, e só ela pode gerar um Token. E uma chave Pública que é concedida aos Clientes. Esta segunda só serve para Validar os Tokens, logo, não há problema se ela for exposta.

Logo, o JWT utiliza dois algoritmos básicos de criptografia para a Chave/Segredo: 

1. **HS256**: Algoritmo que utiliza a mesma chave tanto para gerar quanto para validar a assinatura de um JWT.

2. **RS256**: Algoritmo que utiliza um par de chaves: Chave privada para gerar a assinatura do JWT, e Chave publica para validar a assinatura do JWT.

Outra pergunga: E se um JWT é roubado, então o ladrão pode continuar usando o JWT até a sua experiação?
R: Sim, alguns usam a estratégia de tempos curtos para expiração para minimizar esse problema.

Outra questão a ser observada é: De onde DEVEM vir os _requests_/solicitações? Quando só podemos aceitar solicitações de URLs específicas, fica mais fácil evitar esse tipo de roubo e uso indevido de JWTs usando CORS.



### Single Sign-On (SSO)
Login integrado. O SSO dá acesso a muitos aplicativos inserindo credenciais apenas uma vez.
Em um cenário interno, corporativo, isso é facilmente possível utilizando Cookie e Session. Uma vez que as Sessões podem se extender à Subdomínios.
<https://medium.com/swlh/cookie-recipes-for-your-single-sign-on-sso-authentication-server-e1ddfc407e71>


E para domínios diferentes?  
Aí entra o tão falado OAuth.


Nos links abaixo ainda encontramos formas de cobinar Cookies e JWT:  
<https://stackoverflow.com/questions/40527124/single-sign-on-sso-using-jwt>
<https://github.com/Aralink/ssojwt>






###### Outras Referências:

<https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Cookies>