# Tech Notes 📚

This repository contains my technical learnings, useful insights, and answers to tricky questions I come across.

## Structure
- `shell/` - Notes on shell scripting, commands, and Linux internals.
- `git/` - Git concepts, commands, and troubleshooting.
- `python/` - Python-related insights.
- `networking/` - Networking concepts and debugging.

## Recent Learnings

### 1️⃣ How does `#!/usr/bin/env python3` work?
**File:** [`shell/shebang.md`](shell/shebang.md)

**Question:**  
Why does `#!/usr/bin/env python3` work when `which python3` gives a different path?

**Answer:**  
The `#!/usr/bin/env python3` shebang instructs the system to use the `env` command to locate `python3` in the user's `PATH`, making the script more portable. Instead of hardcoding the path, it finds the first `python3` available in the system.

(Read more in [`shell/shebang.md`](shell/shebang.md))
