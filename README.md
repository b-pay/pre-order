## Documentação integração com BPay

Antes de uma transação ser realizada através do BPay é necessário a criação de um token pelo ambiente server side da loja. 

Pontos importantes que devem ser observados:
* A solicitação do token deve ser realizada pela página de checkout da loja, página onde o cliente conclui todo o processo de carrinho de compra;
* A identificação do pedido na loja deve ser único e enviado na solicitação de criação do token, campo OrderReference;
* Um token tem tempo de expiração padrão de 30 minutos.
* O token é criado através de um POST para o recurso 'pre-order';
* A API do Mundi Checkout está disponível no seguinte endereço:
   * Produção: 
   * Homologação: http://bpay-node.cloudapp.net/pre-order

Conteúdo da documentação:

* [Autenticação](https://github.com/b-pay/pre-order/blob/master/README.md#autenticação)
* [Referência de campos para criação de um token](https://github.com/b-pay/pre-order/blob/master/README.md#referência-de-campos-para-criação-de-um-token)
* [Json de exemplo para criação de token](https://github.com/b-pay/pre-order/blob/master/README.md#json-de-exemplo-para-criação-de-token)
* [Referência de campos da resposta da criação de um token](https://github.com/b-pay/pre-order/blob/master/README.md#referência-de-campos-da-resposta-da-criação-de-um-token)
* [Json de exemplo com resposta de criação de token](https://github.com/b-pay/pre-order/blob/master/README.md#json-de-exemplo-com-resposta-de-criação-de-token)
* [Redirecionamento para página de checkout do bPay](https://github.com/b-pay/pre-order/blob/master/README.md#redirecionamento-para-página-de-checkout-do-bpay)
* [Post de notificação](https://github.com/b-pay/pre-order/blob/master/README.md#post-de-notificação)
* [Operação de consulta](https://github.com/b-pay/pre-order/blob/master/README.md#pperação-de-consulta)

## Autenticação

No header da requisição as chaves/valores abaixo devem ser informados:

* Content-Type: application/json
* Authorization: Basic (usuário e senha codificados em Base64)

### Como criar Header Authorization
1. Concatenar usuário e senha e uma string. Por exemplo, "usuário:senha"
2. Converter string para Base64
3. Adicionar string em Base64 ao header Authorization

Por exemplo, "Authorization: Basic ZnJlZDpmcmVk"

## Referência de campos para criação de um token

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| SellerKey  | UUID | Chave para identificar o sistema que está enviando a requisição. Exemplo: "b733ccec-8099-11e5-8bcf-feff819cdc9f"    |Sim|
| Buyer | [Buyer](https://github.com/b-pay/pre-order/blob/master/README.md#buyer) | Objeto com os dados do comprador    |Sim|
| Order | [Order](https://github.com/b-pay/pre-order/blob/master/README.md#order) | Objeto com os dados do pedido    |Sim|
| Shipping | [Shipping](https://github.com/b-pay/pre-order/blob/master/README.md#shipping) | Objeto com os dados de entrega    |Sim|
| Payment | [Payment](https://github.com/b-pay/pre-order/blob/master/README.md#payment) | Objeto com as opções do pagamento   |Sim|
| Options | [Options](https://github.com/b-pay/pre-order/blob/master/README.md#options) | Objeto com as opções gerais   |Sim|

## Buyer

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| DocumentNumber | String | Número do documento do comprador| Sim |
| PersonType | String | Tipo de pessoa. Valores possíveis - 'Person' ou 'Company' | Sim |
| Name | String | Nome do comprador com no máximo 64 caracteres | Sim |
| Email | String | Endereço de e-mail do comprador com no máximo 64 caracteres | Sim |
| Gender | String | Sexo do comprador. Valores possíveis - 'Male' ou 'Female' | Sim |
| Birthday | String | Data de aniversário do comprador. Formato aceito yyyy-mm-dd | Não |
| HomePhone | String | Telefone residencial do comprador. Ex: 552123011822 | Sim |
| MobilePhone | String | Telefone celular do comprador. Ex: 552197533011 | Sim |
| WorkPhone | String | Telefone comercial do comprador. Ex: 552123011822 | Não |
| BillingAddress | [BillingAddress](https://github.com/b-pay/pre-order/blob/master/README.md#billingaddress) | Objeto com os dados do endereço de cobrança |Sim|

## BillingAddress

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| Street | String | Nome da rua. Máximo de 64 caracteres | Sim |
| Number | String | Número | Sim |
| ZipCode | String | CEP (sem traço) | Sim |
| Complement | String | Complemento. Máximo de 64 caracteres | Não |
| District | String | Bairro. Máximo 64 caracteres | Sim |
| City | String | Cidade. Máximo de 64 caracteres | Sim |
| StateName | String | Estado. Máximo de 32 caracteres | Sim |
| Country | String | País. Máximo de 32 caracteres | Sim |

## Order
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| OrderDescription | String | Descrição do pedido. Máximo de 256 caracteres | Não |
| OrderReference | String | Identificador do pedido na loja. Máximo de 56 caracteres | Não |
| AmountInCents | Integer | Valor do pedido | Sim |
| Items | Array de [Item](https://github.com/b-pay/pre-order/blob/master/README.md#item) | Coleção com itens do carrinho de compras | Sim |

## Item
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| SKU | String | Identificador do produto | Não |
| Name | String | Nome do produto | Sim |
| Category | String | Categoria do produto | Não |
| PriceInCents | Integer | Valor total do item no carrinho | Sim |
| UnitPriceInCents | Integer | Valor unitário do item no carrinho | Sim |
| Quantity | Integer | Quantidade do item | Sim |

## Shipping
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| CostInCents | Integer | Valor do frete | Não |
ShippingAddress | [ShippingAddress](https://github.com/b-pay/pre-order/blob/master/README.md#shippingaddress) | Objeto com dados do endereço de entrega | Sim |

## ShippingAddress
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| Street | String | Nome da rua. Máximo de 64 caracteres | Sim |
| Number | String | Número | Sim |
| ZipCode | String | CEP (sem traço) | Sim |
| Complement | String | Complemento. Máximo de 64 caracteres | Não |
| District | String | Bairro. Máximo 64 caracteres | Sim |
| City | String | Cidade. Máximo de 64 caracteres | Sim |
| StateName | String | Estado. Máximo de 32 caracteres | Sim |
| Country | String | País. Máximo de 32 caracteres | Sim |

## Payment
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| OperationType | String | Tipo da transação a ser realizada. Valores possíveis: 'AuthorizeAndCapture', 'Authorize' | Sim |
| Currency | String | Moeda da transação. Valor aceito - 'BRL' | Sim |
| Installments | Array de [Installment](https://github.com/b-pay/pre-order/blob/master/README.md#installment) | Parcelas disponíveis para pagamento | Sim |

## Installment
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| Number | String | Número da parcela | Sim |
| Text | String | Texto da parcela. Por exemplo, "1x de 100,00 sem juros" | Sim |
| AmountInCents | Integer | Valor da parcela | Sim |

## Options
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| SuccessUrl | String | Url de sucesso. Está página será exibida caso o pagamento seja processado com sucesso. | Sim |
| FailureUrl | String | Url de falha. Está página será exibida caso o pagamento não seja processado. | Sim |
| NotificationUrl | String | Url de notificação de status. O status atual da transação será enviado para esta url | Sim |

## Json de exemplo para criação de token

```json
{
   "SellerKey": "8141BB01-EE19-4081-A4A4-97455505940E",
   "Buyer":{
      "DocumentNumber":"12458253725",
      "PersonType": "Person",
      "Name":"Carlos Peçanha",
      "Email":"cpecanhamundipagg.com",
      "Gender":"Male",
      "Birthday":"1988-08-02",
      "HomePhone":"2123011826",
      "MobilePhone":"21975338448",
      "WorkPhone":null,
      "BillingAddress":{
         "Street":"Rua da Quitanda",
         "Number":199,
         "ZipCode":"20091005",
         "Complement":"Décimo andar",
         "District":"Centro",
         "City":"Rio de Janeiro",
         "StateName":"Rio de Janeiro",
         "Country":"Brasil"
      }
   },
   "Order":{
      "OrderDescription": "Whey Protein Isolado",
      "OrderReference":"Meu pedido",
      "AmountInCents":9999,
      "Items": [
          {
              "SKU": "123",
              "Name":"Whey Protein Isolado",
              "Category":"Suplementos",
              "PriceInCents":100,
              "UnitPriceInCents":10,
              "Quantity":10
          }
      ],
    },
    "Shipping": {
        "CostInCents": 10,
        "Address":{
            "Street":"Rua da Quitanda",
            "Number":199,
            "ZipCode":"20091005",
            "Complement":"Décimo andar",
            "District":"Centro",
            "City":"Rio de Janeiro",
            "StateName":"Rio de Janeiro",
            "Country":"Brasil"
        }
    },
    "Payment": {
        "OperationType": "AuthorizeAndCapture",
        "Currency": "BRL",
        "Installments": [
            { 
                "Number": 1,
                "Text": "1x de 100,00 sem juros",
                "AmountInCents": 100
            }
        ]
    },
    "Options": {
        "SuccessUrl": "https://obrigado.com",
        "FailureUrl": "https://falhou.com",
        "NotificationUrl": "htts://status.com"
    }
}
```

## Referência de campos da resposta da criação de um token

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| Token | UUID | Token de identificação do pedido | Sim |
| ExpiresIn | Integer | Tempo para expiração do Token em <a href="http://www.epochconverter.com/" target="_blank">Epoch Time</a> | Sim |

## Json de exemplo com resposta de criação de token

```json
{
   "Token":"71861931-9F71-4577-82D8-4F68AC2AEA17",
   "ExpiresIn":1446495735
}
```

## Redirecionamento para página de checkout do bPay

http://bpay-checkout.azurewebsites.net/get-checkout?id={TOKEN}

## Post de notificação

Após o processamento de uma transação pelo BPay, um POST de notificação contendo os dados da transação será enviado no formato JSON para a loja. Para isto, é necessário que a loja informe, no momento de criação do token, uma URL que irá receber e interpretar estes dados, campo Option.NotificationUrl.

**É importante que a página que irá receber e interpretar o POST de notificação esteja preparada para receber novos campos além daqueles descritos no manual, para que possamos atualizar e acrescentar campos conforme necessário, de forma a sempre notificar a loja com as informações mais completas sobre suas transações.**

O BPay efetuará três tentativas de notificação com intervalos de aproximadamente 15 minutos entre cada tentativa. Caso ocorra insucesso, o recurso de consulta, Query, está disponível na API para que a loja possa consultar os dados do pedido a qualquer momento. Para que o serviço de notificação do BPay entenda que houve sucesso na notificação, a URL da loja deve responder somente com o texto "OK" no corpo da mensagem de resposta.

Detalhes dos atributos do objeto Notification:

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| SellerKey | UUID | Identificador da loja que realizou a transação | Sim |
| Acquirer | String | Adquirente onde a transação foi processada | Sim |
| TransactionReference | String | Identificador da transação na loja | Não |
| TransactionKey | UUID | Identificador da transação no BPay | Sim |
| TransactionIdentifier | String | Identificador da transação na adquirente | Sim |
| UniqueSequencialNumber | String | Identificador único da transação na adquirente | Sim |
| AuthorizationCode | String | Código de autorização na adquirente | Sim |
| AmountInCents | Integer | Valor da transação em centavos | Sim |
| PreviousTransactionStatus | String | Status prévio da transação no BPay | Sim |
| CurrentTransactionStatus | String | Status atual da transação no BPay | Sim |
| CreateDate | String | Data de criação da transação. Formato yyyy-mm-ddThh:mm:ss | Sim |
| LastChangeDate | String | Data da última alteração da de status da transação. Formato yyyy-mm-ddThh:mm:ss | Sim |

## Operação de consulta
