# AGENTS.md

## Cursor Cloud specific instructions

### Product overview

WatermelonDB (`@nozbe/watermelondb`) is an offline-first SQLite/LokiJS database library for React and React Native. This repo is the library source plus `examples/web`, `examples/native`, and native integration test harnesses. There is no backend service to run for day-to-day development.

### Node.js version (important)

CI (`.travis.yml`) uses **Node 10**. The default VM Node (22+) can hang or crash Jest (`Model/test.js`). Before running tests or builds, put Node 10 first on `PATH`:

```bash
source ~/.nvm/nvm.sh
nvm install 10   # once per VM if missing
export PATH="$HOME/.nvm/versions/node/v10.24.1/bin:$PATH"
node -v   # should print v10.24.1
```

`/exec-daemon/node` may shadow `nvm`; always prepend the Node 10 bin directory as above.

### Standard commands (see `CONTRIBUTING.md`)

| Task | Command |
|------|---------|
| Install deps | `yarn` (repo root) |
| Lint + Flow + unit tests | `yarn ci:check` |
| Production build | `yarn build` → `dist/` |
| Dev compile (watch) | `yarn dev` → `dev/` |
| ESLint only | `yarn eslint` |
| Flow only | `yarn flow` |
| Jest only | `yarn test` |

### Web demo (`examples/web`)

The published npm `0.6.2` schema validator only accepts column type `bool`, while `examples/web/src/models/schema.js` uses `boolean`. Link the local dev build:

```bash
# Terminal 1 (repo root, Node 10 on PATH)
yarn dev

# Terminal 2
cd examples/web && yarn
rm -rf node_modules/@nozbe/watermelondb
ln -s /workspace/dev node_modules/@nozbe/watermelondb
yarn dev
```

Open **http://localhost:8888**. Use **Generate 100 records** to exercise batch creates; open a blog to verify relational queries.

### Optional (not required for library work)

- `examples/native`: Metro + iOS/Android toolchain (`yarn dev`, `yarn start:ios` / `start:android`)
- `yarn test:ios` / `yarn test:android`: Xcode simulator or Android emulator
- `yarn swiftlint` / `yarn ktlint`: native code lint

### Gotchas

- `yarn build` may log unhandled promise warnings from `rambdax` but still writes `dist/`.
- `yarn dev` is a long-running watcher; run it in **tmux** (e.g. session `watermelon-dev`).
- Webpack dev server for the web demo also belongs in tmux (port **8888**).
