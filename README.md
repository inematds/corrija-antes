> **Uso defensivo:** rode só na SUA máquina e nos SEUS repositórios. O kit só lê: não instala, não atualiza, não corrige, não varre outras máquinas e não envia nada: só consulta os servidores de atualização do seu próprio sistema.

# corrija-antes — o relógio de quem defende

[![Corrija antes](guia/assets/banner.jpg)](https://inematds.github.io/corrija-antes/guia/)

**🇧🇷 [Português](README.md) · 🇺🇸 [English](README.en.md) · 🇪🇸 [Español](README.es.md)**

## O que é

Em julho de 2026, as IAs abertas estavam de 4 a 7 meses atrás das fechadas em ataque. Em outubro, num teste de invasão de rede de empresa, essa distância chegou a zero: quem defende perdeu a folga de tempo. corrija-antes é um kit grátis que faz, na sua máquina, as três coisas que ainda funcionam: **atualizar mais rápido, vigiar a rede e achar os seus buracos antes dos outros**. Um script de diagnóstico mostra o que corrigir AGORA e o que pode esperar a semana, e um prompt pronto põe a IA para revisar o seu código. Serve para quem tem computador, servidor ou app no ar e não trabalha com segurança todo dia. Precisa de Python 3 (Linux ou macOS).

## 📖 Guia de uso

Guia completo, com o vídeo: **https://inematds.github.io/corrija-antes/guia/**

Vídeo: [A China soltou de graça a IA hacker que a OpenAI trancou](https://www.youtube.com/watch?v=1WFT-VgGuPM) · [versão vertical](https://youtube.com/shorts/-R65CemeRvc)

## Teste em 1 minuto

```bash
git clone https://github.com/inematds/corrija-antes.git
cd corrija-antes
python3 relogio.py --repo ~/seu-projeto --saida relatorio
```

Sai uma lista na tela e um `relatorio/relatorio.md`. Cada item tem prioridade:

| Prioridade | Quer dizer |
|---|---|
| **AGORA** | correção de segurança esperando, banco/Redis/Ollama/tela remota aberto para a rede, SSH com senha, segredo no código |
| **SEMANA** | atualização comum, porta aberta que você precisa conferir, firewall desligado, imagem Docker com mais de 4 meses |
| **SEM TESTE** | não deu para conferir (falta permissão ou ferramenta): não conta como seguro |
| **OK** | conferido e em dia |

Veja um [relatório de exemplo](exemplos/relatorio-exemplo.md) (servidor fictício de uma loja, dados inventados).

## Os 3 blocos

1. **Atualize**: pacotes do sistema com correção de segurança (apt, dnf, Homebrew, macOS), snaps, pacotes globais do npm, imagens Docker em uso com mais de 4 meses, reinício pendente e **há quantos dias** foi a última atualização.
2. **Vigie a rede**: o que está escutando só na máquina, só na rede interna/VPN ou aberto para qualquer rede; portas críticas (bancos, Redis, Ollama, VNC/RDP, API do Docker); firewall; SSH com senha ou root; portas que o Docker publica **por cima do ufw**.
3. **Seus buracos** (`--repo`): `.env` dentro do git, `.gitignore` sem `.env`, chaves e tokens escritos no código (o valor sai mascarado), dependências sem mudança há mais de 4 meses e o lembrete da revisão com IA.

   Formatos reconhecidos: AWS, OpenAI/Anthropic, Stripe, GitHub, Slack (token e webhook), Google, Telegram, Mercado Pago, Groq, Hugging Face, GitLab, SendGrid, npm, Resend, service_role do Supabase, chave privada, senha em endereço de banco e senhas/chaves do `wp-config.php`. Olha só os arquivos atuais, não o histórico do git; senha genérica (`password=…`) e chave de outros serviços passam sem aviso.

## Opções

```bash
python3 relogio.py                              # só a máquina
python3 relogio.py --repo ~/app --repo ~/api    # máquina + dois repositórios
python3 relogio.py --blocos vigie               # só um bloco: atualize, vigie, codigo
python3 relogio.py --saida relatorio            # grava relatorio.md e relatorio.json
python3 relogio.py --json                       # resultado em JSON na tela
python3 relogio.py --mostrar-ips                # não mascara IPs, usuário e nome da máquina
```

Código de saída: `0` = nada para AGORA · `1` = tem item AGORA · `2` = erro de uso. Sem `sudo`, alguns itens ficam SEM TESTE (ex.: regras do firewall); o script diz qual comando rodar para conferir.

## O processo, do início ao fim

1. 🧠 **Você** clona o kit e roda `python3 relogio.py --repo <seu projeto> --saida relatorio`.
2. ⚙️ **Script** lista os itens AGORA / SEMANA / SEM TESTE / OK e grava o relatório (só lendo).
3. 🧠 **Você** faz backup e confere a saúde do sistema antes de mexer.
4. 🧠 **Você** fecha os itens AGORA, um de cada vez ([ROTINA.md](ROTINA.md)).
5. 🤖 **IA** (Claude Code, Codex ou modelo local) revisa o seu código com o [prompt de revisão](prompts/revisao-com-ia.md).
6. ⚙️ **Script** roda de novo para confirmar; opcional: toda segunda pelo cron.

## Para ir mais fundo (kits INEMA de auditoria e diagnóstico)

| Kit | Para quê |
|---|---|
| [auditoria-seguranca](https://inematds.github.io/auditoria-seguranca/guia/) | testa se suas ferramentas ainda confiam em certificados vazados e se o PHP tem a falha PHP2Shell |
| [blinda-site](https://inematds.github.io/blinda-site/guia/) | diagnóstico do site por dentro: plano, backup, simulação e níveis de risco |
| [invada-seu-proprio-app](https://inematds.github.io/invada-seu-proprio-app/guia/) | as 5 brechas mais comuns de app feito com IA, testadas na sua cópia de teste |
| [agente-blindado](https://inematds.github.io/agente-blindado/guia/) | 5 travas contra injeção de prompt e 10 ataques para testar o seu agente |
| [caixa-do-agente](https://inematds.github.io/caixa-do-agente/guia/) | nota do seu harness de agente e uma caixa Docker descartável, sem rede e sem `.env` |
| [ia-sem-censura-defensiva](https://inematds.github.io/ia-sem-censura-defensiva/guia/) | modelo aberto sem trava, usado só para defesa, rodando na sua máquina |
| [protegendo-seus-repos](https://inematds.github.io/protegendo-seus-repos/guia/) | backup em 3 camadas dos seus repositórios contra bloqueio ou invasão |

## Testes

```bash
python3 testes/run.py        # 72 testes com dados inventados
python3 testes/gera_exemplo.py   # refaz o relatório de exemplo
```

Python 3, sem dependências. Conteúdo autoral. Licença [MIT](LICENSE). Mais em [INEMA.CLUB](https://inema.club).
