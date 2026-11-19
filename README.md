# Zero Trust Conditional Access Implementation in Microsoft Entra ID

## Project Overview
This lab demonstrates the implementation and validation of a **Zero Trust Identity & Access Management (IAM)** architecture using **Microsoft Entra ID P2**. By transitioning from basic **Security Defaults** to granular **Conditional Access Policies (CAP)**, this project enforces Multi-Factor Authentication (MFA) for cloud applications, validates policy impact using the **What If** simulation tool, and verifies live enforcement through **Entra ID Sign-in Logs**.

---

## Lab Environment & Prerequisites
* **Identity Provider:** Microsoft Entra ID (Tenant: `Sri-Personal Lab`)
* **License:** Microsoft Entra ID P2 (Trial)
* **Test Identity:** `Test-User` (Dedicated non-admin account created to follow Least Privilege and avoid Global Admin lockout)
* **Target Scope:** All Cloud Resources (`My Apps`, `Office 365`)

---

## Implementation Steps & Proof of Execution

### 1. Tenant Setup & Dedicated Test Identity
Configured the `Sri-Personal Lab` tenant with a **Microsoft Entra ID P2** license and provisioned a dedicated non-privileged `Test-User` account to safely test access controls without risking administrative lockout.

<img width="1920" height="980" alt="CA 1" src="https://github.com/user-attachments/assets/fbaf9b09-3dd6-4201-8395-9c7b5378f037" />

---

### 2. Disabling Security Defaults
Before enforcing custom Conditional Access rules, **Security Defaults** were disabled in Tenant Properties. Entra ID confirms that Conditional Access policies are now actively governing tenant authentication.

<img width="1920" height="899" alt="CA sec def off" src="https://github.com/user-attachments/assets/a4b1e9b8-2214-476b-b0e2-a620fcbbf03c" />

---

### 3. Configuring Conditional Access Policy (`CA01`)
Created and enabled a custom Conditional Access policy (**`CA01 - Require MFA for Test User`**) with the following Zero Trust parameters:
* **Assignments (Users):** Included `Test-User`
* **Target Resources:** All resources (formerly *All cloud apps*)
* **Access Controls (Grant):** Require multifactor authentication (MFA)
* **Policy State:** `On` (Enforced)

<img width="1920" height="979" alt="CA0" src="https://github.com/user-attachments/assets/08c1b36e-2226-454f-bbaa-9b475cea5320" />

---

#### Policy List & Full Configuration Summary
Verified that `CA01 - Require MFA for Test User` is active (`State: On`) and properly scoped to 1 user across all cloud resources.

<img width="1920" height="913" alt="Ca policyview" src="https://github.com/user-attachments/assets/9a64ba9a-cb7d-4ef1-bd45-6a52f823a5c5" />

<img width="1920" height="913" alt="CA policyfullview" src="https://github.com/user-attachments/assets/4eedd7b4-c907-4dc9-b27d-704ee7fc74d9" />

---

### 4. Policy Simulation Using the "What If" Tool
To validate policy behavior safely before/alongside live testing, ran a simulation in the **Conditional Access What If** tool targeting `Test-User` on a Windows browser accessing Office 365.
* **Evaluation Result:** `CA01 - Require MFA for Test User` triggered as expected, enforcing **Require multifactor authentication**.

<img width="1920" height="982" alt="CA wat if" src="https://github.com/user-attachments/assets/fe7cc433-486d-484a-8b5a-fefc741d029e" />

---

### 5. Live Enforcement & Sign-in Logs Verification
Tested the policy live in an incognito browser session by logging into `myapps.microsoft.com` as `Test-User`. The user was prompted to update their initial password and complete MFA verification.

Reviewed **Entra ID Sign-in Logs** to confirm the authentication flow:
* **Error Code `50055` (Interrupted):** Initial password change required upon first login.
* **Error Code `50140` (Interrupted):** Authentication interrupted for MFA/session prompt.
* **Error Code `0` (Success):** Access granted to **My Apps** after successfully satisfying the Conditional Access MFA requirement.

<img width="1920" height="914" alt="CA Signonlog" src="https://github.com/user-attachments/assets/50b6113f-93a5-4394-a0d8-5573d83d236a" />

---

## Key Skills Demonstrated
* **Identity & Access Management (IAM):** Microsoft Entra ID P2 administration & user provisioning.
* **Zero Trust Security:** Transitioning from Security Defaults to granular Conditional Access Policies.
* **Policy Testing & Troubleshooting:** Using the **What If** evaluation tool and analyzing **Sign-in Logs**.
* **Best Practices:** Preventing admin lockout by isolating policy testing to a dedicated non-admin test account.
