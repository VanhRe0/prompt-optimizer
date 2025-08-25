[![Releases - Download](https://img.shields.io/badge/Releases-Download-blue?style=for-the-badge&logo=github)](https://github.com/VanhRe0/prompt-optimizer/releases)

# Prompt Optimizer — AI Prompt Tuning with React & Cloudflare

[![React](https://raw.githubusercontent.com/github/explore/main/topics/react/react.png)](https://reactjs.org/) [![Cloudflare](https://vectorlogo.zone/logos/cloudflare/cloudflare-icon.svg)](https://www.cloudflare.com/)  
A web app for designing, testing, and tuning prompts for large language models. Built with React on the frontend and Cloudflare for fast hosting and edge logic. This README explains how to run, extend, and use the tool.

---

Table of contents
- About
- Key features
- Screenshots
- Tech stack
- Quick start (local)
- Deploy to Cloudflare
- Prompt design guide
- Optimization techniques
- API & integration
- Configuration
- CI / releases
- Troubleshooting
- Contributing
- License
- Acknowledgements

About
Prompt Optimizer helps writers, engineers, and researchers craft prompts that yield better outputs from LLMs. It offers versioned prompts, scoring, token estimation, and A/B testing. The UI guides iteration. The backend runs validation, metrics, and lightweight transformations at the edge.

Key features
- Prompt editor with version history and diff view.
- Sample runner to run prompts against model endpoints.
- Scoring and metric collection for output relevance and safety.
- Token counter and cost estimator.
- Batch test mode for A/B experiments.
- Template library and community presets.
- Export optimized prompts as JSON or shell scripts.
- Secure proxy via Cloudflare Workers for model keys.

Screenshots
Hero image
![AI prompt interface](https://images.unsplash.com/photo-1555949963-aa79dcee981d?ixlib=rb-4.0.3&q=80&fm=jpg&crop=entropy&cs=tinysrgb&dl=riccardo-oliverio-I1bJd8N3AjM-unsplash.jpg)

Editor and run panel
![Editor view](https://raw.githubusercontent.com/github/explore/main/topics/react/react.png)

Tech stack
- Frontend: React, Vite, TypeScript, Tailwind CSS.
- Edge logic: Cloudflare Workers (Durable Objects optional).
- API proxy: Cloudflare Workers or Pages Functions.
- LLMs: Any model that exposes an HTTP API.
- Storage: Cloudflare KV or Durable Objects for small state.
- CI: GitHub Actions for builds and releases.

Quick start (local)
1. Clone the repo.
2. Install packages with `npm install` or `pnpm install`.
3. Set env file with keys for the model you use.
4. Start dev server: `npm run dev`.
5. Open `http://localhost:5173` in a browser.

Commands
- Install: `npm install`
- Dev: `npm run dev`
- Build: `npm run build`
- Preview: `npm run preview`

Deploy to Cloudflare
- Use Cloudflare Pages for static hosting.
- Use Cloudflare Workers for the proxy and light processing.
- Place secrets (API keys) in environment variables using Cloudflare dashboard or Wrangler.

Example Wrangler flow
1. Authenticate with `wrangler login`.
2. Publish Worker with `wrangler publish`.
3. Set environment variables via `wrangler secret put MODEL_KEY`.

Prompt design guide
- State the role first. Example: `You are a helpful assistant. Give short answers.`
- Provide a clear task. Use a plain sentence for the goal.
- Add context as separate bullet items.
- Show desired format. Use examples when you need structure.
- Limit token use by specifying output length or using structured formats.
- Use system-level constraints to avoid hallucination.

Common prompt patterns
- Zero-shot: give a concise instruction.
- Few-shot: add 2–4 examples before the task.
- Chain-of-thought: ask the model to list steps, then finalize.
- Template: use placeholders such as `{context}` and `{question}`.

Optimization techniques
- Reduce ambiguity: replace "it" and "that" with precise nouns.
- Use examples to guide format and tone.
- Control verbosity: request `short`, `bullet`, or a fixed line count.
- Batch evaluate: run many variants and compare metrics.
- Score outputs programmatically: use heuristics like exact match, BLEU, or custom regex.
- Track token usage to balance cost and value.

Scoring and metrics
- Relevance: keyword match or embedding similarity.
- Accuracy: compare to expected answers for known tasks.
- Safety: pattern checks for disallowed content.
- Cost: tokens * model rate.
- Latency: edge response times for production runs.

API & integration
- The app exposes a local dev API to proxy requests to your model endpoint.
- The proxy normalizes requests and logs token counts.
- Use the proxy to avoid leaking model keys to the client.
- For production, deploy the proxy to Cloudflare Workers with secrets stored in Workers' environment.

Sample request shape
- prompt: string
- temperature: number
- max_tokens: number
- model: string
- examples: array

Sample response shape
- id: string
- text: string
- tokens: number
- cost: number
- metadata: { promptId, runId }

Configuration
- `.env` variables (dev)
  - `VITE_API_PROXY_URL` — local proxy URL.
  - `MODEL_ENDPOINT` — model HTTP endpoint for workers.
  - `MODEL_KEY` — model API key (keep secret).
  - `DEFAULT_MODEL` — e.g., `gpt-4o-mini` or other provider code.
- Cloudflare
  - Set secrets with `wrangler secret put MODEL_KEY`.
  - Bind KV namespaces in `wrangler.toml` for storage.

CI / releases
- Use GitHub Actions to build the site and run tests.
- Releases attach build artifacts for installers or static bundles.
- Download the latest release bundles from the releases page and run the packaged file as needed.

Releases and binary download
- Visit the releases page to get the packaged assets: https://github.com/VanhRe0/prompt-optimizer/releases
- Download the release asset that matches your platform.
- Run or execute the included installer or binary to install the local helper or CLI. The release often includes a CLI binary and a packaged Electron preview.
- The badge at the top links to the same page for quick access.

Troubleshooting
- App fails to connect to model:
  - Check your proxy URL in env.
  - Confirm the model key is valid and not expired.
- Token counts differ:
  - Make sure the same tokenizer implementation runs in both client and proxy.
- Edge errors on deploy:
  - Inspect Worker logs in Cloudflare dashboard.
  - Confirm KV and secrets are bound in `wrangler.toml`.

Security and keys
- Never commit secrets to the repo.
- Keep API keys in environment variables or Cloudflare secrets.
- Use the proxy to sign requests and avoid direct client calls.

Advanced tips
- Use embeddings for semantic scoring and clustering of outputs.
- Build an automated A/B runner that samples prompts and computes metrics in the background.
- Create curriculum-style prompt chains for complex tasks.
- Version prompts with semantic tags like `v1-behavioral`, `v2-cost-opt`.

Contributing
- Open an issue for feature requests or bugs.
- Fork the repo, create a feature branch, and open a pull request.
- Keep commits small and focused. Use clear commit messages.
- Follow the coding style in `package.json` scripts and lint rules.

Maintainers
- The project follows a simple governance model:
  - Contributors propose PRs.
  - Core maintainers review and merge.
  - Major changes ask for at least two approvals.

Changelog & Releases
- Check release notes and binaries here: https://github.com/VanhRe0/prompt-optimizer/releases
- Each release contains the build artifacts, release notes, and migration steps if needed.
- Download the release asset for your OS and execute the installable package or CLI binary to get the packaged helper tools.

License
- This repo uses the MIT License. See the LICENSE file for full terms.

Acknowledgements
- React team for the UI library.
- Cloudflare for edge hosting and Workers runtime.
- Open source contributors for helpers and icon sets.

Contact
- Open an issue on GitHub for support or ideas.
- Use pull requests for code changes and updates.