# Claude-Certified-Engineer
A 4-week lesson plan based on the Claude Certified Architect study guide from https://github.com/paullarionov/claude-certified-architect

---
## Getting Started

1. Clone this repository
2. Make a copy of the file "Anthropic Training -- Practice Questions 1.docx" and call it `questions_local.docx`. This file will be ignored by git to prevent merge conflicts.
3. Create a new project in Cowork and give it access to the repo folder. Copy CLAUDE_TEMPLATE into your own CLAUDE.md and fill in your name/experience, so Claude can act as a tailored personal tutor.
4. Take a look at the [overview](overview.md) document.

---
## Study Tips
- When you sit down to study each day, make sure to run a `git pull` on the main branch to see if any changes have been made to the course content since you last checked. 
- Then create and switch to a new branch with `git switch -c {your name}-day-{number}-suggestions`.
- As you work through the content, write your answers to the quick checks, exercises, and exam questions in your `questions_local.docx` file.
- Whenever you find something unclear in the content, ask your personal tutor! When you understand the the concept, switch to a new git branch off of `main` and tell Claude "I understand". It will propose changes to the weekly document, and commit them once you approve. Feel free to do research online and/or make manual edits too!
- When you finish your work for the day,
	- if you have made any changes to the course content, push your `{your name}-day-{number}-suggestions` branch to GitHub and create a pull request.
	- switch back to your local `main` branch for next time
	- Check GitHub for pull requests from others. If someone has made suggestions about content you've already covered, review their changes. Approve the PR if everything looks good, or leave feedback if you would like to suggest an edit before merging to main. Once the changes have been accepted, the whole team can benefit from them!
- IMPORTANT: DO NOT PUSH YOUR EXERCISE ANSWERS TO GITHUB AND DO NOT ACCEPT PULL REQUESTS THAT INCLUDE ANSWERS TO THE EXERCISES. PLEASE KEEP YOUR ANSWERS IN THE IGNORED `questions_local.docx` FILE.
