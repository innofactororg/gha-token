# GitHub Token

This action can be used to impersonate a GitHub App when
`secrets.GITHUB_TOKEN`'s limitations are too restrictive and a personal
access token is not suitable.

## Usage

```yaml
jobs:
  job:
    runs-on: ubuntu-latest
    steps:
    - name: Generate token
      id: generate_token
      uses: innofactororg/gha-token@v1
      with:
        # GitHub App ID.
        #
        # Required
        app_id: ${{ secrets.APP_ID }}

        # Private key of the GitHub App (can be Base64 encoded).
        #
        # Required
        private_key: ${{ secrets.PRIVATE_KEY }}

        # The GitHub API URL.
        #
        # Default: ${{ github.api_url }}
        github_api_url: https://api.example.com

        # GitHub App installation ID for which the token will be requested.
        #
        # Default: The ID of the repository's installation
        installation_id: 1337

        # The JSON-stringified permissions granted to the token.
        #
        # Default: all the permissions given to the GitHub App
        permissions: |-
          {"members": "read"}

        # The full name of the repository for which the token will be requested.
        #
        # Default: ${{ github.repository }}
        repository: innofactororg/gha-token

        # The JSON-stringified repositories granted to the token.
        #
        # Default: all the repositories that the GitHub App has access to
        repositories: |-
          ["repo1","repo2"]

      - name: Use token
        env:
          TOKEN: ${{ steps.generate_token.outputs.token }}
        run: |
          echo "The generated token is masked: ${TOKEN}"
```
