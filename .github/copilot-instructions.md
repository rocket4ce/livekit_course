# LiveKit Course Phoenix Application - AI Coding Guidelines

This Phoenix application integrates with LiveKit for real-time video/audio communication. The project uses Phoenix v1.8, LiveView, and the `livekitex` Elixir SDK.

## Project Architecture

### Core Stack
- **Phoenix v1.8** with LiveView for real-time UI
- **livekitex v0.1.33** - Custom Elixir SDK for LiveKit integration
- **PostgreSQL** with Ecto for data persistence
- **TailwindCSS + DaisyUI** for styling
- **Bandit** as HTTP adapter (instead of Cowboy)

### LiveKit Integration
The app uses the `livekitex` library (custom Elixir SDK) instead of gRPC clients. Key integration points:

```elixir
# Room service creation
room_service = Livekitex.RoomService.create(api_key, api_secret, host: host)

# Access token generation
token = Livekitex.AccessToken.create(api_key, api_secret, identity: user_id)
|> Livekitex.AccessToken.set_video_grant(%Livekitex.Grants.VideoGrant{
  room_join: true, room: room_name, can_publish: true
})
|> Livekitex.AccessToken.to_jwt()
```

**Configuration Pattern**: LiveKit credentials should be configured via environment variables:
- `LIVEKIT_API_KEY`
- `LIVEKIT_API_SECRET`
- `LIVEKIT_HOST`

## Development Workflows

### Essential Commands
- `mix setup` - Full project setup (deps, database, assets)
- `mix precommit` - Pre-commit validation (compile with warnings as errors, format, test)
- `mix phx.server` - Start development server
- `mix test` - Run tests

### Asset Management
- **Tailwind**: Auto-rebuilds in development via watchers in `config/dev.exs`
- **esbuild**: Bundles JS with ES2022 target
- **Asset commands**: `mix assets.setup`, `mix assets.build`, `mix assets.deploy`

## Code Conventions

### Phoenix v1.8 Specific Patterns
Always start LiveView templates with the layout wrapper:
```heex
<Layouts.app flash={@flash} current_scope={@current_scope}>
  <!-- content -->
</Layouts.app>
```

### LiveView Patterns
- Use `stream/3` for collections to avoid memory issues
- Forms must use `to_form/2` and `<.form for={@form}>` pattern
- Never access changesets directly in templates - use form assigns
- Import `<.icon>` component for hero icons, avoid Heroicons modules

### HTTP Client Convention
**Always use `:req` library** for HTTP requests (already included). Avoid `:httpoison`, `:tesla`, `:httpc`.

### Elixir Guidelines
- Lists don't support index access (`list[i]` is invalid) - use `Enum.at/2`
- Variables are immutable but rebindable - always assign block expression results
- Use `String.to_atom/1` carefully (never on user input)
- Predicate functions end with `?`, avoid `is_` prefix

## Testing Patterns

### LiveView Testing
- Use `Phoenix.LiveViewTest` and `LazyHTML` for assertions
- Reference DOM IDs in tests (always add `id` attributes to key elements)
- Test against element presence with `has_element?/2`, not raw HTML
- For debugging: use `LazyHTML.filter/2` to inspect specific elements

### Form Testing
- Drive tests with `render_submit/2` and `render_change/2`
- Test form element IDs like `#product-form` rather than content

## File Organization

### Key Directories
- `lib/livekit_course/` - Domain contexts and business logic
- `lib/livekit_course_web/` - Web interface (controllers, LiveViews, components)
- `assets/` - Frontend assets (JS, CSS)
- `config/` - Application configuration
- `priv/repo/` - Database migrations and seeds

### Component Structure
- `lib/livekit_course_web/components/core_components.ex` - Shared UI components
- `lib/livekit_course_web/components/layouts.ex` - Layout components
- Import components via `LivekitCourseWeb.CoreComponents` (already aliased)

## Production Considerations

### Deployment
- Uses Coolify for deployment (see `config/runtime.exs`)
- Database configured via `DATABASE_URL` environment variable
- Server starts with `PHX_SERVER=true bin/livekit_course start`

### Environment Configuration
Production requires:
- `SECRET_KEY_BASE`
- `DATABASE_URL`
- `LIVEKIT_API_KEY`, `LIVEKIT_API_SECRET`, `LIVEKIT_HOST`

## Dependencies Notes

### Key Dependencies
- `livekitex` - Custom LiveKit Elixir SDK (room management, tokens)
- `live_debugger` - Development debugging tool
- `req` - HTTP client (preferred over alternatives)
- `credo` - Code quality analysis
- `bandit` - HTTP server adapter

When adding features, check if the `livekitex` SDK already provides the needed functionality before implementing custom solutions.
