# Update [Coder](https://github.com/coder/coder) Template

Update coder templates automatically

## Usage

1. Create a GitHub secret named `CODER_SESSION_TOKEN` with your coder session token
2. Create a `.github/workflows/push-coder-template.yaml` file and use one of the examples below.

## Inputs

| Name                      | Description                                                              | Default                       |
| ------------------------- | ------------------------------------------------------------------------ | ----------------------------- |
| `CODER_URL`               | **Required** The url of coder deployment (e.g. <https://dev.coder.com>). | -                             |
| `CODER_SESSION_TOKEN`     | **Required** The session token of coder.                                 | `secrets.CODER_SESSION_TOKEN` |
| `CODER_TEMPLATE_NAME`     | **Required** The name of template.                                       | -                             |
| `CODER_TEMPLATE_DIR`      | The directory of the template.                                           | `CODER_TEMPLATE_NAME`         |
| `CODER_TEMPLATE_VERSION`  | The version name of the template.                                        | Autogenerated name by Coder   |
| `CODER_TEMPLATE_ACTIVATE` | Activate the new template version.                                       | `true`                        |

## Examples

1. Update a Coder template with the latest commit hash as version name and set it as active.

   ```yaml
   name: Update Coder Template

   on:
     push:
       branches:
         - main

   jobs:
       update:
           runs-on: ubuntu-latest
           steps:
           - name: Checkout
             uses: actions/checkout@v3
           - name: Get latest commit hash
             id: latest_commit
             run: echo "::set-output name=hash::$(git rev-parse --short HEAD)"

           - name: Update Coder Template
               uses: matifali/update-coder-template@v2
               with:
                   CODER_TEMPLATE_NAME: "my-template"
                   CODER_TEMPLATE_DIR: "my-template"
                   CODER_URL: "https://coder.example.com"
                   CODER_TEMPLATE_VERSION: "${{ steps.latest_commit.outputs.hash }}"
                   CODER_SESSION_TOKEN: ${{ secrets.CODER_SESSION_TOKEN }}
   ```

2. Update a Coder template with a random version name without activating.

   ```yaml
   name: Update Coder Template

   on:
     push:
       branches:
         - main

   jobs:
       update:
           runs-on: ubuntu-latest
           steps:
           - name: Checkout
             uses: actions/checkout@v3

           - name: Update Coder Template
               uses: matifali/update-coder-template@v2
               with:
                   CODER_TEMPLATE_NAME: "my-template"
                   CODER_TEMPLATE_DIR: "my-template"
                   CODER_URL: "https://coder.example.com"
                   CODER_TEMPLATE_ACTIVATE: "false"
                   CODER_SESSION_TOKEN: ${{ secrets.CODER_SESSION_TOKEN }}
   ```
