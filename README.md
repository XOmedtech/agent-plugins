# XO Medtech agent plugins

Plugins that connect AI assistants to the XO Medtech platform. This repository is a plugin marketplace with one plugin, [`xo-medtech`](plugins/xo-medtech), which adds outreach skills on top of the XO MCP server at `https://mcp.xomedtech.com/mcp`.

## Install

```
/plugin marketplace add xomedtech/agent-plugins
/plugin install xo-medtech@xo-medtech
```

See the [plugin README](plugins/xo-medtech/README.md) for what the skills do and how sign-in works.

## Layout

```
.claude-plugin/marketplace.json     marketplace listing
plugins/xo-medtech/
  .claude-plugin/plugin.json        plugin manifest
  .mcp.json                         XO MCP server (OAuth, no keys)
  skills/<name>/SKILL.md            one folder per skill
```

## Writing skills

- Skills only. No slash commands, agents or hooks.
- Provider-neutral wording: write "the assistant", not a product name.
- No user configuration and no local credentials. Sign-in is OAuth against the MCP server.
- Each skill lists the tools it uses and states its guardrails: drafts stay drafts until the user approves, nothing is marked sent or replied unless the user says so, and anything that leaves XO needs a confirmation.
- Tool names are written bare (`search_leads`), because each host adds its own prefix.

## Release checklist

1. Update the skills and check each one end to end against a real XO account.
2. Bump `version` in every `plugins/xo-medtech/.*-plugin/plugin.json`. All manifests share one version.
3. Run `claude plugin validate . --strict` and `claude plugin validate plugins/xo-medtech --strict`.
4. Merge to `main`. CI runs the same validation.
5. Tag the commit `v<version>`.

## License

MIT. See [LICENSE](LICENSE).
