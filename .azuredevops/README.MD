# Configuração de Pipelines no Azure DevOps

Este projeto está configurado para executar pipelines de CI/CD tanto no GitHub Actions quanto no Azure DevOps.

## Estrutura de Arquivos
```
/ (raiz do projeto)
├── .github/
│ └── workflows/ # Workflows do GitHub Actions
├── .azuredevops/
│ └── pipelines/ # Pipelines do Azure DevOps
│ ├── build.yml # Pipeline de build principal
│ └── scheduled.yml # Pipeline de execução agendada
└── README.md # Este arquivo
```

## Pipelines no Azure DevOps

### Localização dos Arquivos
- Os arquivos de pipeline podem ficar:
  - **Na raiz do projeto** (mais comum): `azure-pipelines.yml`, `azure-pipelines-scheduled.yml`
  - **Em pasta dedicada** (para organização): `/.azuredevops/pipelines/`

### Extensão dos Arquivos
- Ambos os formatos são válidos: `.yml` ou `.yaml`
- Recomendamos usar `.yml` para consistência com os exemplos oficiais da Microsoft

### Configuração dos Pipelines

1. **Pipeline Principal** (`build.yml`):
   ```yaml
   trigger:
   - main

   pool:
     vmImage: 'ubuntu-latest'

   steps:
   - checkout: self
   - script: echo "Build realizado!"