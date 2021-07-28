# pact action

Several github actions to simplify working with pact tests in github action workflows.

**Please note that this repository is public so it can be accessed by github actions**

## Example

```
env:
  PACTICIPANT_NAME: my-super-service
  PACT_BROKER_URL: https://my-pact-broker.com

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: checkout
        uses: actions/checkout@v2
      
      # Add here any steps needed to test the app.
      # These steps should also generate the pact contract files.
      # In this example, they are placed in the ./pacts/ folder

      - uses: d-i-p/pact-actions/can-i-deploy@main
        with:
          pacticipant-name: ${{ env.PACTICIPANT_NAME }}
          pacts-folder: ${{ github.workspace }}/pacts
          broker-url: ${{ env.PACT_BROKER_URL }}
          broker-username: ${{ secrets.PACT_BROKER_USERNAME }}
          broker-password: ${{ secrets.PACT_BROKER_PASSWORD }}
  
  deploy:
    needs: [pact-can-i-deploy]
    runs-on: ubuntu-latest
    steps:
      # Add your usual deployment steps

      - uses: d-i-p/pact-actions/mark-as-deployed@main
        with:
          pacticipant-name: ${{ env.PACTICIPANT_NAME }}
          broker-url: ${{ env.PACT_BROKER_URL }}
          broker-username: ${{ secrets.PACT_BROKER_USERNAME }}
          broker-password: ${{ secrets.PACT_BROKER_PASSWORD }}
