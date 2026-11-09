# NetSuite Handover Notes

**From:** Northbridge ERP Consulting
**Last updated:** 30 October 2026
**Engagement end:** 30 November 2026

## 1. Structure

- Parent: WeMaintain SAS (France, EUR).
- Subsidiaries: WeMaintain Technologies Ltd (UK, GBP), WeMaintain Technologies Pte Ltd (Singapore, SGD), WeMaintain Limited (Hong Kong, HKD), WeMaintain Technologies GmbH (Germany, EUR; dormant, no transactions).
- Inventory costing: FIFO. Multi-location inventory is on.
- Go-live: France and the UK run fully in NetSuite. Hong Kong has used NetSuite for invoicing and bills since early 2025, but only started tracking stock in NetSuite in late 2025. Our consultant loaded Hong Kong's opening stock in early December 2025 from the old depot's spreadsheet, at zero cost where no cost was known. The December 2025 count was its first in NetSuite.

## 2. Locations

- **Where stock is held:**
  - France: the Stains depot and the Paris office.
  - UK: the London, London East and Manchester offices, since the old warehouse closed.
  - Singapore: the Singapore office.
  - Hong Kong: the Tsing Yi depot. The Wan Chai office location exists but holds no stock.
- **Drop Ship** locations exist in France, the UK and Singapore, for suppliers delivering straight to a client site. Hong Kong doesn't have one.
- An **"n/a"** location per country is a technical default for transactions where no location applies.
- **Dedicated client sites:** a few locations exist for client contracts, in France (Client sites A and B) and Singapore (Client site C).
- **Inactive old locations:** the old warehouses in each country, the Kowloon depot and office in Hong Kong, and a few legacy French stores (Courier store, Legacy partner store, Client site D).
- **Technical locations:** IOT Provider (France) and Lift Address (UK, inactive), used for connected-device kits.

## 3. Items

- Stock parts are **Inventory Items**. The catalogue holds about 20,000 inventory items, shared across all subsidiaries.
- **Service items** cover labour, subcontracting and shipping. By convention, the UK and France also buy parts for **modernisation and repair projects** on the service items "Modernization spare parts", "Repairs spare parts" and "Maintenance spare parts". These parts go straight to the project and are costed to it. They are not stocked.
- Creating a new inventory item takes a request to us (2–5 days).

## 4. Purchase to pay

- Purchase orders are approved through the standard PO approval workflow.
- Goods are received on an **Item Receipt** against the PO. Drop-ship deliveries are closed by the supplier delivery, without a receipt.
- Vendor bills are entered by AP from the supplier invoice. Bills are not linked line by line to receipts. A three-way-match SuiteApp was installed in 2025 but never configured.

## 5. Stock out

- Parts leave stock on an **Item Fulfilment** against a sales order raised for the client job.
- Technicians record parts used in the WeMaintain maintenance app. **There is no integration** between the app and NetSuite for parts consumption. The country office raises the sales order and fulfilment.
- Stock counts: annual count in December in each country. Hong Kong also counted in October 2026.

## 6. Inventory adjustments

- Posted by the country storekeeper or supply lead, to account 60322000 (stock variation).
- No reason codes, no approval workflow (see our separate remediation proposal).

## 7. Known issues

- Duplicate items exist in the catalogue, mainly where the same part was created in English and in French.
- Some POs were raised in the wrong currency on items with costs set in USD.
- Average part costs in Hong Kong are high because of imported drives and inverters (HKD 60k–90k each).
- Revenue recognition (ARM) for maintenance contracts: see the Finance documentation, not covered here.
