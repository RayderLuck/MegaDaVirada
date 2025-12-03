# 🎰 Mega da Virada — Calculadora de Chances

## 📌 Visão Geral do Projeto
O **Mega da Virada** é um calculador de probabilidades da loteria Mega da Virada, desenvolvido em **HTML5 puro**, sem dependências externas.  
O projeto permite que o usuário insira suas apostas e visualize as chances de ganhar, tanto individualmente quanto de forma agregada.

---

## 🏗️ Arquitetura

### Componentes Principais
- **Grid Interativo (1–60):** Tabela com 60 números clicáveis para seleção.  
- **Gerenciamento de Apostas:** Sistema que permite adicionar múltiplas apostas antes do cálculo.  
- **Motor de Cálculo Combinatório:** Utiliza `BigInt` para evitar overflow em números muito grandes.  
- **Exibição de Resultados:** Mostra probabilidade individual por aposta e probabilidade agregada.  

---

## 🔄 Fluxo de Dados
1. Usuário clica em números → adicionados/removidos do `Set selected`.  
2. Clica em **"Adicionar aposta"** → aposta é salva em `tickets`.  
3. Clica em **"Ver Chances"** → cálculo combinatório é realizado → resultados renderizados.  

---

## 📐 Padrões e Convenções
- **Combinatória com BigInt:** Função `nCrBig(n, k)` para precisão.  
- **Uso de simetria:** `C(n, k) = C(n, n-k)` para otimização.  
- **Evita overflow:** Exemplo `C(60,6) = 50,063,860`.  
- **Conversão para Number:** Apenas para exibição final.  

---

## 🗂️ Estrutura de Dados
- `selected`: números atualmente selecionados no grid → `Set<number>`.  
- `tickets`: apostas salvas → `Array<Array<number>>`.  
- Apostas armazenadas ordenadas e com mínimo de **6 números**.  

---

## 🌍 Formatação e Idioma
- Localização: **pt-BR**.  
- Números formatados com `toLocaleString('pt-BR')`.  
- Percentuais exibidos em notação científica com `toExponential(6)`.  

---

## ⚙️ Workflows Críticos

### ➕ Adicionar Aposta
1. Valida `selected.size > 0`.  
2. Converte `Set → Array` ordenado.  
3. Insere em `tickets`.  
4. Limpa `selected` e re-renderiza grid.  

### 📊 Cálculo de Chances
1. Valida `tickets.length > 0`.  
2. Para cada aposta: calcula `C(n, 6) / C(60, 6)`.  
3. Calcula probabilidade agregada via complementar:  
   

\[
   P_{total} = 1 - \prod (1 - p_i)
   \]

  
4. Renderiza resultados em HTML.  

---

## 🛠️ Observações Técnicas
- Sem bundler/framework: **Vanilla JavaScript** em único HTML.  
- CSS embutido: **Dark Mode** com **Accent Laranja (#ff6b35)**.  
- Acessibilidade: Grid com suporte a teclado (`tabIndex`, `Enter`, `Space`).  
- Shadow DOM não utilizado: todos os elementos no DOM global.  

---

## 🎨 Convenções de Estilo
- Núcleos via propriedades CSS customizadas:  
  `--bg`, `--card`, `--accent`, `--muted`, `--cell`, `--cell-alt`.  
- Espaçamento em múltiplos de **4px/8px/12px**.  
- Transições rápidas: **0.08s–0.12s** para feedback visual imediato.  
- Ícones textuais: "Adicionar aposta", "Remover", "Limpar seleção" (sem ícones gráficos).  

---

## 🚀 Extensões Futuras
- Manter uso de **BigInt** para cálculos probabilísticos.  
- Seguir convenção de nomes em **português-BR**.  
- Preservar reatividade do grid sem frameworks.  
- Adicionar validação antes de renderizar resultados.  

---

## 📄 Licença
Este projeto é de código aberto e pode ser utilizado para fins educacionais e experimentais.  

