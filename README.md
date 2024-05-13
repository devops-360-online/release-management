# release-management

## Commit Message Structure

Each commit message should follow this structure:

```bash
<type>[optional scope]: <description>
[optional body]
[optional footer]
```


### Structural Elements
MAJOR.MINOR.PATCH

- `fix`: Fixes a bug (corresponds to PATCH in semantic versioning)
- `feat`: Adds a new feature (corresponds to MINOR)
- `BREAKING CHANGE`: A change that breaks API compatibility (corresponds to MAJOR)
- `docs`: For documentation changes
- `style`: For changes that do not affect the meaning of the code (formatting, etc.)
- `refactor`: For refactoring existing code
- `test`: For adding new tests or making changes to existing tests
- `chore`: For maintenance tasks or others that do not modify the code

## Examples of Commit Messages

### With Description and Footer for a Major Change

```bash
feat: allow configuration to extend other configs
BREAKING CHANGE: the `extends` key is now used to extend other configuration files
```

### With a `!` to Highlight a Major Change

```bash
feat!: send an email to the customer when the product is shipped
```

### Without a Message Body

```bash
docs: correct spelling in CHANGELOG
```

By following this model, we can keep track of changes made to the project in an organized manner, providing useful information about each modification made.

Remember to adapt this model to our own needs and the way we manage changes in our project! Using Conventional Commits helps maintain a structured and clear history, beneficial for both current team members and future collaborators. https://www.conventionalcommits.org/en/v1.0.0-beta.4/#summary

## Creating a Release in GitHub

### Adding a `.releaserc` file:

Create a `.releaserc` file with the following configuration to automate your release process using semantic-release:

```json
{
  "tagFormat": "${version}",
  "repositoryUrl": "https://github.com/devops-360-online/release-management.git",
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    "@semantic-release/github"
  ],
  "branches": ["main"]
}
```

### Creating a GitHub Workflow:

Set up a GitHub Actions workflow to promote Release Candidates to Releases. The workflow should be defined in your repository under `.github/workflows/release.yml`:

```yaml
name: Create a Release
on:
  workflow_dispatch:
jobs:
  release:
    name: Create new release
    runs-on: ubuntu-latest
    steps:
      - name: ⬇️ Checkout
        uses: actions/checkout@v3
      - name: Semantic Release
        uses: cycjimmy/semantic-release-action@v2
        with:
          semantic_version: 17.3.7
          extra_plugins: |
            @semantic-release/commit-analyzer@8.0.1
            @semantic-release/release-notes-generator@9.0.1
            @semantic-release/changelog@5.0.1
            @semantic-release/github@7.2.0
            @semantic-release/exec@5.0.0
            @semantic-release/git@9.0.0
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

This setup ensures your releases are automated, tracked, and managed efficiently within your GitHub repository.

