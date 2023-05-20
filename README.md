# Projeto de Banco de Dados: E-Commerce

## Modelo Entidade-Relacionamento Estendido:


### Dados:
- ### Registro:

- A entidade 'Cliente' guarda o registro de todas as contas de login dos clientes do e-commerce. //FEITO


   | Atributos               | Constraint  |    Relacionamento    |                                  Descrição                                  |
   | ----------------------- | :---------: | :------------------: | :-------------------------------------------------------------------------: |
   | Id_registro             |      PK     |          -           |                         Login de acesso do cliente                          |
   | nome                    |      -      |          -           |                               Nome do cliente                               |
   | email                   |   UNIQUE    |          -           |                              E-mail do cliente                              |
   | senha                   |      -      |          -           |                      Senha do cliente (Criptografada)                       |
   | salt                    |   UNIQUE    |          -           |          Dado aleatório, usado na criptografia da senha do cliente          |


- ### cliente:

A entidade 'Usuário' guarda o registro de todas as contas dos usuários do e-commerce. //FEITO

| Atributos               | Constraint  |    Relacionamento    |                                  Descrição                                  |
| ----------------------- | :---------: | :------------------: | :-------------------------------------------------------------------------: |
| id_cliente              |     PK      |          -           |                               Chave primária                                |
| data_conta_criada       |      -      |          -           |                  Data em que a conta do cliente foi criada                  |
| nome                    |      FK     |          -           |                               Nome do cliente                               |
| sobrenome               |      -      |          -           |                            Sobrenome do cliente                             |
| sexo                    |      -      |          -           |                           Sexo do cliente (M, F)                            |
|telefone                 |      -      |          -           |                          Telefone do cliente                                |


- ### cartao: //FEITO

A entidade '_cartão_' guarda todos os cartões salvos pelos clientes em suas contas no e-commerce.

- Um usuário pode deixar cartões salvos em sua conta para facilitar o processo de compra, ao adicionar um cartão em sua conta é criado um registro nessa tabela com os dados do cartão criptografados.

  - Quando um cliente deleta um cartão salvo de sua conta, o seu vínculo com o cartão na entidade '_cliente_tem_cartao_' será deletado, como um cartão pode estar ligado a mais de um cliente, ele só será deletado dessa tabela caso não haja mais nenhum vínculo com algum cliente.


| Atributos          | Constraint |           Relacionamento           |                         Descrição                         |
| ------------------ | :--------: | :--------------------------------: | :-------------------------------------------------------: |
| id_cartao          |     PK     |                 -                  |                      Chave primária                       |
| numero_cartao      |   UNIQUE   |                 -                  |   Número do cartão (Últimos 4 dígitos e criptografado)    |
| nome_cartao        |     -      |                 -                  |         Nome impresso no cartão  (Criptografado)          |
| data_expiracao     |     -      |                 -                  |        Data de expiração do cartão (Criptografado)        |
| id_cartao_bandeira |     FK     | cartao_bandeira.id_cartao_bandeira |  Chave estrangeira, faz referência a bandeira do cartão   |
| salt               |   UNIQUE   |                 -                  | Dado aleatório, usado na criptografia dos dados do cartão |

- ### cliente_tem_cartao: //Feito

A entidade '_cliente_tem_cartao_' é fruto de um relacionamento N para M entre as entidades '_cliente_' e '_cartao_', um cliente pode ter vários cartões salvos, e um cartão pode estar salvo em várias contas. 

  - Quando um cliente deleta um cartão salvo em sua conta o vínculo será desfeito fazendo a deleção do registro dessa tabela.

| Atributos  | Constraint |   Relacionamento   |                           Descrição                           |
| ---------- | :--------: | :----------------: | :-----------------------------------------------------------: |
| id_cliente |  PK / FK   | cliente.id_cliente | Chave primária / Chave estrangeira, faz referência ao cliente |
| id_cartao  |  PK / FK   |  cartao.id_cartao  | Chave primária / Chave estrangeira, faz referência ao cartão  |

- ### pagamento: //Feito

A entidade '_pagamento_' guarda o registro de todos os pagamentos relacionados aos pedidos feitos no e-commerce.

| Atributos                 | Constraint |          Relacionamento          |                                 Descrição                                  |
| ------------------------- | :--------: | :------------------------------: | :------------------------------------------------------------------------: |
| id_pagamento              |     PK     |                -                 |                               Chave primária                               |
| id_pagamento_tipo         |     FK     | pagamento_tipo.id_pagamento_tipo |           Chave estrangeira, faz referência ao tipo do pagamento           |
| parcelado                 |     -      |                -                 |                    Indica se o pagamento foi parcelado                     |
| quantidade_parcelas       |     -      |                -                 |          Quantidade de parcelas, caso o pagamento seja parcelado           |
| id_boleto                 |     FK     |         boleto.id_boleto         | Chave estrangeira, faz referência ao boleto, caso o pagamento seja via ele |
| pagamento_confirmado      |     -      |                -                 |                    Indica se o pagamento foi confirmado                    |
| data_pagamento_confirmado |     -      |                -                 |                      Data de confirmação do pagamento                      |

- ### boleto: //FEITO

A entidade '_boleto_' guarda o registro de todos os boletos relacionados aos pagamentos do e-commerce.

| Atributos             | Constraint | Relacionamento |             Descrição              |
| --------------------- | :--------: | :------------: | :--------------------------------: |
| id_boleto             |     PK     |       -        |           Chave primária           |
| data_emissao_boleto   |     -      |       -        |     Data de emissão do boleto      |
| numero_boleto         |   UNIQUE   |       -        |          Código do boleto          |
| valor_boleto          |     -      |       -        |          Valor do boleto           |
| boleto_pago           |     -      |       -        | Indica se o boleto foi pago ou não |
| data_pagamento_boleto |     -      |       -        |    Data de pagamento do boleto     |


- ### produto: //FEITO

A entidade '_produto_' guarda o registro de todos os produtos do e-commerce.

| Atributos               | Constraint |                Relacionamento                |                          Descrição                          |
| ----------------------- | :--------: | :------------------------------------------: | :---------------------------------------------------------: |
| id_produto              |     PK     |                      -                       |                       Chave primária                        |
| data_criacao            |     -      |                      -                       |                 Data de criação do produto                  |
| nome_produto            |     -      |                      -                       |                       Nome do produto                       |
| descricao_produto       |     -      |                      -                       |                    Descrição do produto                     |
| valor_produto           |     -      |                      -                       |                      Valor do produto                       |
| id_produto_subcategoria |     FK     | produto_subcategoria.id_produto_subcategoria | Chave estrangeira, faz referência a subcategoria do produto |
| desativado              |     -      |                      -                       |             Indica se o produto foi desativado              |
| data_desativacao        |     -      |                      -                       |               Data de desativação do produto                |

- ### pedido: //FEITO

A entidade '_pedido_' guarda o registro de todos os pedidos feitos no e-commerce.

| Atributos             | Constraint |         Relacionamento         |                         Descrição                         |
| --------------------- | :--------: | :----------------------------: | :-------------------------------------------------------: |
| id_pedido             |     PK     |               -                |                      Chave primária                       |
| id_pedido_status      |     FK     | pedido_status.id_pedido_status |   Chave estrangeira, faz referência ao status do pedido   |
| id_nota_fiscal        |     FK     |   nota_fiscal.id_nota_fiscal   | Chave estrangeira, faz referência a nota fiscal do pedido |
| data_pedido_realizado |     -      |               -                |               Data de realização do pedido                |
| id_cliente            |     FK     |       cliente.id_cliente       |       Chave estrangeira, faz referência ao cliente        |
| id_pagamento          |     FK     |     pagamento.id_pagamento     | Chave estrangeira, faz referência ao pagamento do pedido  |
| id_entrega            |     FK     |       entrega.id_entrega       |   Chave estrangeira, faz referência a entrega do pedido   |
| pedido_concluido      |     -      |               -                |             Indica se o pedido foi concluído              |
| data_pedido_concluido |     -      |               -                |                Data de conclusão do pedido                |

- ### nota_fiscal: //FEITO

A entidade '_nota_fiscal_' guarda o registro de todos as notas fiscais relacionadas aos pedidos do e-commerce.
  - Armazenamento notas em um sistema externo, guardar sua chave de identificação do sistema externo.

| Atributos         | Constraint | Relacionamento |                          Descrição                           |
| ----------------- | :--------: | :------------: | :----------------------------------------------------------: |
| id_nota_fiscal    |     PK     |       -        |                        Chave primária                        |
| data_emissao_nota |     -      |       -        |                Data de emissão da nota fiscal                |
| numero_nota       |   UNIQUE   |       -        | Número da nota (FK para um sistema externo de armazenamento) |
| valor_nota        |     -      |       -        |                        Valor da nota                         |

- ### pedido_tem_produto: //FEITO

A entidade '_pedido_tem_produto_' é fruto de um relacionamento N para M entre as entidades '_pedido_' e '_produto_', um pedido pode ter vários produtos relacionados a ele, e um produto pode estar relacionado a vários pedidos distintos. 

| Atributos      | Constraint |   Relacionamento    |                           Descrição                           |
| -------------- | :--------: | :-----------------: | :-----------------------------------------------------------: |
| id_pedido      |  PK / FK   |  cliente.id_pedido  | Chave primária / Chave estrangeira, faz referência ao pedido  |
| id_produto     |  PK / FK   | endereco.id_produto | Chave primária / Chave estrangeira, faz referência ao produto |
| quantidade     |     -      |          -          |                     Quantidade do produto                     |
| desconto       |     -      |          -          |   Indica se o produto está com desconto para aquele pedido    |
| valor_desconto |     -      |          -          |                       Valor do desconto                       |

- ### entrega: //FEITO

A entidade '_entrega_' guarda o registro de todos as entregas relacionadas aos pedidos feitos no e-commerce.

  - Uma entrega pode ter um endereço e um telefone específico.
  - Informar um telefone específico para e entrega é obrigatório.
  - Caso não seja informado um endereço específico para e entrega, o cliente pode usar um dos seus endereços salvos.

| Atributos             | Constraint |          Relacionamento          |                                Descrição                                 |
| --------------------- | :--------: | :------------------------------: | :----------------------------------------------------------------------: |
| id_entrega            |     PK     |                -                 |                              Chave primária                              |
| data_entrega_inicio   |     -      |                -                 |                        Data de início da entrega                         |
| valor_frete           |     -      |                -                 |                              Valor do frete                              |
| codigo_rastreio       |   UNIQUE   |                -                 |                      Código de rastreio da entrega                       |
| id_endereco           |     FK     |       endereco.id_endereco       |         Chave estrangeira, faz referência ao endereço da entrega         |
| id_transportadora     |     FK     | transportadora.id_transportadora | Chave estrangeira, faz referência a transportadora relacionada a entrega |
| cliente_telefone      |     FK     |       cliente._telefone          |         Chave estrangeira, faz referência ao telefone do cliente         |
| data_entrega_previsao |     -      |                -                 |                       Data de previsão da entrega                        |
| entrega_concluida     |     -      |                -                 |                    Indica se a entrega foi concluída                     |
| data_entrega_fim      |     -      |                -                 |                        Data de entrega do pedido                         |

- ### endereco: //FEITO

A entidade '_endereco_' guarda o registro de todos os endereços relacionados aos clientes, empresas e pedidos do e-commerce.

| Atributos   | Constraint |  Relacionamento  |                             Descrição                              |
| ----------- | :--------: | :--------------: | :----------------------------------------------------------------: |
| id_endereco |     PK     |        -         |                           Chave primária                           |
| cep         |     -      |        -         |                                CEP                                 |
| cidade      |     -      |        -         |                               Cidade                               |
| numero      |     -      |        -         |                               Número                               |
| complemento |     -      |        -         |                            Complemento                             |
| bairro      |     -      |        -         |                               Bairro                               |




- ### pagamento_tipo:

A entidade '_pagamento_tipo_' guarda o registro de todos os tipos de pagamentos disponíveis no e-commerce.

| Atributos         | Constraint | Relacionamento |                                    Descrição                                     |
| ----------------- | :--------: | :------------: | :------------------------------------------------------------------------------: |
| id_pagamento_tipo |     PK     |       -        |                                  Chave primária                                  |
| data_criacao      |     -      |       -        | Data de criação (data em que o e-commerce começou a aceitar o tipo de pagamento) |
| tipo_pagamento    |     -      |       -        |                            Nome do tipo de pagamento                             |
| desativado        |     -      |       -        |                  Indica se o tipo de pagamento está desativado                   |
| data_desativacao  |     -      |       -        |                  Data em que o tipo de pagamento foi desativado                  |

- ### status: //Feito

A entidade 'status' guarda o registro de todos os status relacionados aos pedidos, entregas e pagamentos.

| Atributos        | Constraint | Relacionamento          |              Descrição                        |
| ---------------- | :--------: | :---------------------: | :------------------------------------------:  |
| id               |     PK     |     Status.id           |           Chave primária                      |
| EntregaStatus    |     -      |       -                 |           Status da Entrega                   |
| PedidoStatus     |     -      |       -                 |           Status do Pedido                    |
| PagamentoStatus  |     -      |       -                 |           Status do Pagamento                 |
| id_cliente       |     FK     |       client.id         |  Chave de identificação da tabela do cliente  |
| id_entrega       |     FK     |       entrega.id        |  Chave de identificação da tabela de entrega  |
| id_produto       |     FK     |       produto.id        |  Chave de identificação da tabela de produto  |
| produto_status   |     --     |           --            |              Status do produto                |

- ### produto_categoria: feito

A entidade '_produto_categoria_' guarda o registro de todas as categorias relacionadas aos subcategorias dos produtos.

| Atributos            | Constraint | Relacionamento |               Descrição                |
| -------------------- | :--------: | :------------: | :------------------------------------: |
| id_produto_categoria |     PK     |       -        |             Chave primária             |
| data_criacao         |     -      |       -        |      Data de criação da categoria      |
| categoria            |     -      |       -        |           Nome da categoria            |
| descricao_categoria  |     -      |       -        |         Descrição da categoria         |

- ### transportadora: feito

A entidade '_transportadora_' guarda o registro de todos as transportadoras do e-commerce, quando uma transportadora para de trabalhar com o e-commerce desativamos o seu registro.

| Atributos         | Constraint |   Relacionamento   |                                            Descrição                                            |
| ----------------- | :--------: | :----------------: | :---------------------------------------------------------------------------------------------: |
| id_transportadora |     PK     |         -          |                                         Chave primária                                          |
| unidade           |     -      |         -          |                                   Unidade da Transportadora                                     |
| RazaoSocial       |     -      |         -          |                                     Nome da Transportadora                                      |

-> [Retornar ao índice](#índice)

### Empresa:

- ### fornecedor:

A entidade '_fornecedor_' guarda todas as empresas (transportadoras/fornecedoras) cadastradas no e-commerce, essas empresas podem ter mais de uma unidade, que podem ser fornecedoras ou transportadoras, ou ambas. 

- Não deletamos o registro de uma empresa, fazemos a sua desativação, isso quando todas suas unidades também estão desativadas.

| Atributos        | Constraint | Relacionamento |                                        Descrição                                         |
| ---------------- | :--------: | :------------: | :--------------------------------------------------------------------------------------: |
| id_fornecedor    |     PK     |       -        |                                      Chave primária                                      |
| id_endereço      |     FK     |   endereco.id  |                                Localização do fornecedor                                 |
| nota_fiscal      |     FK     |     nota.id    |                                   Nota fiscal dos produtos                               |
| id_produto       |     FK     |  produto.id    |                           Indica se a empresa está desativada                            |
| data_desativacao |     -      |                |                           Data em que a empresa foi desativada                           |





