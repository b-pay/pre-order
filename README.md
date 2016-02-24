## Documentação integração com B-PAY

Antes de uma transação ser realizada através do B-PAY é necessário a criação de um token pelo ambiente server side da loja. 

Pontos importantes que devem ser observados:
* A solicitação do token deve ser realizada pela página de checkout da loja, página onde o cliente conclui todo o processo de carrinho de compra;
* A identificação do pedido na loja deve ser único e enviado na solicitação de criação do token, campo orderReference;
* Um token tem tempo de expiração padrão de 30 minutos.
* O token é criado através de um POST para o recurso 'tokens';
* A API do B-PAY está disponível no seguinte endereço:
   * Produção: 
   * Homologação: https://checkout-api.meubpay.com.br

Conteúdo da documentação:

* [Autenticação](https://github.com/b-pay/pre-order/blob/master/README.md#autenticação)
* [Referência de campos para criação de um token](https://github.com/b-pay/pre-order/blob/master/README.md#referência-de-campos-para-criação-de-um-token)
* [Json de exemplo para criação de token](https://github.com/b-pay/pre-order/blob/master/README.md#json-de-exemplo-para-criação-de-token)
* [Referência de campos da resposta da criação de um token](https://github.com/b-pay/pre-order/blob/master/README.md#referência-de-campos-da-resposta-da-criação-de-um-token)
* [Json de exemplo com resposta de criação de token](https://github.com/b-pay/pre-order/blob/master/README.md#json-de-exemplo-com-resposta-de-criação-de-token)
* [Redirecionamento para página de checkout do B-PAY](https://github.com/b-pay/pre-order/blob/master/README.md#redirecionamento-para-página-de-checkout-do-B-PAY)
* [Post de notificação](https://github.com/b-pay/pre-order/blob/master/README.md#post-de-notificação)
* [Json de exemplo do Post de notificação](https://github.com/b-pay/pre-order/blob/master/README.md#json-de-exemplo-do-post-de-notificação)
* [Operação de consulta](https://github.com/b-pay/pre-order/blob/master/README.md#operação-de-consulta)
* [Referência de campos da resposta de uma operação de consulta](https://github.com/b-pay/pre-order/blob/master/README.md#referência-de-campos-da-resposta-da-criação-de-um-token)
* [Json de exemplo com resposta de uma operação de consulta](xxxx)

## Autenticação

No header da requisição as chaves/valores abaixo devem ser informados:

* Content-Type: application/json
* Authorization: Basic (usuário e senha codificados em Base64)

#### Como criar Header Authorization
1. Concatenar usuário e senha e uma string. Por exemplo, "usuário:senha"
2. Converter string para Base64
3. Adicionar string em Base64 ao header Authorization

Por exemplo, "Authorization: Basic ZnJlZDpmcmVk"

## Referência de campos para criação de um token

Para criar um token é necessário enviar um POST para o recurso /tokens.

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| SellerKey  | UUID | Identificador da loja que realizou a transação. Exemplo: "b733ccec-8099-11e5-8bcf-feff819cdc9f"    |Sim|
| Buyer | [Buyer](https://github.com/b-pay/pre-order/blob/master/README.md#buyer) | Objeto com os dados do comprador    |Sim|
| Order | [Order](https://github.com/b-pay/pre-order/blob/master/README.md#order) | Objeto com os dados do pedido    |Sim|
| Shipping | [Shipping](https://github.com/b-pay/pre-order/blob/master/README.md#shipping) | Objeto com os dados de entrega    |Sim|
| Payment | [Payment](https://github.com/b-pay/pre-order/blob/master/README.md#payment) | Objeto com as opções do pagamento   |Sim|
| Options | [Options](https://github.com/b-pay/pre-order/blob/master/README.md#options) | Objeto com as opções gerais   |Sim|

#### Buyer

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| DocumentNumber | String | Número do documento do comprador. Máximo 64 caracteres | Sim |
| PersonType | String | Tipo de pessoa. Valores possíveis - 'Person' ou 'Company' | Sim |
| Name | String | Nome do comprador. Máximo 64 caracteres | Sim |
| Email | String | Endereço de e-mail do comprador. Máximo 64 caracteres | Sim |
| Gender | String | Sexo do comprador. Valores possíveis - 'Male' ou 'Female' | Não |
| Birthday | String | Data de aniversário do comprador. Formato aceito yyyy-mm-dd | Não |
| HomePhone | String | Telefone residencial do comprador. Ex: 552123011822 | Não |
| MobilePhone | String | Telefone celular do comprador. Ex: 552197533011 | Não |
| WorkPhone | String | Telefone comercial do comprador. Ex: 552123011822 | Não |
| BillingAddress | [BillingAddress](https://github.com/b-pay/pre-order/blob/master/README.md#billingaddress) | Objeto com os dados do endereço de cobrança |Sim|

#### BillingAddress

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| Street | String | Nome da rua. Máximo de 64 caracteres | Sim |
| Number | Integer | Número | Sim |
| ZipCode | String | CEP (sem traço) | Sim |
| Complement | String | Complemento. Máximo de 64 caracteres | Não |
| District | String | Bairro. Máximo de 64 caracteres | Sim |
| City | String | Cidade. Máximo de 64 caracteres | Sim |
| StateName | String | Estado. Máximo de 32 caracteres | Sim |
| Country | String | País. Máximo de 32 caracteres | Sim |

#### Order
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| OrderReference | String | Identificador do pedido na loja. Máximo de 56 caracteres | Não |
| AmountInCents | Integer | Valor do pedido | Sim |
| Items | Array de [Item](https://github.com/b-pay/pre-order/blob/master/README.md#item) | Coleção com itens do carrinho de compras | Sim |

#### Item
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| Name | String | Nome do produto | Sim |
| Category | String | Categoria do produto | Não |
| PriceInCents | Integer | Valor total do item no carrinho | Sim |
| UnitPriceInCents | Integer | Valor unitário do item no carrinho | Sim |
| DiscountAmountInCents | Integer | Valor de desconto do item no carrinho | Não |
| Quantity | Integer | Quantidade do item | Sim |

#### Shipping
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| CostInCents | Integer | Valor do frete | Não |
Address | [ShippingAddress](https://github.com/b-pay/pre-order/blob/master/README.md#shippingaddress) | Objeto com dados do endereço de entrega | Sim |

#### ShippingAddress
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| Street | String | Nome da rua. Máximo de 64 caracteres | Sim |
| Number | Integer | Número | Não |
| ZipCode | String | CEP (sem traço) | Sim |
| Complement | String | Complemento. Máximo de 64 caracteres | Não |
| District | String | Bairro. Máximo 64 caracteres | Sim |
| City | String | Cidade. Máximo de 64 caracteres | Sim |
| StateName | String | Estado. Máximo de 32 caracteres | Sim |
| Country | String | País. Máximo de 32 caracteres | Sim |

#### Payment
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| OperationType | String | Tipo da transação a ser realizada. Valores possíveis: 'AuthorizeAndCapture', 'Authorize' | Sim |
| Currency | String | Moeda da transação. Valor aceito - 'BRL' | Sim |
| SoftDescriptor | String | Texto que aparecerá na fatura do comprador. Máximo de 12 caracteres | Sim |
| Installments | Array de [Installment](https://github.com/b-pay/pre-order/blob/master/README.md#installment) | Parcelas disponíveis para pagamento | Sim |

#### Installment
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| Number | Integer | Número da parcela | Sim |
| Text | String | Texto da parcela. Por exemplo, "1x de 100,00 sem juros" | Sim |
| AmountInCents | Integer | Valor do pedido | Não |

#### Options
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| ReturnUrl | String | Url de sucesso. Está página será exibida caso o pagamento seja processado com sucesso. | Sim |
| TransactionStatusNotificationUrl | String | Url de notificação de status. O status atual da transação será enviado para esta url | Não |
| PaymentExpnNotificationUrl | String | Url de notificação de pagamento expirado. | Não |

## Json de exemplo para criação de token

```json
{  
   "sellerKey":"1A7848D4-2B7C-42AF-9D04-98B792FB89CA",
   "buyer":{  
      "documentNumber":"11111111111",
      "personType":"Person",
      "name":"Ciclano",
      "email":"ciclano@comprador.com",
      "gender":"Male",
      "birthday":"1988-08-02",
      "billingAddress":{  
         "street":"Rua da Quitanda",
         "number":199,
         "zipCode":"20091005",
         "complement":"Décimo andar",
         "district":"Centro",
         "city":"Rio de Janeiro",
         "stateName":"Rio de Janeiro",
         "country":"Brasil"
      }
   },
   "order":{  
      "orderReference":"CODIGOPEDIDO",
      "amountInCents":1000,
      "items":[  
         {  
            "name":"Magneto vs Sentinel",
            "category":"Colecionáveis",
            "priceInCents":1000,
            "unitPriceInCents":1000,
            "discountAmountInCents": 0,
            "quantity":1
         }
      ]
   },
   "shipping":{  
      "costInCents":0,
      "address":{  
         "street":"Rua da Quitanda",
         "number":199,
         "zipCode":"20091005",
         "complement":"Décimo andar",
         "district":"Centro",
         "city":"Rio de Janeiro",
         "stateName":"Rio de Janeiro",
         "country":"Brasil"
      }
   },
   "payment":{  
      "operationType":"Authorize",
      "currency":"BRL",
      "softDescriptor":"B-PAY",
      "installments":[
          { "number":1, "text": "1x de R$10,00 sem juros" },
          { "number":2, "text": "2x de R$5,00 sem juros" },
          { "number":3, "text": "3x de R$4,00 com juros", "amountInCents":1200 }
      ]
   },
   "options": {
       "returnUrl":"https://obrigado.com",
       "transactionStatusNotificationUrl":"https://status.com",
       "paymentExpnNotificationUrl": "https://expirado.com"
   }
}
```

## Referência de campos da resposta da criação de um token

#### Token criado com sucesso - http 200

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| Token | UUID | Token de identificação do pedido | Sim |
| ExpiresIn | Integer | Tempo para expiração do Token em <a href="http://www.epochconverter.com/" target="_blank">Epoch Time</a> | Sim |

## Json de exemplo com resposta de criação de token

```json
{
   "token":"71861931-9F71-4577-82D8-4F68AC2AEA17",
   "expiresIn":1446495735
}
```

#### Erro na criação do token - http 400

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| msg | String | Descrição do erro | Sim |
| param | String | Identifica campo com o dado inválido | Sim |
| value | String | Valor inválido informado no campo em questão | Sim |

#### Json de exemplo com resposta de erro na criação do token

``` json
[
  {
    "param": "Buyer.Email",
    "msg": "Invalid value",
    "value": "ciclanocomprador.com"
  },
  {
    "param": "Buyer.Birthday",
    "msg": "Invalid value",
    "value": "1988-08-"
  }
]
```

## Redirecionamento para página de checkout do B-PAY

https://checkout.meubpay.com.br/get-checkout?id={TOKEN}

## Post de notificação

Após o processamento de uma transação pelo B-PAY, um POST de notificação contendo os dados da transação será enviado no formato JSON para a loja. Para isto, é necessário que a loja informe, no momento de criação do token, uma URL que irá receber e interpretar estes dados, campo *options.transactionStatusNotificationUrl*.

**É importante que a página que irá receber e interpretar o POST de notificação esteja preparada para receber novos campos além daqueles descritos no manual, para que possamos atualizar e acrescentar campos conforme necessário, de forma a sempre notificar a loja com as informações mais completas sobre suas transações.**

O B-PAY efetuará três tentativas de notificação com intervalos de aproximadamente 15 minutos entre cada tentativa. Caso ocorra insucesso, o recurso de consulta, Query, está disponível na API para que a loja possa consultar os dados do pedido a qualquer momento. Para que o serviço de notificação do B-PAY entenda que houve sucesso na notificação, a URL da loja deve responder   o o HttpStatusCode 200 (OK) no Header da mensagem de resposta.

Detalhes dos campos enviados no Post de Notificação:

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| Payment | [Payment](https://github.com/b-pay/pre-order/blob/master/README.md#payment-1) | Objeto que contém dados do pagamento | Sim |
| Order | [Order](https://github.com/b-pay/pre-order/blob/master/README.md#order-1) | Objeto que contém dados do pagamento | Sim |

#### Payment

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| TransactionType | String | Identificador do tipo de transação. Exemplo "CreditCardTransaction" | Sim |
| CreditCardTransaction | [CreditCardTransaction](https://github.com/b-pay/pre-order/blob/master/README.md#creditcardtransaction) | Objeto que contém dados da transação | Sim |

#### CreditCardTransaction

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| SellerKey | UUID | Identificador da loja que realizou a transação | Sim |
| Acquirer | String | Adquirente onde a transação foi processada | Sim |
| TransactionKey | UUID | Identificador da transação no B-PAY | Sim |
| TransactionIdentifier | String | Identificador da transação na adquirente | Sim |
| UniqueSequencialNumber | String | Identificador único da transação na adquirente | Sim |
| AuthorizationCode | String | Código de autorização na adquirente | Sim |
| AmountInCents | Integer | Valor da transação em centavos | Sim |
| InstallmentCount | Integer | Número de parcelas da transação | Sim |
| PreviousTransactionStatus | String | Status prévio da transação no B-PAY | Sim |
| CurrentTransactionStatus | String | Status atual da transação no B-PAY | Sim |
| CreateDate | String | Data de criação da transação. Formato yyyy-mm-dd hh:mm:ss | Sim |
| LastChangeDate | String | Data da última alteração da de status da transação. Formato yyyy-mm-dd hh:mm:ss | Sim |
| CreditCard | [CreditCard](https://github.com/b-pay/pre-order/blob/master/README.md#creditcard) | Objeto com dados do cartão de crédito utilizado para realizar a transação | Sim |
| History | Array de [History](https://github.com/b-pay/pre-order/blob/master/README.md#history-1) | Histórico de eventos realizadas em uma transação | Sim | 

#### CreditCard

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| MaskedCreditCardNumber | String | Número truncado do cartão de crédito | Sim |
| HolderName | String | Nome do portador do cartão | Sim |
| CreditCardBrand | String | Bandeira do cartão de crédito | Sim |

#### History
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| TransactionStatus | String | Status da transação. Exemplo "Captured" | Sim |
| Date | String | Data em que o evento foi realizado. Formato yyyy-mm-dd hh:mm:ss | Sim |
| AmountInCents | Integer | Valor de acordo com o evento realizado. Autorizado, capturado ou canelado | Sim |
| OperationType | String | Tipo da transação realizada. Valores possíveis: 'AuthorizeAndCapture', 'Authorize' | Sim |
| OrderStatus | String | Status do pedido | Sim |
| AcquirerReturnCode | String | Código de retorno da adquirente de acordo com o evento realizado | Sim |
| AcquirerMessage | String | Mensagem de retorno da adquirente de acordo com o evento realziado | Sim |

#### Order

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| Token | UUID | Token utilizado para realizar a transação | Sim |
| OrderKey | UUID | Identificador do pedido | Sim |
| OrderReference | String | Identificador do pedido enviado pela loja | Sim |
| OrderStatus | String | Status do pedido | Sim |

#### Json de exemplo do post de notificação

```json
{
    "payment": {
        "transactionType": "CreditCardTransaction",
        "transaction": {
            "sellerKey": "1a7848d4-2b7c-42af-9d04-98b792fb89ca",
            "acquirer": "Simulator",
            "transactionKey": "6fdd42b2-6c78-43e2-8a9a-22a507c593b8",
            "transactionIdentifier": "96311",
            "uniqueSequencialNumber": "121073",
            "authorizationCode": "100038",
            "amountInCents": 32918,
            "installmentCount": 0,
            "previousTransactionStatus": null,
            "currentTransactionStatus": "Captured",
            "createDate": "2015-11-06 21:21:46",
            "lastChangeDate": "2015-11-06 21:21:46",
            "creditCard": {
                "maskedCreditCardNumber": "411111****1111",
                "holderName": "carlos a p de paul",
                "creditCardBrand": "Visa"
            },
            "history": [
                {
                    "transactionStatus": "Captured",
                    "date": "2015-11-06 21:21:46",
                    "amountInCents": 32918,
                    "operationType": "AuthorizeAndCapture",
                    "orderStatus": "Paid",
                    "acquirerReturnCode": "0",
                    "acquirerMessage": "Simulator|Simulator|Transação de simulação autorizada com sucesso"
                }
            ]
        }
    },
    "order": {
        "token": "767ac3e0-84cc-11e5-b9d9-7bfe49f91a01",
        "orderKey": "674bc75b-84a4-4026-9f2a-ada73920a1a1",
        "orderReference": "Meupedido2",
        "orderStatus": "Paid"
    }
}
```

#### Json de exemplo do post de pagamento expirado

```json
{
    "order": {
        "token": "767ac3e0-84cc-11e5-b9d9-7bfe49f91a01",
        "orderReference": "Meupedido2",
        "sellerKey": "1a7848d4-2b7c-42af-9d04-98b792fb89ca",
        "orderStatus": "Expired"
    }
}
```

## Operação de consulta

Para consultar transações do B-PAY é necessário enviar um GET para o recurso /transactions. Os parâmetros de pesquisa disponíveis são *transactionKey*, identificador da transação no B-PAY, *orderReference*, identificador do pedido na loja ou *token*, código gerado para exibição do checkout.

Exemplo: https://seller-api.meubpay.com.br/transactions/{transactionKey, orderReference ou token}

#### Observação
`
Os objetos *payment* e *order* possuem os mesmos campos descritos na seção do Post de Notificação. Sendo assim, para simplificar a documentação, o link dos objetos citados acima faz referência aos mesmos objetos do Post de Notificação.
`

## Json de exemplo com resposta de uma operação de consulta

```json
[{
    "payment": {
        "transactionType": "CreditCardTransaction",
        "transaction": {
            "sellerKey": "1a7848d4-2b7c-42af-9d04-98b792fb89ca",
            "acquirer": "Simulator",
            "transactionKey": "6fdd42b2-6c78-43e2-8a9a-22a507c593b8",
            "transactionIdentifier": "96311",
            "uniqueSequencialNumber": "121073",
            "authorizationCode": "100038",
            "amountInCents": 32918,
            "installmentCount": 0,
            "previousTransactionStatus": null,
            "currentTransactionStatus": "Captured",
            "createDate": "2015-11-06 21:21:46",
            "lastChangeDate": "2015-11-06 21:21:46",
            "creditCard": {
                "maskedCreditCardNumber": "411111****1111",
                "holderName": "carlos a p de paul",
                "creditCardBrand": "Visa"
            },
            "history": [
                {
                    "transactionStatus": "Captured",
                    "date": "2015-11-06 21:21:46",
                    "amountInCents": 32918,
                    "operationType": "AuthorizeAndCapture",
                    "orderStatus": "Paid",
                    "acquirerReturnCode": "0",
                    "acquirerMessage": "Simulator|Simulator|Transação de simulação autorizada com sucesso"
                }
            ]
        }
    },
    "order": {
        "token": "767ac3e0-84cc-11e5-b9d9-7bfe49f91a01",
        "orderKey": "674bc75b-84a4-4026-9f2a-ada73920a1a1",
        "orderReference": "Meupedido2",
        "orderStatus": "Paid"
    }
}]
```

## Operação de cancelamento

Para cancelar transações do B-PAY é necessário enviar um POST para o recurso /transactions. Os parâmetros disponíveis são *transactionKey*, identificador da transação no B-PAY, *orderReference*, identificador do pedido na loja ou *token*, código gerado para exibição do checkout. O valor a ser cancelado é opcional e poderá ser enviado no corpo da requisição.

Exemplo: https://seller-api.meubpay.com.br/transactions/{transactionKey, orderReference ou token}/cancel

#### Cancel
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| AmountInCents | Integer | Valor cancelado | Não |

## Json de exemplo para criação de um cancelamento

```json
{
  "amountInCents": 10000
}
```

## Json de exemplo com resposta de uma operação de cancelamento

```json
[{
    "payment": {
        "transactionType": "CreditCardTransaction",
        "transaction": {
            "sellerKey": "1a7848d4-2b7c-42af-9d04-98b792fb89ca",
            "acquirer": "Simulator",
            "transactionKey": "6fdd42b2-6c78-43e2-8a9a-22a507c593b8",
            "transactionIdentifier": "96311",
            "uniqueSequencialNumber": "121073",
            "authorizationCode": "100038",
            "amountInCents": 32918,
            "installmentCount": 0,
            "previousTransactionStatus": "Captured",
            "currentTransactionStatus": "Voided",
            "createDate": "2015-11-06 21:21:46",
            "lastChangeDate": "2015-11-06 21:21:46",
            "creditCard": {
                "maskedCreditCardNumber": "411111****1111",
                "holderName": "carlos a p de paul",
                "creditCardBrand": "Visa"
            },
            "history": [
                {
                    "transactionStatus": "Voided",
                    "date": "2015-11-07 21:21:46",
                    "amountInCents": 32918,
                    "operationType": "Void",
                    "orderStatus": "Voided",
                    "acquirerReturnCode": "0",
                    "acquirerMessage": "Simulator|Transação de simulação autorizada com sucesso"
                }, {
                    "transactionStatus": "Captured",
                    "date": "2015-11-06 21:21:46",
                    "amountInCents": 32918,
                    "operationType": "AuthorizeAndCapture",
                    "orderStatus": "Paid",
                    "acquirerReturnCode": "0",
                    "acquirerMessage": "Simulator|Transação de simulação autorizada com sucesso"
                }
            ]
        }
    },
    "order": {
        "token": "767ac3e0-84cc-11e5-b9d9-7bfe49f91a01",
        "orderKey": "674bc75b-84a4-4026-9f2a-ada73920a1a1",
        "orderReference": "Meupedido2",
        "orderStatus": "Paid"
    }
}]
```
