---
title: "Adaptações do Tema"
date: "2026-11-03"
type: "page"
layout: "single"
---

# Histórico e comparação do tema Cocoa EH

Este documento registra a origem do tema usado neste site, as diferenças entre a cópia local e o tema upstream, as adaptações feitas para o projeto e a situação de cópias ou derivados encontrados. A comparação foi preparada em 2 de novembro de 2026 com base nos arquivos deste repositório, no histórico Git disponível e nas páginas públicas do GitHub.

## Resumo executivo

- [x] O tema base é **Cocoa Enhanced (Cocoa EH)**, derivado do tema Cocoa de Nishanth Shanmugham.
- [x] O repositório upstream `fuegowolf/cocoa-eh-hugo-theme` foi transferido para [`mtn/cocoa-eh-hugo-theme`](https://github.com/mtn/cocoa-eh-hugo-theme) e está arquivado desde 30 de dezembro de 2022.
- [x] A pasta local `themes/cocoa-eh` é uma cópia adaptada, não um submodule ativo.
- [x] O upstream tem exemplos, documentação de contribuição, layouts e ícones que não foram mantidos nesta cópia. A ausência desses arquivos parece refletir a seleção feita para este site, não uma falha comprovada de implementação.
- [x] O projeto acrescentou ou alterou funcionalidades, em especial internacionalização, navegação, apresentação de datas, suporte a PDF, identidade/licença e adaptação para telas pequenas.
- [x] Foram encontrados repositórios não arquivados que reproduzem a linha Cocoa EH, mas seus commits mais recentes verificáveis são de 2017 a 2020. Não há evidência suficiente para chamá-los de temas atualmente mantidos.
- [x] O histórico de IA identifica algumas ferramentas/modelos e usos, mas não permite reconstruir de forma completa todos os modelos empregados desde 2017.

## 1. Identificação e origem

| Item | Evidência / resultado |
|---|---|
| Tema incorporado | Cocoa Enhanced, também referido como Cocoa EH |
| Repositório upstream | [`fuegowolf/cocoa-eh-hugo-theme`](https://github.com/fuegowolf/cocoa-eh-hugo-theme), atualmente redirecionado para [`mtn/cocoa-eh-hugo-theme`](https://github.com/mtn/cocoa-eh-hugo-theme) |
| Tema anterior | [`nishanths/cocoa-hugo-theme`](https://github.com/nishanths/cocoa-hugo-theme), indicado como tema original no README e em `themes/cocoa-eh/theme.toml` |
| Autor indicado nos metadados do tema | Alexis Tacnet (`fuegowolf`) |
| Estado do upstream | Arquivado em 30/12/2022; somente leitura |
| Evidência local | [`themes/cocoa-eh/theme.toml`](themes/cocoa-eh/theme.toml), [`themes/cocoa-eh/README.md`](themes/cocoa-eh/README.md) e referências históricas em [`content/first.en.md`](content/first.en.md) |
| Forma de incorporação atual | Diretório local versionado em `themes/cocoa-eh`; não há `.gitmodules` na raiz nem submodule ativo |

O nome antigo do proprietário (`fuegowolf`) continua em referências e links nos metadados e no README local. O destino atual é o repositório sob `mtn`.

## 2. Comparação com o upstream arquivado

A árvore local foi comparada com uma cópia clonada do upstream. A lista abaixo distingue arquivos exclusivos do upstream dos arquivos exclusivos do projeto; arquivos com o mesmo caminho também diferem em diversos casos.

### 2.1 Elementos do upstream não presentes na cópia local

| Elemento do upstream | Estado local | Observação |
|---|---|---|
| `exampleSite/` | Ausente | Site demonstrativo/configurações de exemplo do tema; não é necessário para o conteúdo deste projeto. |
| `CONTRIBUTING.md` | Ausente | Instruções para contribuir com o tema upstream. |
| `images/` | Ausente | Imagens de documentação/apresentação do tema. |
| `layouts/partials/base_single.html` e `blog_single.html` | Ausentes | Partials do upstream; a cópia local usa outros layouts/partials e deve ser avaliada segundo os templates efetivamente chamados pelo site. |
| `layouts/_default/taxonomy.html` | Presente no upstream, ausente localmente | Não há evidência de que o projeto use taxonomias que dependam desse layout específico. |
| `static/img/500px.svg`, `gitlab.svg`, `instagram.svg`, `itch.io.svg`, `keybase.svg`, `twitch.png` e `untappd.svg` | Ausentes | Ícones opcionais para redes/serviços que não foram mantidos nesta cópia. |

**Conclusão:** esses elementos demonstram que a cópia local não contém toda a distribuição do upstream. A maioria é material demonstrativo, de contribuição ou recursos opcionais. Não foi identificada uma funcionalidade usada pelo site que tenha sido perdida apenas por esses arquivos estarem ausentes. Isso não equivale a uma auditoria de todas as combinações possíveis do tema.

### 2.2 Elementos próprios da cópia local

| Elemento local | Função/uso |
|---|---|
| `layouts/partials/allLanguages.html` | Lista links de idiomas usando as traduções da página inicial. |
| `layouts/partials/date-format.html` | Formata datas e fornece nomes de meses em Nheengatu. |
| `layouts/partials/i18nlist.html` | Partial adicional de internacionalização. |
| `layouts/section/` | Layouts próprios para seções do site. |
| `layouts/blog/` | Layouts próprios para a área de blog. |
| `layouts/shortcodes/pdf.html` | Incorpora PDFs localizados em `static/docs`, validando nome e existência do arquivo. |
| `static/img/mirror.svg` | Ícone de link para o espelho do site no Bitbucket. |

### 2.3 Arquivos do mesmo caminho com alterações

As diferenças de conteúdo entre upstream e cópia local incluem, entre outros:

- `archetypes/default.md`, `CHANGELOG.md` e `README.md`;
- `layouts/404.html`, `layouts/index.html` e layouts em `layouts/_default/`;
- partials `content.html`, `footer.html`, `footer_scripts.html`, `head.html`, `head_includes.html`, `header.html`, `li.html`, `meta.html` e `page-heading.html`;
- folhas de estilo `colors.css`, `main.css`, `min600px.css`, `min769px.css` e `progressively.min.css`;
- `static/js/progressively.min.js`.

O diff confirma que a cópia foi alterada ao longo do tempo, mas não classifica sozinho cada linha como mudança funcional, correção, atualização ou adaptação local.

## 3. Alterações do projeto em relação ao Cocoa EH

O checklist resume as adaptações verificadas na configuração, nos arquivos do tema e no histórico descrito em [`ia-log.md`](ia-log.md).

| Checklist | Área | Alteração do projeto | Principais arquivos / evidências |
|---|---|---|---|
| [x] | Internacionalização | Configuração de seis línguas: português brasileiro, inglês, francês, chinês, Nheengatu e Dothraki; títulos, rótulos e textos de navegação localizados. | [`config.toml`](config.toml), `themes/cocoa-eh/layouts/partials/allLanguages.html`, `footer_scripts.html`, `header.html` |
| [x] | Datas | Formatação de datas ajustada para o site e nomes dos meses fornecidos em Nheengatu; comportamento integrado às páginas e listagens. | `themes/cocoa-eh/layouts/partials/date-format.html`, `page-heading.html`, `li.html`, `single.html`, `index.html` |
| [x] | Navegação | Menu construído a partir das seções/páginas do site, links entre idiomas e exclusões específicas de páginas como License e Home. | `themes/cocoa-eh/layouts/partials/header.html`, `allLanguages.html` |
| [x] | Responsividade | Menu principal alterado para quebrar em várias linhas em telas estreitas, evitando que os links fiquem fora da tela. | `themes/cocoa-eh/layouts/partials/css/main.css`; entrada 5 em `ia-log.md` |
| [x] | PDF | Shortcode para exibir PDFs em `static/docs`, com validação do nome/extensão e da existência, mais página de exemplo. | `themes/cocoa-eh/layouts/shortcodes/pdf.html`, `content/francisco/documentos-pdf.pt-br.md`; entrada 9 em `ia-log.md` |
| [x] | Identidade do site | Títulos, descrições, cores, links sociais, ícones, logotipo e metadados personalizados. | [`config.toml`](config.toml), `themes/cocoa-eh/layouts/partials/` |
| [x] | Espelho | Link com ícone no rodapé para a cópia do site no Bitbucket. | `themes/cocoa-eh/layouts/partials/footer.html`, `themes/cocoa-eh/static/img/mirror.svg`; entrada 8 em `ia-log.md` |
| [x] | URLs e conteúdo | Ajustes em URLs geradas e em elementos de apresentação/SEO, inclusive correção de barra duplicada em URL do site. | `themes/cocoa-eh/layouts/partials/header.html`; entrada 6 em `ia-log.md` |
| [x] | Licenciamento | Aviso separado para a licença original dos arquivos do tema e os direitos dos textos do site. | [`LICENSE`](LICENSE), [`content/license.md`](content/license.md); entrada 5 em `ia-log.md` |
| [x] | Implantação | Migração da configuração de publicação do Bitbucket para GitHub Actions/Pages e retirada de configuração de pipeline anterior. | `.github/workflows/ci.yml`, `.gitignore`, `README.md`; entrada 0 em `ia-log.md` |

Os caminhos de partials acima são relativos à pasta `themes/cocoa-eh/`, salvo quando o caminho completo é explicitado.

## 4. Modelos e ferramentas de IA registrados

Esta seção se limita ao que está registrado no repositório. O log foi iniciado em 2026 e não é um inventário completo da implementação iniciada em 2017.

| Modelo/ferramenta mencionado | Uso documentado | Evidência e ressalvas |
|---|---|---|
| Gemini (predominantemente) | Traduções em mandarim descritas como feitas exclusivamente por LLM, sem supervisão humana; traduções em francês descritas como feitas por LLM e depois revisadas pelo autor. | Entradas de 15/07/2026 em [`ia-log.md`](ia-log.md). |
| Modo IA da busca Google | Conteúdo inserido em quatro arquivos de documentos em outubro de 2026. | Entrada 7 em `ia-log.md`; o modelo subjacente não é identificado. |
| Copilot com GPT-6 Luna | Nota do próprio log afirma que a automação do `ia-log.md` foi feita com Copilot usando GPT-6 Luna. | Comentário final de `ia-log.md`, datado de outubro de 2026; é uma declaração registrada no projeto, não uma auditoria independente. |
| Ferramenta/modelos anteriores | A nota histórica do log atribui intervenções antigas sobretudo à estrutura do site e a traduções supervisionadas. | Declaração do responsável nos comentários finais de `ia-log.md`; modelos e sessões anteriores não são detalhados. |

**Limite da evidência:** não é possível determinar apenas com os arquivos atuais todos os modelos usados em cada alteração, suas versões, datas ou contribuição exata. O histórico Git permite acompanhar mudanças, mas não registra de forma sistemática o modelo utilizado. Portanto, a lista acima não deve ser interpretada como inventário exaustivo.

## 5. Cópias, forks e manutenção atual

O repositório oficial Cocoa EH indicado nos metadados está arquivado. A busca por repositórios públicos localizou cópias com o mesmo nome/README e conteúdo derivado da linha Cocoa EH:

| Repositório | Estado de arquivamento observado no GitHub | Último commit visível consultado | Avaliação |
|---|---:|---:|---|
| [`mtn/cocoa-eh-hugo-theme`](https://github.com/mtn/cocoa-eh-hugo-theme) | Arquivado em 30/12/2022 | 17/05/2020 | Upstream arquivado; não mantido atualmente. |
| [`kobyrwork/cocoa-eh-hugo-theme`](https://github.com/kobyrwork/cocoa-eh-hugo-theme) | Não marcado como arquivado | 10/08/2017 | Cópia que conserva README e código da linha Cocoa EH; sem commits recentes verificáveis. |
| [`rajr5/cocoa-eh-hugo-theme`](https://github.com/rajr5/cocoa-eh-hugo-theme) | Não marcado como arquivado | 10/08/2017 | Cópia que conserva README e código da linha Cocoa EH; sem commits recentes verificáveis. |
| [`fahmifan/cocoa-eh`](https://github.com/fahmifan/cocoa-eh) | Não marcado como arquivado | 05/12/2020 | Repositório relacionado, porém sem atividade recente verificável. |

Os metadados atuais indicam que os três repositórios encontrados não estão marcados como forks pelo GitHub, e a semelhança dos READMEs/código não prova por si só sua relação genealógica por metadados de fork. Assim, a conclusão segura é que existem cópias públicas não arquivadas que preservam o tema ou sua linha; **não foi encontrada evidência de manutenção ativa recente**, nem um sucessor oficial em desenvolvimento.

## 6. Limites e rastreabilidade

- A cópia local foi comparada com o upstream arquivado disponível no momento da pesquisa; uma comparação com revisões históricas intermediárias poderia produzir outras diferenças.
- A árvore do projeto mostra o resultado atual. O histórico Git local contém commits antigos associados a edições feitas no Bitbucket, mas este clone não tem o remoto Bitbucket configurado.
- O `ia-log.md` registra intervenções assistidas por IA apenas a partir de 2026 e declara que modificações mais antigas não foram registradas em detalhe. Não é possível atribuir com rigor cada alteração do tema a uma sessão, modelo ou data.
- O status “não arquivado” no GitHub não significa manutenção. A data do último commit consultado é o critério usado aqui para evitar essa inferência.
- A lista de arquivos diferentes é uma comparação de árvore; não constitui auditoria exaustiva de segurança, acessibilidade, compatibilidade com toda versão de Hugo ou equivalência visual.

## Checklist de conclusão

- [x] Identificar o tema e o repositório upstream.
- [x] Verificar elementos do upstream ausentes na cópia local.
- [x] Registrar as principais alterações locais em relação ao tema.
- [x] Avaliar as evidências de uso de modelos/ferramentas de IA sem presumir um histórico completo.
- [x] Verificar cópias/derivados públicos e distinguir repositórios não arquivados de projetos com manutenção recente comprovável.
- [x] Registrar as limitações da pesquisa para consultas futuras.
