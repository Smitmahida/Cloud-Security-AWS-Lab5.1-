# Cloud-Security-AWS-Lab5.1
Encrypting Data at Rest using AWS KMS.Learn how to use AWS KMS to encrypt S3 objects and EC2 root volumes, and analyze access with CloudTrail.
# 🔐 AWS Lab 5.1: Encrypting Data at Rest using AWS KMS

**Course:** AWS Academy Cloud Foundations  
**Lab Name:** Encrypting Data at Rest by Using AWS KMS  
**Duration:** ~75 minutes  
**Objective:** Learn how to use AWS KMS to encrypt S3 objects and EC2 root volumes, and analyze access with CloudTrail.

---

## 🏛️ Lab Architecture Overview

- S3 Bucket: `ImageBucket` (empty initially)
- EC2 Instance: `LabInstance` with unencrypted root volume
- Services Used: KMS, S3, EC2, CloudTrail

---

## 🚀 Step-by-Step Execution

### ✅ **Task 1: Create a Customer Managed KMS Key**
1. Navigate to **Key Management Service (KMS)**.
2. Click **Customer managed keys > Create key**.
3. **Key type**: Symmetric
4. **Alias**: `MyKMSKey`
5. **Key administrators**: Add `voclabs`
6. **Key users**: Add `voclabs`
7. Review and **Finish**.

✅ *KMS key created and ready for use.*

---

### 📂 **Task 2: Encrypt and Upload Object to S3**
1. Go to **S3 > Buckets > ImageBucket**.
2. Verify **Default Encryption** is enabled.
3. Upload `clock.png`:
   - Under **Properties > Server-side encryption**: Choose SSE-KMS
   - Select `MyKMSKey`

✅ *clock.png uploaded with KMS encryption.*

---

### 👁️ **Task 3: Public Access Test (Fails)**
1. Copy and paste the **Object URL** in a new browser tab → **Access Denied**.
2. Update bucket permissions:
   - Uncheck **Block all public access**
   - Enable **ACLs**
   - Make `clock.png` public
3. Refresh object URL → **Invalid Request (due to SSE-KMS)**

✅ *Public access fails without signed request — confirms encryption protection.*

---

### 🔐 **Task 4: Signed Access (Succeeds)**
1. In S3 console, click **Open** on `clock.png`
2. Image loads using signed URL (SigV4 auth)

✅ *Authenticated user can view encrypted object.*

---

### 📄 **Task 5: Audit KMS in CloudTrail**
1. Open **CloudTrail > Event History**
2. Filter by event source: `kms.amazonaws.com`
3. View:
   - `GenerateDataKey`: triggered during upload
   - `Decrypt`: when opening file in S3

✅ *CloudTrail shows who accessed encryption keys and when.*

---

### 📀 **Task 6: Encrypt EC2 Root Volume**
1. Stop the `LabInstance`
2. Go to **Volumes > Old Volume > Create Snapshot**
3. Go to **Snapshots > Create Volume from Snapshot**
   - Enable **Encryption** with `MyKMSKey`
4. Detach old volume, attach encrypted one to `/dev/xvda`

✅ *New root volume is encrypted using KMS.*

---

### ❌ **Task 7: Disable Key and Observe Failures**
1. Disable `MyKMSKey` in KMS Console
2. Try to start `LabInstance` → **Fails**
3. Try to access `clock.png` → **Fails with KMS.DisabledException**
4. CloudTrail shows errors in `CreateGrant` due to disabled key

✅ *Disabling a key blocks all access to encrypted data.*

---

### ✔️ **Re-enable Key and Recover Access**
1. Re-enable `MyKMSKey` in KMS
2. Start `LabInstance` again → **Now it works**
3. Reopen `clock.png` in S3 → **Loads successfully**

✅ *Access is restored once key is re-enabled.*

---

## 📊 Summary
| Task | Outcome |
|------|---------|
| KMS Key Creation | ✅ |
| S3 Object Encryption | ✅ |
| Public Access Attempt | ❌ |
| Signed Access Attempt | ✅ |
| CloudTrail Monitoring | ✅ |
| EC2 Volume Encryption | ✅ |
| Key Disable/Enable Test | ✅ |

---

## 🔗 References
- [AWS KMS Docs](https://docs.aws.amazon.com/kms/)
- [AWS S3 Encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html)
- [CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)

---

**🎉 Lab Complete!**  
You successfully encrypted data at rest using AWS KMS, enforced access control, monitored key usage with CloudTrail, and understood the importance of key lifecycle management.

