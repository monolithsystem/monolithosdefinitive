# Dados 100% reais da planilha + estado vazio executivo

## O que muda

1. **Gráfico de onda (Painel do Diretor) passa a ler a planilha de verdade**
   - A lista fixa de procedimentos (Limpeza, Aparelho, Geral, Implante, Estética) é apagada.
   - O gráfico agrupa e conta os procedimentos reais da coluna Procedimento, ignorando maiúsculas/minúsculas e espaços extras, e ignorando registros cancelados.
   - O rótulo exibido usa o texto como está escrito na planilha; a linha dourada e o gradiente ficam iguais.

2. **Nenhum dado de exemplo em tela**
   - Hoje, quando a planilha não está conectada ou falha, o sistema mostra 5 pacientes fictícios (Julio, David, Marina, Ricardo, Beatriz). Essa lista deixa de ser exibida — a tela começa vazia e só mostra o que vem da planilha.

3. **Visual "sem conexão / sem registros"**
   - Nos quatro cards da Recepção e do Diretor: em vez de zeros, aparece `--` em dourado fosco.
   - Nos gráficos: linhas e fatias somem e aparece um aviso central em vidro fosco (escuro: fundo preto translúcido, texto dourado suave e subtexto cinza; claro: fundo branco suave com borda cinza fina, texto ouro velho e subtexto grafite).
   - Textos — Recepção: "Aguardando fluxo de registros ativos" / "O motor de busca em tempo real está monitorando o sistema." Diretor: "Painel estratégico aguardando sincronização" / "Insira novas movimentações de pacientes para projetar os indicadores de procedimentos e taxas de confirmação."
   - A tabela da Recepção mostra o mesmo aviso quando não há registros (mantendo a mensagem específica de busca sem resultados).

4. **Tooltip no modo claro mais leve**
   - Fundo branco semitransparente com vidro fosco e borda cinza ultra fina, em vez de branco sólido; textos internos em grafite legível.

5. **Lupa da busca visível no modo escuro**
   - O ícone de lupa passa a cinza claro no escuro (mantendo boa leitura no claro).

## Detalhes técnicos

- `src/lib/sheets.ts`: remover `mockAppointments` (ou substituir por array vazio) e ajustar quem importa.
- `src/hooks/use-appointments.ts`: estado inicial `[]`; expor um sinal de "sem dados" (lista vazia) e manter o polling silencioso de 10s intacto.
- `src/components/director-view.tsx`: `PROCEDURE_DATA` substituído por `useMemo` que agrupa `a.procedimento` (chave normalizada com `.toLowerCase().trim()`, rótulo original), filtrando `isCancelado`; renderizar `<EmptyState>` quando não houver dados; cards mostram `--` em `text-amber-500/60` / `text-amber-700/80`.
- `src/components/reception-view.tsx`: cards e tabela com o mesmo tratamento vazio; `Search` com `text-slate-500 dark:text-zinc-400`.
- `src/lib/theme-classes.ts`: `tooltipStyles` no modo claro → `rgba(255,255,255,0.8)`, `backdropFilter: blur(12px)`, borda `rgba(228,228,231,0.6)`.
- Componente de aviso compartilhado (título + subtítulo por props) para os dois painéis.
