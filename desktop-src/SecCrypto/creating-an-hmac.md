---
Description: Explains how to create an HMAC.
ms.assetid: b1747b7e-a505-4b23-93bc-cef4e77bf825
title: Creating an HMAC
ms.technology: desktop
ms.prod: windows
ms.author: windowssdkdev
ms.topic: article
ms.date: 05/31/2018
---

# Creating an HMAC

**To compute an HMAC**

1.  Get a pointer to the Microsoft [*Cryptographic Service Provider*](https://msdn.microsoft.com/db46def4-bfdc-4801-a57d-d568e94a2dbb) (CSP) by calling [**CryptAcquireContext**](/windows/desktop/api/Wincrypt/nf-wincrypt-cryptacquirecontexta).
2.  Create a handle to an [*HMAC*](https://msdn.microsoft.com/4165b820-30fc-477e-a690-81109f161323)[*hash object*](https://msdn.microsoft.com/4165b820-30fc-477e-a690-81109f161323) by calling [**CryptCreateHash**](/windows/desktop/api/Wincrypt/nf-wincrypt-cryptcreatehash). Pass CALG\_HMAC in the *Algid* parameter. Pass the handle of a [*symmetric key*](https://msdn.microsoft.com/3e9d7672-2314-45c8-8178-5a0afcfd0c50) in the *hKey* parameter. This symmetric key is the key used to compute the HMAC.
3.  Specify the type of hash to be used by calling [**CryptSetHashParam**](/windows/desktop/api/Wincrypt/nf-wincrypt-cryptsethashparam) with the *dwParam* parameter set to the value HP\_HMAC\_INFO. The *pbData* parameter must point to an initialized [**HMAC\_INFO**](/windows/desktop/api/Wincrypt/ns-wincrypt-_hmac_info) structure.
4.  Call [**CryptHashData**](/windows/desktop/api/Wincrypt/nf-wincrypt-crypthashdata) to begin computing the HMAC of the data. The first call to **CryptHashData** causes the key value to be combined using the XOR operator with the inner string and the data. The result of the XOR operation is hashed, and then the target data for the HMAC (pointed to by the *pbData* parameter passed in the call to **CryptHashData**) is hashed. If necessary, subsequent calls to **CryptHashData** may then be made to finish the hashing of the target data.
5.  Call [**CryptGetHashParam**](/windows/desktop/api/Wincrypt/nf-wincrypt-cryptgethashparam) with the *dwParam* parameter set to HP\_HASHVAL. This call causes the inner hash to be finished and the outer string to be combined using XOR with the key. The result of the XOR operation is hashed, and then the result of the inner hash (completed in the previous step) is hashed. The outer hash is then finished and returned in the *pbData* parameter and the length in the *dwDataLen* parameter.

> [!Note]  
> Do not use the same [*symmetric*](https://msdn.microsoft.com/3e9d7672-2314-45c8-8178-5a0afcfd0c50) ([*session*](https://msdn.microsoft.com/3e9d7672-2314-45c8-8178-5a0afcfd0c50)) key for both message encryption and [*Message Authentication Code*](https://msdn.microsoft.com/4c4402e9-7455-4868-978f-3899a8fd86c1) (MAC) generation. Using the same key for both greatly increases the risk of messages being decoded by attackers.

 

 

 


