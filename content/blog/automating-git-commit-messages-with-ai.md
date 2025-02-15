---
title: "Automating Git Commit Messages with AI"
date: 2025-02-15
categories: [AI, git, programming]
description: "Automate git commit messages with a script that uses AI to generate concise messages."
---

Do you just want the code? [Here it is](https://github.com/PandaSekh/scripts/blob/main/ai-commit.sh).

Git commit messages are important but can be time-consuming to write. They're fundamental in any project, providing a clear history of changes for your team. But let's be honest—commit messages are often neglected or oversimplified. They should:
- provide a short recap of what changed;
- explain WHY it changed;
- explain HOW it changed, if necessary;
- associate any relevant information (like the ticket with the requirements);

But fear not, here's how I used AI to help me. Here's a script that sends the diff of your staged files to a local Ollama instance (for privacy and expecially cost), which then generates a commit message.

## Script Analysis
[Full updated script here.](https://github.com/PandaSekh/scripts/blob/main/ai-commit.sh).
Here’s the breakdown of the core parts of the script:

1. Getting the Diff
```bash
#!/bin/bash

git add .
git_diff_output=$(git diff --cached --diff-algorithm=minimal -- . ":(exclude)package.json" ":(exclude)package-lock.json" ":(exclude)*.lock" ":(exclude)*.test.*" ":(exclude).changeset/*")

if git diff --cached --exit-code >/dev/null; then
  echo "⚠️ No staged changes detected. Aborting."
  exit 1
fi
```
This part grabs the diff of staged changes, excluding files you don’t want to check (like lock files and test files).

2. Preparing the AI Prompt
```bash
prompt=$(cat <<EOF
You are an AI assistant that generates high-quality git commit messages.
Generate a concise git commit message using the conventional commit format:
- First line: type: short description
  - Types: feat, fix, docs, style, refactor, perf, test, chore, etc.
  - do not put any scope in parenthesis
- Optionally, bullet points for details **Add them ONLY if necessary to add more details**:
  - List only **actual code changes** present in the diff.
  - Do NOT add assumptions, features, validation, or anything that is not explicitly in the diff.
  - Keep bullets very short and to the point.
  - If the change is minor, do not use bullet points.

Do NOT include explanations, greetings, or formatting beyond the commit message.

Examples:
feat: add user auth system

- Add JWT tokens for API auth
- Handle token refresh for long sessions

fix: resolve memory leak in worker pool

- Clean up idle connections
- Add timeout for stale workers

Simple change example:
fix: typo in README.md

Very important: Do not respond with any of the examples. Your message must be based on the diff that is about to be provided.

Here\'s the diff:

$git_diff_output
EOF
)
```
This is the part where we prepare the message to send to the AI, outlining the format and rules for the commit message.

3. Calling the AI API
```bash
json_escaped_prompt=$(jq -Rs '.' <<< "$prompt")

response=$(curl -s -X POST http://127.0.0.1:11434/api/generate \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen2.5-coder:7b",
    "prompt": '"$json_escaped_prompt"',
    "stream": false,
    "temperature": 0.1
  }')
```
We send the prompt to a local Ollama instance. The model `qwen2.5-coder:7b` is lightweight and works well on my Macbook Pro M3. For less performant devices, try the `3b` version.

4. Handling the Response
```bash
 Extract AI response
commit_message=$(echo "$response" | jq -r '.response' | sed 's/^ *//g')

# Validate AI response
if [ -z "$commit_message" ] || [[ "$commit_message" == "null" ]]; then
  echo "🚫 Failed to generate a commit message from AI."
  echo "⚠️ API Response: $response"
  exit 1
fi

# Keep only the first bullet points if there are more than 2
commit_message=$(echo "$commit_message" | awk 'NR==1 || (NR<=3 && /^-/)')

# 🛠 Remove redundant bullet points
first_line=$(echo "$commit_message" | head -n 1)
filtered_message=$(echo "$commit_message" | awk -v title="$first_line" 'NR==1 || (NR==3 && $0 != "- " title)')

# Extract branch name and potential JIRA ticket
current_branch=$(git rev-parse --abbrev-ref HEAD)
jira_ticket=$(echo "$current_branch" | grep -oE '[A-Z]+-[0-9]+' || echo "")

# Ensure JIRA ticket is at the end of the first line of the commit message
if [ -n "$jira_ticket" ]; then
  filtered_message=$(echo "$filtered_message" | sed -E "s/^([^:]+): (.+)$/\1: \2 ($jira_ticket)/")
fi
```
The AI-generated message is cleaned up. If the current branch name contains a JIRA ticket, it’s added to the commit message to track the related task.


```bash
# Copy commit message to clipboard
if command -v pbcopy &> /dev/null; then
  echo "$filtered_message" | pbcopy
  echo "✅ Commit message copied to clipboard (MacOS)."
elif command -v xclip &> /dev/null; then
  echo "$filtered_message" | xclip -selection clipboard
  echo "✅ Commit message copied to clipboard (Linux)."
elif command -v clip &> /dev/null; then
  echo "$filtered_message" | clip
  echo "✅ Commit message copied to clipboard (Windows)."
else
  echo "⚠️ Clipboard copy tool not found. Manually copy the commit message below:"
fi

# Output the commit message
echo "$filtered_message"
```
The commit message is copied and written to terminal. We already automated the creation, why wasting time selecting and copying the message?

## Known Issues
- It struggles with new or deleted files.
- The AI can occasionally hallucinate details not present in the diff. That's AI for you.

[Here is the updated script.](https://github.com/PandaSekh/scripts/blob/main/ai-commit.sh).

{{< comments >}}
