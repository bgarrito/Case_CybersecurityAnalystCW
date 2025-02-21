O dataset contém 30.000 registros e 15 colunas, relacionadas a requisições HTTP de clientes entre os dias 05/11/2024 e 14/11/2024. As colunas para análise de segurança incluem:

ClientIP - IP de origem da requisição
ClientASN -  Autonomous System Number do IP de origem
ClientCountry - Pais de origem
ClientDeviceType - Tipo de dispositivo da origem

ClientRequestBytes - tamanho da request

ClientRequestHost - host requisitado

ClientRequestMethod - metodo utilizado na requisição

ClientRequestPath - path solicitado

ClientRequestReferer - pagina de referencia

ClientRequestScheme - scheme http/https

ClientRequestURI - uri solicitada

ClientRequestUserAgent - UA utilizado na requisição

ClientSrcPort - porta da origem

EdgeStartTimestamp - horario da requisição

ZoneName - zona de destino infinitepay.io


Com o conhecimento dos dados disponiveis, foi criado um painel para entendimento de padroes e anomalias volumetricas em um SIEM.
![image](https://github.com/user-attachments/assets/832cd6ff-05ee-42a8-9e4c-8e5f8db54de6)

A primeira anomalia visual identificada foi a quantidade de IPs de paises como indonesia e US. Tais origens são reconhecidos por hospedagens com conteudo malicioso, indicando possivel tentativa de exploração dessas origens.

Na analise de volume de requests por IP sem os paises US e IN, temos um volume medio de 1 requests por IP, destoando dos volumes vistos por IPs dos paises IN e US.

![image](https://github.com/user-attachments/assets/4c66ed80-0a2f-4163-b0b1-5252defa09f7)

Tais alterações se aplicam tambem em quantidade de paths requisitados por IP e diferentes paths requisitados por hosts em buscas com e sem paises US e IN.

Avaliando as requisições, foram identificadas requisições com caracteristicas de ataque XSS, SQLI e directory transversal.

exemplos de path:
/../../../windows/win.ini

/<marquee><img src=1 onerror=alert(1)></marquee>

/%00%01%02%03%04%05%06%07

/api/v1/users?search=; DROP TABLE users;--


Foi identificado requisições maliciosas partindo de IPs com baixo volume de eventos com as mesmas caracteristicas.

Verificado que não existe padrao de ASN ofensiva gerando requisições maliciosas.

Pela falta de conhecimento do ambiente e dos usuarios, não é possivel afirmar que existe algum grupo de origem malicioso, visto que existem redes validas gerando acesso.

As requisiçoes sugerem uma possivel injeção de cabeçalho host em todas as requisições baseado no ZoneName, porem a falta de conhecimento do ambiente também impossibilita afirmar.


