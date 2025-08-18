<p align="center">
    <img src="src/assets/images/logo.svg" alt="CookinUp Logo" height="120" />
</p>

<h1 align="center">CookinUp</h1>
<p align="center">Selecione ingredientes. Descubra receitas. Cozinhe com o que você já tem.</p>

<p align="center">
    <a href="https://cookin-up-vue-psi.vercel.app" target="_blank"><img src="https://img.shields.io/badge/ACESSAR%20APP-Online-00c853?style=for-the-badge&logo=vercel&logoColor=white" alt="Acessar aplicação" /></a>
</p>

<div align="center">
    <img alt="Status" src="https://img.shields.io/badge/status-active-success.svg" />
    <img alt="Vue" src="https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white" />
    <img alt="Vite" src="https://img.shields.io/badge/Vite-4-646CFF?logo=vite&logoColor=white" />
    <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white" />
    <img alt="License" src="https://img.shields.io/badge/license-MIT-informational" />
    <a href="https://cookin-up-vue-psi.vercel.app" target="_blank"><img alt="Deploy" src="https://img.shields.io/badge/deploy-Vercel-000?logo=vercel" /></a>
</div>

---

## 📌 Sumário

-   [Visão Geral](#visao-geral)
    -   [Acesse Agora](#acesso)
-   [Demonstração](#demo)
-   [Funcionalidades](#funcionalidades)
-   [Como Funciona o Filtro](#filtro)
-   [Arquitetura e Tecnologias](#tecnologias)
-   [Primeiros Passos](#primeiros-passos)
-   [Scripts Disponíveis](#scripts)
-   [Estrutura de Pastas](#estrutura)
-   [API / Dados](#api)
-   [Roadmap / Ideias Futuras](#roadmap)
-   [Contribuição](#contribuicao)
-   [Autor](#autor)

## 🍳 Visão Geral <a id="visao-geral"></a>

### 🔥 Acesse Agora <a id="acesso"></a>

👉 **Produção:** https://cookin-up-vue-psi.vercel.app

*(Se o link não abrir diretamente, copie e cole no navegador.)*

CookinUp é uma aplicação web que ajuda o usuário a descobrir receitas a partir
dos ingredientes que ele já tem em casa. Basta selecionar ingredientes por
categoria e o sistema retorna somente as receitas que contêm TODOS os
ingredientes selecionados (ignorando itens básicos como sal, água e pimenta).
Perfeito para evitar desperdício e ganhar tempo.

## 🖼️ Demonstração <a id="demo"></a>

| Banner                                                                    | Exemplo de Card                                                                      |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| <img src="src/assets/images/foto-banner.png" alt="Banner" height="180" /> | <img src="public/imagens/receitas/pao_de_alho.png" alt="Pão de Alho" height="180" /> |

> Pode adicionar capturas adicionais (GIF de fluxo, mobile, etc.).

## ✅ Funcionalidades <a id="funcionalidades"></a>

-   Seleção de ingredientes por categoria com feedback visual.
-   Filtro de receitas que exigem todos os ingredientes selecionados (receitas
    podem ter mais itens além disso).
-   Normalização de acentos, caixa e espaços para comparação robusta.
-   Ignora ingredientes básicos configurados (sal, pimenta, água, óleo) para não
    restringir resultados.
-   Cards de receitas com imagem e nome.
-   Interface responsiva (componentes se reorganizam em diferentes larguras).
-   Estrutura em componentes Vue reutilizáveis (Tag, Botão, Card, etc.).

## 🧪 Como Funciona o Filtro <a id="filtro"></a>

1. Usuário marca ingredientes (estado central em `ConteudoPrincipal`).
2. Ao navegar para a tela de receitas (`MostrarReceitas.vue`), busca-se o JSON
   remoto de receitas.
3. Cada receita só aparece se TODOS os ingredientes selecionados (normalizados)
   estiverem presentes na lista da receita (itens básicos são ignorados na
   verificação).
4. Caso nenhum ingrediente seja selecionado, nada é mostrado para evitar ruído.

Pseudocódigo simplificado:

```ts
const contemSelecao = selecionados.every((sel) =>
    receitaFiltrada.includes(sel)
);
```

## 🛠️ Arquitetura e Tecnologias <a id="tecnologias"></a>

-   Vue 3 (API de opções) + TypeScript
-   Vite (dev server e build)
-   Fetch API para dados estáticos (Gist)
-   Organização por domínio: `components/`, `http/`, `interfaces/`, `operações/`

## 🚀 Primeiros Passos <a id="primeiros-passos"></a>

Pré-requisitos:

-   Node.js 18+
-   npm (ou pnpm / yarn – scripts usam npm)

Instalação:

```bash
git clone https://github.com/CamposCodes/CookinUp-Vue.git
cd CookinUp-Vue
npm install
```

Ambiente de desenvolvimento:

```bash
npm run dev
# Abra: http://localhost:5173 (ou porta alternativa mostrada no terminal)
```

Build de produção + preview:

```bash
npm run build
npm run preview
# Preview padrão: http://localhost:4173
```

## 📜 Scripts Disponíveis <a id="scripts"></a>

| Script       | Descrição                            |
| ------------ | ------------------------------------ |
| `dev`        | Servidor de desenvolvimento Vite     |
| `build`      | Type-check + build de produção       |
| `build-only` | Apenas build (sem type-check)        |
| `type-check` | Verificação TypeScript com `vue-tsc` |
| `preview`    | Servir resultado de `dist/`          |

## 🗂️ Estrutura de Pastas <a id="estrutura"></a>

```
src/
    assets/          # CSS global e imagens internas
    components/      # Componentes Vue (UI e fluxos)
    http/            # Funções de acesso a dados remotos
    interfaces/      # Tipagens (ICategoria, IReceita)
    operações/       # Funções utilitárias (listas, etc.)
public/
    imagens/         # Imagens estáticas servidas diretamente
```

## 🌐 API / Dados <a id="api"></a>

Os dados vêm de dois endpoints (JSON estático em Gist):

-   Categorias: `categorias.json`
-   Receitas: `receitas.json`

Implementação central em `src/http/index.ts` usando `fetch` genérico
(`obterDadosURL`).

## 🧭 Roadmap / Ideias Futuras <a id="roadmap"></a>

-   [ ] Mostrar porcentagem de cobertura e ingredientes faltantes
-   [ ] Modo alternativo: receitas que tenham pelo menos 1 ingrediente
        selecionado
-   [ ] Persistir seleção em LocalStorage
-   [ ] Trocar API estática por backend dinâmico
-   [ ] Dark mode
-   [ ] Testes unitários (Vitest) e integração (Cypress)
-   [ ] Internacionalização (i18n)

## 🤝 Contribuição <a id="contribuicao"></a>

1. Faça um fork
2. Crie uma branch: `git checkout -b feat/nome-feature`
3. Commit: `git commit -m "feat: descrição"`
4. Push: `git push origin feat/nome-feature`
5. Abra um Pull Request

Padrão sugerido de commits: _Conventional Commits_ (feat, fix, chore,
refactor...).

## 👤 Autor <a id="autor"></a>

Gabriel Campos Lima Alves ([@CamposCodes](https://github.com/CamposCodes))

---

> Dica: adicione um link de deploy assim que disponível e considere inserir um
> GIF curto de uso. Se quiser, posso automatizar badges de cobertura ou pipeline
> posteriormente.

