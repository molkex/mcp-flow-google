# MCP Flow Google — image and video generation for your AI agent

**[openflowmcp.com](https://openflowmcp.com)** · an [MCP](https://modelcontextprotocol.io) server
that lets Claude Code, Cursor, Cline and any other MCP client generate **images and video through
Google Flow** — using *your own* Google account and its credits.

No API key to buy from us. No SDK to install. One line of config, and your agent can draw.

```
Agent: generate a paper boat on a puddle, 16:9
  → image in ~18 seconds, saved into your project
```

---

## Why this exists

Google Flow makes excellent images and video, and has no public API. This service is the missing
piece: it speaks MCP to your agent on one side, and drives Flow on the other. Your generations run
on **your own** Google account, so nobody meters you but Google.

|  | |
|---|---|
| **Images** | free — Google's image models cost no credits. A fresh key gets 25 before you link a GitHub account (one click, also free), after which there is no limit of ours |
| **Video** | costs Google credits from your own account, plus a $2 subscription here |
| **Setup** | connect one Google account, one time, from your own machine |
| **Speed** | ~18 s per image under load, ~41 s per 4-second video (measured, not promised) |

---

## Quick start

### 1. Add the server to your MCP client

**Claude Code**

```bash
claude mcp add --transport http openflow https://openflowmcp.com/mcp
```

**Cursor / Cline / anything else** — in your MCP config:

```json
{
  "mcpServers": {
    "openflow": {
      "type": "http",
      "url": "https://openflowmcp.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_KEY" }
    }
  }
}
```

### 2. Get a key

Ask your agent to call `flow_status`. If it has no key yet, it registers one itself — the service
issues keys to agents, no forms, no waiting. You can also
[open the site](https://openflowmcp.com) and get one there.

### 3. Connect your Google account

Generations run on your Google account's credits, so the service needs a token from it. **Your
password never leaves your machine**: a small local script opens Google's own login page in your
browser, you sign in, and it prints a one-time token that the agent passes on.

Ask your agent: *"connect my Google account to openflow"* — it knows the steps
(`add_account_token`). Or follow the same flow in a browser at
[openflowmcp.com](https://openflowmcp.com).

### 4. Generate

```
"draw a red fox in the snow, vertical"
"make a 4-second video of that fox running"
"upscale it"
```

---

## What your agent gets

**29 tools**, described so the agent picks the right one without being told twice.

| Group | Tools |
|---|---|
| **Images** | `generate_image`, `upscale_image`, `generate_scene` (build a location from a 3D layout map, so several shots share one geometry) |
| **Video** | `generate_video`, `generate_video_from_image`, `generate_video_between_frames`, `generate_video_with_reference`, `extend_video`, `edit_video`, `upscale_video` |
| **Characters** | `create_character_from_photo`, `create_character_from_description`, `generate_with_face`, `generate_character_turnaround`, `generate_character_expression_sheet`, `list_characters`, `update_character` |
| **Voice** | `generate_voice` — Gemini TTS with named voices, performance styles and tempo control |
| **Status** | `flow_status`, `flow_health`, `flow_dashboard`, `check_credits`, `check_job`, `list_models`, `fetch_media` |

Plus five ready-made **scenarios** (MCP prompts): UGC video with a character, character turnaround,
expression sheet, generate-then-refine, one location from several cameras.

### Models

Images: **Nano Banana 2** (default, ~30 s), **Nano Banana Pro** (best, separate daily quota),
**Nano Banana 2 Lite**.
Video: 4/6/8/10-second clips, image-to-video, reference-to-video, interpolation between two frames,
extension of an existing clip. `list_models` always returns the current, verified list with prices
in credits — the service never guesses a model id.

---

## What it costs

- **Images — free.** Your Google account generates them at no credit cost. A brand-new key can
  make 25 of them before it asks you to link a GitHub account; linking is one click and free, and
  removes the limit for good. The point of the limit is anonymous floods, not you.
- **Video — your Google credits** (7 credits for 4 seconds, 10 for 6, 12 for 8) **plus a $2
  subscription** on this service, which is what keeps it running.
- **The service itself takes nothing else.** No per-image fee, no markup on credits.

---

## Honest limits

Written down because finding them yourself costs time:

- **One Google account is one pool of credits.** Video is where they go. Images do not spend them.
- **Concurrency is capped per account** (8 simultaneous generations), measured against Google's own
  ceiling. Exceeding it returns a wait, not an error — jobs go back into the queue and retry.
- **Voice runs on Gemini TTS**, which is metered by a separate free key you create at
  [aistudio.google.com/apikey](https://aistudio.google.com/apikey). The free tier allows about
  3 clips per minute. `add_voice_key` attaches yours.
- **Generated files stay on the server until it runs low on disk**, and even then only files you
  already downloaded are removed. Anything you never fetched is never deleted. Every result comes
  back with a direct download link — save it into your project and it is yours for good.
- **Nano Banana Pro has its own daily quota** per Google account, separate from credits.

---

## Privacy

- Your Google password never reaches this service — the login happens in your browser, on your
  machine, and only a token is passed on.
- Tokens are stored encrypted (AES-256-GCM), with the encryption key kept apart from the database.
- Your generations are visible only to your key. The public gallery on the site shows only work
  whose owner left it public.
- Full details: [openflowmcp.com/trust](https://openflowmcp.com/trust)

---

## Links

| | |
|---|---|
| Service | [openflowmcp.com](https://openflowmcp.com) |
| MCP endpoint | `https://openflowmcp.com/mcp` |
| Your cabinet | [openflowmcp.com/account](https://openflowmcp.com/account) |
| What is stored | [openflowmcp.com/trust](https://openflowmcp.com/trust) |
| Terms | [openflowmcp.com/terms](https://openflowmcp.com/terms) |
| Support | [t.me/mxmtkchk](https://t.me/mxmtkchk) — the only support channel, and it works |

---

## Questions people actually ask

**Do I need a Google Flow subscription?**
No. A normal Google account is enough for images. Video spends that account's credits, which come
with Google AI plans.

**Is my account at risk?**
The service uses the same tokens Google's own apps use, and nothing else. Rate limits are respected
deliberately — the concurrency cap exists precisely so your account is never hammered.

**Can I run several accounts?**
Yes. Attach as many as you like; work is spread across them, and the more you attach, the higher
your own rate limits go.

**What if something breaks?**
Every refusal from this service says what happened, what to do next, and whether waiting helps.
If one ever doesn't, that is a bug worth reporting to [t.me/mxmtkchk](https://t.me/mxmtkchk).

---

*This repository is documentation. The service itself is closed-source.*
