Escaner Escritório Contabilidade"

 Novas Escolhas (Abordagens de Solução)

Além da estruturação inicial, aqui estão mais duas opções para o projeto:

1. **Escolha 2 (Foco em OCR de Nuvem):** Utilizar uma API de inteligência artificial (OCR) para extrair dados de boletos com alta precisão, garantindo que o sistema aprenda novos formatos de boletos automaticamente[3].
2. **Escolha 3 (Foco em Integração Direta):** Implementar um "Robô de Captura" que acessa os e-mails dos clientes ou portais de prefeituras para baixar o PDF antes mesmo de ele virar papel, eliminando a necessidade física do malote[3][4].

### Novos Critérios de Aceitação (Dado, Quando, Então)

Com base na História: *"Como usuário, quero um sistema que lê todos os boletos para que o meu tempo seja remido"*[5][6].

* **Critério 2:** Dado que a imagem do boleto foi capturada pela webcam, quando o sistema processar a leitura, então o código de barras deve ser identificado e validado em até 2 segundos[7][8].
* **Critério 3:** Dado que o arquivo CSV foi gerado com as informações extraídas, quando o usuário realizar a importação para o sistema contábil, então os dados devem ser preenchidos nos campos correspondentes sem erros de formatação[9][10].

---

## Parte 2: Novo Caso - "Onde deixei minhas ferramentas?"

Este caso simula um problema comum em ambientes de trabalho compartilhados ou oficinas.

### 1\. Empatia

"Por que eu perco tanto tempo procurando uma ferramenta específica toda vez que preciso dela, sendo que eu sei que ela deveria estar no armário?"

### 2\. Definição

A desorganização e a falta de rastreio de itens compartilhados geram perda de produtividade e estresse, pois o usuário não sabe se o item está guardado no lugar errado ou se está com outra pessoa[11][12].

### 3\. Ideação

1. **App de Inventário com QR Code:** Cada ferramenta e cada gaveta possuem um QR Code; o usuário escaneia ao pegar e ao devolver[13].
2. **Painel Magnético Inteligente:** Um painel que detecta a ausência do metal e acende uma luz vermelha no lugar da ferramenta que saiu.
3. **Chatbot de Localização:** Um bot no WhatsApp onde você digita "onde está a furadeira?" e ele responde com base no último registro de uso.

### 4\. Ideia Escolhida

**App de Inventário com QR Code.** É uma solução de baixo custo e alta portabilidade[14][15].

### 5\. Histórias de Usuário (Como, Quero, Para)

1. **História 1:** Como colaborador, quero **escanear o QR Code de uma gaveta**, para **ver a lista de todas as ferramentas que deveriam estar dentro dela sem precisar abri-la**[5][16].
2. **História 2:** Como gestor, quero **receber um alerta se uma ferramenta cara não for devolvida em 24 horas**, para **evitar perdas e compras desnecessárias**[5][17].
3. **História 3:** Como usuário, quero **buscar o nome de um item no app**, para **saber exatamente em qual armário e prateleira ele está guardado no momento**[6][18].

### 6\. Critérios de Aceitação (Dado, Quando, Então)

1. **Cenário de Busca:** Dado que o usuário pesquisa por "Multímetro", quando o sistema processa a busca, então ele exibe a localização exata (Ex: Armário A, Gaveta 2) e o status (Disponível ou Em uso)[9][18].
2. **Cenário de Retirada:** Dado que o usuário escaneou a ferramenta, quando ele clica em "Retirar", então o sistema registra o horário e o nome do usuário, mudando o status do item para "Ocupado" instantaneamente[9][19].
3. **Cenário de Inventário:** Dado que o QR Code da gaveta foi lido, quando a lista carregar, então o app deve mostrar fotos em miniatura de cada item para facilitar a identificação visual em menos de 1,5 segundo
