# pre-order

## Referência dos campos

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| SellerKey  | UUID (string)   | Chave secreta para identificar o sistema que está enviando a requisição. Exemplo: "b733ccec-8099-11e5-8bcf-feff819cdc9f"    |Sim|
| Buyer | [Buyer](https://github.com/b-pay/pre-order/blob/master/README.md#buyer) | Objeto com os dados do comprador    |Sim|
| Order | Order | Objeto com os dados do pedido    |Sim|
| Shipping | Shipping | Objeto com os dados de entrega    |Sim|
| Payment | Payment | Objeto com as opções do pagamento   |Sim|

## Buyer

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| DocumentNumber | String | Numerto do documento do comprador| Sim |
| PersonType | String | Tipo de pessoa. Valores possíveis - 'Person' ou 'Company' | Sim |
| Name | String | Nome do comprador com no máximo 64 caracteres | Sim |
| Email | String | Endereço de e-mail do comprador com no máximo 64 caracteres | Sim |
| Gender | String | Sexo do comprador. Valores possíveis - 'Male' ou 'Female' | Sim |
| Birthday | String | Data de aniversário do comprador. Formato aceito yyyy-mm-dd | Não |
| HomePhone | String | Telefone residencial do comprador. Formatos aceitos: (DDD)999999999 ou DDI(DDD)999999999 | Sim |
| MobilePhone | String | Telefone celular do comprador. Formatos aceitos: (DDD)999999999 ou DDI(DDD)999999999 | Sim |
| WorkPhone | String | Telefone comercial do comprador. Formatos aceitos: (DDD)999999999 ou DDI(DDD)999999999 | Sim |
| BillingAddress | [BillingAddress](https://github.com/b-pay/pre-order/blob/master/README.md#billingaddress) | Objeto com os dados do endereço de cobrança |Sim|

## BillingAddress

| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| Street | String | Nome da rua. Máximo de 64 caracteresres | Sim |
| Number | String | Número | Sim |
| ZipCode | String | CEP (sem traço) | Sim |
| Complement | String | Complemento. Máximo de 64 caracteresres | Não |
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
| Items | [Item](https://github.com/b-pay/pre-order/blob/master/README.md#item) | Coleção com itens do carrinho de compras | Sim |

## Item
| Campo|Tipo|Descrição|Obrigatório |
| --------|---------|-------|-------|
| SKU | String | Identificador do produto | Não |
| Name | String | Nome do produto | Sim |
| Category | String | Categoria do produto | Não |
| PriceInCents | Integer | Valor total do item no carrinho | Sim |
| UnitPriceInCents | Integer | Valor unitário do item no carrinho | Sim |
| Quantity | Integer | Quantidade do item | Sim |

## Requisição Json de exemplo

```json
{
   "SecretKey": "8141BB01-EE19-4081-A4A4-97455505940E",
   "Buyer":{
      "DocumentNumber":"12458253725",
      "PersonType": "Person",
      "Name":"Carlos Peçanha",
      "Email":"cpecanhamundipagg.com",
      "Gender":"Male",
      "Birthday":"1988-08-02",
      "Phone":"2123011826",
      "Mobile":"21975338448",
      "BillingAddress":{
         "Street":"Rua da Quitanda",
         "Number":199,
         "ZipCode":"20091005",
         "Complement":"Décimo andar",
         "District":"Centro",
         "City":"Rio de Janeiro",
         "StateName":"Rio de Janeiro"
      }
   },
   "Order":{
      "OrderDescription": "Whey Protein Isolado",
      "OrderReference":"Meu pedido",
      "AmountInCents":9999,
      "Items": [
          {
              "SKU": "",
              "Name":"",
              "Category":"",
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
            "StateName":"Rio de Janeiro"
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
    }
}
```
