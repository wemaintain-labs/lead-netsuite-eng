# Data guide

`data/netsuite.sqlite` is a SQLite file. Open it with `sqlite3`, DBeaver, Python, or any tool you like. All columns are stored as text; cast numbers and parse dates yourself. `data/platform_hk_parts_used.csv` comes from our own maintenance platform, not from NetSuite.

## Period

- Purchases, receipts, bills and fulfilments: dated 1 Nov 2025 to 2 Nov 2026.
- Stock adjustments: from 1 Jan 2025.
- Stock on hand: a snapshot taken on 2 Nov 2026.
- Platform export: Hong Kong jobs from late November 2025 (when Hong Kong started tracking stock in NetSuite), closed up to 2 Nov 2026.
- Hong Kong has no stock adjustments before December 2025, for the same reason.
- Dates are `DD/MM/YYYY` in the NetSuite tables and `YYYY-MM-DD` in the platform export.

## Tables

| Table | One row per |
| --- | --- |
| `po_lines` | purchase order line |
| `receipt_lines` | item receipt line (goods received against a PO) |
| `bill_lines` | vendor bill item line (bills with only expense lines are not included) |
| `fulfilment_lines` | item fulfilment line (parts leaving stock for a client job, from a sales order) |
| `adjustment_lines` | inventory adjustment line (plus a few inventory transfers, `type = InvTrnfr`) |
| `stock_on_hand` | item × location with non-zero stock |
| `items` | inventory item (the shared catalogue) |
| `vendors`, `locations`, `subsidiaries` | reference lists |

## How tables join

- `item_id` links every transaction table to `items.item_id`. The `item` column in transaction tables is the item code (`items.item_code`).
- `receipt_lines.po_id` = `po_lines.po_id`.
- Bills have no line link to POs or receipts.
- Fulfilments link to the sales order (`source_order`) and the client (`customer`), not to POs.
- The platform export has its own work-order numbers, assigned when a job is closed (`closed_date`), which can be weeks after the job (`job_date`). Its `part_code` is what technicians type, meant to match `items.item_code`.

## Quirks you'll meet

- **Subsidiary names** are shown with their parent, for example `WeMaintain SAS : WeMaintain Limited` for Hong Kong.
- **Currencies.** In `po_lines` and `bill_lines`, `rate` and `amount` are in the transaction currency (`currency`). Multiply by `exchangerate` for the subsidiary's own currency. In `adjustment_lines`, `value` is already in the subsidiary's currency: EUR for France, GBP for the UK, SGD for Singapore, HKD for Hong Kong.
- **Signs.** `po_lines.quantityreceived` is stored as a negative number. Adjustment `quantity` and `value` are negative for stock going down.
- **Line 0.** Receipts carry a header row with `line = 0` and no item: filter receipts on `line > 0` for item analysis. In fulfilments, `line = 0` is an ordinary item line; keep it.
- **Item type.** `item_type` on PO and bill lines tells stock items (`Inventory Item`) from services (`Service`).
- **Memos.** `memo` is the transaction memo, `line_memo` the line memo. Either can be empty.
- **Item codes** aren't always unique: a few codes are shared by more than one item. Join on `item_id` where you can.
- **Exchange rates.** For rough euro figures, use these approximate rates: 1 HKD ≈ €0.11, 1 GBP ≈ €1.17, 1 SGD ≈ €0.68.
