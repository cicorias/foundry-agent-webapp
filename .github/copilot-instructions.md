# AI Agent Web App - Copilot Instructions

**Purpose**: AI-powered web application with Entra ID authentication and Azure AI Foundry Agent Service integration.

## Architecture

**Single Container**: ASP.NET Core serves REST API (`/api/*`) + React SPA (same origin).

**Auth Flow**: Browser → MSAL.js (PKCE) → JWT → Backend → Azure AI Foundry (ManagedIdentity)

**Config**: Local uses `.env` files (gitignored); Production uses environment variables.

**Subagent Strategy**: Skills include delegation patterns for context-heavy operations (screenshots, multi-repo research, log analysis). See skill docs for specific patterns.

## Key Files

| File | Purpose |
|------|---------|
| `backend/WebApp.Api/Program.cs` | Middleware + JWT + API endpoints |
| `backend/WebApp.Api/Services/AzureAIAgentService.cs` | Azure AI Foundry Agent SDK integration |
| `frontend/src/config/authConfig.ts` | MSAL configuration |
| `frontend/src/services/chatService.ts` | SSE streaming + state dispatch |
| `frontend/src/reducers/appReducer.ts` | Central state management |
| `deployment/hooks/preprovision.ps1` | Entra app + AI Foundry discovery |
| `infra/main-app.bicep` | Container App configuration |

## Essential Rules

### ✅ Always Do
- Use `.RequireAuthorization("RequireChatScope")` on all API endpoints
- Accept and propagate `CancellationToken` in async methods
- Use `ErrorResponseFactory.CreateFromException()` for error responses
- Try `acquireTokenSilent()` first, fallback to `acquireTokenPopup()`
- Access `import.meta.env.*` at module level only

### ❌ Never Do
- Commit `.env*` files
- Use `.Result` or `.Wait()` on async methods
- Expose internal error details in production
- Reorder middleware pipeline
- Access `import.meta.env.*` inside functions

## Skills Available

For specialized guidance, these skills are available on-demand:

- **deploying-to-azure** - Deployment commands, phases, troubleshooting
- **researching-azure-ai-sdk** - SDK research patterns and resources
- **testing-with-playwright** - Browser testing workflow
- **writing-csharp-code** - C#/ASP.NET Core coding standards
- **writing-typescript-code** - TypeScript/React patterns
- **writing-bicep-templates** - Bicep infrastructure patterns
- **implementing-chat-streaming** - SSE streaming patterns
- **troubleshooting-authentication** - MSAL/JWT debugging
