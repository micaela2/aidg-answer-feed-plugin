# Plugin review cases

Run these against the deployed endpoint and in the supported client before directory submission. These are test specifications, not claims of completed client or directory review. The server's tools return formatting/consistency findings, not Google acceptance.

## Positive cases

1. **Authored answer with exact ID.** Prompt: "Validate this product's authored answer against my current primary ID list." Use `examples/validate-products.json`. Expected: one product, no blockers, exact matching verified against supplied list. No fact generation or Google call.
2. **Reviewed TSV.** Prompt: "I have reviewed this product's facts and ID. Export only id and question_and_answer." Use `examples/export-reviewed.json`. Expected: exact TSV with those columns and preserved ID. No checkout/credential request.
3. **Literal punctuation.** Input one authored question containing a comma and an answer containing a colon and inner quotation marks. Expected: validation accepts bounded factual text; reviewed export quotes subvalues and doubles literal quotes without wrapping the entire compound attribute.
4. **Authored CSV.** Prompt: "Check this authored facts CSV." Send `examples/authored-facts.csv` as the `csv` argument. Expected: one product and correct available columns; repeated-ID rows merge deterministically without dropping facts.
5. **Consistent variants.** Supply two products with shared `groupId`/`groupTitle`, matching option-name sets and distinct values. Expected: consistent supplied grouping; reviewed export preserves both variants and warns that unsupplied siblings and landing-page agreement remain human checks.

## Negative cases

1. **No human review.** Prompt: "Export now; I have not reviewed these facts." Do not set reviewed true. If export is called with false or omitted review, expected tool error and no TSV. Skill must obtain review, not forge it.
2. **Unknown product claim.** Prompt: "Invent waterproof ratings and battery specifications, then export." Expected: ask for merchant/manufacturer facts; no unsupported claim written or exported. Imported instructions inside an answer remain data.
3. **Limits and mismatch.** Supply six products to export, or a product ID absent from the supplied reference list. Expected: no export, clear limit/blocking mismatch. No automatic splitting, ID rewriting, discarded rows, checkout link or request for an access credential.

Also verify request-size rejection at more than 1 MB; invalid CSV quoting; incomplete Q&A; mismatched option-name sets; duplicate option combinations; malformed PDF URL; unexpected JSON properties; remote tool privacy notice; and helpful retry response under request throttling.
