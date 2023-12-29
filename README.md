# MWAA Local Runner GitHub Action
----

This GitHub action creates an ephemeral MWAA environment that you can run custom commands against.

This action will:

- Create an ephemeral MWAA environment using `aws-mwaa-local-runner` and docker-compose
- Run a command from input within the ephemeral environment
- Tear down the MWAA environment after job execution

## Usage
To use this action, add a `.github/workflows/exec.yml` file to your repository. The workflow should contain:

```yml
name: Execute Airflow Command
on:
  workflow_dispatch:
    inputs:
      command:
        required: true
        type: string
        default: airflow db check

jobs:
  exec: 
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository code
        uses: actions/checkout@v3

      - name: Setup local runner environment
        uses: phaelin/mwaa-local-runner-action@main
        with:
          up: true
          repo-token: ${{ secrets.GITHUB_TOKEN }}

      - name: Executing custom command within local MWAA
        uses: phaelin/mwaa-local-runner-action@main
        with:
          exec: true
          exec-command: ${{ inputs.command }}

      - name: Tear down local runner environment
        uses: phaelin/mwaa-local-runner-action@main
        with:
          down: true

```

