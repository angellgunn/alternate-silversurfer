# Registro de uso de IA

Este arquivo registra procedimentos assistidos por IA no repositório, com o objetivo de manter transparência sobre a participação da ferramenta nas alterações do projeto.

## Propósito

- Servir como histórico claro de intervenções assistidas por IA.
- Permitir que qualquer pessoa consulte, em um único lugar, quando uma ação foi realizada, qual foi o prompt do usuário e quais arquivos foram afetados.
- Manter a transparência sem duplicar detalhes já preservados no versionamento do Git.

## Regras de manutenção

1. Cada procedimento assistido por IA deve receber uma nova entrada neste arquivo.
2. Cada entrada deve incluir, no mínimo:
   - data e hora do procedimento;
   - prompt do usuário, sem modificações;
   - lista de arquivos modificados.
3. As alterações em si devem permanecer registradas no histórico do Git; este log não precisa repetir o conteúdo das mudanças.
4. Se o usuário solicitar, este arquivo pode ser expandido para incluir também contexto adicional, como objetivo da tarefa, ferramentas utilizadas ou observações da execução.
5. Durante o chat atual e em futuras sessões, o log deve ser mantido atualizado em tempo real sempre que houver uma ação assistida por IA, incluindo novas entradas imediatamente após a execução.
6. Para futuras manutenções, o sistema/assistente deve considerar este arquivo como a fonte oficial de transparência operacional sobre uso de IA neste repositório.

## Estrutura recomendada para novas entradas

### Entrada N

- Data e hora do procedimento:
- Prompt do usuário (sem modificações):
- Arquivos modificados:

## Entrada -7

- Data e hora do procedimento: 2026-07-10 20:42:00 -03
- Prompt do usuário (sem modificações):

> Criei a página "sereia" deste projeto a partir do substack de Baldolino Calvino. Lá, além dos textos de Francisco, existem alguns textos de Calvino mesmo. Gostaria de criar uma postagem dele a partir da transcrição dessa postagem do Substack:
> https://open.substack.com/pub/baldolinocalvino/p/rascunho-aleatorio-numero-2

- Arquivos modificados:
  - content/calvino/rascunho-aleatorio.pt-br.md

## Entrada -6

- Data e hora do procedimento: 2026-07-10 20:42:00 -03
- Prompt do usuário (sem modificações):

> Ficou bom. Vamos fazer o mesmo com outra postagem do Substack de Calvino:
> https://baldolinocalvino.substack.com/p/mais-de-um-ano-depois

- Arquivos modificados:
  - content/calvino/mais-de-um-ano-depois.pt-br.md

## Entrada -5

- Data e hora do procedimento: 2026-07-12 19:20:00 -03
- Prompt do usuário (sem modificações):

> Solicitação 1: há mais uma postagem do Substack do Calvino para transcrever:
> https://baldolinocalvino.substack.com/p/e-tudo-culpa-do-kant
>
> Solicitação 2: nessa ultima postagem existem muitos links q não foram transcritos. Muitos conceitos tem links para definições. É possível transcrever isso?
>
> Solicitação 3: gosto de organizar os links ao final do arquivo markdown e deixar links relativos no texto
>
> Solicitação 4: neste texto, localize todas as primeiras letras de sentenças, q normalmente são maiúsculas,  e as torne minúsculas. se já estiverem minusculas, manter assim

- Arquivos modificados:
  - content/calvino/e-tudo-culpa-do-kant.pt-br.md

## Entrada -4

- Data e hora do procedimento: 2026-07-14 16:35:00 -03
- Prompt do usuário (sem modificações):

> neste projeto, a internacionalização segue um padrão instalado em 2022. o sistema está dando mensagens de deprecação de pelo menos parte desse padrão. seria interessante revisar as modificações necessárias para atualização dessa caracteristica do projeto

- Arquivos modificados:
  - config.toml
  - themes/cocoa-eh/layouts/partials/header.html
  - themes/cocoa-eh/layouts/partials/allLanguages.html
  - themes/cocoa-eh/layouts/partials/footer_scripts.html
  - themes/cocoa-eh/layouts/partials/page-heading.html

## Entrada -3

- Data e hora do procedimento: 2026-07-14 20:37:00 -03
- Prompt do usuário (sem modificações):

> Solicitação 1: ok, ficou bom realmente.mas estamos com outro problema. a data que aparece nas postagens e nas páginas índices das postagens segue opadrão de cada língua, mas está malformada para a língua nheengatu. Existe apenas uma página em nheengatu,"Muruxawamirĩ", porém a data aparece sem o nome do mês tanto na postagem quanto na página onde ela aparece listada
>
> Solicitação 2: Verdade, na única postagem em Nheengatu o mês aparece em português. Na página inicial do idioma Nheengatu, "Ana Papera",q lista as postagens mais recentes, o mês deveria aparecer,mas ainda não está visível, todavia.
>
> Solicitação 3: ficou ótimo. Última coisa, por ora, existe algum recurso q permita q os nomes dos meses em nheengatu apareçam nessa lingua e não em portugues?

- Arquivos modificados:
  - themes/cocoa-eh/layouts/partials/date-format.html
  - themes/cocoa-eh/layouts/partials/page-heading.html
  - themes/cocoa-eh/layouts/partials/li.html
  - themes/cocoa-eh/layouts/partials/single.html
  - themes/cocoa-eh/layouts/partials/index.html

# Entrada -2

- Data e hora do procedimento: 2026-07-15 16:28:00 -03
- Prompt do usuário (sem modificações):

> Solicitação 1: este projeto é multilingue, porém a origem das traduções é heterogenea. As traduções em mandarim foram feitas exclusivamente por LLM (Gemini, predominantemente) e não tiveram supervisão humana. Gostaria de colocar um pós-escrito em todas as postagens em mandarim com essa declaração (em mandarim, claro). Antes de introduzir o P.S. mostre-me o texto a ser inserido, pfv
>
> Solicitação 2: ficou bom assim, obrigado. Pode inserir o texto em todas as postagens em mandarim
>
> Solicitação 3: perfeito. Agora uma nota semelhante deve ser acrescentada nos textos em francês (exceto micromegas). A diferença é que a declaração deve afirmar que ocorreu revisão humana.
>
> Solicitação 4: faltou informar na última frase da nota em francês q a tradução foi feita por LLM (predominantemente Gemini) e depois foi feita a revisão humana
>
> Solicitação 5: ficou bom assim, porém me dei conta de que será melhor declarar explicitamente q a revisão foi feita pelo autor

- Arquivos modificados:
  - todos os arquivos em mandarim
  - todos os arquivos em francês (exceto micrômegas)

# Entrada -2

- Data e hora do procedimento: 2026-07-15 22:43:00 -03
- Prompt do usuário (sem modificações):

> Solicitação 1: este arquivo tem o Dothraki Language Dictionary. na época, não consegui completa-lo (na verdade, transcrevi apenas parte dos vocábulos iniciados pela letra "A"). seria possível completar a transcrição? também verificar antes se existe uma versão mais recente e acrescentar as declarações e de atribuição de autoria, direitos e colocar links para o original
>
> Solicitação 2: Uma coisa q notei é a ausência de linhas entre os vocábulos. É necessário acrescentar um espaço entre cada linha referente a cada vocábulo a fim de interpretar o markdown como linhas separadas
>
> Solicitação 3: os termos entre colchetes são IPA e usam símbolos próprios dessa terminologia. por exemplo, o símbolo `&#643;` foi substituído por S dentro dos colchetes. é possível trocar esses S maiúsculos (apenas dentro de colchetes) por `&#643;`?
>
> Solicitação 4: o mesmo ocorre com R e `&#638;`
>
> Solicitação 5: da mesma forma, o sinal  deve ser substituído por `&#810;`
>
> Solicitação 6: o mesmo com Z e `&#658;`
>
> Solicitação 7: o mesmo com T e `&#952;`
>
> Solicitação 8: o mesmo com A e `&#593;`
>
> Solicitação 9: o mesmo com O e `&#596;`

- Arquivos modificados:
  - content/ase/dld.dt.md

## Entrada -1

- Data e hora do procedimento: 2026-07-27 17:24:42 -03
- Prompt do usuário (sem modificações):

> crie uma nova postagem em portugues de Francisco, com titulo de "Ilanavia" e status de rascunho. deixe o conteúdo vazio

- Arquivos modificados:
  - content/francisco/ilanavia.pt-br.md

## Entrada 0

- Data e hora do procedimento: 2026-08-02 11:16:24 -03
- Prompt do usuário (sem modificações):

> Solicitação 1: este projeto está sendo migrado do bitbucket para o github. inicialmente, vaos retirar tudo relacionado ao bitbucket. em seguida, preparar para CI no github
>
> Solicitação 2: ok, prossiga
>
> Solicitação 3: nesse readme tem links vazios q desejo linkar para um erro 401. se não tivermos uma página de erro 401 no projeto, vamos fazer uma, com a logo do projeto aparecendo grande no fundo, num tom bem claro
>
> Solicitação 4: a partir de agora, desejo que o projeto seja transparente em relação ao uso de IA. talvez possamos criar um log de ia no root e deixar lá para consultas. o primeiro registro deve ser desta sessão de chat atual (sessão completa, não apenas o q foi feito hoje). o q o log deve conter: data e hora do procedimento, o prompt do usuário sem modificações, a lista de arquivos modificados (as modificações em si ficam registradas no versionamento, então não há necessidade de duplicidade de informações).

- Arquivos modificados:
  - README.md
  - config.toml
  - bitbucket-pipelines.yml (removido)
  - content/calangro/sexta.pt-br.md
  - content/calangro/sixieme.fr.md
  - content/calangro/sixth.en.md
  - .github/workflows/ci.yml
  - .gitignore
  - static/401.html

## Entrada 1

- Data e hora do procedimento: 2026-08-02 11:22:32 -03
- Prompt do usuário (sem modificações):

> inclua as modificações realizadas nesse chat q ainda não o foram. enquanto o chat atual durar, mantenha o log atualizado em tempo real

- Arquivos modificados:
  - ia-log.md
  - README.md

## Entrada 2 (registro de confirmação)

- Data e hora do procedimento: 2026-08-02 11:22:32 -03
- Prompt do usuário (sem modificações):

> verifique se existem registros de solicitações anteriores aquelas que já registramos q possam ser acrescentadas ao log, mantendo a data correspondente

- Arquivos modificados:
  - ia-log.md

## Entrada 3

- Data e hora do procedimento: 2026-08-03 08:15:35 -03
- Prompt do usuário (sem modificações):

> verifique o arquivo ia-log.md e o complete, seguindo suas instruções, com as informações das sessões de chat q estão registradas

- Arquivos modificados:
  - ia-log.md

## Entrada 4

- Data e hora do procedimento: 2026-08-03 08:15:35 -03
- Prompt do usuário (sem modificações):

> a data registrada nos registros sobre procedimento anteriores deve ser aquela relacionada aos procedimentos, e não a essa sessão atual. além disso, as solicitações mais recentes, feitas nesta sessão, debem incluidas no log, como as demais

- Arquivos modificados:
  - ia-log.md

<!-- A Copilot gerou a estrutura básica e os registros de 0 a 4 (eram 1 a 5, eu mudei). Todavia, ao solicitar q revisasse sessões anteriores, o resultado ficou muito ruim, gerando apenas os registros que agora são -1 e -7 (foram colocados depois dos outros, como registros 6 e 7), com erros de data e arquivos modificados. Tive que fazer a revisão e acrescentar os demais registros à mão. Fica a se ver se esse logo "automarizado" de solicitações de IA vai dar certo. Declaro logo que modificações mais antigas por IA não ficaram registradas, porém incluíram exclusivamente estrutura da página e traduções supervisionadas (de acordo com as declarações). Nada de texto escrito por IA por aqui, a não ser nas postagens sobre isso de Angell. -->
