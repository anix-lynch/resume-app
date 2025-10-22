# Deployment Gates

This document tracks the pass/fail criteria for each deployment gate.

## Gate Status

### Docker Gate
**Status:** PENDING
**Criteria:**
- Docker build succeeds (no cache)
- Container serves `/` with HTTP 200 on `localhost:8501`
- Response within 20 seconds
- Streamlit boot logs show successful startup
**Logs:** N/A

### GitHub Gate
**Status:** PENDING
**Criteria:**
- Repository `anix-lynch/resume-app` exists
- Changes committed to `main` branch
- Commit message follows template: `chore(resume-app): sync assets + bump`
- Push successful
- Optional: Tag created (e.g., `v0.1.0`)
**Commit Hash:** N/A

### HF Space Gate
**Status:** PENDING
**Criteria:**
- Space `anix-lynch/resume-app` exists or created
- SDK set to `streamlit`
- Repository contents pushed
- Build status: SUCCESS
- Space reachable and health check passes
**Space URL:** N/A
**Build Logs:** N/A

### Vercel Gate
**Status:** PENDING
**Criteria:**
- Project linked to repository
- Page exists that embeds Space URL in iframe
- Preview build successful
- Optional: Promoted to production
- Site loads and displays embedded app
**Preview URL:** N/A
**Prod URL:** N/A
**Build Logs:** N/A

### GitHub Pages Gate
**Status:** PENDING
**Criteria:**
- Docs site updated at `https://anix-lynch.github.io/`
- Shows latest commit hash
- Includes Space URL
- Includes Vercel URL
- Includes changelog excerpt from last 10 commits
**Docs URL:** https://anix-lynch.github.io/

## Summary
**Overall Status:** PENDING
**Last Updated:** $(date)
**Next Action:** Run Docker gate