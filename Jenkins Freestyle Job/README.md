# Introduction

This repository demonstrates a simple **Jenkins Freestyle Project**, executes a shell script, and prints `ASCII Artwork`.

---

## Prerequisites
Before setting up the Jenkins pipeline, ensure you have the following installed:
- **Jenkins** (Latest version) - [Download Jenkins](https://www.jenkins.io/download/) and refer installation process here https://github.com/zippyzap2/Jenkins-Projects/tree/main/Jenkins%20Setup 
- **JQ Plugin** (to fetch or extract data)
- **Cowsay Plugin** (to generate artwork)
- **Shell Execution** (Depends on OS: `bash` for Linux/macOS, `cmd` for Windows)

---

## Steps to Create the Jenkins Freestyle Project

### 1️⃣ Open Jenkins
1. Open your Jenkins UI: `http://youripaddress:8080` or your Jenkins server URL.
2. Log in as an administrator.

<img width="500" alt="Screenshot 2025-01-11 at 3 01 55 PM" src="https://github.com/user-attachments/assets/af868a14-9fb6-4dcf-903c-ad03fb777040" />


### 2️⃣ Create a New Project
1. Click **New Item**.
2. Enter a project name (e.g., `Generate ASCII Artwork`).
3. Select **Freestyle Project**.
4. Click **OK**.

<img width="500" alt="Screenshot 2025-01-11 at 3 06 02 PM" src="https://github.com/user-attachments/assets/b98e32ef-66ba-4771-8007-b5d36eda0308" />


### 3️⃣ Configure Source Code Management (Optional)
If you want Jenkins to pull code from a Git repository:
1. Scroll to **Source Code Management**.
2. Select **Git**.
3. Enter the **Repository URL** (e.g., `https://github.com/your-repo.git`).
4. Provide credentials if required.

### 4️⃣ Add a Build Step
1. Scroll to the **Build** section.
2. Click **Add build step** → **Execute shell** (Linux/macOS) or **Execute Windows batch command** (Windows).
3. Add the following script:

#### For Linux/macOS:
```sh

# Build a message by invoking ADVICESLIP API
curl -s https://api.adviceslip.com/advice > advice.json
cat advice.json

# Test to make sure the advice message has more than 5 words.
cat advice.json | jq -r .slip.advice > advice.message
[ $(wc -w < advice.message) -gt 5 ] && echo "Advice has more than 5 words" || (echo "Advice - $(cat advice.message) has 5 words or less" && exit 1)

# Deploy
echo $PATH
export
PATH="$PATH:/usr/games:/usr/games/cowsay"
cat advice.message | cowsay -f $(ls /usr/share/cowsay/cows | shuf -n 1)
```

<img width="500" alt="Screenshot 2025-01-11 at 4 16 49 PM" src="https://github.com/user-attachments/assets/96fbe14f-ad10-4435-99c1-fa963ab7337d" />


### 5️⃣ Save and Build
1. Click **Save**.
2. Click **Build Now**.
3. Navigate to the **Console Output** to see the execution logs.

---

## Expected Output
After a successful build, the console output should display:

<img width="500" alt="Screenshot 2025-01-11 at 4 13 44 PM" src="https://github.com/user-attachments/assets/7c7a3ba2-8aef-428e-a4b0-129f2b739d8f" />


---

## Enhancements
You can enhance this project by:
- Integrating **Git, Maven, or Docker**
- Automating deployments to a **server or cloud**
- Adding **post-build actions** like notifications or artifact storage

---

## License
This project is open-source and free to use.

---

Happy Coding! 🚀
