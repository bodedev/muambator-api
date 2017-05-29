# Muambator - API (V1)

O projeto Muambator tem como objetivo gerenciar e monitorar os pacotes de encomendas junto as operadoras logísticas. Toda vez que detectarmos uma alteração no tracking desses pacotes, enviamos uma notificação para os usuários cadastrados.

## O que é API?

API (Application Program Interface) é um conjunto de protocolos e interfaces que permitem que softwares "conversem entre si". Por exemplo, através de uma API é possível que um sistema diferente do Facebook possa fazer um post numa timeline, ou até mesmo alterar sua foto de perfil. Atualmente diversos sistemas expõem suas funcionalidades através de API, incluindo Instagram, Gmail, Spotify, Slack, Twitter, Google Drive, Dropbox… Enfim, hoje praticamente qualquer sistema consegue interagir com qualquer outro.

## Tá, e o Muambator suporta esse "esquema"?

Sim! O Muambator suporta a integração com outros sistemas através de uma API. E SIMMMMMM! Você consegue integrar aquele seu super sistema de comércio eletrônico.

## Por favor, não leia abaixo se você não é desenvolvedor. Será uma perda de tempo. É Oficial.

## Qual o formato de troca de mensagens?

REST & [JSON](https://www.youtube.com/watch?v=q1RYs9BTkBg).

## E por quê vocês não suportam XML?

Mais uma foca acaba de morrer. E para cada vez que esse formato é apresentado, morre um tigre de bengala. É oficial, não vamos integrar nunca com essa entronha. Mas se você precisar muito, envie um [e-mail](aindaestounosanos90@nuncameenganou.com). Mas se você quiser continuar contestando, Phillip Morris apresenta melhor como você anda defasado através deste [post](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#json-responses).

## É "de grátis"?

Não. Ainda não existe almoço grátis, e alguém tem que ajudar a pagar nosso Ela é oferecida apenas para clientes com um volume considerável de pacotes. Ah mas vocês querendo ficar ricos cobrando de todo mundo por um simples acesso! Se você ainda não está convencido, [vem conosco nesse post](https://medium.com/muambator/vamos-falar-sobre-sustentabilidade-b81d75f89284).

## Putz, mas pelo menos é fácil fazer uma integração?

Claro serhumaninho. É bem simples mesmo. Abaixo apresentamos um código de exemplo para adicionar um pacote através de um script escrito em Python. [Uma série de scripts está disponível aqui.](https://github.com/bodedev/muambator-client-python). 

```python
# -*- encoding: utf-8 -*-


import requests


if __name__ == "__main__":
    host = "www.muambator.com.br"
    api_token = "insira-seu-token-aqui"
    codigo_pacote = "PL970650197BR"
    r = requests.post('http://%s/api/clientes/v1/pacotes/%s/?api-token=%s' % (host, codigo_pacote, api_token), headers={"Content-type": "application/json"})
    print r.status_code
    print r.text
```

## Uata porra is dati!?

O código acima é apenas um exemplo. Sim, sabemos que está bem simples, mas é a maneira mais simples que podemos fazer. (E não vem dizer que em Java dá pra fazer com menos linhas que isso hein!)

O código acima basicamente irá apresentar a seguinte reposta:

```
201
{"status":"OK","message":"","results":{"PL970650197BR":{"pais_origem":"Brasil","tributado":false,"icone":"","nome":"","extraviado_ou_devolvido":false,"tags":[],"entregue":true,"sigla_pais_origem":"br","tracking":[{"local":"CEE NORTE - Porto Alegre/RS","map":"Porto Alegre, RS, Brasil","datahora":"08/05/2017 16:18","situacao":"Entrega Efetuada","icone":"green"},{"local":"CEE NORTE - Porto Alegre/RS","map":"Porto Alegre, RS, Brasil","datahora":"08/05/2017 10:03","situacao":"Objeto saiu para entrega ao destinat\u00e1rio","icone":"blue"},{"local":"CTE PORTO ALEGRE - PORTO ALEGRE/RS Objeto encaminhado","map":"PORTO ALEGRE, RS, Brasil","datahora":"05/05/2017 04:13","situacao":"Em tr\u00e2nsito para CEE NORTE - Porto Alegre/RS","icone":"orange"},{"local":"CTE CURITIBA - CURITIBA/PR Objeto encaminhado","map":"CURITIBA, PR, Brasil","datahora":"30/04/2017 12:26","situacao":"Em tr\u00e2nsito para CTE PORTO ALEGRE - PORTO ALEGRE/RS","icone":"orange"},{"local":"AGF SAO CAMILO - Pinhais/PR Objeto encaminhado","map":"Pinhais, PR, Brasil","datahora":"27/04/2017 10:20","situacao":"Em tr\u00e2nsito para CTE CURITIBA - CURITIBA/PR","icone":"orange"},{"local":"AGF SAO CAMILO - Pinhais/PR","map":"Pinhais, PR, Brasil","datahora":"26/04/2017 14:05","situacao":"Objeto postado ap\u00f3s o hor\u00e1rio limite da ag\u00eancia","icone":"yellow"}],"arquivado":false,"marcado_como_entregue":false,"numero_dias":12,"carrier":"correios","numero_dias_uteis":7,"codigo":"PL970650197BR","ultimo_tracking":{"local":"CEE NORTE - Porto Alegre/RS","map":"Porto Alegre, RS, Brasil","datahora":"08/05/2017 16:18","situacao":"Entrega Efetuada","icone":"green"}}}}
```

O 201 é o código HTTP para informar que um objeto foi criado. [Clique aqui](http://www.restpatterns.org/HTTP_Status_Codes/201_-_Created) para maiores informações.

O restante do código é o resultado da execução serializado em JSON. Todas as requisições de API do Muambator são apresentados seguindo o seguinte schema:

```
{
  "status": "",
  "message": "",
  "results": null
}
```

O padrão acima apresenta o resultado da chamada do nosso servidor. O atributo ```status``` sempre será ```OK``` ou ```ERROR```. No campo ```message```, apresentamos uma mensagem para informar o problema relatado na execução, e atualmente utilizamos apenas para informar o motivo do erro (quando o status for ```ERROR```). No campo ```results```, apresentamos o resultado. Quando tiver qualquer problema, o campo ```results``` sempre será apresentado como ```null```.

## Já sei! Vou fazer um teste aqui e vou executar esse código várias vezes!

Chablaw! Toma essa resposta!

```
// Execução 1
409
{"status":"ERROR","message":"Este pacote j\u00e1 foi adicionado nesta conta!","results":null}
// Execução 2
409
{"status":"ERROR","message":"Este pacote j\u00e1 foi adicionado nesta conta!","results":null}
// Execução 3
409
{"status":"ERROR","message":"Este pacote j\u00e1 foi adicionado nesta conta!","results":null}
// Execução N
409
{"status":"ERROR","message":"Este pacote j\u00e1 foi adicionado nesta conta!","results":null}
```

O nosso sistema não permite que mais um pacote com o mesmo código seja adicionado ao mesmo usuário.

## Hummmm. E como eu posso fazer para excluir um pacote da minha conta?

*Show me the code little grasshopper.*

```python
# -*- encoding: utf-8 -*-


import requests


if __name__ == "__main__":
    host = "www.muambator.com.br"
    api_token = "insira-seu-token-aqui"
    codigo_pacote = "PL970650197BR"
    r = requests.delete('http://%s/api/clientes/v1/pacotes/%s/?api-token=%s' % (host, codigo_pacote, api_token), headers={"Content-type": "application/json"})
    print r.status_code
    print r.text
```

Neste caso, a execução acima apresentará o ```status code``` como ```204``` (No Content) e não trará nada no corpo. Simples né não?

## Agora vamos ao resumo executivo para pessoas realmente ocupadas

Neste parágrafo é pra você apresentar naquela reunião que marcaram para falar 2 horas sobre a possível integração com o [Muambator](http://www.muambator.com.br):

URL do endpoint: http://www.muambator.com.br/api/clientes/v1/

### Chamada para adicionar um pacote

URL: http://www.muambator.com.br/api/clientes/v1/pacotes/[codigo do pacote aqui]/

Método: POST

Content-Type: application/json

#### Quais parâmetros eu posso repassar para a criação do pacote?

| Nome do Parâmetro     | Tipo   | Tamanho Máximo | Valor Padrão | Observação                               |
| --------------------- | ------ | -------------- | ------------ | ---------------------------------------- |
| nome                  | string | 250 bytes      | null         |                                          |
| tags                  | list   | 250 bytes      | []           |                                          |
| categoria             | string | 30             | null      | [Qualquer id válido de categoria](#chamada-para-buscar-as-categorias) |
| cep_origem            | string | 10             | null         | Formato: 99.999-999                      |
| cep_destino           | string | 19             | null         | Formato: 99.999-999                      |
| data_previsao_entrega | date   | 10             | null         | Formato: yyyy-mm-dd                      |
| valor                 | float  |                | 0            |                                          |
(#categorias)
### Chamada para remover um pacote 

URL: http://www.muambator.com.br/api/clientes/v1/pacotes/[codigo do pacote aqui]/

Método: DELETE

Content-Type: application/json

### Chamada para buscar as categorias 

URL: http://www.muambator.com.br/api/clientes/v1/categorias/

Método: GET

### Listagem de pacotes

#### Todos os pacotes vinculados na conta

URL: http://www.muambator.com.br/api/clientes/v1/pacotes/

Método: GET

#### Todos os pacotes pentendes (que ainda não foram entregues) na conta

URL: http://www.muambator.com.br/api/clientes/v1/pacotes/pendentes/

Método: GET

#### Todos os pacotes entregues na conta

URL: http://www.muambator.com.br/api/clientes/v1/pacotes/entregues/

Método: GET

#### Todos os pacotes arquivados (que já foram entregues e foram "arquivados") na conta

URL: http://www.muambator.com.br/api/clientes/v1/pacotes/arquivados/

Método: GET

#### Todos os pacotes tributados (aqueles mesmo, que você pensou que chegariam suave na nave e você levou o talagaço) na conta

URL: http://www.muambator.com.br/api/clientes/v1/pacotes/tributados/

Método: GET

#### Todos os pacotes atrasados na conta (aqueles que ainda não foram entregues e possuem o atributo `data_previsao_chegada` para uma data anterior ao dia/hora atual)

URL: http://www.muambator.com.br/api/clientes/v1/pacotes/atrasados/

Método: GET
