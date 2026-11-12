História 1

Como cliente, quero avaliar o pedido depois da entrega, para ajudar outros clientes a escolherem melhor.
RNF: A tela de avaliação deve ser carregada em até 1 segundo após o reconhecimento da entrega concluída - Eficiência de Desempenho
RNF: O sistema deve garantir a integridade dos dados, permitindo que apenas o usuário autenticado que realizou o pedido possa enviar a avaliação. - Segurança
RNF: O serviço de validação de conteúdo deve processar o comentário e identificar termos proibidos em menos de 500ms - Segurança
 - Dado que o pedido foi entregue, quando o cliente abre o app, então aparece a opção de avaliar o pedido - 

 - Dado que o cliente avalia com nota e comentário, quando confirma o envio, então a avaliação aparece no perfil do restaurante

História 2

Como cliente, quero salvar um cartão de pagamento, para não digitar os dados a cada compra.
RNF: Os dados sensíveis do cartão (PAN e CVV) não devem ser armazenados localmente no dispositivo do usuário e devem ser transmitidos via protocolo HTTPS - Segurança
RNF: O formulário de cadastro de cartão deve permitir que o usuário conclua o preenchimento em no máximo 45 segundos - Usabilidade
RNF: O sistema deve garantir que o cartão salvo esteja disponível para uso em 99,9% das tentativas de checkout - Confiabilidade

Dado que o cliente cadastra um cartão válido, quando confirma o cadastro, então o cartão fica disponível para escolha no checkout.
Dado que o cliente tem um cartão salvo, quandofaz um novo pedido, então pode selecionar esse cartão sem redigitar os dados.


História 3

Como dono de restaurante, quero ver um resumo diário de vendas, para acompanhar o desempenho do dia.

RNF: É criado um dashboard de venda do total de vendas, cancelamentos, e pedidos em processamentos - somente aparecerá no modulo administrador (DONO) - Disposição para donwload em excel e pdf - em 4 segundos -Eficiência de Desempenho 
RNF: O gráfico de resumo de vendas deve ser exibido corretamente em dispositivos Android e iOS com diferentes resoluções de tela - Portabilidade
RNF: O acesso ao painel de faturamento deve exigir reautenticação se a sessão estiver inativa por mais de 15 minutos - Segurança

Dado que o dia comercial termina, quando o restaurante abre o painel de vendas, então vê o total de pedidos e o faturamento do dia.
Dado que o restaurante seleciona um período diferente, quandoaplica o filtro, então o resumo é recalculado para aquele período
