The Nautilus DevOps team is focusing on improving their data security by using AWS KMS. Your task is to create a KMS key and manage the encryption and decryption of a pre-existing sensitive file using the KMS key.  
Specific Requirements:  
    Create a symmetric KMS key named nautilus-kms-key to manage encryption and decryption.  
    Encrypt the provided SensitiveData.txt file (located in /home/bob/terraform), base64 encode the ciphertext, and save the encrypted version as EncryptedData.bin in the /home/bob/terraform directory.  
    Try to decrypt the same and verify that the decrypted data matches the original file.  
    Create main.tf file (do not create a separate .tf file) to provision a KMS key, encrypt and decrypt the file.  
    Create outputs.tf file to output the following:  
        kke_kms_key_name: name of the key created.  


Solution:  
`main.tf:`  
```terraform
resource "aws_kms_key" "example" {
  key_usage                = "ENCRYPT_DECRYPT"
  customer_master_key_spec = "SYMMETRIC_DEFAULT"
  tags = {
    Name = "nautilus-kms-key"
  }
}

locals {
  sensitive_file_path = "/home/bob/terraform/SensitiveData.txt"
  sensitive_plaintext = file(local.sensitive_file_path)
}

resource "aws_kms_ciphertext" "encrypt" {
  key_id    = aws_kms_key.example.arn
  plaintext = local.sensitive_plaintext
}

resource "local_file" "encrypted_file" {
  filename = "/home/bob/terraform/EncryptedData.bin"
  content  = resource.aws_kms_ciphertext.encrypt.ciphertext_blob
}

data "aws_kms_secrets" "decrypt" {
  secret {
    name    = "decrypted"
    payload = resource.aws_kms_ciphertext.encrypt.ciphertext_blob
  }
}

resource "local_file" "decrypted_file" {
  filename = "/home/bob/terraform/DecryptedData.txt"
  content  = data.aws_kms_secrets.decrypt.plaintext["decrypted"]
}
```
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/kms_ciphertext)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)
