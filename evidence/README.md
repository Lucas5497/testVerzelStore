# Evidências

Capturas e registros da execução. O documento consolidado está em [`../reports/evidencias.md`](../reports/evidencias.md).

## Convenção

- Uma pasta por caso de teste (`CT-…`) ou por bug (`BUG-…`).
- Arquivos numerados na ordem dos passos, nome em minúsculas sem acento.
- Cada captura deve mostrar **subtotal, desconto, frete e total** quando o caso envolver valores.
- Evidências de API: salvar requisição/resposta em `.md` ou `.json` (sem dados sensíveis).
- Os testes automatizados geram o relatório HTML do Playwright (`playwright/playwright-report`, ignorado pelo Git); o resumo da última execução fica em `reports/`.
