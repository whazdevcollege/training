# deploy-infra.yaml
name: Deploy Bicep template

on:
  push:

env:
  # Change this if more then one user is deploying to
  # the same subscription
  RESOURCE_GROUP_NAME: github-action-bicep-rg
  RESOURCE_GROUP_LOCATION: westeurope
  ENV_NAME: devd4

jobs:
  deploy-infra:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Azure Login
        uses: Azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}

      - name: Create Resource Group
        run: >
          az group create
          -l ${{ env.RESOURCE_GROUP_LOCATION }}
          -n ${{ env.RESOURCE_GROUP_NAME }}

      - name: Deploy Bicep template
        uses: azure/arm-deploy@v1
        id: infra
        with:
          resourceGroupName: ${{ env.RESOURCE_GROUP_NAME }}
          template: ./infra.bicep
          # Here we pass the template parameters to the deployment
          parameters: >
            env=${{ env.ENV_NAME }}

      - name: Print WebApp endpoint
        # Here we read the outputs of our previously deployed template
        run: echo https://${{ steps.infra.outputs.webAppEndpoint }}
