#Secure Developer Environment on Windows

##🎯 Objective

To simulate a real-world secured development environment where source code integrity and Git identity security are enforced — ensuring developers cannot modify Git credentials or system-wide configurations.

⚙️ Steps Performed
1️⃣ Create Developer Account

Run the following command in Command Prompt (Admin):

net user developer Developer@123 /add


This creates a standard (non-admin) developer account.

<img width="415" height="59" alt="image" src="https://github.com/user-attachments/assets/31378ce3-c19f-4842-8b95-8a23f3b4466e" />
<img width="416" height="167" alt="image" src="https://github.com/user-attachments/assets/691158be-80da-476d-b438-c7d6af209fb5" />



2️⃣ Log in as Developer

Log out of the administrator account and log in using the newly created developer account.
<img width="416" height="267" alt="image" src="https://github.com/user-attachments/assets/a2ac289c-8b91-475b-87b1-8d95941492d4" />
<img width="416" height="204" alt="image" src="https://github.com/user-attachments/assets/28f414b2-d3e7-4d53-aa41-5256eb0f2fc3" />




3️⃣ Check Git Installation

Open Command Prompt (as developer) and verify Git version:

git --version
<img width="333" height="52" alt="image" src="https://github.com/user-attachments/assets/433a6e07-ccf1-4541-a478-a8b8015bd189" />


4️⃣ Create Project Directory

In your developer account, create a working repo folder:

mkdir C:\Repos\SampleRepo
cd C:\Repos\SampleRepo


5️⃣ Open Project in VS Code

Open the folder in Visual Studio Code:

code .
<img width="358" height="513" alt="image" src="https://github.com/user-attachments/assets/38fc5776-72e6-4da5-8540-c0d59c470818" />




6️⃣ Secure System Git Configuration

Now switch back to Administrator mode.
Run the following commands in Command Prompt (Admin):

icacls "C:\Program Files\Git\etc\gitconfig" /inheritance:r
icacls "C:\Program Files\Git\etc\gitconfig" /grant "Administrators:F" "SYSTEM:F"
icacls "C:\Program Files\Git\etc\gitconfig" /remove "Users"
attrib +R "C:\Program Files\Git\etc\gitconfig"


These commands remove user modification rights and make the system Git config file read-only.

<img width="416" height="122" alt="image" src="https://github.com/user-attachments/assets/a07ce46c-4780-44c0-ac26-b1833fe48b37" />


7️⃣ Verify Git Identity

Switch back to developer account and run:

git config --list --show-origin


It should display:

system  C:\Program Files\Git\etc\gitconfig   user.name=Ravinder
system  C:\Program Files\Git\etc\gitconfig   user.email=ravinder@gmail.com


<img width="415" height="167" alt="image" src="https://github.com/user-attachments/assets/8a241998-f597-4ce1-bbdc-44b987c9edc7" />


8️⃣ Test Security — Attempt Unauthorized Edits

Try editing Git identity or modifying system gitconfig:

git config --global user.name "newUser"
git config --global user.email "newUser@example.com"


Expected:

Permission denied, or

No change reflected in git config --list.

<img width="415" height="97" alt="image" src="https://github.com/user-attachments/assets/0e733ed3-e4e7-424c-b465-e97c873de57d" />


9️⃣ Attempt to Edit gitconfig via VS Code

As the developer user:

Open VS Code → File → Open File → C:\Program Files\Git\etc\gitconfig

Try to save after making an edit.

Expected:
Error such as “Failed to save: Access Denied” or “read-only file”.

<img width="415" height="163" alt="image" src="https://github.com/user-attachments/assets/46c66f09-c8df-4638-9d4b-975a681b3af5" />


🔟 Validate Commit Author

Create or edit a test repo file, make a commit, and check the author details:

git add .
git commit -m "test commit"

it should show permission denied
<img width="415" height="108" alt="image" src="https://github.com/user-attachments/assets/a33f3e27-2a70-4ffa-8923-c4c75bf4b9af" />
<img width="963" height="168" alt="Screenshot 2025-11-08 135505" src="https://github.com/user-attachments/assets/6ba9b272-531d-4b0f-84d4-561fb52c3adb" />




✅ Validation Summary
Test	Expected Result	Outcome
Developer created	Yes	✅ Passed
Git version verified	Yes	✅ Passed
Folder structure set	Yes	✅ Passed
Git system file locked	Yes	✅ Passed
Developer cannot modify Git config	Yes	✅ Passed
Commit attempt shows “Permission denied” (protected)	Yes	✅ Passed

🧩 Key Learnings

Git system-wide config is stored at:
C:\Program Files\Git\etc\gitconfig
Proper user role separation ensures source integrity and secure development practices.

🏁 Conclusion

This simulation successfully demonstrates:

A restricted developer environment on Windows.

Developers have coding privileges but cannot change Git identity or system configurations.

Git credentials remain consistent and verified across all sessions.
