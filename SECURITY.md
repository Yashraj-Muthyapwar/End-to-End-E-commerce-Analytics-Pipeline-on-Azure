# Security Guide — Clearing Sensitive Information from Git History

Even after removing credentials from the latest commit, **sensitive data may still be accessible in older commits**. Below are the recommended approaches to fully purge it from your repository's Git history.

---

## 1. Using `git filter-repo` (Recommended)

[`git filter-repo`](https://github.com/newren/git-filter-repo) is the modern, officially recommended replacement for `git filter-branch`.

### Install

```bash
pip install git-filter-repo
```

### Replace sensitive strings across all history

Create a file called `replacements.txt` with entries in the format `LITERAL_TEXT==>REPLACEMENT`:

```
ACTUAL_DB_HOST==><your-mysql-host>
ACTUAL_DB_NAME==><your-placeholder>
ACTUAL_STORAGE_ACCOUNT==><your-storage-account>
```

Then run:

```bash
git filter-repo --replace-text replacements.txt --force
```

### Remove a file that contained secrets from all history

```bash
git filter-repo --path <file-path> --invert-paths --force
```

After running `git filter-repo`, you must **force-push** to update the remote:

```bash
git push origin --force --all
git push origin --force --tags
```

> **Note:** All collaborators must re-clone the repository after a history rewrite.

---

## 2. Using BFG Repo-Cleaner

[BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/) is a simpler, faster alternative to `git filter-branch` for removing secrets.

### Install

Download the JAR from [https://rtyley.github.io/bfg-repo-cleaner/](https://rtyley.github.io/bfg-repo-cleaner/).

### Replace sensitive text across all history

Create a file called `passwords.txt` with one secret per line:

```
ACTUAL_DB_HOST
ACTUAL_DB_NAME
ACTUAL_STORAGE_ACCOUNT
```

Then run:

```bash
java -jar bfg.jar --replace-text passwords.txt
git reflog expire --expire=now --all
git gc --prune=now --aggressive
git push origin --force --all
```

### Delete a file from all history

```bash
java -jar bfg.jar --delete-files <filename>
git reflog expire --expire=now --all
git gc --prune=now --aggressive
git push origin --force --all
```

---

## 3. Using `git filter-branch` (Legacy)

> **Warning:** This approach is slow and error-prone for large repositories. Prefer `git filter-repo` or BFG instead.

### Remove a file from all history

```bash
git filter-branch --force --index-filter \
  'git rm --cached --ignore-unmatch <file-path>' \
  --prune-empty --tag-name-filter cat -- --all
```

### Clean up and force-push

```bash
git reflog expire --expire=now --all
git gc --prune=now --aggressive
git push origin --force --all
```

---

## 4. Rotate All Exposed Credentials

Rewriting history removes secrets from future clones, but **anyone who previously cloned or forked the repository may still have access to the old data**. You must:

1. **Rotate (regenerate) every credential** that was ever committed — passwords, API keys, connection strings, SAS tokens, client secrets, etc.
2. **Revoke old credentials** so they can no longer be used.
3. **Audit access logs** for any unauthorized usage during the exposure window.

---

## 5. GitHub-Specific Steps

After rewriting history, take these additional steps on GitHub:

1. **Contact GitHub Support** to remove cached views and pull request references that may still contain the old commits: [https://support.github.com/contact](https://support.github.com/contact)
2. **Force all collaborators to rebase** (not merge) any outstanding work off the rewritten history:
   ```bash
   git fetch origin
   git rebase origin/main
   ```
3. **Delete any forks** that may still contain the old history, or notify fork owners to update.
4. **Enable secret scanning** on the repository to detect any future accidental commits of secrets:
   - Go to **Settings → Code security and analysis → Secret scanning** and enable it.

---

## 6. Prevention Best Practices

- **Use `.gitignore`** to exclude `.env` and other secret files (already configured in this repo).
- **Use environment variables** or Azure Key Vault for all credentials at runtime.
- **Use placeholder values** (`<your-storage-account>`, `<your-mysql-host>`) in committed configuration files.
- **Enable GitHub secret scanning** and **push protection** to block secrets before they are pushed.
- **Use pre-commit hooks** (e.g., [`gitleaks`](https://github.com/gitleaks/gitleaks), [`detect-secrets`](https://github.com/Yelp/detect-secrets)) to catch secrets locally before committing.

---

## Sensitive Information Scan Summary

The following files were scanned and remediated in this repository:

| File | Issue Found | Status |
|---|---|---|
| `linkedService/filessSQLDB.json` | Hardcoded MySQL hostname, database, and username | ✅ Replaced with placeholders |
| `linkedService/ADLSForCSV.json` | Hardcoded Azure storage account URL | ✅ Replaced with placeholder |
| `linkedService/SQLToADLSLinkedService.json` | Hardcoded Azure storage account URL | ✅ Replaced with placeholder |
| `synapse/linkedService/olistdata-synapse-workspace-WorkspaceDefaultStorage.json` | Hardcoded Azure storage account URL | ✅ Replaced with placeholder |
| `synapse/sqlscript/SQL script 1.json` | Hardcoded Azure storage account URL | ✅ Replaced with placeholder |
| `synapse/sqlscript/CREATE VIEW.json` | Hardcoded Azure storage account URL | ✅ Replaced with placeholder |
| `synapse/sqlscript/VIEW FINAL.json` | Hardcoded Azure storage account URL | ✅ Replaced with placeholder |
| `synapse/sqlscript/SQLto gold layer.json` | Commented-out credential creation SQL (masked) | ✅ Already uses placeholders |
| `synapse/linkedService/olistdata-synapse-workspace-WorkspaceDefaultSqlServer.json` | Connection string | ✅ Already uses placeholder |
| `factory/olist-ecomm-project-data-factory.json` | Principal ID and Tenant ID | ✅ Already masked with asterisks |
| `notebooks/Databricks Code for Transformation.ipynb` | Storage account, app ID, directory ID, client secret, MongoDB credentials | ✅ Already masked with asterisks |
| `notebooks/DataIngestionToSQL.ipynb` | MySQL credentials | ✅ Uses environment variables |
| `notebooks/DataIngestionToNoSQL.ipynb` | MongoDB credentials | ✅ Uses environment variables |
| `.env.example` | Template credentials | ✅ Contains only placeholders |
