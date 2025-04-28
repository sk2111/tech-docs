---
sidebar_position: 1
---

# Deleting All Workflow Runs

Delete all GitHub Actions workflow runs from a repository using PowerShell and the GitHub CLI.
While GitHub’s retention settings only clear the logs inside workflows, they don’t remove the workflow entries themselves.

To solve this, we can write a PowerShell script as highlighted in the below blog

Source Blog : [Delete all github workflow runs using powershell](https://den.dev/blog/delete-all-github-workflow-runs-powershell/)

Create a powershell script named `run.ps1`

```powershell
param (
    [string]$Owner,
    [string]$Repo
)

if (-not (Get-Command gh -ErrorAction SilentlyContinue)) {
    Write-Host "GitHub CLI (gh) is not installed. Please install it from https://cli.github.com/" -ForegroundColor Red
    exit 1
}

if (-not $Owner -or -not $Repo) {
    Write-Host "Usage: .\run.ps1 -Owner <GitHub-Owner> -Repo <GitHub-Repo>" -ForegroundColor Yellow
    exit 1
}

$workflowRuns = gh api repos/$Owner/$Repo/actions/runs --paginate --jq '.workflow_runs[].id'

foreach ($runId in $workflowRuns) {
    Write-Host "Deleting workflow run ID: $runId"
    gh api repos/$Owner/$Repo/actions/runs/$runId -X DELETE
}

Write-Host "All workflow runs have been deleted."
```

This script does a few things:

1. Accepts the owner and the repository as arguments.

2. Run it as: ```.\run.ps1 -Owner <GitHub-Owner> -Repo <GitHub-Repo>.```

3. Avoids hard-coding values so it can run across different repositories easily.

4. Checks if the GitHub CLI (gh) is installed by calling Get-Command on gh.

5. Validates that both the owner and repository name are provided.

6. Retrieves all workflow runs by calling the repos/$Owner/$Repo/actions/runs endpoint with --paginate to fetch all pages of results.

7. For each workflow run ID, sends a DELETE request to repos/$Owner/$Repo/actions/runs/$runId to remove the run.

Before running the script, make sure you’re authenticated with your GitHub account:

`gh auth login`

Once you logged in, call the script with a repo to which you have access and where you want to erase all workflow runs.

Example command:
`.\run.ps1 -Owner glibnub -Repo waittimes-aggregator`
