# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Purpose

This project is a prompt-driven N8N workflow builder. The user describes an automation or AI agent they want, and Claude builds it directly in their N8N instance. There is no application code to write or deploy — the deliverable is always a working N8N workflow.

## Available Tools

Two tools are available for building workflows:

1. **N8N Skills** — slash-command skills scoped to N8N workflow construction (invoked via the `Skill` tool).
2. **N8N MCP** — a Model Context Protocol server connected to the user's live N8N instance, enabling Claude to read, create, update, and activate workflows directly via MCP tool calls.

Always prefer using both together: use MCP tools to interact with the instance (list existing workflows, create nodes, wire connections) and skills for structured workflow generation patterns.

## How to Approach a Workflow Request

1. **Clarify intent first** if the request is ambiguous — understand the trigger, the data transformations, and the end action before building.
2. **Check existing workflows** via MCP before creating new ones; the user may want an extension of something already there.
3. **Build incrementally** — create the workflow, verify it appears in the instance, then activate or test it.
4. **Prefer native N8N nodes** over HTTP Request nodes when an official integration exists (e.g., use the Gmail node, not a raw HTTP call to Gmail).
5. **AI Agent workflows** should use the N8N AI Agent node with an appropriate chain (tool-calling, conversational, etc.) rather than hand-rolling LLM API calls via HTTP nodes.

## Workflow Design Principles

- Keep workflows focused: one trigger, one clear outcome. Chain workflows via the Execute Workflow node rather than building monoliths.
- Use expressions (`{{ $json.field }}`) and the N8N data pinning feature during development to avoid burning live API calls on every test run.
- When credentials are needed, note which credential type is required and ask the user to confirm it exists in their instance before wiring it up.
- For error handling, add an Error Trigger workflow rather than cluttering every workflow with catch branches.
