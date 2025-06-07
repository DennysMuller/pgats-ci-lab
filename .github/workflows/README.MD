# Workflows de CI/CD

Este documento descreve os workflows de automação utilizados no projeto.

## Workflow `n1/n2/n3`

### Job: `e2e-tests`
- **Passos**:
  1. Instalar dependências (`installer deeps`)
  2. Executar scripts de teste (`executor script de testes`)
  3. Publicar resultados (`publicar resultados`)

---

## Workflow `n4`

### Bloco 1
#### Job: `inspector`
- **Passos**:
  1. Instalar dependências
  2. Executar script de Lint

#### Job: `unidade`  
- **Passos**:
  1. Instalar dependências
  2. Executar testes unitários

### Bloco 2
#### Job: `e2e-tests`
- **Passos**:
  1. Instalar dependências
  2. Subir aplicação localmente
  3. Instalar browsers/navegadores
  4. Executar testes E2E
  5. Publicar resultados (formato ZIP ou online)

### Bloco 3
#### Job: `deploy`
- **Passos**:
  1. Instalar dependências
  2. Gerar artefatos da aplicação
  3. Publicar em ambiente destino

> **Nota**: Substitua termos como "deeps" por "dependencies" caso necessário para padronização.