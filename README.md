🔍 Scanner de Portas TCP com Multithreading em Python

Este repositório contém uma ferramenta de varredura (scan) de portas TCP desenvolvida em Python, projetada para oferecer alto desempenho através de multithreading e uso da estrutura thread-safe Queue.
O objetivo é identificar portas abertas em um host alvo por meio de conexões TCP rápidas e concorrentes.

⚙️ Como o Scanner Funciona
1️⃣ Definição do alvo

O usuário informa um endereço IPv4 ou hostname para ser analisado.

2️⃣ Geração da lista de portas

Um intervalo de portas (por padrão 1–1023) é armazenado em uma Queue, garantindo segurança e controle no acesso por múltiplas threads.

3️⃣ Criação das threads

O programa cria diversas threads, cada uma responsável por processar uma porta de forma simultânea, executando a função worker().

4️⃣ Tentativa de conexão

Cada worker utiliza a função portscan() para:

criar um socket TCP,

definir timeout reduzido (0.3s),

tentar conexão com a porta atual,

registrar portas abertas caso a conexão seja bem-sucedida.

5️⃣ Sincronização

O programa aguarda todas as threads finalizarem (join()) para então exibir o resultado consolidado.

🧠 Por que utilizar Multithreading?

Scanners tradicionais, sequenciais, sofrem com:

tempo somado de múltiplos timeouts,

latência natural da rede,

processamento linear pouco eficiente.

Com threads, o trabalho é distribuído entre diversas rotinas simultâneas, permitindo testar centenas de portas ao mesmo tempo. O resultado é uma varredura muito mais rápida, especialmente útil em redes de maior latência.

🧩 Principais Funções
portscan(port)

Responsável por:

instanciar um socket TCP (socket.SOCK_STREAM);

aplicar timeout;

tentar conexão;

retornar True quando a porta está aberta.

fill_queue(port_list)

Adiciona todas as portas da lista à fila compartilhada (Queue), de onde serão consumidas pelas threads.

worker()

Executado por cada thread. Faz o loop:

retira uma porta da fila,

chama portscan(),

registra portas abertas em open_ports.

Thread principal

Controla:

criação e inicialização das threads (start()),

sincronização delas (join()),

impressão do resultado final:

print("Open ports:", open_ports)

🛡️ Aviso Importante

Este scanner deve ser utilizado somente para fins educacionais, testes pessoais ou auditoria em ambientes onde você possui permissão explícita.
O uso indevido pode violar leis de segurança digital

print("open ports are :", open_ports)

⚠️ Aviso Importante

Este software deve ser utilizado exclusivamente para fins educacionais e em ambientes onde você possui permissão explícita para realizar testes de segurança.
Realizar port scanning em sistemas de terceiros sem autorização pode violar leis e políticas de uso
