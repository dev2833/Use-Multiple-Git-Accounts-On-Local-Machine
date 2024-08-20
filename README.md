Using multiple GitHub accounts on a local machine requires managing SSH keys and configuring Git to use the correct credentials for each account. Here's how you can do it:

### Step 1: Generate SSH Keys for Each Account

1. **Generate a New SSH Key:**
   - Open your terminal.
   - Run the following command to generate a new SSH key (replace `email@example.com` with the email associated with the GitHub account):

     ```bash
     ssh-keygen -t rsa -b 4096 -C "email@example.com"
     ```

   - When prompted, save the key with a unique name `/c/Users/USER_NAME/.ssh/id_rsa_account1`
     ```bash
     Enter file in which to save the key (/c/Users/USER_NAME/.ssh/id_rsa): /c/Users/USER_NAME/.ssh/id_rsa_account1
     ```


2. **Ensure the ssh-agent is running by executing:**
     ```bash
    eval "$(ssh-agent -s)"
     ```

3. **Add the SSH Key to the SSH Agent:**

   ```bash
   ssh-add ~/.ssh/id_rsa_account1
   ```

4. **Copy the SSH Key to GitHub:**

   - Copy the SSH key to your clipboard:

     ```bash
     clip < ~/.ssh/id_rsa_account1.pub
     ```

   - Go to your GitHub account settings, then to "SSH and GPG keys," and add the key.

4. **Repeat the above steps for the second (or additional) GitHub account.** Be sure to name the keys differently (e.g., `id_rsa_account2`).

### Step 2: Create a Config File for SSH

1. **Edit the SSH Config File:**
   - Open your `~/.ssh/config` file in a text editor. If it doesn’t exist, create it.

2. **Add Configurations for Each Account:**
   - Add the following configuration, replacing the paths and usernames as needed:

     ```bash
     # Account 1
     Host github-account1
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_rsa_account1

     # Account 2
     Host github-account2
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_rsa_account2
     ```

### Step 3: Clone Repositories Using the Correct Account

When cloning repositories, specify the host alias you defined in your SSH config:

- For the first account:

  ```bash
  git clone git@github-account1:username/repo.git
  ```

- For the second account:

  ```bash
  git clone git@github-account2:username/repo.git
  ```

### Step 4: Configure Git for Each Repository

Inside each repository, you can set the user name and email to match the corresponding GitHub account:

```bash
git config user.name "Your Name"
git config user.email "email@example.com"
```

You can also set this globally if you prefer to have a default identity:

```bash
git config --global user.name "Your Default Name"
git config --global user.email "default@example.com"
```

### Step 5: Using Different Accounts for Push and Pull

When working with existing repositories, make sure to change the remote URL to use the correct SSH alias:

```bash
git remote set-url origin git@github-account1:username/repo.git
```

### Summary

- **SSH keys:** Create a unique SSH key for each GitHub account.
- **SSH config:** Set up your `~/.ssh/config` file to use different keys for each account.
- **Cloning:** Use the appropriate SSH alias to clone repositories.
- **Git config:** Set user name and email per repository or globally as needed.

This setup allows you to manage multiple GitHub accounts on the same machine without conflicts.
