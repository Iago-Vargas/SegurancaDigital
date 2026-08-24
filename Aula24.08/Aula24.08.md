# Kali Linux
- Distruibuicao Debian para testes de invasao e auditoria de seguranca
- Estrutura de sistema: FHS 

## Comandos
- cp origem destino - Copia o arquivo ou destino
- mv origem destino - Move ou renomeia o arquivo
- rm arquivo - Remove arquivo
- cat arquivo - Abre todo conteudo do arquivo
- less arquivo - Abre todo arquivo mas com rolagem
- nano arquivo - Editor do texto via terminal

## Coringas
- ls * .txt - Lista somente os que possuem final do dominio que voce quiser
- ls img[abs].png - Qualquer caracter dentro do arquivo

## Encontrar arquivos
> Find e Locate executam essa busca por toda arvore com nome, tipo, data e permissao
- find . -type f -active -7 - Arquivo modificados nos outros 7 dias

# CHMOD, CHOWN
- O CHMOD altera as permissoes de um arquivo
- O CHOWN altera o dono do grupo ou do arquivo

# Buscas
- history - Verificamos todos comandos rodados na Machine
- Ctrol + R - Coloca um pedaco do comando que já foi rodado que ele busca no history e tras na exibicao

# Processos
- ps - Lista processos que estao em execucao na sessao atual
- ps aux - Lista todos os processos do sistema, com todos usuarios
- top / htop - Monitor interativo de processos em tempo real. (CPU, Memoria)
- kill PID - Encerra o processo pelo identificador

# Espaco em Disco
- df -lh - Espaco livre e usado por particao, formato legivel
- dv -sh pasta/ - Espaco livre de uma pasta especifica

# Redes
- ip a - Mostra interface de rede e enderecos ip
- curl / wget url - Faz requisicoes HTTP direto do terminal
- nc host porta - Conecta manualmente na porta especifica