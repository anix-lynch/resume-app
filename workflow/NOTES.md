# Deployment Notes

## Missing Assets

The `dp.png` image file is referenced in the project but cannot be created programmatically. Please add this file manually:

- Place `dp.png` in the `resume-app/` directory
- This should be a profile/display picture for the resume app

## MCP Tool Integration

The deployment script currently handles local Docker testing and basic Git operations. For full multi-host deployment, the following MCP tools need to be integrated:

### Required MCP Servers
- **GitHub MCP**: For repository management and commits ✅ (configured)
- **Hugging Face Spaces MCP**: For Streamlit app deployment
- **Vercel MCP**: For web hosting
- **Supabase MCP**: For secrets management ✅ (configured)
- **Docker MCP**: Already partially implemented in script

### Tool Calls Needed
See `ORCHESTRATION.md` for detailed MCP tool call mappings.

## Secrets Management Setup

The system now uses **Supabase** for secure credential management:

### Initial Setup
1. **Supabase Project**: Already configured at `https://wupklsodnzlixjtahxah.supabase.co`
2. **Schema Applied**: Run `config/supabase_schema.sql` in Supabase SQL editor
3. **Environment Config**: Copy `config/.env.template` to `.env` and fill credentials
4. **Sync Secrets**: Run `python config/sync_secrets.py` to populate `.env`

### Required Secrets to Add
Add these encrypted secrets to your Supabase `secrets_vault` table:
- `GITHUB_TOKEN`: GitHub Personal Access Token
- `HF_TOKEN`: Hugging Face API Token
- `VERCEL_TOKEN`: Vercel API Token

## Manual Steps Required

1. **GitHub Repository Setup**: Create `anix-lynch/resume-app` repository on GitHub
2. **Hugging Face Space**: Create space `anix-lynch/resume-app` with Streamlit SDK
3. **Vercel Project**: Link existing portfolio project to the repository
4. **GitHub Pages**: Set up docs site at `anix-lynch.github.io`

## Testing the Script

To test the current implementation:

```bash
# First sync secrets from Supabase
python config/sync_secrets.py

# Then deploy
./deploy.sh resume-app
```

This will:
- Build and test the Docker container locally
- Commit and attempt to push changes to GitHub
- Update gate statuses in `workflow/GATES.md`

## Next Development Phase

The script provides a foundation. Future enhancements should include:

1. MCP tool integration for remote deployments (HF Spaces, Vercel)
2. Automated secret loading from Supabase ✅ (implemented)
3. Better error handling and rollback mechanisms
4. Integration with CI/CD pipelines
5. Automated GitHub Pages documentation updates
6. Encrypted secret storage with AES-256 ✅ (implemented)