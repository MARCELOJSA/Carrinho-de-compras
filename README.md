O diagrama UML foi construído com base nos requisitos fornecidos, garantindo que o sistema seja capaz de atender às funcionalidades necessárias, como gerenciar clientes, carrinhos, produtos, itens e pedidos. A seguir, explicamos o processo de modelagem e os relacionamentos estabelecidos entre as entidades.

1. Identificação dos Elementos do Sistema
Primeiramente, identificamos as principais entidades do sistema com base nas funcionalidades descritas:

Cliente: Representa a pessoa que utiliza o sistema. Deve armazenar dados básicos como id_cliente, nome e email.
Carrinho: Representa a estrutura onde o Cliente adiciona os produtos que deseja comprar antes de finalizar a compra. Contém uma referência ao Cliente e aos Itens.
Item: Representa um produto que foi adicionado ao Carrinho ou Pedido, armazenando detalhes como quantidade e preço no momento da compra.
Produto: Representa um item disponível no catálogo. Cada Item está vinculado a um Produto, que contém informações como nome e preço.
Pedido: Representa a compra finalizada, vinculando os Itens adquiridos a um Cliente e contendo informações de data e hora da transação.

2. Definição dos Relacionamentos e Cardinalidades
Cliente ⇨ Carrinho
Descrição: Um Cliente pode ter vários Carrinhos ao longo do tempo (ex.: carrinhos antigos que não foram finalizados ou carrinhos de diferentes sessões).
Cardinalidade: 1:N (um Cliente pode ter muitos Carrinhos, mas cada Carrinho pertence a apenas um Cliente).
Chave Primária: id_cliente na tabela Cliente.
Chave Estrangeira: id_cliente na tabela Carrinho.
Direção da seta: <--- (o Carrinho "conhece" o Cliente, mas o Cliente não rastreia diretamente os carrinhos).

Carrinho ⇨ Item
Descrição: Um Carrinho pode conter vários Itens, representando os produtos que o Cliente deseja comprar. Cada Item pertence a um único Carrinho.
Cardinalidade: 1:N (um Carrinho pode ter muitos Itens, mas cada Item está vinculado a um único Carrinho).
Chave Primária: id_carrinho na tabela Carrinho.
Chave Estrangeira: id_carrinho na tabela Item.
Direção da seta: <--- (o Item conhece o Carrinho ao qual pertence).

Item ⇨ Produto
Descrição: Cada Item no Carrinho ou Pedido está associado a um Produto específico. O Produto armazena informações como nome e preço, enquanto o Item armazena detalhes específicos, como quantidade.
Cardinalidade: N:1 (um Produto pode aparecer em muitos Itens de diferentes Carrinhos ou Pedidos, mas cada Item está vinculado a apenas um Produto).
Chave Primária: id_produto na tabela Produto.
Chave Estrangeira: id_produto na tabela Item.
Direção da seta: <--- (o Item conhece o Produto associado).

Carrinho ⇨ Pedido
Descrição: Quando um Cliente finaliza a compra, o Carrinho é convertido em um Pedido. Cada Pedido corresponde a um único Carrinho.
Cardinalidade: 1:1 (cada Carrinho pode gerar um único Pedido, e cada Pedido é derivado de um único Carrinho).
Chave Primária: id_carrinho na tabela Carrinho.
Chave Estrangeira: id_carrinho na tabela Pedido.
Direção da seta: <--- (o Pedido conhece o Carrinho do qual foi gerado).

Cliente ⇨ Pedido
Descrição: Um Cliente pode ter vários Pedidos ao longo do tempo (histórico de compras). Cada Pedido pertence a um único Cliente.
Cardinalidade: 1:N (um Cliente pode ter muitos Pedidos, mas cada Pedido pertence a apenas um Cliente).
Chave Primária: id_cliente na tabela Cliente.
Chave Estrangeira: id_cliente na tabela Pedido.
Direção da seta: <--- (o Pedido conhece o Cliente ao qual pertence).

3. Organização dos Dados para Garantir a Associação de Itens a Pedidos
Os Itens no Carrinho são inicialmente associados a um Carrinho específico. Cada Item contém informações como quantidade, preço e ID do Produto.
Quando a compra é finalizada, o sistema converte o Carrinho em um Pedido, e todos os Itens do Carrinho passam a ser vinculados ao Pedido gerado. Isso é feito transferindo a referência do Carrinho para o Pedido nos registros de Itens.
O Pedido, por sua vez, armazena a data e hora da compra e é associado diretamente ao Cliente.

4. Solução para Registrar Itens no Carrinho e Vinculá-los ao Pedido
O Cliente adiciona um Produto ao Carrinho, criando um registro na tabela Item com a referência do Produto e do Carrinho.
Quando a compra é finalizada:
Um Pedido é criado com referência ao Cliente e ao Carrinho.
Todos os Itens associados ao Carrinho têm sua referência atualizada para o novo Pedido.
O Carrinho pode ser descartado ou mantido como histórico, dependendo da lógica do sistema.
