---
id: mcp-integration
name: MCP Integration
description: Integrate Model Context Protocol (MCP) tools and resources into agent workflows with schema-aware calls, safety checks, and fallback handling. Use when adding external tool capabilities to coding assistants.
category: DevOps
requires: []
examples:
  - Add a new MCP server and document how the agent should call its tools safely.
  - Build a workflow that reads tool schemas before every MCP invocation.
---

# MCP Integration

Implement MCP integrations that are reliable, discoverable, and safe to operate.

## When to Use

- You are enabling external APIs/tools through MCP servers.
- Agents must call tools consistently with schema validation.
- You need clear error handling and fallback behavior for tool failures.

## Workflow

1. Discover available MCP servers, tools, and resources in the project context.
2. Read each tool descriptor before use and confirm required parameters.
3. Define a call pattern: validate input, invoke tool, parse response, handle errors.
4. Add guardrails for auth scope, side effects, and rate limits.
5. Document test cases for success paths and failure/retry scenarios.

## Concrete MCP Patterns

**Tool discovery via descriptor files.** MCP tools are described in JSON descriptor files (e.g., under `mcps/<server>/tools/`). List and read these descriptors before invoking any tool. Each descriptor defines parameters, types, and required fields. Use this to build a runtime contract; never assume parameter names or types from documentation alone.

**Schema validation before invocation.** Before calling a tool, validate that all required parameters are present and match the expected types. Reject malformed requests early. Log validation failures for debugging. This prevents cryptic runtime errors and reduces wasted calls.

**Error handling for tool timeouts/failures.** Tools can timeout, return errors, or be unavailable. Define a retry policy: max attempts, backoff, and which error codes are retryable. For non-retryable errors, return a clear message to the caller and optionally trigger a fallback path. Never silently swallow tool failures in multi-step workflows.

**Resource fetching patterns.** Resources are read-only data from MCP servers. Use `list_mcp_resources` to discover available resources, then `fetch_mcp_resource` with server name and URI. Cache resource content when appropriate to avoid repeated fetches. Handle fetch failures gracefully (e.g., stale cache, fallback content).

## Common Pitfalls

**Invoking tools without reading schema.** Calling a tool with guessed parameters leads to runtime errors and inconsistent behavior. Always read the tool descriptor first. Use the schema to construct the correct argument structure.

**Assuming tool availability without checking server status.** MCP servers can be down, misconfigured, or rate-limited. Before critical workflows, verify server connectivity or have a fallback. Do not assume tools are always available.

**Ignoring side effects of write tools.** Tools that create, update, or delete data have side effects. Design workflows to require explicit confirmation for such tools. Log all write operations. Consider dry-run or preview modes where possible.

**Not handling partial failures in multi-tool workflows.** When a workflow calls multiple tools, one failure can leave the system in an inconsistent state. Design for partial completion: identify which steps are idempotent, which require compensation, and what the user sees when a step fails mid-workflow.

## Output Format

```markdown
## MCP Integration Plan
- Server: <name>
- Tools in scope: <list>
- Required auth/secrets: <list>
- Resources (if any): <list>

## Invocation Contract
1. Read schema/descriptor from <path or source>
2. Validate required fields and types per schema
3. Invoke tool with minimal required args
4. Parse response and map errors to user-facing messages
5. Retry policy: <attempts, backoff, retryable errors>
6. Timeout: <seconds>

## Error Handling
- Timeout: <action>
- Tool unavailable: <action>
- Validation failure: <action>
- Partial failure in multi-tool flow: <action>

## Validation Checklist
- [ ] Schema read before invocation
- [ ] Required parameters validated
- [ ] Error and retry strategy documented
- [ ] Side-effecting tools explicitly gated
- [ ] Server status considered before critical workflows
```

## Constraints

- Never invoke MCP tools without checking schema first.
- Keep least-privilege permissions for sensitive integrations.
- For side-effecting operations, require explicit confirmation in workflow design.