# Lista de Exercícios — Segurança Digital 

| Campo | Preenchimento |
|---|---|
| Nome do(a) aluno(a) | Iago Vargas|
| Matrícula | 2023010193 |
| Data de entrega | 05 / 09 / 2026 |

---

## Parte 1 — Visão Geral da Segurança da Informação (Aula 01)

### Questão 1 — Os três pilares da tríade CIA

- Confidencialidade
A tríade CIA é o modelo base da segurança da informação: todo controle de segurança existe para sustentar pelo menos um desses três pilares.
Garantia de que a informação só é acessível a quem tem autorização explícita para vê-la. Envolve controle de acesso, autenticação e criptografia, se um dado é lido por quem não deveria, houve quebra de confidencialidade, mesmo que nada tenha sido alterado ou destruído.
> **Exemplo de violação:** um funcionário do setor administrativo de uma clínica, sem qualquer necessidade de trabalho, acessa o prontuário eletrônico de um paciente conhecido por curiosidade e tira uma foto da tela com o celular. Nenhum dado foi alterado nem ficou indisponível, mas a informação chegou a quem não tinha autorização.

- Integridade
Garantia de que a informação não foi alterada de forma indevida ou não autorizada, seja por ação maliciosa, erro humano ou falha técnica. Envolve hashes, assinaturas digitais, logs de auditoria e controle de versão.
> **Exemplo de violação:** um atacante em posição de *man-in-the-middle* na rede Wi-Fi de uma empresa intercepta o PDF de um boleto enviado por e-mail e altera a linha digitável e a chave Pix antes de repassá-lo ao destinatário. O documento chega, é legível e parece autêntico mas o conteúdo já não corresponde ao original.

- Disponibilidade
Garantia de que a informação e os serviços estejam acessíveis aos usuários legítimos sempre que forem necessários. Envolve redundância, backup, balanceamento de carga e plano de recuperação de desastres.
> **Exemplo de violação:** um ransomware criptografa os servidores de arquivos de uma prefeitura e o sistema de emissão de alvarás e notas fiscais fica fora do ar por cinco dias. Os dados não vazaram e continuam íntegros dentro dos arquivos criptografados, mas ninguém consegue usá-los.

---

### Questão 2 — Associação de termos

| Termo | Letra | Definição |
|:---|:---:|:---|
| **Ativo** | **c** | Qualquer recurso de valor para a organização (dado, sistema, pessoa, reputação). |
| **Ameaça** | **e** | Evento ou agente potencialmente capaz de causar dano a um ativo. |
| **Vulnerabilidade** | **a** | Fraqueza que pode ser explorada para comprometer um ativo. |
| **Risco** | **d** | Probabilidade de uma ameaça explorar uma vulnerabilidade, combinada ao impacto resultante. |
| **Controle** | **b** | Medida (técnica, administrativa ou física) para reduzir a exposição a um risco. |

---

### Questão 3 — Cálculo da sub-rede 192.168.10.0/26

#### a) Máscara de sub-rede em notação decimal

```
255.255.255.192
```

#### b) Número de hosts utilizáveis

```
2⁶ − 2 = 64 − 2 = 62 hosts utilizáveis
```

Subtraem-se 2 porque o primeiro endereço do bloco é o **endereço de rede** e o último é o **endereço de broadcast**, nenhum dos dois pode ser atribuído a uma interface.

#### c) Endereço de broadcast

```
192.168.10.63
```

#### d) Primeiro e último endereço de host válidos

```
Primeiro host válido: 192.168.10.1
Último host válido:   192.168.10.62
```

#### Resumo da sub-rede

| Item | Valor |
|:---|:---|
| Endereço de rede | 192.168.10.0 |
| Máscara | 255.255.255.192 (/26) |
| Primeiro host | 192.168.10.1 |
| Último host | 192.168.10.62 |
| Broadcast | 192.168.10.63 |
| Total de endereços | 64 |
| Hosts utilizáveis | 62 |
| Próxima sub-rede | 192.168.10.64/26 |

---

### Questão 4 — Fases de um teste de invasão (pentest)

As cinco fases clássicas, na ordem em que são executadas:

#### 1. Reconhecimento 
Coleta do máximo de informação possível sobre o alvo **antes** de tocar nele de forma agressiva. Divide-se em:

- **Passivo:** sem interagir diretamente com a infraestrutura do alvo consultas WHOIS, registros DNS, Google Hacking, LinkedIn, vazamentos públicos, metadados de documentos.
- **Ativo:** já há contato com o alvo — resolução de subdomínios, banner grabbing, ping sweep.

#### 2. Varredura e Enumeração
Identificação técnica do que está exposto: hosts vivos, portas abertas, serviços e versões, sistema operacional, compartilhamentos, usuários, diretórios web. É aqui que entram ferramentas como Nmap, Nessus/OpenVAS, enum4linux e Gobuster.

#### 3. Obtenção de acesso 
Exploração efetiva das vulnerabilidades encontradas para conseguir execução de código, credenciais ou uma sessão no sistema — exploits públicos, injeção de SQL, força bruta, engenharia social, Metasploit.

#### 4. Manutenção do acesso 
Garantir persistência no ambiente comprometido e avançar a partir dele: escalada de privilégios (de usuário comum para root/Administrator), movimentação lateral para outras máquinas, coleta de credenciais e criação de backdoors controlados.

#### 5. Limpeza de rastros e Relatório 
Em um pentest autorizado, esta fase é essencialmente de **higiene e documentação**: remover artefatos deixados no ambiente (backdoors, contas de teste, arquivos enviados), devolver o sistema ao estado original e entregar o relatório final com escopo, metodologia, evidências, classificação de severidade e recomendações de correção.

---

### Questão 5 — O processo DORA do DHCP

O DHCP (Dynamic Host Configuration Protocol) atribui configuração de rede automaticamente. Usa **UDP**, com o **servidor na porta 67** e o **cliente na porta 68**. O nome DORA vem das iniciais das quatro mensagens trocadas.

#### D — Discover (Descoberta)
O cliente acaba de entrar na rede e **não tem endereço IP nenhum**. Por isso envia um `DHCPDISCOVER` em **broadcast**, com endereço de origem `0.0.0.0` e destino `255.255.255.255`. A mensagem carrega o endereço MAC do cliente, que é o que o identifica nesta etapa.
> *"Existe algum servidor DHCP nesta rede? Preciso de um endereço."*

#### O — Offer (Oferta)
Todo servidor DHCP que receber o Discover reserva temporariamente um endereço livre do seu escopo e responde com um `DHCPOFFER`. A oferta inclui o IP proposto, a máscara de sub-rede, o gateway padrão, os servidores DNS e o tempo de concessão.
> *"Posso te emprestar o 192.168.1.50, com máscara /24, gateway .1, por 24 horas."*

#### R — Request (Requisição)
O cliente pode receber mais de uma oferta (redes com vários servidores DHCP). Ele escolhe uma — normalmente a primeira que chegar — e envia um `DHCPREQUEST`, **também em broadcast**, identificando qual servidor foi aceito. O broadcast aqui é proposital: serve para avisar os demais servidores que suas ofertas foram recusadas, liberando os endereços que haviam reservado.
> *"Aceito a oferta do servidor X. Os demais podem liberar o que reservaram."*

#### A — Acknowledge (Confirmação)
O servidor escolhido confirma com um `DHCPACK`, grava a concessão na sua base de leases e reenvia os parâmetros definitivos. O cliente então aplica a configuração na interface — normalmente enviando antes um ARP gratuito para checar se ninguém mais está usando aquele IP.
> *"Confirmado. O 192.168.1.50 é seu até amanhã neste horário."*

---

### Questão 6 — NAT estático, NAT dinâmico e PAT

O NAT (Network Address Translation) traduz endereços IP privados (RFC 1918) em endereços públicos roteáveis na Internet. Existem três variações:

#### NAT Estático (*Static NAT*)
Mapeamento **1:1 fixo e permanente** entre um IP privado e um IP público, configurado manualmente pelo administrador. O host interno sempre sai com o mesmo endereço público, e conexões vindas de fora conseguem alcançá-lo.
- **Uso típico:** servidores internos que precisam ser acessíveis a partir da Internet (web, e-mail, VPN).
- **Limitação:** exige um IP público para cada host — não economiza endereços.

#### NAT Dinâmico (*Dynamic NAT*)
Mapeamento **1:1 também, mas temporário**. O roteador mantém um *pool* de IPs públicos e aloca um deles ao host interno no momento em que ele inicia uma conexão, devolvendo-o ao pool quando a sessão termina.
- **Uso típico:** empresas que possuem um bloco de IPs públicos menor que o número de estações, mas nunca com todas conectadas ao mesmo tempo.
- **Limitação:** se o pool se esgotar, os próximos hosts simplesmente ficam sem tradução e sem acesso externo. Como o IP muda a cada sessão, não serve para receber conexões de fora.

#### PAT (*Port Address Translation* / NAT Overload)
Mapeamento **N:1** muitos hosts internos compartilham **um único** IP público. A distinção é feita pela **porta de origem**: o roteador reescreve a porta de cada conexão e guarda em uma tabela de tradução a associação entre `IP privado:porta` e `IP público:porta traduzida`.

#### Qual é a mais usada em redes domésticas e por quê?
**O PAT (NAT overload).** A razão é direta: o provedor de Internet entrega ao assinante residencial **um único endereço IP público**, enquanto a casa tem celulares, notebooks, TVs, consoles e dispositivos IoT — facilmente vinte ou mais dispositivos. Só o PAT permite que todos eles compartilhem esse único IP simultaneamente, multiplexando as conexões pelas portas de origem.
É também o mecanismo que mais adiou o esgotamento do IPv4, e vem habilitado por padrão em praticamente todo roteador doméstico. Como efeito colateral, ele funciona como um firewall implícito: como não existe entrada pré-mapeada na tabela de tradução, conexões não solicitadas vindas da Internet não têm para onde ser encaminhadas e são descartadas por isso é preciso configurar *port forwarding* manualmente para expor um serviço interno.

---
### Questão 7 — Three-way handshake do TCP e o SYN Scan

#### As três etapas do handshake

O TCP é orientado à conexão: antes de trocar dados, cliente e servidor sincronizam seus números de sequência em três passos.

**1. SYN — Cliente → Servidor**

O cliente envia um segmento com a flag `SYN` ligada, contendo seu número de sequência inicial (ISN). É o pedido de abertura de conexão.

```
Cliente ──── SYN (seq=x) ────► Servidor
```

**2. SYN-ACK — Servidor → Cliente**

Se a porta estiver aberta e aceitando conexões, o servidor responde com `SYN` **e** `ACK` ligados ao mesmo tempo: confirma o número do cliente (`ack = x+1`) e envia o seu próprio ISN (`seq = y`).

```
Cliente ◄─── SYN-ACK (seq=y, ack=x+1) ──── Servidor
```

**3. ACK — Cliente → Servidor**

O cliente confirma o número de sequência do servidor (`ack = y+1`). A conexão passa ao estado `ESTABLISHED` e os dados podem começar a trafegar.

```
Cliente ──── ACK (ack=y+1) ────► Servidor
                                  [ESTABLISHED]
```

#### Como o SYN Scan (`-sS`) se aproveita disso

O SYN Scan — também chamado de ***half-open scan*** ou "varredura meio-aberta" — deliberadamente **interrompe o handshake no segundo passo**. O Nmap não usa a pilha TCP do sistema operacional: ele monta pacotes brutos (*raw packets*) e interpreta as respostas diretamente.

```
    Porta ABERTA                    Porta FECHADA
─────────────────────           ─────────────────────
 Nmap ── SYN ──►                 Nmap ── SYN ──►
 Nmap ◄─ SYN-ACK ─               Nmap ◄─ RST-ACK ─
 Nmap ── RST ──►                        (fim)
   (aborta)
```
---

### Questão 8 — Interpretação dos comandos Nmap

#### a) `nmap -sS -p 1-1000 192.168.1.0/24`

**resumo:** faz uma varredura SYN das mil primeiras portas TCP em todos os hosts da rede 192.168.1.0/24. É o comando típico de **mapeamento inicial** de um segmento de rede — descobre quais máquinas existem e quais serviços comuns estão expostos.

#### b) `nmap -sV -O 192.168.1.10`

**resumo:** varre o host único 192.168.1.10 identificando os serviços com suas versões e tentando descobrir o sistema operacional. É a fase de **enumeração**: saber a versão exata é o que permite pesquisar CVEs aplicáveis. O `-O` também exige root.

#### c) `nmap -sU -p 53,161 192.168.1.10`

**resumo:** verifica se o host está executando serviços DNS e SNMP. São dois alvos clássicos: um DNS mal configurado permite transferência de zona ou amplificação de DDoS, e o SNMP com *community string* padrão (`public`/`private`) entrega o inventário completo do dispositivo.

#### d) `nmap -T4 -A -oN resultado.txt 192.168.1.10`

**resumo:** executa a varredura mais completa e barulhenta do conjunto contra o host 192.168.1.10, de forma rápida, e salva tudo em arquivo para análise e documentação posteriores. É o comando de **enumeração profunda** — muito ruidoso, facilmente detectado por IDS/IPS, e por isso restrito a ambientes autorizados.

---

### Questão 9 — Análise da saída de `ls -l`

```
-rwxr-xr-- 1 root root 4096 ago 24 10:00 scan.sh
```

#### Campos da saída completa

| # | Campo | Valor | Significado |
|:--:|:---|:---|:---|
| 1 | Tipo + permissões | `-rwxr-xr--` | Tipo do arquivo e os três conjuntos de permissão |
| 2 | Contador de links | `1` | Número de *hard links* apontando para o inode |
| 3 | Proprietário | `root` | Usuário dono do arquivo |
| 4 | Grupo | `root` | Grupo dono do arquivo |
| 5 | Tamanho | `4096` | Tamanho em bytes |
| 6 | Data/hora | `ago 24 10:00` | Última modificação (*mtime*) |
| 7 | Nome | `scan.sh` | Nome do arquivo |

#### Conversão para notação octal
Cada conjunto de três bits vira um dígito:

| Permissão | Peso |
|:---:|:---:|
| `r` (leitura) | **4** |
| `w` (escrita) | **2** |
| `x` (execução) | **1** |

| Conjunto | Bits | Cálculo | Dígito |
|:---|:---:|:---|:---:|
| Dono | `rwx` | 4 + 2 + 1 | **7** |
| Grupo | `r-x` | 4 + 0 + 1 | **5** |
| Outros | `r--` | 4 + 0 + 0 | **4** |

> ### Resposta: **754**
>
> Equivalente ao comando `chmod 754 scan.sh`

---

### Questão 10 — `su` vs. `sudo` e por que o Nmap precisa de privilégios

#### Diferença entre `su` e `sudo`

**`su` — *substitute user***

Troca a identidade da sessão inteira: abre um novo shell atuando como outro usuário (por padrão, `root`). Exige a **senha do usuário de destino** ou seja, quem usa `su` para virar root precisa conhecer a senha do root. A partir daí, **todos** os comandos digitados rodam com privilégio total até que se execute `exit`.

```bash
su          # vira root, mantendo boa parte do ambiente atual
su -        # shell de login: carrega o ambiente completo do root (PATH, HOME)
su iago     # troca para o usuário iago
```

**`sudo` — *superuser do***

Executa **um único comando** com privilégios elevados. Exige a **senha do próprio usuário**, não a do root, e a autorização é definida pelo administrador no arquivo `/etc/sudoers` (editado com `visudo`).

```bash
sudo nmap -sS 192.168.1.10   # só este comando roda como root
sudo -i                      # abre um shell root interativo (equivale a su -)
sudo -u iago comando         # executa como outro usuário específico
```

#### Por que o Nmap exige `sudo` para scans como o `-sS`
Em uma conexão normal, um programa chama `connect()` e o kernel cuida de todo o handshake. Mas o `-sS` precisa fazer algo que a API normal de sockets **não permite**: enviar um `SYN` e depois responder com `RST` em vez de completar o handshake. Não existe forma de pedir isso ao kernel pela interface convencional.

---
