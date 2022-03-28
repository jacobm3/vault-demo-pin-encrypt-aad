# About
This repo contains resources used to demonstrate how Vault Enterprise can be used to support encryption of batches of IMEI PINs with varying workflow and algorithm requirements.

# Resources
Vault setup with Azure Active Directory (AAD) OIDC and external groups

https://learn.hashicorp.com/tutorials/vault/oidc-auth-azure?in=vault/auth-methods

### az cli commands
```
# login using device code, required for WSL
$ az login --use-device-code --tenant <TENANT ID>

# create test groups
$ for x in a b c; do  az ad group create --display-name project-${x} --mail-nickname project-${x}; done

# review groups
$ az ad group list | jq '.[].displayName'
 
"project-a"
"project-b"
"project-c"
"AAD DC Administrators"
"VAULT_ADMINS"

$ export AD_AZURE_DISPLAY_NAME=vault-acer-demo

$ export AD_VAULT_APP_ID=$(az ad app create \
   --display-name ${AD_AZURE_DISPLAY_NAME} \
   --reply-urls "http://localhost:8250/oidc/callback" \
   "${VAULT_ADDR}/ui/vault/auth/oidc/oidc/callback" | \
   jq -r '.appId')

$ echo $AD_VAULT_APP_ID
9104467f-149c-4b1e-b22b-f75930ca546e


$ vault namespace create emei-pin

```

