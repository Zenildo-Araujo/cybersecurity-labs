# Privilege Escalation – SUID

## Objetivo do Laboratório

Compreender como o bit **SUID (Set User ID)** pode ser explorado para realizar **escalada de privilégios** em sistemas Linux. O laboratório consistiu em identificar binários com permissões especiais e analisar como podem permitir acesso a ficheiros sensíveis ou execução com privilégios elevados.

---

## Ferramentas Utilizadas

* Linux Terminal
* find
* nano
* openssl
* unshadow
* John the Ripper
* GTFOBins

---

## Procedimentos Realizados

### 1. Identificação de ficheiros com SUID

Foi utilizado o seguinte comando para localizar binários com o bit **SUID** ativo:

```bash
find / -type f -perm -04000 -ls 2>/dev/null
```

Esse comando lista todos os ficheiros que possuem permissões especiais de execução.

---

### 2. Análise de binários exploráveis

Os binários encontrados foram comparados com a base de dados **GTFOBins**, que apresenta programas do Linux que podem ser explorados para **privilege escalation**.

---

### 3. Exploração do binário nano

Foi identificado que o editor de texto `nano` possuía o bit **SUID** ativo.
Isso permitiu:

* Ler ficheiros protegidos
* Editar ficheiros do sistema com privilégios elevados

Exemplo:

```bash
nano /etc/shadow
```

---

### 4. Extração de hashes de senha

Após obter acesso aos ficheiros sensíveis, os ficheiros `/etc/passwd` e `/etc/shadow` foram combinados utilizando:

```bash
unshadow passwd.txt shadow.txt > passwords.txt
```

O ficheiro resultante pode ser utilizado para tentativa de quebra de hash com **John the Ripper**.

---

### 5. Criação de um utilizador com privilégios root

Outra abordagem consiste em criar um utilizador com privilégios administrativos.

Primeiro foi gerado um hash de senha:

```bash
openssl passwd senha123
```

Depois foi adicionada uma nova entrada no ficheiro `/etc/passwd`, permitindo login com privilégios de root.

---

## Lições Aprendidas

* O bit **SUID** pode representar um risco de segurança quando aplicado a binários inadequados.
* A enumeração de ficheiros com permissões especiais é essencial durante testes de **privilege escalation**.
* Ferramentas como **GTFOBins** ajudam a identificar rapidamente possíveis vetores de exploração.
* O acesso a ficheiros sensíveis como `/etc/shadow` pode permitir a quebra de credenciais.
* Alterações no ficheiro `/etc/passwd` podem resultar em acesso administrativo ao sistema.
