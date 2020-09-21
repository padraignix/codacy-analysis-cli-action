# GitHub Action for Codacy Analysis CLI

This is a GitHub Action for invoking the [Codacy Analysis CLI](https://github.com/codacy/codacy-analysis-cli) and returning identified issues in the code.

## GitHub Action

The following is an example GitHub Action workflow that uses the Codacy Analysis CLI
to analyze each commit and pull request.

```yaml
name: codacy-analysis-cli

on: ["push"]

jobs:
  codacy-analysis-cli:
    runs-on: ubuntu-latest
    name: Codacy Analysis CLI
    steps:
      - name: Checkout code
        uses: actions/checkout@master
      - name: Run codacy-analysis-cli
        uses: codacy/codacy-analysis-cli-action@master
```

Running the action with the default configurations will:

- Analyze the current commit or pull request by running all supported static code analysis tools, with their default configurations,
  for the languages you are using.
- Print analysis results on the console, visible on the GitHub Action's workflow panel.
- Fail the workflow if at least one issue is found in your code.

Read the next section to learn how you can further configure this action.

### Caveats

This action supports all [Codacy Analysis CLI configuration options](https://github.com/codacy/codacy-analysis-cli#commands-and-configuration), with the following exceptions:

- `--commit-uuid` -- **Not supported**. The action will always analyze the commit that triggered it.
- `--api-token`, `--username`, and `--project` -- **Not supported**. Use [`--project-token`](https://github.com/codacy/codacy-analysis-cli#project-token) instead.

The command `validate-configuration` is also **not supported**.

When using `--project-token` make sure that you use [GitHub security features](https://docs.github.com/en/actions/reference/encrypted-secrets)
to avoid committing the secret token to your repository. For example, if you store your Codacy project
token in GitHub, this is how you would use it in the action workflow:

```yaml
# ...
uses: codacy/codacy-analysis-cli-action@master
with:
    project-token: ${{ secrets.<PROJECT_TOKEN_NAME> }}
# ...
```

## Contributing

We love contributions, feedback, and bug reports.
If you run into issues while running this action,
[open an issue](https://github.com/codacy/codacy-analysis-cli-action/issues) in this repository.

## More information

For documentation on Codacy itself, including policy language and capabilities see the [Codacy Documentation](https://docs.codacy.com)
