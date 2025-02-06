# Example workflow using this composite action

```yml
name: GitFlow Release Workflow

on:
  pull_request:
    branches:
      - master
    types: [closed]

permissions:
  contents: write
  pull-requests: write

jobs:
  release:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Ensures full history is available

      - name: Set up GitHub token
        run: echo "GITHUB_TOKEN=${{ secrets.GITHUB_TOKEN }}" >> $GITHUB_ENV

      - name: Extract branch information
        run: |
          echo "merged_branch_name=${{ github.event.pull_request.head.ref }}" >> $GITHUB_ENV
          echo "merged_branch_sha=${{ github.event.pull_request.head.sha }}" >> $GITHUB_ENV

      - name: Use the Gitflow release action
        uses: District09-Ent/github_action_auto-release@main
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          merged_branch_name: ${{ env.merged_branch_name }}
          merged_branch_sha: ${{ env.merged_branch_sha }}
          tag_branch: main  # or specify the branch where the tag should be created
          backmerge_branch: develop  # or specify the branch where the merged branch should be merged back to

```
