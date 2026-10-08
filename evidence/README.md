# Evidências

Capturas e registros da execução. O documento consolidado está em [`../reports/evidencias.md`](../reports/evidencias.md).

## Convenção

```
evidence/
├── CT-CUP-001/01-carrinho-antes.png
├── CT-CUP-001/02-cupom-aplicado.png
├── CT-FRT-006/01-subtotal-200.png
├── ...
└── BUG-001/01-resultado-obtido.png
```

- Uma pasta por caso de teste (`CT-…`) ou por bug (`BUG-…`).
- Arquivos numerados na ordem dos passos, nome em minúsculas sem acento.
- Cada captura deve mostrar **subtotal, desconto, frete e total** quando o caso envolver valores.
- Evidências de API: salvar requisição/resposta em `.md` ou `.json` (sem dados sensíveis).
- Os testes automatizados geram o relatório HTML do Playwright (`playwright/playwright-report`, ignorado pelo Git); o resumo da última execução fica em `reports/`.
