# 🔍 Scanner de Portas TCP com Multithreading em Python

Este repositório contém uma ferramenta de varredura (scan) de portas TCP desenvolvida em Python, projetada para oferecer alto desempenho através de multithreading e uso da estrutura thread-safe **Queue**.  
O objetivo é identificar portas abertas em um host alvo por meio de conexões TCP rápidas e concorrentes.

---

## ⚙️ Como o Scanner Funciona

### 1️⃣ Definição do alvo  
O usuário informa um endereço IPv4 ou hostname para ser analisado.

### 2️⃣ Geração da lista de portas  
Um intervalo de portas (por padrão **1–1023**) é armazenado em uma `Queue`, garantindo segurança e controle no acesso por múltiplas threads.

### 3️⃣ Criação das threads  
O programa cria diversas threads, cada uma responsável por processar uma porta de forma simultânea, executando a função `worker()`.

### 4️⃣ Tentativa de conexão  
Cada worker utiliza a função `portscan()` para:

- criar um socket TCP,  
- definir timeout reduzido (0.3s),  
- tentar conexão com a porta atual,  
- registrar portas abertas caso a conexão seja bem-sucedida.

### 5️⃣ Sincronização  
O programa aguarda todas as threads finalizarem (`join()`) para então exibir o resultado consolidado.

---

## 🧠 Por que utilizar Multithreading?

Scanners sequenciais sofrem com:

- tempo somado de múltiplos timeouts,  
- latência natural da rede,  
- processamento linear pouco eficiente.

Com **multithreading**, o trabalho é distribuído entre várias rotinas simultâneas, permitindo testar **centenas de portas ao mesmo tempo**.  
O resultado é uma varredura **muito mais rápida**, especialmente útil em redes de maior latência.

---

## 🧩 Principais Funções

### **portscan(port)**
Responsável por:

- instanciar um socket TCP (`socket.SOCK_STREAM`),  
- aplicar timeout,  
- tentar conexão,  
- retornar **True** quando a porta está aberta.

---

### **fill_queue(port_list)**
Adiciona cada porta à fila compartilhada (`Queue`), consumida pelas threads worker.

---

### **worker()**
Executado por cada thread, realizando:

- retirar uma porta da fila,  
- chamar `portscan()`,  
- registrar portas abertas em `open_ports`.

---

### **Thread principal**
Responsável por:

- criar e iniciar as threads (`start()`),  
- sincronizar todas (`join()`),  
- imprimir o resultado final:

```python
print("Open ports:", open_ports)
```

---

## 🛡️ Aviso Importante

Este scanner deve ser utilizado somente para:

- fins educacionais,  
- testes pessoais,  
- auditoria em ambientes onde você possui permissão explícita.

O uso indevido pode infringir leis de segurança digital.

---

## ⚠️ Aviso Legal

Este software é **exclusivamente para fins educacionais** e deve ser executado apenas em sistemas onde você tem autorização explícita para realizar port scanning.  
Varreduras não autorizadas podem violar leis e políticas de uso.

