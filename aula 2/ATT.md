Item 1 - Acompanhar o Pedido — Should Have (Importante)

Justificativa: É fundamental para a experiência e retenção do cliente, pois reduz a ansiedade de saber se a refeição vai chegar e diminui drasticamente chamados no suporte ("onde está meu pedido?"). No entanto, no primeiro dia do aplicativo, o serviço básico de compra e entrega ainda conseguiria funcionar com notificações mais simples ou SMS, o que a coloca logo abaixo do gerenciamento de estoque/cardápio.

>> Historia << Como cliente do aplicativo, quero acompanhar o status do meu pedido em tempo real para saber exatamente quando a comida vai chegar e me preparar para recebê-la.

Critérios de Aceitação 
Cenário 1 (Caminho feliz):Dado que fiz um pedido com sucesso no aplicativo,  Quando acesso a tela de acompanhamento,  Então vejo a etapa atual do pedido (ex: "Em preparação", "Saiu para entrega") atualizada dinamicamente.

Cenário 2 (Falha de conexão):Dado que estou na tela de acompanhamento e fico sem conexão com a internet,  Quando o sistema tenta atualizar o status,  Então vejo um aviso informando sobre a falta de conexão e mantendo o último status carregado.

Cenário 3 (Conclusão do pedido):Dado que o entregador confirma a entrega do pedido,  Quando atualizo a tela de acompanhamento,  Então o status muda para "Entregue" e o sistema sugere a avaliação do pedido.


Item 2 - Cardápio Indisponível — Must Have (Indispensável)

Justificativa: É a funcionalidade crítica para a operação básica do negócio. Se um cliente compra um prato que o restaurante não tem em estoque, o pedido precisa ser cancelado, gerando estorno financeiro, frustração do cliente e atrito com o restaurante. Resolver isso na origem evita falhas em todas as etapas seguintes da cadeia de entrega.

>> Historia << Como gerente do restaurante, quero poder alterar o status de um item do cardápio para "indisponível" no meu painel para que os clientes não consigam comprar pratos que não posso preparar no momento.   

Critérios de Aceitação 
Cenário 1 (Bloqueio de compra do item indisponível): Dado que o restaurante marcou o item "Batata Frita" como indisponível no painel,  Quando o cliente visualiza o cardápio no aplicativo,  Então o item "Batata Frita" aparece cinza/desabilitado e o cliente não consegue adicioná-lo ao carrinho.
Cenário 2 (Item indisponível durante a montagem do carrinho): Dado que o cliente tem o item "Batata Frita" no carrinho, mas o restaurante o marca como indisponível antes da finalização,  Quando o cliente tenta confirmar o pedido,  Então o sistema exibe um aviso informando a indisponibilidade e remove o item do carrinho antes do pagamento.
Cenário 3 (Reativação do item no cardápio): Dado que o restaurante volta a ter estoque e altera o status do item para "disponível" no painel,  Quando o cliente atualiza ou abre o cardápio,  Então o item volta a ficar liberado para seleção e compra normalmente.


Item 3 -Entregador Reportar Problema — Could Have (Desejável)

Justificativa: Trata de cenários de exceção na ponta final do processo (ex: pneu furado ou cliente que não atende a porta). Embora traga eficiência para a operação, no início da plataforma esses imprevistos podem ser contornados manualmente (o entregador ligando direto para o suporte ou restaurante) enquanto a ferramenta automática no app do entregador é desenvolvida.

>> Historia << Entregar tem a necessidade de reportar u pedido realizado sem ninguém para recebe-lo, e/ou precisará retornar daqui algum tempo (para isso é realizado o pedido e um novo trajeto) ou somente confirmação para retornar ao centro de distribuição

Cenário 1 (Cliente ausente - Notificação ao suporte/cliente):Dado que cheguei ao endereço de entrega e o cliente não responde após tentativas de contato,  Quando clico na opção "Cliente não encontrado" no aplicativo,  Então o sistema dispara uma notificação urgente para o cliente e inicia um contador de espera na tela do entregador.

Cenário 2 (Tempo esgotado e instrução de retorno):Dado que o tempo limite de espera encerrou sem resposta do cliente,  Quando confirmo o término do tempo de espera no aplicativo,  Então o pedido é cancelado por ausência e o sistema exibe a rota de retorno do pedido ao estabelecimento/restaurante.

Cenário 3 (Problema no veículo durante o percurso):Dado que estou a caminho do endereço e meu veículo sofre uma avaria (pneu furado/quebra),  Quando seleciono "Reportar problema na rota" e informo a imprevisto,  Então o suporte é notificado para realocar outro entregador e o cliente é avisado sobre o atraso no pedido.

Must Have: 
Should Have:
Could Have:
