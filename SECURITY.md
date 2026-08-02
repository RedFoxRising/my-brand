# Security

This repository builds a static site. There is no backend, no database, no user
accounts, and no form that submits anywhere — so the usual web vulnerability
classes mostly don't apply.

What's still worth reporting:

- Anything that lets someone alter what the deployed site serves
- A supply-chain problem in the build (the GitHub Actions workflow, the Hugo
  version pinned in it, or the PaperMod submodule)
- Exposed credentials or personal data I published by mistake

**How to report:** email am.guerreroa@uniandes.edu.co. Please don't open a
public issue for something that could be abused before it's fixed.

I maintain this in my spare time, so I can't promise a response window — but I
do read the inbox.
