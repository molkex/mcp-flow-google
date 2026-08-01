# Claude Code, from zero to a picture

Three commands and one sign-in. Nothing to install.

## 1. Add the server

```bash
claude mcp add --transport http openflow https://openflowmcp.com/mcp
```

## 2. Let the agent get itself a key

In the session, just ask:

```
check openflow status
```

The agent calls `flow_status`. With no key it registers one on the spot and stores it. There is no
form to fill and nobody to wait for.

## 3. Connect your Google account

```
connect my Google account to openflow
```

The agent runs a small grabber on **your** machine. It opens Google's own login page in a real
browser; you sign in as usual and press Accept. The script prints a one-time token, the agent
passes it to the service, and that is the whole setup.

Your password is typed into Google's page, in your browser. It never passes through this service.

## 4. Work

```
draw a paper boat on a puddle, 16:9
```

```
make it evening, keep the composition
```

```
turn that into a 4-second video
```

The result comes back with a download link and a suggested filename — ask the agent to save it into
the project and it lands in `./media/`.

## Useful phrasings

| You say | The agent calls |
|---|---|
| "what can openflow do" | `list_models` — real, current model list with prices |
| "how many credits are left" | `check_credits` |
| "show my accounts and load" | `flow_dashboard` |
| "is my job done yet" | `check_job` — reports queue position and elapsed time, not just "waiting" |
| "make a character from this photo" | `create_character_from_photo`, then `generate_with_face` |

## When it refuses

Refusals from this service are written for the agent to act on, not to look at:

- **no Google account connected** → it will tell you exactly that, and offer to run the grabber.
- **rate limited** → says how many seconds to wait, and retries by itself.
- **out of credits** → says which account and how much is left.

If a refusal ever leaves you guessing, that is a bug: [t.me/mxmtkchk](https://t.me/mxmtkchk).
