# App SECC — Fase 1: Fundação (Login + Estrutura Organizacional)

Esta é a primeira fase do sistema: autenticação, permissões e a estrutura
Secretaria → Subsecretaria → Núcleo. Os próximos módulos (Cadastro de
profissionais, Produção, Folgas, Relatório, Prestação de Contas) serão
construídos em cima dessa base, nas próximas etapas.

## Como colocar isso no ar

### 1) Google Sheets (banco de dados)
1. Crie uma planilha nova no Google Sheets.
2. Copie o ID dela na URL: `docs.google.com/spreadsheets/d/ESTE_ID/edit`.

### 2) Google Apps Script (backend)
1. Na própria planilha: **Extensões > Apps Script**.
2. Apague o conteúdo padrão e cole o arquivo `backend/Codigo.gs`.
3. Substitua `COLOQUE_AQUI_O_ID_DA_SUA_PLANILHA` pelo ID copiado no passo anterior.
4. Rode a função `configurarPlanilhaInicial` uma única vez (menu **Executar**), pra criar todas as abas e cabeçalhos automaticamente. Na primeira vez o Google vai pedir autorização — é normal, autorize.
5. Crie manualmente **uma linha na aba USUARIOS** para o primeiro Administrador (é o único usuário que não é criado "em cascata", já que não existe ninguém acima dele). Preencha `Email`, `NivelAcesso = ADMIN`, `Ativo = TRUE`, `PrimeiroAcesso = TRUE`, e gere a senha provisória rodando a função auxiliar abaixo pelo editor (ou me avise que eu te dou um pequeno script pra fazer isso automaticamente).
6. Publique: **Implantar > Nova implantação > Tipo: App da Web**. Em "Executar como", escolha "Eu". Em "Quem pode acessar", escolha "Qualquer pessoa". Copie a URL gerada.

### 3) GitHub Pages (front-end)
1. Suba o arquivo `frontend/index.html` para um repositório no GitHub.
2. Ative o GitHub Pages nas configurações do repositório, apontando pra branch onde está o arquivo.
3. Abra `index.html` e substitua `COLE_AQUI_A_URL_DO_SEU_APPS_SCRIPT_WEB_APP` pela URL copiada no passo anterior.

## Estrutura de abas já criada nesta fase

| Aba | Para que serve |
|---|---|
| `SECRETARIAS` | Cadastro das secretarias (só o Admin cria) |
| `SUBSECRETARIAS` | Cadastro das subsecretarias, vinculadas a uma secretaria |
| `NUCLEOS` | Núcleos de cada secretaria/subsecretaria, com endereço da Google Agenda |
| `USUARIOS` | Login, senha (com hash), nível de acesso e vínculo hierárquico de cada pessoa |
| `LOG_AUDITORIA` | Registro de quem fez o quê e quando, em todo o sistema |
| `SESSOES` | Controle dos tokens de login ativos |

## O que já funciona nesta fase

- Login com e-mail/senha, senha nunca guardada em texto puro (hash SHA-256 + salt).
- Troca obrigatória de senha no primeiro acesso.
- Recuperação de senha por e-mail.
- Admin cria Secretaria e Subsecretaria.
- Secretário/Subsecretário cria Núcleo dentro da própria área, com endereço da Google Agenda daquele núcleo.
- Criação de usuários em cascata (cada nível só cria o nível imediatamente abaixo do seu).
- Log de auditoria por e-mail em toda ação de criação.
- Cada pessoa só enxerga a estrutura da própria secretaria (menos o Admin, que vê tudo).

## Roadmap dos próximos módulos (nesta ordem sugerida)

1. **Cadastro de profissionais** (aba `REDACAO`): dados sensíveis x dados editáveis pelo próprio profissional, botão "visualizar dados" para a hierarquia acima, campos dinâmicos configuráveis.
2. **Produção**: criação de pauta, papéis da equipe (assessor, vídeo, foto, digital, motorista), cálculo de diária por cidade (aba `REGIAO`), detecção de duplicidade entre núcleos, notificação para profissionais escalados fora do núcleo criador.
3. **Folgas**: leitura das Google Agendas de cada núcleo, reconhecimento do padrão "FOLGA NOME", confirmação de ciência pelo profissional, trava automática 12h após a data.
4. **Plantão**: aviso automático 10 dias antes, regra de segunda equipe e cobertura de feriadão.
5. **Relatório** e **Prestação de Contas**: geração mensal automática a partir dos dados de Produção.

Qualquer ajuste que você quiser nesta fase 1 (nomes de campos, textos, cores), me avisa antes de seguirmos pro próximo módulo.
