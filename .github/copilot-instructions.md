# Mega da Virada — Instruções para Agentes de IA

## Visão Geral do Projeto

**Mega da Virada** é um calculador de probabilidades de loteria (Mega da Virada) em HTML5 puro, sem dependências externas. O projeto calcula as chances de ganhar na loteria com base nas apostas do usuário.

## Arquitetura

### Componentes Principais

- **Grid Interativo (1–60)**: Tabela com 60 números clicáveis para seleção
- **Gerenciamento de Apostas**: Sistema que permite adicionar múltiplas apostas antes do cálculo
- **Motor de Cálculo Combinatório**: Usa `BigInt` para evitar overflow em números muito grandes
- **Exibição de Resultados**: Mostra probabilidade individual por aposta e probabilidade agregada

### Fluxo de Dados

1. Usuário clica em números (adicionado/removido do `Set selected`)
2. Clica "Adicionar aposta" → aposta é salva em array `tickets`
3. Clica "Ver Chances" → cálculo combinatório em `tickets` → resultados renderizados

## Padrões e Convenções

### Combinatória com BigInt

- Função `nCrBig(n, k)` calcula combinações C(n,k) usando `BigInt` para precisão
- Uso de simetria: `C(n, k) = C(n, n-k)` para otimizar cálculos
- Evita overflow em números grandes (ex: C(60,6) = 50.063.860)
- Conversão para `Number` ocorre **apenas** para exibição final

### Estrutura de Dados

- `selected`: `Set<number>` — números atualmente selecionados no grid
- `tickets`: `Array<Array<number>>` — apostas salvas, cada aposta é um array ordenado
- Apostas são armazenadas **ordenadas** e com **mínimo de 6 números**

### Formatação e Idioma

- Localização: **português-BR** (`pt-BR`)
- Números formatados com `toLocaleString('pt-BR')` para separadores
- Notação científica em percentuais: `toExponential(6)` para legibilidade

## Workflows Críticos

### Adicionar Aposta
```javascript
// Botão "Adicionar aposta"
1. Valida selected.size > 0
2. Converte Set → Array ordenado
3. Insere em tickets
4. Limpa selected e re-renderiza grid
```

### Cálculo de Chances
```javascript
// Botão "Ver Chances"
1. Valida tickets.length > 0
2. Para cada aposta: calcula C(n, 6) / C(60, 6)
3. Calcula probabilidade agregada via complementar: P_total = 1 - ∏(1 - p_i)
4. Renderiza resultados em HTML
```

## Observações Técnicas

- **Sem bundler/framework**: Código vanilla JavaScript em HTML único
- **CSS-in-style**: Estilos embutidos; design dark mode com accent laranja (`#ff6b35`)
- **Acessibilidade**: Grid com `tabIndex` e handlers de keyboard (`Enter`/`Space`)
- **Shadow DOM não usado**: Todos os elementos no DOM global

## Convenções de Estilo

- Cores via CSS custom properties: `--bg`, `--card`, `--accent`, `--muted`, `--cell`, `--cell-alt`
- Espaçamento em múltiplos de 4px/8px/12px
- Transições rápidas: `.08s–.12s` para feedback visual imediato
- Ícones textuais: "Adicionar aposta", "Remover", "Limpar seleção" (nenhum ícone gráfico)

## Extensões Futuras

Se adicionando features:
- Manter BigInt para cálculos probabilísticos
- Seguir convenção de nomes português-BR
- Preservar reatividade do grid (sem framework)
- Adicionar validação antes de renderizar resultados
