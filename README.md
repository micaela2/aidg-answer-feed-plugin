# AIDG Answer Feed

Prepare a reviewed Google Merchant Center supplemental TSV from merchant-authored facts. This repository contains integration configuration, a workflow skill, examples and review cases. The hosted service is operated by Micaela Brown LLC d/b/a AI Discovery Group.

## Install the repository plugin

Codex supports this repository's `.agents/plugins/marketplace.json`:

```text
codex plugin marketplace add micaela2/aidg-answer-feed-plugin
```

In a supported desktop client, refresh the plugin sources, select **AIDG Answer Feed**, and install **answer-feed**. Marketplace addition registers the source; it does not itself confirm installation. Restart or refresh as your client requires.

Claude Code:

```text
/plugin marketplace add micaela2/aidg-answer-feed-plugin
/plugin install answer-feed@aidg-answer-feed
```

For other clients with remote MCP support, add an HTTP/Streamable HTTP connection to:

```text
https://aidg-answer-feed.connect675501.chatgpt.site/api/mcp
```

The endpoint needs no authentication. The example `examples/direct-http.json` uses the Claude-style `type: http` configuration; use your client's supported remote transport declaration. The portable package uses `type: streamable-http` with no credential headers or authentication extension, which validates against the current portable MCP schema. Native public endpoint access is unauthenticated; select no authentication where a client asks. Do not paste either configuration into a client that only accepts local `command` configuration.

ChatGPT custom remote connections depend on the user's plan, developer-mode settings and workspace permissions. Where available, use the client's custom MCP connection settings with the endpoint above. A private native Sites-created Personal plugin is available only to its owner or permitted workspace; it does not publish this package in the universal directory. Public directory approval is separate from repository installation. The repository is public and the remote server is registered in the official MCP Registry; neither is an OpenAI or Anthropic public-directory approval.

## Use the tools

- `validate_answer_feed`: validates 1–1,000 products or an authored facts CSV, with optional exact primary-ID comparison. Returns blockers, review notes and available columns.
- `export_answer_feed`: returns a TSV for 1–5 products after human review. Supply `reviewed: true` and explicit export columns starting with `id`.

The complete JSON request is limited to 1 MB; reference-ID lists to 10,000 entries. Call `tools/list` for the current schemas. Supply exactly one of `products` or `csv`. The public connector does not perform purchases, receive access passes or submit data to Google.

Start with `examples/validate-products.json` as tool arguments, review the returned findings, and use `examples/export-reviewed.json` only after checking the authored facts. These example values demonstrate the input shape; use your own exact Merchant Center IDs and verified facts. A SKU or handle is not necessarily the ID produced by your primary-feed provider.

CSV headers:

```csv
id,question,answer,document_link,item_group_id,item_group_title,variant_name,variant_value
```

Repeat the same exact ID on multiple CSV rows to add pairs, document links or variant dimensions. Repeated IDs are merged by the CSV parser; conflicting group fields are rejected. In a `products` array use one object per distinct product, with `qa`, `documents`, `groupId`, `groupTitle` and `options` as shown in the example. The export header choices are `id`, `question_and_answer`, `document_link`, `item_group_id`, `item_group_title`, `variant_option`.

Keep input and original files. Fix blockers rather than dropping rows. Select only the attributes you intend to update; blank selected fields may affect combined Merchant data. The tool preserves IDs and quotes compound TSV subvalues. It does not write new facts, fetch documents, validate PDF contents/permissions/size, certify Google compliance or guarantee acceptance, rankings, citations or sales. Group checks cover the siblings you supply.

[Manual upload guide](https://aidg-answer-feed.connect675501.chatgpt.site/guide) · [Integration guide](https://aidg-answer-feed.connect675501.chatgpt.site/integrations)

## Privacy and support

Your AI client transmits tool arguments to AIDG's hosted service for processing. The application does not intentionally store catalog facts; AI-client conversations, provider logs and transient request-rate data are separate. Browser editing and free browser export take place locally; remote MCP processing does not. See [privacy](https://aidg-answer-feed.connect675501.chatgpt.site/privacy), [terms](https://aidg-answer-feed.connect675501.chatgpt.site/terms) and [support](https://aidg-answer-feed.connect675501.chatgpt.site/support). Do not send customer data, credentials or confidential material in product facts or support requests.

## Package status and rights

The public plugin repository is available at [micaela2/aidg-answer-feed-plugin](https://github.com/micaela2/aidg-answer-feed-plugin). The hosted server was published with HTTP domain ownership proof in the official MCP Registry as `site.chatgpt.connect675501.aidg-answer-feed/answer-feed`, version `1.1.0`, with active status. [Read the registry record](https://registry.modelcontextprotocol.io/v0.1/servers/site.chatgpt.connect675501.aidg-answer-feed%2Fanswer-feed/versions/1.1.0).

Registry registration is metadata publication, not certification or endorsement. Repository distribution and private Personal-plugin availability are distinct from OpenAI or Anthropic public-directory listing. No public-directory approval is claimed.

Copyright © 2026 AI Discovery Group. Installation and use of this connector configuration, workflow and supplied examples with AIDG Answer Feed are permitted. No additional software redistribution, modification or open-source license rights are granted by this notice. The hosted service remains proprietary and subject to its published terms. No paid application implementation, business evidence or credentials are included in this repository.
