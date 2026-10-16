# Converbelt 2.0

A [Claude Code](https://claude.com/claude-code) skill that runs a website or app idea down a
7-station "company assembly line". Claude plays each team role, writes that role's real deliverable
as a file, and checks it against a quality gate before the next station starts.

```
1 Planning -> 2 Design -> 3 Engineering -> 4 QA & Security -> 5 Content & SEO -> 6 Marketing -> 7 Launch & Support
```

## Status: early release

This is version 2.0 of a personal workflow. **It has not been run end to end yet.**

- Tested: nothing yet.
- Not tested: all 7 stations, the checkout animation, the sub-agent model routing.

Expect rough edges. Issues and fixes are welcome.

## Cost warning

The skill uses **Opus**, Claude's strongest and most expensive model, for all coding, security,
design and gate checks. It also starts many helper Claude jobs (sub-agents). A full run can use up
your usage limit fast. Start with `plan-only`.

## What is in it

- **7 stations** with real roles, deliverables and gates.
- **Research team:** Finder (web search), Reader (page reading), Brainstormer (turns research into the top 3 ideas).
- **Senior team:** optional specialist skills (architecture, frontend, backend, DevOps, QA, security) and a Senior Reviewer that checks the gates of stations 2, 3 and 4.
- **Design engine:** uses the optional `max-design` skill if you have it, plus a built-in "anti-slop" rule list (no purple gradients, no glass panels, no fake testimonials, and so on).
- **Model routing:** Haiku for easy jobs, Sonnet for normal jobs, Opus for coding, design and security.
- **Checkout payment animation spec** (press, processing, success, error) in `references/`.

## Install

1. Download this repo (**Code > Download ZIP**) and unzip it.
2. Copy the inner `converbelt` folder to your Claude Code skills folder:
   - Windows: `C:\Users\<your name>\.claude\skills\`
   - Mac/Linux: `~/.claude/skills/`
3. Check that `.claude/skills/converbelt/SKILL.md` exists.
4. Restart Claude Code.

## Use

```
/converbelt a booking website for a barber shop
/converbelt plan-only a one-page site for a local bakery
/converbelt design redesign my landing page
/converbelt brainstorm names for a pet-sitting app
/converbelt only marketing
/converbelt checkout-demo
```

The skill stops for your approval after the design station, and before it deploys or publishes anything.

## Optional companion skills

Converbelt works without them. If they are installed, it uses them: `max-design`, `senior-pm`,
`senior-architect`, `senior-fullstack`, `senior-frontend`, `senior-backend`, `senior-devops`,
`senior-qa`, `senior-security`, `senior-secops`. These are separate skills and are not included here.

## Credits

- The anti-slop rules and the checkout animation spec were supplied as prompts by the project owner. Their original author is not known. If you wrote them, open an issue and I will add credit or remove them.

## License

MIT. See [LICENSE](LICENSE).
