# TeamBalance — Relatório consolidado de testes

**Escopo:** RN01 a RN54 · Gestão de capacidade de equipes

**Aplicação:** https://roxanenayara.github.io/teambalance/

**Especificação:** [Regras de negócio — versão 1.0](Regras_de_Negocio_v1.0.md)

> **Versão pública:** consolidação de resultados, sem anexos, capturas de tela ou links privados de evidências. Os resultados registrados refletem as execuções relatadas pelos integrantes.

> **Atualização da consolidação:** RN08 e RN18 foram confirmadas pelo grupo como testadas e aprovadas. Mantêm-se as observações e defeitos registrados nas demais regras.

## 1. Visão geral

O TeamBalance é uma aplicação de gestão de capacidade de equipes. A especificação funcional (aprovada pelo grupo) define 54 regras de negócio (RN01 a RN54) em seis áreas. Este documento consolida os relatórios de teste produzidos pelo grupo, apresentando técnicas, artefatos, testes executados em ordem de regra, defeitos, aprendizados e conclusão.

| Área | Regras | Fonte consolidada |
| --- | --- | --- |
| Gestão de colaboradores | RN01–RN08 | Técnicas de teste aplicadas às regras RN01 a RN08 |
| Gestão de atividades | RN09–RN20 | Relatório de execução de testes (Team Balance – Oficial) |
| Cálculo de capacidade e Simulador de distribuição | RN21–RN36 | Tabela de requisitos e casos de teste RN21–RN36 |
| Visão geral e filtros | RN37–RN45 | Relatório TeamBalance (Notion), casos CT01–CT15 |
| Persistência e proteção dos dados | RN46–RN54 | Relatório de execução RN46–RN54 |

**Resultado consolidado:** as 54 regras foram testadas. Destas, 52 apresentam resultado aprovado nos cenários executados (algumas com observações ou defeitos de apresentação associados) e 2 apresentam não conformidade: RN02 (cadastro de função contendo apenas espaços) e RN54 (erro de gravação apresentado atrás do modal). RN08 e RN18 estão aprovadas. Foram registrados 4 defeitos no total; as observações e dúvidas de refinamento permanecem documentadas.

## 2. Técnicas utilizadas

As técnicas abaixo foram aplicadas, recomendadas e organizadas por finalidade.

Partição de equivalência: dividir entradas em classes válidas e inválidas (nome, função, capacidade, nomes duplicados, esforço, ativo × inativo, busca por texto).

Análise de valor-limite: testar limites de campos e valores (capacidade 0, 1, 167, 168 e 169 h; 70% e 100% de utilização; 40 h × 40,01 h).

Tabela de decisão: validar combinações de condições (status do colaborador × operação; filtros combinados de responsável, prioridade e status).

Teste baseado em estados e transições de estados: verificar status inicial, inativação, reativação (Ativo → Inativo → Ativo) e mudanças de status das atividades.

Testes funcionais baseados em regras de negócio e em cenários: comportamento esperado de cada regra, em testes manuais (RN09 a RN20 e RN46 a RN54) e cenários completos, como concluir ou cancelar atividade e simular distribuição (RN21 a RN36).

Testes de cálculo: conferência de horas alocadas, disponíveis, percentuais e médias com resultados calculados antes da execução.

Verificação de integridade e integração: comparar dados antes e depois da simulação; conferir se indicadores são atualizados.

Testes exploratórios e heurísticas (Siga os dados, Atualizar a página, Interrupções, Abordagem contrária, Testes de strings): investigação de persistência, isolamento entre navegadores e sessões, atualização de página, segunda aba aberta.

Injeção controlada de falha: simulação de QuotaExceededError no localStorage (RN54).

Shift-Left (revisão estática) e testes baseados em sessão: confronto das regras com o código antes da execução, gerando perguntas de refinamento para o P.O.; testes baseados em sessão (Session-Based Test Management) na RN54.

Testes de regressão (recomendados): reexecutar casos relacionados após correções.

## 3. Artefatos utilizados

| Artefato | Finalidade / uso no projeto |
| --- | --- |
| Documento de Regras de Negócio (v1.0) | Base de teste: define o comportamento esperado de RN01 a RN54 |
| Plano / estratégia de testes | Escopo, objetivos, abordagem, riscos, prioridades e critérios de entrada e saída |
| Casos de teste | Pré-condições, dados de entrada, passos, resultado esperado, resultado obtido, status e prioridade |
| Matriz de rastreabilidade | Liga cada regra de negócio aos casos de teste, evidências e defeitos |
| Massa de dados de teste | Colaboradores ativos e inativos, capacidades válidas e inválidas, atividades em diferentes status, dados de exemplo com valores pré-calculados |
| Modelo de condições de teste (ISO 29119-3) | Referência para organizar a documentação dos testes de Gestão de atividades |
| Evidências de execução | Capturas de tela, vídeos, console/DevTools (Application > Local Storage), registros de resultados |
| Checklist de dúvidas / perguntas de refinamento | Ambiguidades levadas ao P.O. (P01–P05 no relatório RN37–RN45) |
| Código-fonte e aplicação publicada | GitHub roxanenayara/teambalance e GitHub Pages; fonte das mensagens e do comportamento real |
| Relatório de sessão (SBTM) | Charter, notas, defeitos e perguntas da sessão exploratória da RN54 |
| Relatório de defeitos e relatório de execução | Registro de falhas (passos, esperado, obtido, severidade) e consolidação de aprovados, reprovados e pendentes |
| Ambiente | Google Chrome e Safari, janelas convencionais e anônimas |

## 4. Testes executados (em ordem de regra de negócio)

*Legenda de status: verde = passou/atendida; amarelo = observação, pendência ou aguarda decisão; vermelho = falhou.*

4.1 Gestão de colaboradores (RN01–RN08)

| RN | Regra de negócio | Cenário / caso de teste | Resultado obtido | Status |
| --- | --- | --- | --- | --- |
| RN01 | Nome obrigatório e não pode conter só espaços | RN01-01 nome válido (João Silva); RN01-02 só espaços; RN01-03 espaços nas extremidades; RN01-04 nome de 1 caractere (A) | Cadastros válidos aceitos; só espaços rejeitados; espaços nas pontas normalizados | Passou (4/4) |
| RN02 | Função obrigatória e não pode conter só espaços | RN02-01 função válida; RN02-02 sem função; RN02-03 função só com espaços; RN02-04 espaços nas extremidades | RN02-03: cadastro foi realizado com a função contendo somente espaços | Falhou (RN02-03); demais passaram |
| RN03 | Capacidade semanal > 0 e ≤ 168 h | Valores -1, 0, 1, 167, 168, 169, vazio e texto (abc) | -1, 0, 169, vazio e texto rejeitados; 1, 167 e 168 aceitos | Passou (8/8) |
| RN04 | Sem nomes duplicados (ignora maiúsculas/minúsculas e espaços nas pontas) | Maria Silva; repetido; “maria silva”; “MARIA SILVA”; “ Maria Silva ”; Maria Souza; diferença só em espaços internos | Duplicados rejeitados; nome diferente aceito | Passou (7/7) |
| RN05 | Colaborador novo inicia como Ativo | Cadastro de um e de vários colaboradores válidos | Todos cadastrados como ativos | Passou (2/2) |
| RN06 | Inativos não recebem novas atividades, mas mantêm as existentes e o histórico | Atribuir a ativo; atribuir a inativo; inativar quem já tem atividades; reativar | Atribuição a inativo bloqueada; atividades preservadas; reativação permite novas atribuições | Passou (4/4) |
| RN07 | Inativos não entram na utilização média | Média só com ativos; inativar um colaborador; inativar todos | Média recalculada sem inativos; sem ativos, nenhum dado retornado | Passou (3/3) |
| RN08 | Alteração da capacidade semanal atualiza disponibilidade e indicadores | Teste com os colaboradores na aba anônima e convencional. | Capacidade semanal reflete nos indicadores e disponibilidade do colaborador. | Passou 2/2 |

4.2 Gestão de atividades (RN09–RN20)

| RN | Regra de negócio | Cenário / caso de teste | Resultado obtido | Status |
| --- | --- | --- | --- | --- |
| RN09 | Título obrigatório e não pode conter só espaços | Cadastro com título válido (prioridade alta) | Título cadastrado com sucesso | Passou |
| RN10 | Descrição opcional | Salvar atividade sem descrição (prioridade baixa) | Atividade cadastrada sem descrição | Passou |
| RN11 | Responsável cadastrado e ativo | Atividade vinculada a responsável ativo (Alta) | Salva e vinculada ao responsável | Passou |
| RN12 | Esforço estimado > 0 | Atividade com esforço válido (0,01 h) (Alta) | Salva com sucesso | Passou |
| RN13 | Prioridade Baixa, Média ou Alta | Seleção de prioridade válida (Média) | Salva mantendo a prioridade | Passou |
| RN14 | Prazo de nova atividade ≥ data atual | Prazos 21/10/2026 e 22/10/2026 (Alta) | Cadastradas com sucesso | Passou |
| RN15 | Atividade vencida pode ser editada sem alterar o prazo | Edição mantendo prazo vencido de 20/10/2026 (Média) | Editada e salva com o prazo mantido | Passou |
| RN16 | Status: Pendente, Em andamento, Concluída ou Cancelada | Seleção dos status na lista (Alta) | Salva mantendo o status | Passou |
| RN17 | Cadastro pode iniciar em qualquer status válido | Cadastro direto em Em andamento e Concluída (Média) | Status inicial mantido | Passou |
| RN18 | Concluídas e canceladas não consomem capacidade, inclusive se cadastradas direto nesses status | Cadastro direto como Concluída (Alta) | Indicadores atualizados | Passou |
| RN19 | Alteração de esforço, responsável ou status atualiza os indicadores | Alteração e conferência da Visão geral (Alta) | Indicadores atualizados | Passou |
| RN20 | Exclusão exige confirmação; ao cancelar, dados inalterados | Excluir atividade e clicar em Voltar (Alta) | Confirmação exibida; atividade mantida | Passou |

4.3 Cálculo de capacidade e Simulador de distribuição (RN21–RN36)

| RN | Regra de negócio | Cenário / caso de teste | Resultado obtido | Status |
| --- | --- | --- | --- | --- |
| RN21 | Horas alocadas = soma dos esforços pendentes + em andamento | Bruno Lima: 28 h + 10 h | 38 h | Passou |
| RN22 | Horas disponíveis = capacidade − alocadas | Bruno Lima: 40 h − 38 h | 2 h disponíveis | Passou |
| RN23 | Utilização = alocadas ÷ capacidade × 100 | Bruno Lima: 38 ÷ 40 | 95% | Passou |
| RN24 | Utilização < 70% = Disponibilidade confortável | Percentual inferior a 70% | Exibe “Disponibilidade confortável” | Passou |
| RN25 | Entre 70% e 100% (inclusive) = Atenção à capacidade | Percentual de 70% a 100% | Exibe “Atenção à capacidade” | Passou |
| RN26 | Acima de 100% = Sobrecarga | Percentual acima de 100% | Exibe “Sobrecarga” | Passou |
| RN27 | Disponibilidade pode ser negativa | Horas excedentes | Exibe horas negativas | Passou |
| RN28 | Concluir/cancelar libera o esforço da carga alocada | Concluir (Camila Santos) e cancelar (Diego Costa) | Esforço liberado. Atenção: o registro de Camila (48 h → 24 h disponíveis após concluir 32 h) precisa ser conferido | Passou (com ressalva de conferência) |
| RN29 | Simulador lista somente colaboradores ativos | Ana Souza inativa | Ana não aparece na seleção | Passou |
| RN30 | Esforço na simulação > 0 | Esforço = 0; esforço < 0; esforço válido após inválido | 0 e negativo bloqueados. Após corrigir, a mensagem “Informe um esforço maior que zero” permanece na tela | Passou, com observação |
| RN31 | Carga projetada = carga atual + esforço novo | Soma da carga atual com a nova atividade | Soma correta | Passou |
| RN32 | Percentual projetado usa a capacidade do colaborador selecionado | Percentual projetado | Calculado com a capacidade correta | Passou |
| RN33 | Projeção > 100% exibe alerta com horas excedentes | Percentual acima de 100% | Alerta exibido | Passou |
| RN34 | Distribuição com sobrecarga só salva após confirmação explícita | Aprovação de sobrecarga | Salva somente após confirmar | Passou |
| RN35 | Simulação não altera registros nem indicadores antes da confirmação | Simular sem confirmar | Nada alterado | Passou |
| RN36 | Concluir distribuição vincula a atividade e atualiza indicadores | Confirmar distribuição | Atividade vinculada e indicadores atualizados | Passou |

4.4 Visão geral e filtros (RN37–RN45)

| RN | Regra de negócio | Cenário / caso de teste | Resultado obtido | Status |
| --- | --- | --- | --- | --- |
| RN37 | Total de ativos conta só status Ativo | CT01: desativar e reativar Diego (5→4→5) | Contador correto a cada passo | Passou |
| RN38 | Em aberto = Pendentes + Em andamento | CT02: concluir uma atividade (8→7). CT03: desativar Bruno | CT02 correto. CT03: continua 8; regra não diz se atividades de inativos entram | CT02 Passou; CT03 Observado (P01) |
| RN39 | Sobrecarga = ativos com utilização > 100% | CT04: 40 h e 40,01 h. CT05: desativar Camila (1→0) | CT04 e CT05 corretos. Defeito DEF02 (100% exibido com “Sobrecarga”) | Passou; DEF02 falhou |
| RN40 | Utilização média = média aritmética dos percentuais dos ativos | CT06: painel inicial (75%). CT07: desativar Camila (63,75%) | Valores corretos (e não 76,04%) | Passou |
| RN41 | Sem ativos, média = 0%, sem erro | CT08: desativar todos e restaurar a aplicação | 0%, sem NaN nem erro | Passou |
| RN42 | Pesquisa por título ignora maiúsculas/minúsculas | CT09: “PESQUISA”, “pesquisa”, “ pesquisa ”. CT10: “regressão” sem acento | CT09 encontra-se nos três casos. CT10 não encontra (diferencia acentos) | CT09 Passou; CT10 Observado (P03) |
| RN43 | Filtros funcionam individualmente e combinados | CT11: Camila + Alta + Pendente. CT12: Diego + Alta | Combinações corretas; mensagem de lista vazia correta. DEF01 ao recarregar exemplos | CT11/CT12 Passou; DEF01 Falhou |
| RN44 | Limpar filtros restaura a listagem sem alterar registros | CT13: 4 filtros, limpar e recarregar | Listagem restaurada, registros intactos. DEF01: filtro de responsável pode ficar ativo sem aparecer | Passou (CT13); DEF01 Falhou |
| RN45 | Dashboard reflete alterações em cadastros, atividades e distribuição | CT14: distribuir 8 h para Ana. CT15: segunda aba aberta | CT14: Ana 80%, 9 em aberto, média 79%. CT15: a outra aba só atualiza ao recarregar | CT14 Passou; CT15 Observado (P05) |

4.5 Persistência e proteção dos dados (RN46–RN54)

| RN | Regra de negócio | Cenário / caso de teste | Resultado obtido | Status |
| --- | --- | --- | --- | --- |
| RN46 | Registros armazenados localmente no navegador | Inspeção do Local Storage em cadastros, edições, exclusões, exemplos e restauração; janela anônima | Dados locais; janela anônima não herda os dados | Atendida |
| RN47 | Atualizar a página não perde dados | Recarregar com dados próprios e exemplos; Chrome e Safari; outro navegador e sessão anônima | Dados mantidos; isolamento correto | Atendida |
| RN48 | Carregar exemplos exige confirmação | Clique em “Carregar dados de exemplo” | Alerta de confirmação exibido | Atendida |
| RN49 | Cancelar o carregamento preserva registros | Voltar no modal, a partir de Minha equipe e Atividades | Registros inalterados | Atendida |
| RN50 | Confirmar substitui os registros, sem mesclar | Carregar exemplos após dados próprios; repetir; Chrome, Safari e anônima | Substituição total, sem duplicidade | Atendida |
| RN51 | Restaurar exige confirmação | Clique em “Restaurar aplicação” | Alerta de confirmação exibido | Atendida |
| RN52 | Cancelar a restauração preserva dados | Voltar no modal, a partir de Minha equipe e Atividades | Dados preservados no armazenamento local | Atendida |
| RN53 | Restauração confirmada limpa os registros no armazenamento local | Apagar todos os dados; Chrome, Safari e anônima | Dados e localStorage removidos | Atendida |
| RN54 | Falha ao persistir deve ser comunicada, sem falso sucesso | Injeção de QuotaExceededError em localStorage.setItem; cadastro de colaborador e de atividade | Cadastro bloqueado e sem falso sucesso, mas a mensagem de erro aparece atrás do modal | Não atendida integralmente |

## 5. Defeitos reportados

| ID | Regra | Descrição | Esperado | Obtido | Severidade |
| --- | --- | --- | --- | --- | --- |
| DEF-01 | RN02 | Função aceita contendo somente espaços (caso RN02-03) | Cadastro rejeitado | Cadastro realizado | Média |
| DEF-02 | RN54 | Mensagem de erro de persistência exibida atrás do modal, ilegível para o usuário | Mensagem claramente visível | Operação bloqueada, mensagem oculta pelo modal | Média |
| DEF-03 | RN43 / RN44 | Filtro de responsável fica invisível: ao recarregar exemplos, o filtro guarda o ID antigo e a lista fica vazia, enquanto o campo mostra “Todos os responsáveis” | Lista com 10 atividades ou filtro visível | Lista vazia (“Nenhuma atividade encontrada”). Limpar filtros resolve | Média |
| DEF-04 | RN39 | Com 40,001 h para 40 h, a tela mostra “100%” com classificação “Sobrecarga” (arredondamento na tela, classificação com valor completo) | Percentual e classificação coerentes | “100%” + “Sobrecarga”; card conta 1 | Baixa |

**Nota sobre identificadores:** Os IDs DEF-01 a DEF-04 foram atribuídos nesta consolidação. No relatório original do Notion, os defeitos se chamam DEF01 (filtro invisível) e DEF02 (100% com sobrecarga). A severidade do DEF-01 não foi informada nos documentos.

### Observações, pendências e dúvidas ao P.O.

| Item | Descrição |
| --- | --- |
| RN30 | A mensagem “Informe um esforço maior que zero” permanece após o usuário corrigir o valor. |
| RN28 | Conferir o registro de Camila Santos: 48 h, após concluir 32 h aparecem 24 h disponíveis; os valores informados não permitem confirmar o cálculo. |
| RN34 | Segundo o relatório RN37–RN45, a confirmação de sobrecarga existe no Simulador, mas não foi verificada na tela Atividades (outro caminho para o mesmo resultado). |
| RN38 (P01) | Atividades abertas de inativos entram em “Atividades em aberto” e no “Radar de prioridades”? Aguarda o P.O. |
| RN42 (P03) | A pesquisa deve ignorar acentos e buscar na descrição? Aguarda o P.O. |
| RN45 (P05) | Quais indicadores mudam em cada ação? O painel deve atualizar entre abas? Aguarda o P.O. |
| RN20 (evidência) | Na captura da exclusão aparece uma atividade com esforço de 100.000.000 h aceita pelo sistema. Não há limite superior definido na regra; vale levar ao P.O. (observação desta consolidação, não registrada pelo grupo). |
| RN09–RN17 | Os testes registrados cobrem principalmente o caminho feliz (ex.: título só com espaços, esforço 0 e prazo no passado não aparecem como casos negativos). |

## 6. Principais aprendizados

Técnica adequada a cada regra. Escolher a técnica certa para cada regra (partição, valor-limite, tabela de decisão, estados) gera testes objetivos e evita redundância.

Esclarecer ambigüedades antes de testar. Requisitos ambíguos ou incompletos (RN38, RN42, RN45) geram dupla interpretação entre QA, DEV e P.O. A dúvida deve virar pergunta de refinamento, e o status correto é “Observado, aguarda P.O.”, não “Passou” nem “Falhou”.

Defeitos nos limites e na comunicação ao usuário. Os defeitos apareceram nas fronteiras (40,001 h; função só com espaços) e naquilo que o usuário vê, como a mensagem da RN54 atrás do modal. Uma regra pode estar tecnicamente atendida e ainda ter problema perceptível; é preciso validar o que é exibido, não só o que é calculado.

Cálculos, entradas inválidas e operações destrutivas. Conferir cálculos de forma independente, validar mensagens após a correção da entrada (RN30) e testar operações destrutivas tanto no caminho de confirmação quanto no de cancelamento (RN20, RN48–RN53). Simulação não deve alterar dados antes da confirmação.

Rastreabilidade, independência dos testes e múltiplos caminhos. Relacionar regras, casos de teste, evidências e defeitos facilita a cobertura e a comunicação. Cada caso deve preparar sua própria massa de dados, e a regra validada em um caminho não está garantida em todos (RN34 no Simulador × tela Atividades). Alterações em um cadastro também afetam indicadores em outras telas.

## 7. Conclusão

As 54 regras de negócio do TeamBalance foram testadas; 52 tiveram resultado aprovado nos cenários executados e 2 apresentaram não conformidade (RN02 e RN54). A avaliação contemplou: validações de cadastro, cálculos de capacidade, classificação de utilização, simulador, filtros, indicadores, persistência local e confirmações de operações destrutivas.

Foram identificados quatro defeitos: aceitação de função com somente espaços (RN02), mensagem de erro de persistência ocultada pelo modal (RN54), filtro de responsável invisível após recarregar os dados de exemplo (RN43/RN44) e percentual “100%” exibido junto de “Sobrecarga” (RN39). RN08 e RN18 foram executadas e aprovadas. Permanecem as dúvidas de refinamento sobre RN38, RN42 e RN45, além das demais observações detalhadas neste relatório.

O trabalho mostrou o valor de combinar técnicas formais com exploração, confrontar a especificação com o código antes da execução e manter evidências e rastreabilidade. Os resultados refletem as execuções relatadas pelo grupo e não garantem a ausência de outros defeitos.

## 8. Integrantes do grupo

- Roxane Nayara
- Marluce Vitor
- Rubiana Mendes
- Gustavo Siqueira
- Rayane Rocha

---

*Esta edição pública não disponibiliza as evidências individuais produzidas pelos integrantes. Para investigações ou auditorias, consulte os registros originais diretamente com seus responsáveis.*
