# Execução dos testes

> Resultado de **cada** cenário da modelagem. Legenda: ✅ Passou · ❌ Falhou (ver bug) · ⚠️ Passou com ressalva / ambiguidade · ⛔ Bloqueado · ⬜ Pendente

- **Ambiente:** https://verzel-store.qa-test-verzel-store.workers.dev/

- **Navegador / SO:** Chrome 139.0.7258.128 · Windows

- **Período de execução:** 06/10/2026 a 09/10/2026

- **Executor:** (preencher nome)

## Resumo

| Total | ✅ | ❌ | ⚠️ | ⛔ | ⬜ |
|---:|---:|---:|---:|---:|---:|
| 37 | 24 | 5 | 3 | 5 | 0 |

## Casos de teste

| ID | Cenário | CA | Técnica | Tipo | Resultado | Obtido (resumo) | Bug | Evidência |
|---|---|---|---|---|:---:|---|---|---|
| CT-CUP-001 | Aplicar cupom válido | CA01 | Particionamento de Equivalência | Manual + Automatizado | ✅ | Cupom aceito; desconto de 10% aplicado corretamente sobre o subtotal. | | [`## CT-CUP-001`](evidencias.md#ct-cup-001--aplicar-cupom-válido) |
| CT-CUP-002 | Aplicar cupom utilizando letras minúsculas | CA02 | Particionamento de Equivalência | Manual | ✅ | Cupom aceito em minúsculas (`bemvindo10`); mesmo desconto de `BEMVINDO10`. | | [`## CT-CUP-002`](evidencias.md#ct-cup-002--aplicar-cupom-utilizando-letras-minúsculas) |
| CT-CUP-003 | Aplicar cupom com espaços no início | CA02 | Error Guessing | Manual | ✅ | Espaços no início ignorados; cupom aceito e desconto aplicado. | | [`## CT-CUP-003`](evidencias.md#ct-cup-003--aplicar-cupom-com-espaços-no-início) |
| CT-CUP-004 | Aplicar cupom com espaços no final | CA02 | Error Guessing | Manual | ✅ | Espaços no final ignorados; cupom aceito e desconto aplicado. | | [`## CT-CUP-004`](evidencias.md#ct-cup-004--aplicar-cupom-com-espaços-no-final) |
| CT-CUP-005 | Aplicar cupom inexistente | CA03 | Particionamento de Equivalência | Manual + Automatizado | ✅ | Sistema retorna mensagem de cupom inválido e não aplica desconto. | | [`## CT-CUP-005`](evidencias.md#ct-cup-005--aplicar-cupom-inexistente) |
| CT-CUP-006 | Aplicar cupom expirado | CA04 | Particionamento de Equivalência | Manual | ✅ | Sistema retorna mensagem de cupom expirado e não aplica desconto. | | [`## CT-CUP-006`](evidencias.md#ct-cup-006--aplicar-cupom-expirado) |
| CT-CUP-007 | Tentar aplicar segundo cupom sem remover o primeiro | CA05 | Teste de Transição de Estados | Manual | ✅ | Após aplicar um cupom, só resta a opção de remover; não permite dois cupons ativos. | | [`## CT-CUP-007`](evidencias.md#ct-cup-007--tentar-aplicar-segundo-cupom-sem-remover-o-primeiro) |
| CT-CUP-008 | Remover cupom e aplicar outro | CA05 | Transição de Estados | Manual | ✅ | Botão "Remover cupom" remove o desconto e recalcula o carrinho corretamente. | | [`## CT-CUP-008`](evidencias.md#ct-cup-008--remover-cupom-e-aplicar-outro) |
| CT-FRT-001 | Subtotal abaixo de R$ 200,00 | CA07 | Particionamento de Equivalência | Manual | ⚠️ | Subtotal R$ 199,90: cobra frete R$ 19,90 e informa "Faltam R$ 0,10". Fronteira R$ 199,99 não alcançável com o catálogo. | | [`## CT-FRT-001`](evidencias.md#ct-frt-001--subtotal-abaixo-de-r-20000) |
| CT-FRT-002 | Subtotal exatamente R$ 200,00 | CA06 | Valor Limite | Manual + Automatizado | ❌ | Subtotal R$ 200,00 ainda cobra frete R$ 19,90 e exibe "Faltam R$ 0,00 para o frete grátis". | BUG-001 | [`## CT-FRT-002`](evidencias.md#ct-frt-002--subtotal-exatamente-r-20000) |
| CT-FRT-003 | Subtotal acima de R$ 200,00 | CA06 | Valor Limite | Manual | ⚠️ | Subtotal R$ 209,40: frete grátis (R$ 0,00). Fronteira R$ 200,01 não alcançável com o catálogo. | | [`## CT-FRT-003`](evidencias.md#ct-frt-003--subtotal-acima-de-r-20000) |
| CT-FRT-004 | Subtotal imediatamente abaixo do limite | CA07 | Valor Limite | Manual | ⛔ | Bloqueado: não existe combinação de produtos que produza subtotal exato de R$ 199,99 (catálogo só em múltiplos de R$ 0,10). | | [`## CT-FRT-004`](evidencias.md#ct-frt-004--subtotal-imediatamente-abaixo-do-limite) |
| CT-FRT-005 | Subtotal significativamente abaixo do limite | CA07 | Particionamento de Equivalência | Manual | ✅ | Subtotal R$ 150,00: frete R$ 19,90 e mensagem "Faltam R$ 50,00 para o frete grátis". | | [`## CT-FRT-005`](evidencias.md#ct-frt-005--subtotal-significativamente-abaixo-do-limite) |
| CT-FRT-006 | Frete deve considerar subtotal antes do desconto | CA08 | Tabela de Decisão | Manual | ⚠️ | Subtotal R$ 209,40 + cupom: frete permanece R$ 0,00 após desconto (correto). Ressalva por dependência do BUG-001 no limite exato de R$ 200,00. | | [`## CT-FRT-006`](evidencias.md#ct-frt-006--frete-deve-considerar-subtotal-antes-do-desconto) |
| CT-FRT-007 | Desconto não incide sobre o frete | CA09 | Tabela de Decisão | Manual | ✅ | Subtotal R$ 100,00 + cupom: desconto só sobre produtos; frete permanece R$ 19,90. | | [`## CT-FRT-007`](evidencias.md#ct-frt-007--desconto-não-incide-sobre-o-frete) |
| CT-QTD-001 | Adicionar 4 unidades | CA10 | Valor Limite | Manual | ✅ | Sistema permite adicionar 4 unidades do mesmo produto (UI e página de produtos). | | [`## CT-QTD-001`](evidencias.md#ct-qtd-001--adicionar-4-unidades) |
| CT-QTD-002 | Adicionar exatamente 5 unidades | CA10 | Valor Limite | Manual | ✅ | Sistema permite adicionar exatamente 5 unidades do mesmo produto. | | [`## CT-QTD-002`](evidencias.md#ct-qtd-002--adicionar-exatamente-5-unidades) |
| CT-QTD-003 | Tentar adicionar 6 unidades pela interface | CA10 | Valor Limite / Error Guessing | Manual | ✅ | UI bloqueia a 6ª unidade e exibe "Limite de 5 unidades por produto". | | [`## CT-QTD-003`](evidencias.md#ct-qtd-003--tentar-adicionar-6-unidades-pela-interface) |
| CT-QTD-004 | Solicitar 6 unidades através da API | CA10 | Valor Limite / Error Guessing | Manual + Automatizado (API) | ❌ | API aceita 6+ unidades em `/carrinho/calcular` e em `/pedidos` (HTTP 200). | BUG-002 | [`## CT-QTD-004`](evidencias.md#ct-qtd-004--solicitar-6-unidades-através-da-api) |
| CT-QTD-005 | Solicitar exatamente 5 unidades através da API | CA10 | Valor Limite | Manual + Automatizado (API) | ⛔ | Bloqueado pela ausência de validação de limite na API (BUG-002); qualquer quantidade > 0 é aceita. | BUG-002 | [`## CT-QTD-005`](evidencias.md#ct-qtd-005--solicitar-exatamente-5-unidades-através-da-api) |
| CT-QTD-006 | Comparar regra de quantidade entre UI e API | CA10 | Consistência UI × API | Manual | ❌ | UI respeita o limite de 5; API não respeita. Divergência entre camadas. | BUG-002 | [`## CT-QTD-006`](evidencias.md#ct-qtd-006--comparar-regra-de-quantidade-entre-ui-e-api) |
| CT-VAL-001 | Desconto com resultado de duas casas decimais | CA11 | Regra de Negócio | Manual | ✅ | Valores monetários exibidos com 2 casas decimais e cálculo consistente. | | [`## CT-VAL-001`](evidencias.md#ct-val-001--desconto-com-resultado-de-duas-casas-decimais) |
| CT-VAL-002 | Desconto que exige arredondamento | CA11 | Regra de Negócio | Manual | ⛔ | Bloqueado: método de arredondamento (meio-centavo) não está especificado na documentação; único cupom que geraria o caso (15%) está expirado. | | [`## CT-VAL-002`](evidencias.md#ct-val-002--desconto-que-exige-arredondamento) |
| CT-VAL-003 | Consistência dos valores do pedido | CA11 | Consistência UI × API | Manual | ⛔ | Bloqueado: validação conjunta UI/API do total (`subtotal − desconto + frete`) ficou pendente de execução formal. | | [`## CT-VAL-003`](evidencias.md#ct-val-003--consistência-dos-valores-do-pedido) |
| CT-API-CUP-001 | Aplicação de cupom válido pela API | CA01 | Particionamento de Equivalência / API | Manual + Automatizado (API) | ✅ | API aplica 10% corretamente; status HTTP e valores consistentes. | | [`### CT-API-CUP-001`](evidencias.md#ct-api-cup-001--aplicação-de-cupom-válido) |
| CT-API-CUP-002 | Aplicação de cupom em letras minúsculas pela API | CA02 | Particionamento de Equivalência / API | Manual + Automatizado (API) | ✅ | API aceita cupom em minúsculas e calcula o desconto corretamente. | | [`### CT-API-CUP-002`](evidencias.md#ct-api-cup-002--cupom-informado-em-letras-minúsculas) |
| CT-API-CUP-003 | Aplicação de cupom com espaços em branco pela API | CA02 | Error Guessing / API | Manual + Automatizado (API) | ✅ | API ignora espaços nas pontas e aplica o cupom corretamente. | | [`### CT-API-CUP-003`](evidencias.md#ct-api-cup-003--cupom-com-espaços-em-branco) |
| CT-API-CUP-004 | Rejeição de cupom inexistente pela API | CA03 | Particionamento de Equivalência / API | Manual + Automatizado (API) | ✅ | API recusa cupom inexistente (HTTP 422, mensagem de cupom inválido). | | [`### CT-API-CUP-004`](evidencias.md#ct-api-cup-004--cupom-inexistente) |
| CT-API-CUP-005 | Rejeição de cupom expirado pela API | CA04 | Particionamento de Equivalência / API | Manual + Automatizado (API) | ✅ | API recusa cupom expirado (HTTP 422, mensagem de cupom expirado). | | [`### CT-API-CUP-005`](evidencias.md#ct-api-cup-005--cupom-expirado) |
| CT-API-FRT-001 | Subtotal R$ 199,90 pela API | CA07 | Particionamento de Equivalência / API | Manual + Automatizado (API) | ✅ | API cobra frete R$ 19,90 para subtotal abaixo de R$ 200,00 (HTTP 200). | | [`### CT-API-FRT-001`](evidencias.md#ct-api-frt-001--subtotal-abaixo-do-limite-de-frete-grátis) |
| CT-API-FRT-002 | Subtotal exatamente R$ 200,00 pela API | CA06 | Valor Limite / API | Manual + Automatizado (API) | ❌ | API não concede frete grátis em R$ 200,00 (`frete: 19.9`, `freteGratis: false`, `valorFaltanteFreteGratis: 0`). | BUG-001 | [`### CT-API-FRT-002`](evidencias.md#ct-api-frt-002--limite-exato-para-frete-grátis) |
| CT-API-FRT-003 | Subtotal R$ 209,40 pela API | CA06 | Valor Limite / API | Manual + Automatizado (API) | ✅ | API retorna frete R$ 0,00 e frete grátis para subtotal acima de R$ 200,00. | | [`### CT-API-FRT-003`](evidencias.md#ct-api-frt-003--subtotal-acima-do-limite-de-frete-grátis) |
| CT-API-FRT-004 | Subtotal exatamente R$ 199,99 pela API | CA07 | Valor Limite / API | Manual + Automatizado (API) | ⛔ | Bloqueado: não existe combinação de produtos que produza subtotal exato de R$ 199,99. | | [`### CT-API-FRT-004`](evidencias.md#ct-api-frt-004--limite-imediatamente-abaixo-do-frete-grátis) |
| CT-API-FRT-005 | Frete grátis considerando subtotal antes do desconto | CA08 | Tabela de Decisão / Regra de Negócio / API | Manual + Automatizado (API) | ✅ | API calcula frete com base no subtotal antes do desconto (comportamento correto). | | [`### CT-API-FRT-005`](evidencias.md#ct-api-frt-005--frete-grátis-baseado-no-subtotal-antes-do-desconto) |
| CT-API-FRT-006 | Desconto não altera o valor do frete | CA09 | Tabela de Decisão / API | Manual + Automatizado (API) | ✅ | Desconto do cupom não altera o valor do frete na API. | | [`### CT-API-FRT-006`](evidencias.md#ct-api-frt-006--frete-pago-após-aplicação-do-cupom) |
| CT-API-QTD-007 | Impedir criação de pedido com 6 unidades | CA10 | Regra de Negócio / Error Guessing / API | Manual + Automatizado (API) | ❌ | `/api/carrinho/calcular` e `/api/pedidos` aceitam 6+ unidades (HTTP 200). | BUG-002 | [`### CT-API-QTD-007`](evidencias.md#ct-api-qtd-007--bloqueio-de-quantidade-superior-ao-limite) |
| CT-API-VAL-001 | Consistência dos valores monetários pela API | CA11 | Regra de Negócio / API | Manual + Automatizado (API) | ✅ | Valores com 2 casas; total = subtotal − desconto + frete (R$ 100 − R$ 10 + R$ 19,90 = R$ 109,90). | | [`### CT-API-VAL-001`](evidencias.md#ct-api-val-001--consistência-dos-valores-monetários) |

## Sessões exploratórias

Cada sessão tem *charter* e tempo definido; anotar o que foi explorado, o que foi encontrado e as perguntas geradas.

| Sessão | Charter | Tempo | Achados | Bugs / dúvidas |
|---|---|---:|---|---|
| SE-01 | Explorar comportamentos de estado e entradas não cobertas nos testes estruturados: cupom vazio, somente espaços, caixa mista, caracteres especiais, reaplicação após remoção e troca de cupom. | 30 min | | |
| SE-02 | Explorar combinações dinâmicas do carrinho não cobertas pelos casos estruturados: alterar quantidade, adicionar/remover produtos e verificar o recálculo de cupom, frete e total após mudanças. | 30 min | | |

## Regressão

Após correções (ou ao final da execução), reexecutar os fluxos P1 pela automação e registrar o resultado:

| Data | Comando | Resultado |
|---|---|---|
| | `npm test` (em `playwright/`) | |