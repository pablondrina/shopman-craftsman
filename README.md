# shopman-craftsman

Gestão de produção para Django. Receitas com BOM (Bill of Materials), ordens de produção com lifecycle completo, integração com estoque para materialização automática.

Part of the [Django Shopman](https://github.com/pablondrina/django-shopman) commerce framework.

## Domínio

- **Recipe** — receita de produção. Lista de insumos (RecipeItem) com quantidades e coeficientes.
- **RecipeItem** — insumo da receita (SKU + quantidade + coeficiente de rendimento).
- **WorkOrder** — ordem de produção. Status: PLANNED → IN_PROGRESS → DONE / CANCELLED.
- **WorkOrderItem** — item da ordem (produto + quantidade planejada/produzida).
- **WorkOrderEvent** — timeline de eventos da produção (início, pausa, conclusão).
- **CodeSequence** — geração sequencial de códigos de produção.

## CraftService

| Método | O que faz |
|--------|-----------|
| `create_work_order(recipe, qty)` | Cria ordem de produção |
| `start_work_order(wo_id)` | Inicia produção |
| `complete_work_order(wo_id, qty_produced)` | Finaliza e registra produção |
| `suggest_production(sku, qty)` | Sugere produção baseado em estoque + demanda |
| `get_pending_orders()` | Ordens pendentes e em andamento |

## Contribs

- `craftsman.contrib.demand` — Planejamento de demanda baseado em histórico de vendas.
- `craftsman.contrib.stocking` — Bridge craftsman↔stockman. Signal `holds_materialized` materializa holds quando produção finaliza.
- `craftsman.contrib.admin_unfold` — Admin com Unfold theme.

## Signals

- `production_changed(sender, work_order, event_type)` — disparado em mudanças de status da produção.

## Instalação

```bash
pip install shopman-craftsman
```

```python
INSTALLED_APPS = [
    "shopman.craftsman",
    "shopman.craftsman.contrib.demand",    # opcional: demand planning
    "shopman.craftsman.contrib.stocking",  # opcional: bridge com estoque
]
```

## Development

```bash
git clone https://github.com/pablondrina/django-shopman.git
cd django-shopman && pip install -e packages/craftsman
make test-craftsman  # ~158 testes
```

## License

MIT — Pablo Valentini
