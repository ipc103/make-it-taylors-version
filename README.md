# Make it Taylor's Version 🎤✨

A GitHub Action that automatically renames any Issue, Pull Request, or Discussion numbered **1989** by appending "(Taylor's Version)" to the title. Because we all know 1989 deserves a special treatment! 💜

## ✨ What Does This Do?

When you create Issue #1989, PR #1989, or Discussion #1989 in your repository, this action automatically renames it from:
```
Your Original Title
```
to:
```
Your Original Title (Taylor's Version)
```

## 📋 Prerequisites

- A GitHub repository where you have admin access
- That's it! No configuration needed.

## 🚀 Installation

### Option 1: Quick Setup (Recommended)

1. **Create the workflow directory** (if it doesn't exist):
   ```bash
   mkdir -p .github/workflows
   ```

2. **Create a new file** `.github/workflows/make-it-taylors-version.yml`

3. **Copy and paste** this complete workflow:

```yaml
name: Make it Taylor's Version

permissions:
  issues: write
  pull-requests: write
  discussions: write

on:
  issues:
    types:
     - opened
  pull_request:
    types:
     - opened
     - ready_for_review
  discussion:
    types:
     - created

jobs:
  add-label:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/github-script@v6
        with:
          github-token: ${{secrets.GITHUB_TOKEN}}
          script: |
            let type = '';
            let item = null;
            if (context.payload.issue) {
              item = context.payload.issue;
              type = 'issue';
            } else if (context.payload.pull_request) {
              item = context.payload.pull_request;
              type = 'pull_request';
            } else if (context.payload.discussion) {
              item = context.payload.discussion;
              type = 'discussion';
            }
            if (item.number !== 1989) {
                return;
            }

            if (item.title.indexOf("Taylor's Version") !== -1) {
              return;
            }
            
            // Update the issue, pull_request, or discussion title to be "#{title} (Taylor's Version)"
            if (type === 'issue'){
                github.rest.issues.update({
                    owner: context.repo.owner,
                    repo: context.repo.repo,
                    issue_number: item.number,
                    title: `${item.title} (Taylor's Version)`
                })
            } else if (type === 'pull_request'){
                github.rest.pulls.update({
                    owner: context.repo.owner,
                    repo: context.repo.repo,
                    pull_number: item.number,
                    title: `${item.title} (Taylor's Version)`
                })
            } else if (type === 'discussion'){
                console.log(item)
              // Update the discussion title via the GraphQL API
                const query = `mutation {
                    updateDiscussion(input: {discussionId: "${item.node_id}", title: "${item.title} (Taylor's Version)"}) {
                    discussion {
                        title
                    }
                    }
                }`;
                github.graphql(query);
            }
```

4. **Commit and push** to your repository:
   ```bash
   git add .github/workflows/make-it-taylors-version.yml
   git commit -m "Add Taylor's Version workflow"
   git push
   ```

### Option 2: Download from This Repository

Simply download the [workflow file](.github/workflows/make-it-taylors-version.yml) from this repository and place it in your `.github/workflows` directory.

## 💡 How to Use

1. **Install the workflow** using the instructions above
2. **Create Issue, PR, or Discussion #1989** in your repository
3. **Watch the magic happen!** The title will be automatically updated within seconds.

### Example

**Before:**
> Issue #1989: "Fix the bug in authentication"

**After:**
> Issue #1989: "Fix the bug in authentication (Taylor's Version)"

## 🎯 How It Works

The workflow:
- Triggers when an issue, pull request, or discussion is opened/created
- Checks if the number is exactly **1989**
- Verifies "(Taylor's Version)" isn't already in the title
- Automatically appends "(Taylor's Version)" to the title
- Uses GitHub's built-in `GITHUB_TOKEN` (no secrets to configure!)

## 🔧 Customization

Want to customize this action? Here are some ideas:

- **Change the number**: Modify the condition `if (item.number !== 1989)` to any number you prefer
- **Change the suffix**: Replace `(Taylor's Version)` with your own custom text
- **Add more triggers**: Add additional event types to the `on:` section

## ❓ FAQ

**Is this a joke?**

Yes. But it's a fully functional one! 😄

**But does it work?**

Absolutely! The workflow uses GitHub's official APIs and has been tested.

**What if I hate this?**

Haters gonna hate, hate, hate, hate, hate.
Baby, I'm just gonna shake, shake, shake, shake, shake.

**Do I need to configure any secrets?**

Nope! The workflow uses the built-in `GITHUB_TOKEN` that GitHub automatically provides to every workflow.

**Will this work on private repositories?**

Yes! The workflow works on both public and private repositories.

## 📝 License

MIT License - See [LICENSE](LICENSE) file for details

## 🤝 Contributing

Found a bug or have a suggestion? Feel free to open an issue or submit a pull request!

---

Made with 💜 for all the Swifties out there!
