# Deep into git :)
This repository contains how git works internally

**✅ What Is Git?**
Git is a distributed version control system (VCS) designed to
Track changes in source code
Allow safe collaboration across teams
Manage branches, versions, and the history of a project

**Key Features:**
Distributed: Every developer has a full copy of the project history.
Fast: Most operations are local.
Reliable: Git uses cryptographic hashing (SHA-1) to ensure integrity.
Branching/Merging: Git makes branching easy and cheap.

**2. 🛠️ What Happens When You Install Git?**
On Windows (git.exe)
Installs Git binaries (git.exe, git.cmd, etc.).
Adds Git tools to your system PATH (e.g., git, bash, ssh).
Optionally installs:
Git Bash (a Unix-like shell)
Git GUI (a visual Git client)
Sets up default configuration directories:
C:\Program Files\Git\
C:\Users\<YourName>\.gitconfig (global config)

On Linux (sudo apt install git)
Installs Git to system paths like /usr/bin/git
Makes Git accessible from any terminal

**3. 🚀 What Happens When You Run git init?**
Command:
git init
Effect:
Initializes an empty Git repository in your project.
Creates a hidden .git/ folder that contains everything Git needs to track changes in your project.
Doesn’t track any files until you explicitly add them (git add).

**4. 🗂️ What Is the Folder Structure Inside .git?**
arduino
Copy
Edit
.git/
├── HEAD
├── config
├── description
├── hooks/
├── info/
├── objects/
├── refs/
│   ├── heads/
│   └── tags/

**5. 📄 File-by-File Breakdown of .git**
.git/HEAD: Points to the current branch you’re on:
ref: refs/heads/master: Meaning: "You're on branch master."
.git/config: Stores local repository settings
.git/description: Human-readable repo description.
.git/hooks/: Contains scripts that run at various Git lifecycle events: -> pre-commit, pre-push, post-merge, etc, Sample scripts are provided with .sample extension.
.git/info/: Contains exclude, which works like .gitignore, but is local-only and not version-controlled.
.git/objects/: This is Git's content-addressable object database.Stores all data (blobs, trees, commits, tags).

**Organized using SHA-1 hashes**:
.git/objects/e6/87db1a6a0d3b4878f6d7bc3b61fdb3b88f263a
Git Object Types:
Type	Description
blob	File content
tree	A directory (points to blobs and trees)
commit	A snapshot with metadata (points to a tree)
tag	A named pointer to a commit

Git doesn’t store diffs — it stores complete snapshots indexed by SHA-1.
.git/refs/: Stores references (pointers) to commit objects.
.git/refs/heads/: One file per local branch (e.g., master)Each file contains the SHA-1 of the latest commit on that branch
.git/refs/tags/: Contains references to specific commits tagged with names


**📌 Summary Diagram**
.git/
├── HEAD                → points to current branch (e.g., master)
├── config              → local repository settings
├── description         → repo description (for web tools)
├── hooks/              → event-triggered custom scripts
├── info/exclude        → unversioned ignore rules
├── objects/            → actual Git content database (blobs, trees, commits)
├── refs/heads/         → local branches
└── refs/tags/          → tags

**6. 🔍 How Git Works Internally Under the Hood**
🔑 Content-Addressable Filesystem
Git stores data by content, not by name or location.
Each piece of content is hashed using SHA-1.
Files, directories, commits — all are saved as immutable objects.

**📦 Git Object Types Recap**
Object	Description
blob	Contents of a file
tree	Directory; maps names to blobs/trees
commit	Points to a tree; includes author, message, and parent commits
tag	A human-readable name pointing to a commit

🧬 Example Flow
You create a file:

**echo "Hello Git" > hello.txt
You run:
git add hello.txt**

Internally:
Git creates a blob with the file content.
Computes its SHA-1 hash and saves it in .git/objects/.

**You commit:
git commit -m "First commit**"

Internally:
Git creates a tree for the current directory.
Creates a commit object that points to the tree.
Updates the branch reference (refs/heads/master) with the new commit hash.

**⚙️ Git Internals Visualization**
Working Directory → (git add) → Staging Area (Index)
                     ↓
               Blob Objects (in .git/objects/)
                     ↓
          (git commit) → Tree → Commit → refs

**🧠 SHA-1: The Core of Git**
Git uses SHA-1 to:
Name objects (blobs, trees, commits, tags)
Ensure data integrity
Avoid duplication (same content = same hash)
Even a 1-character change in a file leads to a completely different SHA-1.
