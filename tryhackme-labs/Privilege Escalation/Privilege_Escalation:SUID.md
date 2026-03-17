# Privilege Escalation: SUID


O SUID permite que um programa seja executado com os privilégios do proprietário do ficheiro.

Se o proprietário for root, qualquer utilizador que execute o programa poderá executá-lo com privilégios de root.

Exemplo de permissões
```
-rwsr-xr-x 1 root root 54256 passwd
```
### Encontrar ficheiros SUID

Durante uma auditoria ou teste de penetração em Cibersegurança, é comum procurar ficheiros com SUID ativo.
Comando:
```
find / -perm -4000 2>/dev/null
```
