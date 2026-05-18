# Step 1: Setup — VS Code + GitHub Copilot

> Get your development environment ready. ~15 minutes.

## 1.1 Install VS Code

If you don't already have it: [code.visualstudio.com](https://code.visualstudio.com/)

## 1.2 Install GitHub Copilot Extension

1. Open VS Code
2. Press `Ctrl+Shift+X` (Extensions panel)
3. Search **"GitHub Copilot"** → Install
4. Also install **"GitHub Copilot Chat"** if listed separately
5. Sign in with your GitHub account when prompted

**Verify:** Bottom-right status bar shows Copilot icon with a checkmark (✓).

## 1.3 Open Copilot Chat

| Method | How | When to Use |
|--------|-----|-------------|
| **Inline** | Start typing → Copilot suggests | Line-by-line code |
| **Chat panel** | `Ctrl+Shift+I` or click Copilot icon | Multi-file generation, questions |

**We use Chat mode for this course.** Open it now: `Ctrl+Shift+I`

## 1.4 Create Your Workspace Folder

```powershell
mkdir my-portfolio
cd my-portfolio
```

Or: File → Open Folder → create `my-portfolio` somewhere you'll remember.

**This is where everything lives.** All 3 sessions build inside this one folder.

## 1.5 What is Copilot Doing?

Copilot is an AI coding assistant that:
- **Suggests code** as you type (autocomplete on steroids)
- **Generates files** from natural language descriptions
- **Answers questions** about code in chat

**It is NOT magic.** Vague instructions → vague output. Precise constraints → precise output. Your job: learn to give precise constraints.

## Checklist

- [ ] VS Code open
- [ ] Copilot extension installed + signed in
- [ ] Copilot Chat panel accessible (`Ctrl+Shift+I`)
- [ ] `my-portfolio/` folder created and open
