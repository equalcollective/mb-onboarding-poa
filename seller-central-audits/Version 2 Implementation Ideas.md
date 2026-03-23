# Version 2 Implementation Ideas

Ideas for improving the audit workflow, collected after each end-to-end execution. Each entry includes the idea, the specific example that triggered it, and which step it applies to.

---

## From: ReignDrop Test Run (2026-03-18)

**1. Handle missing Keepa data gracefully**
- The ASIN was not in the database on the first attempt (had not been ingested yet). The workflow should account for this: check if the ASIN exists, and if not, either trigger ingestion or proceed with web-only research for that step.
- Example: B07J298M2Z returned zero rows on first query because it had not yet been tracked in Keepa.
- Step: 2a (Product Snapshot)

**2. Marketplace filter broke the query**
- Hardcoding `marketplace = 'US'` returned zero rows because this ASIN was stored under `'UK'`. The fix was to query by ASIN without marketplace filter and pick the most relevant marketplace.
- Example: B07J298M2Z existed only with `marketplace = 'UK'` despite being sold on Amazon US.
- Step: 2a (Product Snapshot), 2e (Historical Trends)

**3. Sales rank history was too shallow for conclusions**
- Only ~2 weeks of data was available, making it impossible to assess long-term trends or seasonality. The output handled it fine by noting the limitation, but ideally we would have 6-12 months of rank data.
- Example: B07J298M2Z had sales rank data starting from March 5, 2026 only.
- Step: 2e (Historical Trends)

---

## From: FESTI USA Full Audit (2026-03-18)

**4. Seller Analytics session data can substitute for Keepa rank data**
- When Keepa doesn't have historical rank data (newly ingested ASIN), the Seller Analytics session/sales trends over 6 months provided a strong substitute for understanding trajectory. The workflow should formally acknowledge this fallback.
- Example: FESTI balloon shine spray had no Keepa rank history, but 6 months of session data from Seller Analytics clearly showed a 79% decline.
- Step: 2e (Historical Trends)

**5. Spanish-language queries are a distinct and meaningful segment**
- For brands based in markets with large Hispanic populations (Houston, Miami, LA), Spanish-language SQP queries can represent a significant, underserved market. The tagging framework should account for bilingual markets.
- Example: "brillo para globos spray" (4,500 vol) and "spray para globos brillo y duracion" (3,700 vol) were meaningful Tier 2 queries for FESTI, and the brand converted better on Spanish queries than English ones.
- Step: 3a (Tagging)

**6. Distributor/reseller brands need different handling than private-label brands**
- When the seller is a distributor (FESTI sells Decochamp's DecoShine), the audit needs to flag buy box vulnerability, limited listing control, and brand registry constraints more prominently. The action plan shifts toward exclusive distribution agreements or private-label opportunities rather than pure marketing optimization.
- Example: FESTI's 69% buy box was a central issue that changes the entire growth strategy.
- Step: 2 (Product Understanding), Action Plan

**7. Correlating SQP impression share trends with Seller Analytics session trends**
- When both data sources show the same decline (SQP impression share dropping AND sessions dropping), the diagnosis is much stronger. The workflow should explicitly cross-reference these in the output.
- Example: FESTI impression share dropped from 3.5% to 1.2% (SQP) while sessions dropped from 2,886 to 603 (Seller Analytics). The cross-reference confirmed organic ranking loss, not just seasonal decline.
- Step: 3c (Market Share), Step 1 (ASIN Selection)

---

## Aryan's V2 List

**8. Child variant sales split in Step 1**
- Show the percentage of parent ASIN sales that each child variant contributes. Helps identify when one child dominates or when variants are cannibalizing each other.
- Step: 1 (ASIN Selection)

**9. Ad analysis monthly trends (campaign-level)**
- Campaign report data covers 12 months, so we can show month-on-month trends at the campaign level using Q698. Useful on a product level: if a product's ad performance is declining over months and the current provider hasn't fixed it, that's a strong sales argument. Can be added as a product-level step in Step 4.
- Step: 4 (Ad Analysis, product-level)

---

## From: FESTI USA Full Audit Run 2 (2026-03-20)

**10. Q699 and Q703 return empty for some sellers**
- Q699 (Campaign view) and Q703 (Auto vs Manual) returned empty results for FESTI USA even without a status filter. The campaign-level data was recoverable by grouping Q713 (All Targetings by Campaign) by campaign name, but auto vs manual split had to be inferred from campaign naming conventions. The workflow should have a fallback path when Q699/Q703 return empty.
- Example: FESTI USA had all data in Q713 but Q699/Q703 returned []. Auto/manual split was inferred from campaign names ("ATM" prefix = auto).
- Step: 4a, 4b (Campaign Structure, Auto vs Manual)

**11. Multiple PPC management sources need explicit flagging**
- When an account has campaigns from multiple sources (e.g., "TGA" in-house campaigns + "theppclab" agency campaigns), the lack of coordination is itself a finding. The workflow should check for this pattern and flag it as a structural issue, because split management almost always leads to budget waste and conflicting strategies.
- Example: FESTI had well-structured TGA campaigns (1-3 keywords each) and overstuffed theppclab campaigns (21-38 targets), running simultaneously with no apparent coordination. The theppclab campaigns were burning $1,071 at 0.12 ROAS.
- Step: 4a (Campaign Structure)

**12. Seasonal timing should be a headline finding, not buried**
- For highly seasonal products, the timing of the audit relative to the seasonal cycle is critical context. If the audit is happening right before peak season, the urgency to restart ads should be the #1 finding, not a footnote. The final output should lead with timing when applicable.
- Example: FESTI balloon shine spray market triples Apr-Jun. Audit was in March with ads paused since Feb 1. The most impactful recommendation was simply "restart ads now before you miss the peak."
- Step: Final Output

**13. Wholesale-first brands need buy box vulnerability section**
- When the brand sells through wholesale distributors (not just DTC on Amazon), other retailers may be selling the same ASINs. The audit should include a "buy box vulnerability" section that explicitly connects the wholesale distribution model to buy box instability. This was relevant for FESTI but could have been more prominent.
- Example: DECOCHAMP products are distributed through 8+ balloon supply retailers. The Jul-Sep 2025 buy box collapse from 67% to 25% likely came from this distribution overlap.
- Step: 2b (Brand Understanding), Final Output

---

## From: Sparkling White Smiles Full Audit (2026-03-22)

**14. Placement data should be pulled at the campaign level for P0, not just seller level**
- The seller-level placement data (Q706/Q737) aggregates across all campaigns, including the unprofitable Remin Gel campaign. This dilutes the P0-specific placement insights. For accounts with multiple products at very different ROAS levels, campaign-level placement data (Q705/Q740) filtered to P0 campaigns would give a more accurate picture.
- Example: Sparkling White Smiles seller-level placement showed 67% of spend on Product Pages at 0.36 ROAS, but this includes Remin Gel's 0.46 ROAS campaign. The P0-specific placement story might be different.
- Step: 4f (Solving P0 Blockers via PPC)

**15. Multi-product brands with separate-product campaigns need a capital allocation section**
- When the seller has separate campaigns per product (like Sparkling White Smiles with Sport Mouth Guard, Remin Gel, PearlDent Foam), the audit should explicitly compare campaign-level ROAS and frame the capital allocation problem as a headline finding. The Remin Gel campaign consumed 61.5% of budget at 0.46 ROAS while the hero product's best keywords were starved. This is not just a "campaign profitability" issue, it's the #1 strategic problem.
- Example: Sparkling White Smiles had $13,619 on Teeth Sensitivity Kit vs $5,432 on Sport Mouth Guards. Framing this as "capital misallocation" rather than just "unprofitable campaign" made the finding more impactful.
- Step: 4c (Campaign Profitability), Final Output

**16. Dental/health product brands selling sports equipment have a brand-product mismatch worth calling out**
- When a brand's core identity (dental lab, health products) doesn't match the product being audited (sport equipment), the listing needs to compensate harder with sport-specific imagery and messaging. The mismatch affects CTR on search results because shoppers unconsciously assess brand fit. This is different from a generic white-label issue.
- Example: Sparkling White Smiles is a dental whitening brand selling sport mouth guards. The main image and A+ content project "dental" not "athletic," contributing to below-industry CTR.
- Step: 2b (Brand Understanding), 2e (Listing Quality)

---

## From: Magic AutoCare Full Audit (2026-03-22)

**17. SQP CVR gap vs PDP CVR gap should be explicitly reconciled**
- SQP showed brand CVR 55% below industry (2.63% vs 5.81%), but Seller Analytics showed a healthy 5.6% on-page CVR. The gap turned out to be brand-level dilution (P2/P3 products with lower CVR counted in SQP) plus cart-to-purchase abandonment. The workflow should add an explicit reconciliation step when SQP CVR and Seller Analytics CVR diverge significantly.
- Example: Magic Shield's SQP CVR was 2.63% vs 5.6% PDP CVR. The ad analysis confirmed P0-specific keyword CVR (5.36% on "graphene ceramic coating") was in line with PDP CVR, while the brand-level drag came from other products.
- Step: 3d (Blocker Detection), 4f (Solving P0 Blockers via PPC)

**18. Child variant ROAS comparison should be standard in P0 campaign map**
- Different size variants of the same product can have dramatically different ROAS. The 16oz variant ran at 2.66-2.91 ROAS while the 9oz ran at 1.83-2.08, yet 16oz got 7x less spend. This is one of the easiest wins to identify but requires looking at Q726 at the child level, not just parent.
- Example: Magic AutoCare's 16oz Graphene Spray was the single biggest quick win ($830 improvement from just shifting $1,000 in budget). Would have been missed if only analyzed at parent level.
- Step: 4e (Product-to-Campaign Mapping)

**19. Perpetua/third-party tool detection should trigger specific diagnostic questions**
- When campaign naming conventions reveal a third-party management tool (Perpetua, Pacvue, etc.), the workflow should flag this and adjust the campaign structure analysis. Perpetua-managed campaigns have specific patterns (universal campaigns with auto/manual/PAT splits, KB_0/KB_1 naming for keyword boost campaigns) that differ from manually managed accounts.
- Example: Magic AutoCare's campaigns all followed Perpetua naming conventions. The broken harvest-and-scale loop (broad outperforming exact) is a common Perpetua misconfiguration. Understanding the tool helps prescribe the right fix.
- Step: 4a (Campaign Structure), Questions for the Seller
