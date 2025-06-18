# GitHub Pages Deployment

This repository uses GitHub Actions to automatically build and deploy the React application to GitHub Pages.

## Workflow Overview

The deployment workflow (`.github/workflows/deploy.yml`) performs the following steps:

1. **Checkout**: Retrieves the repository code including submodules
2. **Setup Node.js**: Installs Node.js 18 with npm caching
3. **Install Dependencies**: Runs `npm ci` in the React project directory
4. **Environment Setup**: Creates `.env` file with secrets
5. **Build**: Runs `npm run build` to create production build
6. **Deploy**: Copies build output to root and deploys to GitHub Pages

## Required GitHub Secrets

Configure the following secrets in your GitHub repository settings (`Settings > Secrets and variables > Actions`):

### Required Secrets

- `REACT_APP_API_URL` - Base URL for your API endpoints
- `REACT_APP_CHAT_API_URL` - URL for the chat/messaging API
- `REACT_APP_GITHUB_TOKEN` - GitHub token for API access
- `REACT_APP_GOOGLE_CLIENT_ID` - Google OAuth client ID for authentication

### Optional Secrets

- `CUSTOM_DOMAIN` - Your custom domain (e.g., `example.com`) - only needed if using a custom domain

## GitHub Pages Setup

1. Go to repository `Settings > Pages`
2. Set **Source** to "GitHub Actions"
3. The workflow will automatically deploy on pushes to `main` or `master` branch

## Project Structure

```
zhao-xuan.github.io/
├── .github/
│   └── workflows/
│       └── deploy.yml          # Deployment workflow
├── zhao-xuan.github.io.dev/
│   └── react-homepage/         # React source code
│       ├── src/
│       ├── public/
│       ├── package.json
│       └── ...
├── index.html                  # Built output (generated)
├── static/                     # Built assets (generated)
├── CNAME                       # Custom domain config
└── ...                        # Other built files
```

## Manual Deployment

You can manually trigger a deployment by:

1. Going to the **Actions** tab in your GitHub repository
2. Selecting the "Build and Deploy to GitHub Pages" workflow
3. Clicking "Run workflow"

## Troubleshooting

### Build Failures

- Check that all required secrets are properly configured
- Ensure the React project builds successfully locally with `npm run build`
- Review the Actions logs for specific error messages

### Deployment Issues

- Verify GitHub Pages is enabled in repository settings
- Check that the workflow has proper permissions (should be automatic)
- Ensure the `main` or `master` branch is the default branch

### Environment Variables

- All secrets starting with `REACT_APP_` will be embedded in the client-side build
- Never put sensitive server-side secrets in `REACT_APP_` variables
- The `.env` file is created during the build process and not committed to the repository

## Development Workflow

1. Make changes in `zhao-xuan.github.io.dev/react-homepage/`
2. Test locally with `npm start`
3. Commit and push to trigger automatic deployment
4. Monitor the deployment in the Actions tab

## Security Notes

- GitHub Secrets are encrypted and only accessible during workflow execution
- The built application will contain any `REACT_APP_` environment variables
- Ensure no sensitive data is included in client-side environment variables 