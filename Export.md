# Exportando um Repositório Git Local para o Azure DevOps

Este guia detalha os passos para exportar (ou "enviar") um repositório Git local existente para um novo repositório no Azure DevOps.

## Passo 1: Criar um Novo Repositório Vazio no Azure DevOps

1.  **Acesse o Azure DevOps:**

    - Faça login na sua organização e navegue até o projeto desejado.

2.  **Vá para "Repos":**

    - No menu lateral do projeto, clique em "Repos" (Repositórios).

3.  **Crie um Novo Repositório:**

    - Clique no botão "New repository" (Novo repositório) ou no seletor de repositórios no canto superior e escolha "New repository".

4.  **Configurações do Novo Repositório:**

    - **Repository type (Tipo de repositório):** Selecione "Git".
    - **Repository name (Nome do repositório):** Dê um nome ao seu repositório (ex: `pgats-ci-lab` ou o nome do seu projeto).
    - **Importante:** **Não** marque as opções para inicializar o repositório com um `README`, `.gitignore` ou licença. O objetivo é criar um repositório completamente vazio para receber seu código existente.

5.  **Copie o URL do Repositório:**
    - Após a criação, o Azure DevOps exibirá o URL do novo repositório. Copie o URL HTTPS (recomendado para começar) ou SSH (se você tiver chaves SSH configuradas).
    - O URL HTTPS terá um formato similar a:
      `https://<SUA_ORGANIZACAO>@dev.azure.com/<SUA_ORGANIZACAO>/<SEU_PROJETO>/_git/<NOME_DO_REPOSITORIO>`

## Passo 2: Configurar o Remote do Azure DevOps no seu Repositório Local

Abra seu terminal (Git Bash, PowerShell, CMD, ou o terminal integrado do VS Code) na pasta raiz do seu projeto local (ex: `pgats-ci-lab`).

1.  **Verificar Remotes Existentes (Opcional):**
    Para ver se já existem remotes configurados (como um `origin` para o GitHub):

    ```bash
    git remote -v
    ```

2.  **Adicionar o Repositório do Azure DevOps como um Novo Remote:**
    Você dará um nome a este novo remote. Um nome comum é `azure_devops`, `ado`, ou `origin` (se este for se tornar o principal destino).

    ```bash
    git remote add <NOME_DO_REMOTE> <URL_DO_SEU_REPOSITORIO_NO_AZURE_DEVOPS>
    ```

    Substitua `<NOME_DO_REMOTE>` pelo nome que você escolheu (ex: `azure_devops`) e `<URL_DO_SEU_REPOSITORIO_NO_AZURE_DEVOPS>` pelo URL copiado no Passo 1.

    Exemplo:

    ```bash
    git remote add azure_devops https://sua_org@dev.azure.com/sua_org/MeuProjeto/_git/pgats-ci-lab
    ```

3.  **Verificar a Adição do Novo Remote:**
    Confirme se o remote foi adicionado corretamente:
    ```bash
    git remote -v
    ```
    Você deverá ver o `<NOME_DO_REMOTE>` listado com os URLs de `fetch` (busca) e `push` (envio).

## Passo 3: Enviar (Push) seu Repositório para o Azure DevOps

1.  **Enviar Todos os Branches Locais:**
    Para enviar todos os seus branches locais para o novo remote:

    ```bash
    git push <NOME_DO_REMOTE> --all
    ```

    Exemplo:

    ```bash
    git push azure_devops --all
    ```

    _Alternativa (enviar branch atual e configurar rastreamento):_
    Se quiser enviar apenas o branch atual (ex: `main`) e configurá-lo para rastrear o branch correspondente no remote:

    ```bash
    git push -u <NOME_DO_REMOTE> <NOME_DO_BRANCH_LOCAL>
    ```

    Exemplo:

    ```bash
    git push -u azure_devops main
    ```

2.  **Enviar Todas as Tags (se aplicável):**
    Se seu repositório utiliza tags (para marcar versões, por exemplo):
    ```bash
    git push <NOME_DO_REMOTE> --tags
    ```
    Exemplo:
    ```bash
    git push azure_devops --tags
    ```

## Passo 4: Verificar no Azure DevOps

1.  Retorne à página do seu repositório no portal do Azure DevOps.
2.  Atualize a página (F5).
3.  Você deverá ver todos os seus arquivos, o histórico de commits e os branches que foram enviados.

## Observações Importantes

- **Autenticação:** Ao executar `git push`, o Git solicitará suas credenciais do Azure DevOps.
  - **Personal Access Token (PAT):** É a forma mais segura e recomendada. Crie um PAT no Azure DevOps (em "User settings" -> "Personal access tokens") com as permissões adequadas (pelo menos "Code: Read & Write"). Use seu nome de usuário do Azure DevOps e o PAT como senha no prompt do Git.
  - **Gerenciador de Credenciais:** Sistemas como Windows e macOS, ou o Git Credential Manager, podem armazenar suas credenciais após o primeiro login bem-sucedido.
- **Se já existe um `origin`:**
  - Os passos acima adicionam o Azure DevOps como um _novo_ remote. Seu `origin` original (ex: GitHub) permanecerá.
  - Se desejar que o Azure DevOps seja o `origin` principal:
    1.  Remova o `origin` antigo: `git remote rm origin`
    2.  Renomeie seu novo remote para `origin`: `git remote rename azure_devops origin`
  - Ou, se você quiser _substituir_ o URL de um `origin` existente pelo do Azure DevOps:
    ```bash
    git remote set-url origin <URL_DO_SEU_REPOSITORIO_NO_AZURE_DEVOPS>
    # E então faça o push para o origin atualizado
    git push origin --all
    git push origin --tags
    ```
