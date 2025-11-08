<h1 align="center">🛡️ Secure Developer Environment on Windows</h1>

---

## 🎯 **Objective**

To simulate a **real-world secured development environment** where **source code integrity** and **Git identity security** are enforced — ensuring developers cannot modify Git credentials or system-wide configurations.

---

## ⚙️ **Steps Performed**

### 1️⃣ Create Developer Account
Run the following command in **Command Prompt (Admin)**:
```cmd
net user developer Developer@123 /add
Creates a standard (non-admin) developer account.
```

<img width="415" src="https://github.com/user-attachments/assets/31378ce3-c19f-4842-8b95-8a23f3b4466e" /> <img width="416" src="https://github.com/user-attachments/assets/691158be-80da-476d-b438-c7d6af209fb5" />


2️⃣ Log in as Developer
Log out of the administrator account and log in using the newly created developer account.

<img width="416" src="https://github.com/user-attachments/assets/a2ac289c-8b91-475b-87b1-8d95941492d4" /> <img width="416" src="https://github.com/user-attachments/assets/28f414b2-d3e7-4d53-aa41-5256eb0f2fc3" />


3️⃣ Check Git Installation
Open Command Prompt (as developer) and verify Git installation:

```cmd
Copy code
git --version
```
<img width="333" src="https://github.com/user-attachments/assets/433a6e07-ccf1-4541-a478-a8b8015bd189" />


4️⃣ Create Project Directory
Create a working repo folder:

```cmd
Copy code
mkdir C:\Repos\SampleRepo
cd C:\Repos\SampleRepo
```


5️⃣ Open Project in VS Code
Open the folder in Visual Studio Code:

```cmd
Copy code
code .
```
<img width="358" src="https://github.com/user-attachments/assets/38fc5776-72e6-4da5-8540-c0d59c470818" />


6️⃣ Secure System Git Configuration
Switch back to Administrator mode and execute:

```cmd
Copy code
icacls "C:\Program Files\Git\etc\gitconfig" /inheritance:r
icacls "C:\Program Files\Git\etc\gitconfig" /grant "Administrators:F" "SYSTEM:F"
icacls "C:\Program Files\Git\etc\gitconfig" /remove "Users"
attrib +R "C:\Program Files\Git\etc\gitconfig"
```
These commands remove user modification rights and make the Git system config file read-only.

<img width="416" src="https://github.com/user-attachments/assets/a07ce46c-4780-44c0-ac26-b1833fe48b37" />


7️⃣ Verify Git Identity
Switch back to the developer account and run:

```cmd
Copy code
git config --list --show-origin
```
It should display:


```cmd
system  C:\Program Files\Git\etc\gitconfig   user.name=Ravinder
system  C:\Program Files\Git\etc\gitconfig   user.email=ravibeniwal931@gmail.com
```
<img width="415" src="https://github.com/user-attachments/assets/8a241998-f597-4ce1-bbdc-44b987c9edc7" />


8️⃣ Test Security — Attempt Unauthorized Edits
Try to modify Git identity:

```cmd
git config --global user.name "newUser"
git config --global user.email "newUser@example.com"
```
Expected:

Permission denied ❌

Or no change reflected in git config --list

<img width="415" src="https://github.com/user-attachments/assets/0e733ed3-e4e7-424c-b465-e97c873de57d" />


9️⃣ Attempt to Edit gitconfig via VS Code
As the developer user:
Open C:\Program Files\Git\etc\gitconfig in VS Code → try to save it.

Expected:
Error such as Access Denied or file opens as read-only.

<img width="415" src="https://github.com/user-attachments/assets/46c66f09-c8df-4638-9d4b-975a681b3af5" />


🔟 Validate Commit Behavior
Create or edit a test file, then run:

```cmd
git add .
git commit -m "test commit"
```
Result: Permission denied — commit cannot override protected Git identity.

<img width="415" src="https://github.com/user-attachments/assets/a33f3e27-2a70-4ffa-8923-c4c75bf4b9af" /> <img width="963" src="https://github.com/user-attachments/assets/6ba9b272-531d-4b0f-84d4-561fb52c3adb" />


✅ Validation Summary

Test	Expected Result	Outcome

Developer created	Yes	✅ Passed

Git version verified	Yes	✅ Passed

Folder structure set	Yes	✅ Passed

Git system file locked	Yes	✅ Passed

Developer cannot modify Git config	Yes	✅ Passed

Commit attempt shows “Permission denied” (protected)	Yes	✅ Passed


🧩 Key Learnings

System-wide Git config is stored at:

C:\Program Files\Git\etc\gitconfig

icacls and attrib are powerful for applying ACL restrictions.

Proper user role separation ensures source integrity and secure development practices.

🏁 Conclusion

This simulation successfully demonstrates:

✅ A restricted developer environment on Windows.

✅ Developers can edit code but cannot modify Git identity or system configurations.

✅ Git credentials remain consistent and protected across sessions.
