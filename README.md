# Claude Code — Troca de Contas

O Claude Code não permite usar mais de uma conta ao mesmo tempo. Se você tem uma conta pessoal e uma do trabalho, precisa fazer logout e login toda vez que quiser trocar — o que é lento e inconveniente.

Esse projeto resolve isso com um comando simples no terminal:

```bash
use-work      # ativa a conta do trabalho
use-personal  # ativa a conta pessoal
```

Os nomes são definidos por você durante a configuração. Funciona com 2 ou 3 contas.

---

## Como funciona

O Claude Code salva a sessão ativa em arquivos no seu computador. Esse projeto mantém um backup separado para cada conta e troca esses arquivos quando você pede — sem precisar fazer login de novo.

---

## Segurança

Nenhuma credencial passa por esse script ou fica nesse repositório. Tudo fica salvo localmente na sua máquina, protegido pelas permissões do seu sistema operacional.

Para uma camada extra de proteção, ative a criptografia de disco: **FileVault** no macOS ou **LUKS** no Linux.

---

## Como instalar

**1. Baixe os arquivos**

Clique em **Code → Download ZIP** no GitHub, extraia a pasta.

Ou, se você usa git, abra o **terminal do seu computador** (no Mac: aplicativo Terminal, no Windows: Prompt de Comando) e rode:

```bash
git clone <url-do-repositorio>
```

**2. Abra o terminal do seu computador na pasta baixada**

No Mac: clique com o botão direito na pasta → "Novo Terminal na Pasta".  
No Windows: abra a pasta no Explorer → clique na barra de endereço, digite `cmd` e pressione Enter.

**3. Rode o script de instalação**

No terminal do seu computador, execute:

```bash
bash setup-claude-accounts.sh
```

O script vai te guiar pelo restante: perguntar quantas contas você quer configurar, pedir um nome para cada uma e abrir o login do Claude para cada conta.

**4. Recarregue o terminal**

Ainda no terminal do seu computador, rode:

```bash
source ~/.zshrc
```

Pronto. Use os comandos que você definiu para trocar de conta. Depois de trocar, **reinicie o VS Code** para a mudança ter efeito.

---

Para o guia técnico completo, veja o [INSTALLATION.md](INSTALLATION.md).

---

## Créditos

Feito com carinho por [Juliana Miller](https://github.com/Julianamiller).

---

## Licença

MIT — use, modifique e distribua à vontade, mantendo o crédito.
