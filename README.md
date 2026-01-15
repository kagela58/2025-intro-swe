# 2025 Introduction to Software Engineering

Welcome to the Introduction to Software Engineering course repository! This repository contains lab materials, student submissions, and course resources.

## 🚀 Getting Started

### New Students
1. **Fork this repository** to your GitHub account
2. Follow the instructions in [labs/lab1.md](labs/lab1.md) or [labs/lab2.md](labs/lab2.md)
3. Create your personal folder in `students/YOUR_USERNAME/`
4. Complete the lab assignments

### ⚠️ Having Trouble Syncing Your Fork?

If you see errors when trying to sync your fork or warnings about losing commits:

**→ See the [Fork Sync Guide](FORK_SYNC_GUIDE.md)** for step-by-step instructions in English and Croatian.

## 📚 Course Materials

### Lab Assignments
- [Lab 0](labs/lab0.md) - Development Environment Setup
- [Lab 1](labs/lab1.md) - Git, GitHub, CLI, Dev Containers (Croatian)
- [Lab 2](labs/lab2.md) - Fork, Branch, and Pull Request
- [Lab 3](labs/lab3.md) - Fork → Codespaces → PR (with Sync + Conflicts)
- [Lab 4](labs/lab4.md) - Project Work

### Important Documentation
- **[Fork Sync Guide](FORK_SYNC_GUIDE.md)** - How to safely sync your fork without losing data
- [Contributing Guidelines](CONTRIBUTING.md) - How to contribute to this repository
- [Review Guidelines](REVIEW_GUIDELINES.md) - Guidelines for peer code review
- [Development Container Setup](README_devcontainer.md) - Using Codespaces and Dev Containers

## 📁 Repository Structure

```
2025-intro-swe/
├── labs/                   # Lab documentation and materials
│   ├── lab0.md            # Development environment setup
│   ├── lab1.md            # Git and GitHub basics (Croatian)
│   ├── lab2.md            # GitHub workflow (fork, branch, PR)
│   └── lab3.md            # Codespaces and sync conflicts
├── students/              # Student submissions
│   └── [username]/        # Individual student folders
│       └── lab1/
│           └── intro.py   # Student's Python submission
├── projects/              # Student projects
├── scripts/               # Utility scripts
├── templates/             # Code templates
├── FORK_SYNC_GUIDE.md     # ⭐ Comprehensive fork sync guide
├── CONTRIBUTING.md        # Contribution guidelines
└── requirements.txt       # Python dependencies
```

## 🔧 Development Environment

This repository supports two development approaches:

### GitHub Codespaces (Recommended)
- Click "Code" → "Codespaces" → "Create codespace on main"
- Fully configured cloud development environment
- No local setup required

### Local Development
- Requires Python 3.11+
- Docker Desktop (optional)
- See [Lab 0](labs/lab0.md) for detailed setup instructions

## 🆘 Common Issues

### Can't Sync Fork
**Problem:** GitHub shows "This branch is X commits behind" but sync fails or warns about losing commits.

**Solution:** See [Fork Sync Guide](FORK_SYNC_GUIDE.md) - includes 4 different methods with step-by-step instructions.

### Push Rejected / Conflicts
**Problem:** `git push` fails with "non-fast-forward" error.

**Solution:** See [Fork Sync Guide - Method 1](FORK_SYNC_GUIDE.md#method-1-safe-sync-with-uncommitted-changes-recommended)

### Codespace Won't Start
**Problem:** Codespace is stuck or showing errors.

**Solution:** Delete the Codespace and create a new one. Your work is safe in the Git repository.

## 📖 Learning Resources

### Git and GitHub
- [Fork Sync Guide](FORK_SYNC_GUIDE.md) - Our comprehensive guide
- [GitHub Docs: Forking](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)
- [Git Documentation](https://git-scm.com/doc)

### Python
- [Python Official Tutorial](https://docs.python.org/3/tutorial/)
- Course materials in `labs/` directory

## 🤝 Contributing

Students should:
1. Work only in your own folder: `students/YOUR_USERNAME/`
2. Follow the lab instructions carefully
3. Use clear commit messages
4. Submit pull requests for review

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## 📞 Getting Help

1. **Read the documentation** - Start with the relevant lab guide
2. **Check the [Fork Sync Guide](FORK_SYNC_GUIDE.md)** - If you have fork/sync issues
3. **Search for your error** - Copy the error message and search online
4. **Ask your AI assistant** - Claude, ChatGPT, etc. can help debug
5. **Course forum** - Post questions with error messages and screenshots
6. **Office hours** - Attend instructor/TA office hours

## 🎓 Academic Integrity

- All submitted work must be your own
- Properly cite any external sources
- AI assistance is allowed but you must understand the code
- Follow your institution's academic integrity policies

## 📝 License

This repository is licensed under the Apache License 2.0. See [LICENSE](LICENSE) for details.

---

**Course Repository:** `nibzard/2025-intro-swe`  
**Academic Year:** 2024-2025

For questions or issues, please use the course forum or contact your instructor.
