The Nautilus DevOps team is focusing on improving their data security by using Azure Key Vault. Your task is to create a Key Vault with an RSA key and manage the encryption and decryption of a pre-existing sensitive file using this key.  
Specific Requirements:  
Create a Key Vault:  
&nbsp;&nbsp;&nbsp;Name the Key Vault datacenter-RAN.  
&nbsp;&nbsp;&nbsp;Create the vault in the East US region.  
&nbsp;&nbsp;&nbsp;Use the Standard pricing tier.  
&nbsp;&nbsp;&nbsp;Set Soft Delete retention to 7 days.  
&nbsp;&nbsp;&nbsp;Use the Vault access policy permission model.  
&nbsp;&nbsp;&nbsp;Configure an access policy that allows Get, List, Encrypt, and Decrypt permissions for the lab identity.  
Create an RSA Key:  
&nbsp;&nbsp;&nbsp;Create a key named datacenter-key within the Key Vault.  
&nbsp;&nbsp;&nbsp;Key type: RSA.  
&nbsp;&nbsp;&nbsp;RSA key size: 4096.  
&nbsp;&nbsp;&nbsp;Leave all other settings as default.  
Encrypt the Sensitive Data:  
&nbsp;&nbsp;&nbsp;Use the key to encrypt the provided SensitiveData.txt file (located in /root/) on the azure-client host.  
&nbsp;&nbsp;&nbsp;Use the RSA-OAEP algorithm.  
&nbsp;&nbsp;&nbsp;Base64 encode the plaintext before encryption.  
&nbsp;&nbsp;&nbsp;Save the encrypted version as EncryptedData.bin in the /root/ directory.  
Note: If you encounter a permissions error, retrieve the Service Principal ID using:  
&nbsp;&nbsp;&nbsp;az account show --query user.name -o tsv  
&nbsp;&nbsp;&nbsp;and grant the required Key Vault permissions.  
Verify Decryption:  
&nbsp;&nbsp;&nbsp;Decrypt EncryptedData.bin.  
&nbsp;&nbsp;&nbsp;Base64 decode the decrypted output.  
&nbsp;&nbsp;&nbsp;Save the result as DecryptedData.txt in /root/.  
&nbsp;&nbsp;&nbsp;Ensure the decrypted file matches the original SensitiveData.txt.  
&nbsp;&nbsp;&nbsp;Ensure that the Key Vault and key are correctly configured. The validation script will test your configuration by decrypting the EncryptedData.bin file using the key you created.  

Solution:  
```
RG=$(az group list --query "[0].name" -o tsv)
-
az keyvault create \
  --name datacenter-RAN \
  --resource-group $RG \
  --location "East US" \
  --retention-days 7 \
  --enable-rbac-authorization false
-
az keyvault key create \
  --vault-name datacenter-RAN \
  --name datacenter-key \
  --kty RSA \
  --size 2048
-
base64 /root/SensitiveData.txt > /root/plain.b64
-
az keyvault key encrypt \
  --vault-name $KEY_VAULT \
  --name $KEY_NAME \
  --algorithm RSA-OAEP \
  --value "$(cat /root/plain.b64)" \
  --query result -o tsv \
  > /root/EncryptedData.b64
-
base64 -d /root/EncryptedData.b64 > /root/EncryptedData.bin
-
az keyvault key decrypt \
  --vault-name datacenter-RAN \
  --name datacenter-key \
  --algorithm RSA-OAEP \
  --value "$(cat /root/EncryptedData.b64)" \
  --query result -o tsv \
  > /root/DecryptedData.b64
-
base64 -d /root/DecryptedData.b64 > /root/DecryptedData.txt
-
diff /root/SensitiveData.txt /root/DecryptedData.txt
```
[reference](https://learn.microsoft.com/en-us/azure/key-vault/general/quick-create-cli)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)
