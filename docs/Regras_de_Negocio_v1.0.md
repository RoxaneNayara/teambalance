# TeamBalance | Regras de negócio

Versão 1.0

Gestão de capacidade de equipes  •  Especificação funcional

## Objetivo

Objetivo: estabelecer as regras que orientam o funcionamento do TeamBalance, permitindo ao grupo avaliar se a aplicação atende aos comportamentos esperados.

Referência: interface do TeamBalance e definição funcional acordada pelo grupo.

## 1. Gestão de colaboradores

| ID | Regra de negócio |
| --- | --- |
| RN01 | O nome do colaborador é obrigatório e não pode conter somente espaços. |
| RN02 | A função é obrigatória e não pode conter somente espaços. |
| RN03 | A capacidade semanal deve ser um número maior que zero e menor ou igual a 168 horas. |
| RN04 | Não é permitido cadastrar dois colaboradores com o mesmo nome, desconsiderando maiúsculas, minúsculas e espaços nas extremidades. |
| RN05 | Todo colaborador recém-cadastrado deve iniciar com status Ativo. |
| RN06 | Colaboradores inativos não podem receber novas atividades, mas mantêm as atividades existentes e seu histórico. |
| RN07 | Colaboradores inativos não entram no cálculo da utilização média da equipe. |
| RN08 | Alterações na capacidade semanal devem atualizar a disponibilidade e os indicadores relacionados. |

## 2. Gestão de atividades

| ID | Regra de negócio |
| --- | --- |
| RN09 | O título é obrigatório e não pode conter somente espaços. |
| RN10 | A descrição é opcional. |
| RN11 | Toda atividade deve possuir um responsável cadastrado e ativo no momento da atribuição. |
| RN12 | O esforço estimado deve ser um número maior que zero. |
| RN13 | A prioridade deve ser Baixa, Média ou Alta. |
| RN14 | O prazo de uma nova atividade deve ser igual ou posterior à data atual. |
| RN15 | Atividades existentes com prazo vencido podem ser editadas sem alterar o prazo, desde que ele já estivesse salvo anteriormente. |
| RN16 | A atividade deve possuir um dos status: Pendente, Em andamento, Concluída ou Cancelada. |
| RN17 | O cadastro pode iniciar em qualquer um dos quatro status válidos, conforme a seleção do usuário. |
| RN18 | Atividades concluídas e canceladas não consomem capacidade, inclusive quando cadastradas diretamente nesses status. |
| RN19 | A alteração do esforço, responsável ou status deve atualizar os indicadores afetados. |
| RN20 | A exclusão de uma atividade deve exigir confirmação. Ao cancelar, seus dados permanecem inalterados. |

## 3. Cálculo de capacidade

| ID | Regra de negócio |
| --- | --- |
| RN21 | As horas alocadas correspondem à soma dos esforços das atividades Pendentes e Em andamento atribuídas ao colaborador. |
| RN22 | As horas disponíveis correspondem à capacidade semanal menos as horas alocadas. |
| RN23 | O percentual de utilização corresponde às horas alocadas divididas pela capacidade semanal, multiplicado por 100. |
| RN24 | Utilização inferior a 70% deve ser classificada como Disponibilidade confortável. |
| RN25 | Utilização entre 70% e 100%, incluindo ambos os valores, deve receber a classificação de atenção à capacidade. |
| RN26 | Utilização superior a 100% deve ser classificada como Sobrecarga. |
| RN27 | A disponibilidade pode apresentar valores negativos, indicando horas excedentes. |
| RN28 | A conclusão ou o cancelamento de uma atividade deve liberar automaticamente seu esforço da carga alocada. |

## 4. Simulador de distribuição

| ID | Regra de negócio |
| --- | --- |
| RN29 | O simulador deve permitir selecionar somente colaboradores ativos. |
| RN30 | O esforço informado na simulação deve ser maior que zero. |
| RN31 | A carga projetada corresponde à soma da carga atual com o esforço da nova atividade. |
| RN32 | O percentual projetado deve utilizar a capacidade semanal do colaborador selecionado. |
| RN33 | Quando a utilização projetada ultrapassar 100%, deve ser apresentado um alerta com as horas excedentes. |
| RN34 | Uma distribuição que gere sobrecarga só pode ser salva após confirmação explícita do usuário. |
| RN35 | A simulação não pode modificar registros ou indicadores antes da confirmação final do cadastro. |
| RN36 | Ao concluir a distribuição, a atividade deve ser vinculada ao colaborador selecionado e os indicadores devem ser atualizados. |

## 5. Visão geral e filtros

| ID | Regra de negócio |
| --- | --- |
| RN37 | O total de colaboradores ativos deve contabilizar exclusivamente os colaboradores com status Ativo. |
| RN38 | O total de atividades em aberto deve considerar apenas atividades Pendentes e Em andamento. |
| RN39 | O indicador de sobrecarga deve contabilizar colaboradores ativos com utilização superior a 100%. |
| RN40 | A utilização média deve ser calculada pela média aritmética dos percentuais individuais dos colaboradores ativos. |
| RN41 | Quando não houver colaboradores ativos, a utilização média deve ser apresentada como 0%, sem erro de divisão. |
| RN42 | A pesquisa por título deve desconsiderar diferenças entre letras maiúsculas e minúsculas. |
| RN43 | Os filtros de responsável, prioridade e status devem funcionar individualmente e combinados. |
| RN44 | A opção Limpar filtros deve restaurar a listagem completa, sem modificar os registros. |
| RN45 | O dashboard deve refletir alterações realizadas nos cadastros, atividades e distribuição. |

## 6. Persistência e proteção dos dados

| ID | Regra de negócio |
| --- | --- |
| RN46 | Os registros devem ser armazenados localmente no navegador utilizado. |
| RN47 | A atualização da página não deve provocar perda dos dados previamente salvos. |
| RN48 | A opção Carregar dados de exemplo deve solicitar confirmação antes de substituir os registros existentes. |
| RN49 | Cancelar o carregamento dos exemplos deve preservar todos os registros atuais. |
| RN50 | Confirmar o carregamento deve substituir os registros atuais pelo conjunto demonstrativo, sem mesclar dados. |
| RN51 | A opção Restaurar aplicação deve solicitar confirmação antes de apagar os registros. |
| RN52 | Cancelar a restauração deve preservar integralmente os dados. |
| RN53 | A restauração confirmada deve limpar os registros da aplicação apenas no armazenamento local utilizado. |
| RN54 | Uma falha ao persistir os dados deve ser comunicada, sem apresentar uma confirmação falsa de sucesso. |

## Premissas de cálculo

- Horas alocadas = soma dos esforços de atividades pendentes e em andamento.

- Horas disponíveis = capacidade semanal − horas alocadas.

- Utilização (%) = (horas alocadas ÷ capacidade semanal) × 100.

- Carga projetada = carga atual + esforço da nova atividade.

- O cálculo agrega atividades abertas e não distribui esforço por dia ou semana.

## Pontos de atenção para evolução da especificação

- Precisão: admitir até duas casas decimais nas horas de capacidade e esforço?

- Fluxo de status: todas as transições entre Pendente, Em andamento, Concluída e Cancelada serão permitidas?

- Inativação: como apresentar e tratar as atividades abertas do colaborador inativo nos relatórios?

- Prazo: ao editar atividade vencida, o prazo antigo pode permanecer; é permitido mudar para outra data também vencida?

## Observações gerais

A publicação no GitHub Pages e o uso de localStorage não representam compartilhamento de dados entre usuários. Cada navegador possui armazenamento independente. Questões de usabilidade, acessibilidade e responsividade devem ser documentadas à parte como critérios não funcionais ou de interface.

## Aprovadores

| Participante | Parecer | Data |
| --- | --- | --- |
| Marluce Vitor | Aprovado | — |
| Gustavo Siqueira | Aprovado | — |
| Rubiana Mendes | Aprovado | — |
| Roxane Nayara | Aprovado | — |

Status: Aprovado pelo grupo.
