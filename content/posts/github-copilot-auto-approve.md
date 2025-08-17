---
title: "Github Copilot Auto Approve"
date: 2025-08-12
---

## Introduction

While the Copilot team has taken [measures](https://github.com/microsoft/vscode/issues/252496#issuecomment-3127077875) to prohibit the agent from executing disastrous commands, auto approving commands will always hold an inherent risk.

## Auto Approve Everything

To auto approve all commands the agent intends to run, follow the steps below.

- Open the VS Code setting with `cmd` + `,`
- In the *Search settings* text field, type `chat auto approve`
  - Click the check box to enable the setting
- Again, in *Search settings*, type `chat max requests`
  - For me, this setting was set at a default of `25` which was not sufficient for more sophisticated tasks, so I increased this value to `100`

## Auto Approve Selected Commands

As described in [this GitHub issue](https://github.com/microsoft/vscode/issues/252496#issuecomment-3107360161), the Copilot agent now supports explicit enabling and disabling of commands:

```json
"chat.agent.terminal.autoApprove": {
  "foo": true,
  "bar": false
}
```
