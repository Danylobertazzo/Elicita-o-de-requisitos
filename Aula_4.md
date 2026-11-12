PROCESSO 1: Vendedores devem acessar o sistema pra registrar pedidos

Critérios de Aceitação
Cenário 1 (Login): Dado que o vendedor insere um login e senha válidos, quando clica em 'Entrar', então o sistema libera o acesso ao painel de pedidos
Cenário 2 (Vínculo de Venda): Dado que o vendedor está autenticado, quando finaliza um novo pedido, então o sistema deve salvar automaticamente o nome/ID do vendedor junto aos dados da venda
Requisitos Não Funcionais (RNF) Propriedades e restrições de qualidade baseadas na ISO/IEC 25010

RNF 1 (Segurança): "As credenciais de acesso devem ser transmitidas via protocolo HTTPS e o sistema deve bloquear o acesso após 3 tentativas de login incorretas."
RNF 2 (Eficiência de Desempenho): "A tela de registro de pedidos deve ser carregada em até 2 segundos após a autenticação bem-sucedida."

Fonte 1: Utilizada para estruturar a História de Usuário seguindo o padrão ágil e para definir os Critérios de Aceitação no formato testável "Dado/Quando/Então"
Fonte 2: Utilizada para classificar os Requisitos Não Funcionais dentro das características de Segurança e Eficiência de Desempenho da norma ISO/IEC 25010

Processo 2: Cadastro e Remoção de Produtos
História de Usuário: Como Administrador, quero cadastrar e remover produtos, para manter o catálogo de vendas sempre atualizado e preciso para os clientes."
Requisito Funcional: "O sistema deve permitir que o usuário administrador realize a criação (incluindo nome, preço e descrição) e a exclusão de registros de produtos na base de dados central."

Cenário 1 (Cadastro): Dado que o administrador preenche todos os campos obrigatórios, quando clica em 'Salvar', então o novo produto deve ficar imediatamente visível na lista de gestão.
Cenário 2 (Remoção): Dado que o administrador seleciona um produto existente, quando confirma a exclusão, então o sistema deve remover o item e impedir que ele seja exibido no catálogo de compras dos clientes.
Requisitos Não Funcionais (RNF):
RNF 1 (Eficiência de Desempenho): "A atualização do catálogo após o cadastro ou remoção de um produto deve ser processada e refletida na base de dados em até 2 segundos." (Este requisito resolve a ambiguidade do termo "prontidão" que você anotou originalmente).
RNF 2 (Segurança): "Apenas usuários autenticados com o perfil 'Administrador' podem acessar as funcionalidades de alteração de produtos."

PROCESSO 3: Controle de Estoque (Time de Estoque)
História de Usuário (User Story): Como Equipe de Estoque, quero registrar entradas e saídas de produtos, para manter o saldo de inventário preciso e evitar vendas sem produto físico."
Requisito Funcional (RF): O sistema deve atualizar o saldo de estoque automaticamente após cada venda concluída e permitir o registro manual de novas remessas de mercadoria.

Cenário 1 (Entrada de Estoque): Dado que uma nova remessa chegou, quando o operador insere a quantidade recebida, então o saldo total do produto deve ser somado no banco de dados.
Cenário 2 (Bloqueio por falta): Dado que o saldo de um item chega a zero, quando um cliente visualiza o cardápio, então o sistema deve exibir o item como "indisponível" automaticamente.
RNF (Confiabilidade): O sistema deve garantir a integridade dos dados, impedindo que o saldo de estoque fique negativo mesmo em casos de múltiplos acessos simultâneos.
RNF (Eficiência de Desempenho): A baixa no estoque após uma venda deve ser refletida no catálogo de vendas em no máximo 3 segundos para evitar vendas duplicadas do último item.
RNF (Usabilidade): O operador de estoque deve conseguir realizar a baixa ou entrada de um produto com no máximo 2 toques na tela ou uma leitura de código de barras.
Fontes Utilizadas:
Fonte 1: Utilizada para embasar a lógica de indisponibilidade de itens e controle de fluxo.
Fonte 2: Utilizada para as métricas quantitativas de desempenho e confiabilidade da ISO 25010.
