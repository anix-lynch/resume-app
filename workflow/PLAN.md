# Deployment Plan

This document outlines the step-by-step plan for deploying the resume-app to multiple platforms.

## Overview

The deployment process follows a gate-based approach with the following stages:

1. **Local Docker Test** - Build and test the app locally
2. **GitHub Commit & Push** - Update the repository
3. **Hugging Face Spaces Deploy** - Deploy to HF Spaces
4. **Vercel Deploy** - Deploy to Vercel
5. **GitHub Pages Update** - Update documentation site

## Detailed Steps

### 1. Docker Gate
- Build Docker image from Dockerfile
- Run container locally
- Health check: HTTP 200 on localhost:8501 within 20s
- Verify Streamlit boot logs
- Clean up containers/images on success

### 2. GitHub Gate
- Ensure repo `anix-lynch/resume-app` exists
- Commit changes with message: `chore(resume-app): sync assets + bump`
- Push to main branch
- Optional: Create tag (e.g., `v0.1.0`)

### 3. HF Space Gate
- Create/verify Space: `anix-lynch/resume-app`
- Set SDK to `streamlit`
- Push repository contents
- Set `HF_TOKEN` secret if needed
- Poll build status until SUCCESS
- Capture Space URL

### 4. Vercel Gate
- Link project to repo if not already linked
- Ensure page exists that embeds Space URL
- Set environment variables
- Trigger preview deploy
- Optional: Promote to production
- Capture preview/prod URLs

### 5. GitHub Pages Gate
- Update docs site at `https://anix-lynch.github.io/`
- Include latest commit hash
- Include Space URL
- Include Vercel URL
- Include changelog from last 10 commits

## Success Criteria

- All gates pass without failure
- App is accessible on all three platforms
- Links are captured and documented
- GitHub comment posted with deployment summary