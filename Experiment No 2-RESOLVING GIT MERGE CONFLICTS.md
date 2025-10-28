### **AIM:**

To simulate and resolve merge conflicts in Git during a collaborative workflow, demonstrating conflict resolution techniques.
### **PROCEDURE:**
	Prerequisites
		Install Git on your system.
		Create or clone a Git repository.
		Ensure at least two branches (main and feature) exist.
		Setup and Simulate a Merge Conflict
		Initialize a Git Repository
### 🪜 Step 1 — Create the Local Repository

```bash
mkdir merge-conflict-demo
cd merge-conflict-demo
git init
```

---

### 🪜 Step 2 — Create an initial file and commit

```bash
echo "Initial content" > file.txt
git add file.txt
git commit -m "Initial commit"
```

---

### 🪜 Step 3 — Create and switch to `feature` branch

```bash
git checkout -b feature
```

Make a **change on the feature branch**:

```bash
echo "Change from feature branch" > file.txt
git add file.txt
git commit -m "Update from feature branch"
```

---

### 🪜 Step 4 — Go back to `main` and make a different change

Now switch back:

```bash
git checkout main
```

Change the same file **differently**:

```bash
echo "Change from main branch" > file.txt
git add file.txt
git commit -m "Update from main branch"
```

---

### 🪜 Step 5 — Try merging (Conflict guaranteed 🚨)

```bash
git merge feature
```

Now you’ll see:

```
Auto-merging file.txt
CONFLICT (content): Merge conflict in file.txt
Automatic merge failed; fix conflicts and then commit the result.
```

---

### ✅ Step 6 — Resolve it

Open `file.txt`, and you’ll see this:

```plaintext
<<<<<<< HEAD
Change from main branch
=======
Change from feature branch
>>>>>>> feature
```

Manually fix it:

```plaintext
Change from both branches
```

Then mark as resolved:

```bash
git add file.txt
git commit -m "Resolved merge conflict"
```

### OUTPUT:

Before Resolving Conflict:

```
Auto-merging file.txt
CONFLICT (content): Merge conflict in file.txt
Automatic merge failed; fix conflicts and then commit the result.
```

After Resolving Conflict:

```
Resolved merge conflict
```

### RESULT:
		Thus, the merge conflict was successfully simulated and resolved in Git, 
	ensuringthe repository reflects the desired state of the file.txt content

---

### 🪄 Bonus: Prevent Fast-Forward Merge (Optional)

If you *want Git to always show a merge* (even if no conflict exists), use:

```bash
git merge --no-ff feature
```

**Explanation:**

* `--no-ff` = “no fast-forward”
* Forces Git to create a **merge commit**, even when branches can be merged linearly.

---

