# Scanner-de-Portas-TCP-com-Multithreading-em-Python
Ferramenta em Python para varredura rápida de portas TCP usando multithreading e fila (Queue) para processamento concorrente.

🔍 Port Scanner Multithreaded em Python

Este projeto implementa um scanner de portas TCP de alta performance utilizando multithreading para paralelizar tentativas de conexão.
O objetivo é identificar portas abertas em um host alvo por meio de conexões TCP síncronas com timeout reduzido.

⚙️ Funcionamento Técnico

Definição do alvo
O usuário especifica um endereço IPv4/hostname a ser analisado.

Geração da lista de portas
Um intervalo de portas (por padrão 1–1023) é inserido em uma estrutura thread-safe Queue.

Criação de múltiplas threads
São instanciadas centenas de threads, cada uma executando a função worker() de maneira concorrente.

Execução da função portscan()
Cada thread retira uma porta da fila e tenta estabelecer uma conexão TCP (socket.SOCK_STREAM) com timeout de 0.3 segundos.

Registro das portas abertas
Caso a conexão seja bem-sucedida, a porta é adicionada à lista global open_ports.

Sincronização
O programa aguarda (join()) todas as threads finalizarem antes de exibir o resultado final.

🧵 Justificativa do Design Multithreaded

Scanners sequenciais sofrem com tempo de espera acumulado devido a timeouts de rede.
O uso de threads permite aproveitar latência ociosa, distribuindo milhares de tentativas de conexão simultaneamente, reduzindo drasticamente o tempo total de varredura.

📄 Estrutura das Funções
portscan(port)

Função responsável por:

instanciar um socket TCP,

aplicar timeout,

tentar conectar à porta,

retornar True caso a porta esteja aberta.

fill_queue(port_list)

Insere portas na fila compartilhada (Queue) para serem consumidas pelas threads.

worker()

Loop executado por cada thread, responsável por:

retirar portas da fila,

chamar portscan(),

registrar portas abertas.

Threading principal

Responsável por:

instanciar a lista de threads,

inicializar e sincronizar (start() / join()),

gerar a saída final.

📌 Exemplo de Código
import socket
import threading
from queue import Queue

target = "***.*.*.*"  # The target IP address for the scan; 'localhost', or the local machine.
queue = Queue()
open_ports = []

def portscan(port):
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(0.3)
        sock.connect((target, port))
        sock.close
        return True
    except:
        return False

def fill_queue(port_list):
    for port in port_list:
        queue.put(port)

def worker():
    while not queue.empty():
        port = queue.get()
        if portscan(port):
            print("port {} is open!".format(port))
            open_ports.append(port)

port_list = range(1, 1024)
fill_queue(port_list)

thread_list = []

for t in range(500):
    thread = threading.Thread(target=worker)
    thread_list.append(thread)

for thread in thread_list:
    thread.start()

for thread in thread_list:
    thread.join()

print("open ports are :", open_ports)

⚠️ Aviso Importante

Este software deve ser utilizado exclusivamente para fins educacionais e em ambientes onde você possui permissão explícita para realizar testes de segurança.
Realizar port scanning em sistemas de terceiros sem autorização pode violar leis e políticas de uso
