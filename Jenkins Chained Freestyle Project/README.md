# Jenkins Chained Freestyle Project (Build → Test → Deploy) with Automatic Trigger & Copy Artifact Plugin

This repository demonstrates a **Jenkins Chained Freestyle Project** where:
1. **Build_Project** builds the application and generates an artifact.
2. **Test_Project** runs tests and copies the artifact from Build_Project using **Copy Artifact Plugin**.
3. **Deploy_Project** deploys the artifact to a server.

All jobs are **automatically triggered** in sequence.

---

## 🔹 Prerequisites
- **Jenkins Installed** – [Download Jenkins](https://www.jenkins.io/download/)
- **Required Plugins**:
  - ✅ **Copy Artifact Plugin**
  - ✅ **Parameterized Trigger Plugin**

---

## 🔹 Step-by-Step Configuration

### 1️⃣ Install Required Plugins
1. Open **Jenkins Dashboard** → **Manage Jenkins** → **Manage Plugins**.
2. Search and install:
   - ✅ **Copy Artifact Plugin**
   - ✅ **Parameterized Trigger Plugin**.
3. Restart Jenkins after installation.

---

### 2️⃣ Create `Build_Project` (Job 1)
1. Click **New Item** → Enter `Build_Project` → Select **Freestyle Project** → Click **OK**.
2. **Source Code Management**:
   - Select **Git** (if using a repository).
   - Enter your repository URL.
3. **Build Steps** → Click **Add build step** → **Execute shell**, add:

   ```sh
   # Build a message by invoking ADVICESLIP API
   curl -s https://api.adviceslip.com/advice > advice.json
   cat advice.json
   ```

4. **Post-build Actions** → Select:
   - **Archive the Artifacts** → Enter `advice.json`.
   - **Build Other Projects** → Enter `Test_Project` → Select **Trigger only if build is stable**.

5. **Click Save**.

---

### 3️⃣ Create `Test_Project` (Job 2)
1. Click **New Item** → Enter `Test_Project` → Select **Freestyle Project** → Click **OK**.
2. **Build Steps**:
   - **Copy Artifacts from Another Project**:
     - **Project Name** → Enter `Build_Project`
     - **Which Artifacts to Copy** → `advice.json`
     - **Target Directory** → `copied_artifacts`
   - **Execute Shell**, add:

     ```sh
     # Test to make sure the advice message has more than 5 words.
     cat advice.json | jq -r .slip.advice > advice.message
     [ $(wc -w < advice.message) -gt 5 ] && echo "Advice has more than 5 words" || (echo "Advice - $(cat advice.message) has 5 words or less" && exit 1)
     ```

3. **Post-build Actions** → Select:
   - **Build Other Projects** → Enter `Deploy_Project` → Select **Trigger only if build is stable**.

4. **Click Save**.

---

### 4️⃣ Create `Deploy_Project` (Job 3)
1. Click **New Item** → Enter `Deploy_Project` → Select **Freestyle Project** → Click **OK**.
2. **Build Steps**:
   - **Copy Artifacts from Another Project**:
     - **Project Name** → Enter `Test_Project`
     - **Which Artifacts to Copy** → `advice.message`
     - **Target Directory** → `deploy_output`
   - **Execute Shell**, add:

     ```sh
     # Deploy
     echo $PATH
     export
     PATH="$PATH:/usr/games:/usr/games/cowsay"
     cat advice.message | cowsay -f $(ls /usr/share/cowsay/cows | shuf -n 1)
     ```

3. **Click Save**.

---

## 🔹 Triggering the Chained Jobs
- Go to **`Build_Project`** → Click **Build Now**.
- Jenkins will automatically execute:
  ✅ **`Build_Project`** → ✅ **`Test_Project`** → ✅ **`Deploy_Project`**

---

## 🔹 Verifying the Artifacts
After the build process:
1. **`Test_Project`** will have `advice.json` copied from `Build_Project`.
2. **`Deploy_Project`** will have `advice.message` copied from `Test_Project`.

You can verify this by checking the **Console Output** in Jenkins.

---

## 🔹 Enhancements
✔ **Failure Handling** – Use **Conditional Build Steps** to prevent broken deployments.
✔ **Notifications** – Add **Slack or Email notifications** in **Post-build Actions**.
✔ **Parameterized Builds** – Use parameters for different environments (e.g., Dev, Staging, Production).

---

## License
This project is open-source and free to use.

---

Happy CI/CD! 🚀
