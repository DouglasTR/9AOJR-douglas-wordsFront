# Douglas Words Front

## 📋 Sobre o Projeto
**Douglas Words Front** é uma aplicação web desenvolvida para consumir e exibir um vocabulário técnico de forma estruturada e agradável. O projeto consome uma API BFF (Backend for Frontend) que retorna palavras, definições e exemplos de uso em inglês.

A aplicação foi construída com foco em performance, usabilidade e design limpo, permitindo filtrar termos, copiar palavras facilitando o estudo e persistir os últimos resultados para consulta rápida.

## 🚀 Tecnologias Utilizadas (Stack)
- **Vue.js 3** (Composition API)
- **Vite** (Build tool e dev server rápido)
- **Tailwind CSS** (Estilização utilitária)
- **Fetch API** (Consumo de dados)

## 📦 Como Executar Localmente

Siga os passos abaixo para rodar o projeto em sua máquina:

1. **Clone o repositório:**
   ```bash
   git clone https://github.com/DouglasTR/9AOJR-douglas-wordsFront.git
   cd 9AOJR-douglas-wordsFront
   ```

2. **Instale as dependências:**
   ```bash
   npm install
   ```

3. **Execute o servidor de desenvolvimento:**
   ```bash
   npm run dev
   ```
   O projeto estará rodando em `http://localhost:5173` (ou outra porta indicada no terminal).

## 🛠️ Build e Deploy

### Gerar Build de Produção
Para gerar os arquivos otimizados para produção (pasta `dist`):
```bash
npm run build
```
Para visualizar o build localmente antes de subir:
```bash
npm run preview
```

### Deploy no Netlify (Passo a Passo)
1. Crie uma conta no [Netlify](https://www.netlify.com/).
2. Clique em **"Add new site"** > **"Import an existing project"**.
3. Conecte com o **GitHub** e selecione o repositório `9AOJR-douglas-wordsFront`.
4. Nas configurações de build:
   - **Build command:** `npm run build`
   - **Publish directory:** `dist`
5. Clique em **"Deploy site"**.

## 🔗 Links Importantes
- **Repositório Front-end:** [GitHub - Douglas Words Front](https://github.com/DouglasTR/9AOJR-douglas-wordsFront)
- **Deploy (Produção):** [Acessar Aplicação](https://fanciful-madeleine-be2c10.netlify.app/)
- **API (Endpoint):** [https://fiap-bff-doug.onrender.com/ask](https://fiap-bff-doug.onrender.com/ask)
- **Repositório API:** [GitHub - BFF API](https://github.com/DouglasTR/fiap-bff-doug/tree/06-configure-actions)

## 📊 Lighthouse / Web Vitals

**Link Lighthouse:** [Clique aqui para visualizar o relatório completo](https://drive.google.com/file/d/1ZnUq2bFOHf6m6tXm37tXblnHc1RLqC-l/view?usp=drive_link)

### Métricas Principais (Core Web Vitals):
- **LCP (Largest Contentful Paint):** Mede o tempo de carregamento. É o tempo que leva para o maior elemento de conteúdo (imagem ou texto) aparecer na tela. O ideal é ser **menos de 2.5s**.
- **INP (Interaction to Next Paint):** Mede a interatividade. Avalia a latência das interações (cliques, toques, teclado). O ideal é ser **menos de 200ms**.
- **CLS (Cumulative Layout Shift):** Mede a estabilidade visual. Quantifica o quanto o layout muda inesperadamente durante o uso. O ideal é ser **menos de 0.1**.

## 👥 Integrantes
- **Douglas Teixeira Rodrigues - RM364392**