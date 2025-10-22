# MCP Orchestration

This document maps each deployment gate to the corresponding MCP tool calls and commands.

## MCP Servers Used

- **GitHub MCP**: Repository management, commits, releases
- **Docker MCP**: Local container testing
- **Hugging Face Spaces MCP**: Streamlit app deployment
- **Vercel MCP**: Web hosting and deployment
- **Filesystem MCP**: File operations

## Gate-to-MCP Mapping

### Docker Gate
```
docker.build(image_name="resume-app:latest", dockerfile="Dockerfile", context=".")
docker.run(image_name="resume-app:latest", ports={"8501": "8501"}, detach=true)
docker.healthcheck(container_id, endpoint="/", port=8501, timeout=20)
docker.logs(container_id, follow=false)
docker.rm(container_id)
```

### GitHub Gate
```
github.create_or_update_repo(owner="anix-lynch", repo="resume-app", private=false)
github.commit_push(owner="anix-lynch", repo="resume-app", branch="main",
                   message="chore(resume-app): sync assets + bump", files=["."])
github.create_tag(owner="anix-lynch", repo="resume-app", tag="v0.1.0", sha=commit_sha)
```

### HF Space Gate
```
hf.space.create_or_update(owner="anix-lynch", space="resume-app", sdk="streamlit")
hf.space.push(owner="anix-lynch", space="resume-app", local_path=".")
hf.space.set_secret(owner="anix-lynch", space="resume-app", key="HF_TOKEN", value=token)
hf.space.status(owner="anix-lynch", space="resume-app")  # poll until SUCCESS
hf.space.logs(owner="anix-lynch", space="resume-app")
```

### Vercel Gate
```
vercel.project.link(owner="anix-lynch", repo="resume-app", project_name="resume-app")
vercel.env.set(project_id, key="HF_SPACE_URL", value=space_url)
vercel.deploy(project_id, production=false)  # preview deploy
vercel.logs(deployment_id)
vercel.deploy(project_id, production=true)  # optional prod promote
```

### GitHub Pages Gate
```
# Manual or scripted: Update docs/index.html with latest URLs
github.create_or_update_file(owner="anix-lynch", repo="anix-lynch.github.io",
                            path="docs/resume-app.html", content=html_content)
```

## Orchestration Flow

1. **Prep Phase**
   - Load secrets from global `.env` or Airtable
   - Register MCP servers
   - Validate all required tokens present

2. **Sequential Gate Execution**
   - Docker → GitHub → HF Space → Vercel → GitHub Pages
   - Stop on first failure
   - Log all outputs to `workflow/GATES.md`

3. **Reporting Phase**
   - Aggregate URLs into `workflow/URLS.md`
   - Post GitHub comment with summary
   - Update gate statuses

## Error Handling

- Each gate failure should surface specific logs
- Cleanup operations for failed deployments
- Retry logic for transient failures (network timeouts, etc.)
- Rollback options for critical failures

## Tool Dependencies

- Docker MCP: `docker` CLI available
- GitHub MCP: `GITHUB_TOKEN` set
- HF Spaces MCP: `HF_TOKEN` set
- Vercel MCP: `VERCEL_TOKEN` set
- All MCP servers properly configured in `.mcp.json`