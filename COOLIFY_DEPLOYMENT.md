# E2B Deployment Guide for Coolify

This guide will help you deploy the E2B web application to Coolify.

## Prerequisites

- A Coolify instance up and running
- GitHub account with the forked repository
- Required API keys (if using external services)

## Repository Setup

Your forked repository is available at: https://github.com/miguelsuredasuau/E2B

## Deployment Steps

### 1. Create New Application in Coolify

1. Log in to your Coolify dashboard
2. Click "New Resource" → "Application"
3. Select your deployment server

### 2. Configure Source

1. Choose "GitHub" as the source
2. Connect your GitHub account if not already connected
3. Select the repository: `miguelsuredasuau/E2B`
4. Choose the branch: `main` (or your preferred branch)

### 3. Build Configuration

Configure the following build settings:

- **Build Pack**: Docker
- **Dockerfile Location**: `apps/web/Dockerfile`
- **Docker Context**: `/` (root of repository)
- **Port**: 3000

### 4. Environment Variables

Add the following environment variables in Coolify:

#### Required Variables:
```env
NODE_ENV=production
NEXT_TELEMETRY_DISABLED=1
PORT=3000
HOSTNAME=0.0.0.0
```

#### Optional Variables (configure as needed):
```env
# Your domain configuration
PRODUCTION_URL=https://your-domain.com
NEXT_PUBLIC_BASE_URL=https://your-domain.com

# Supabase (if using authentication)
NEXT_PUBLIC_SUPABASE_URL=your-supabase-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# Sentry (for error tracking)
SENTRY_AUTH_TOKEN=your-sentry-token
NEXT_PUBLIC_SENTRY_DSN=your-sentry-dsn

# E2B API
E2B_API_KEY=your-e2b-api-key

# Mux (for video features)
MUX_TOKEN_ID=your-mux-token-id
MUX_TOKEN_SECRET=your-mux-token-secret
```

### 5. Advanced Settings

1. **Health Check Path**: `/` (or create a custom `/api/health` endpoint)
2. **Resource Limits**: 
   - Minimum: 1 CPU, 1GB RAM
   - Recommended: 2 CPU, 2GB RAM
3. **Auto Deploy**: Enable for automatic deployments on push

### 6. Docker Compose Alternative

If you prefer using Docker Compose:

1. Select "Docker Compose" as the build pack
2. Set the compose file path to: `docker-compose.yml`
3. Configure the same environment variables

### 7. Deploy

1. Click "Deploy" to start the deployment
2. Monitor the build logs for any issues
3. Once deployed, access your application at the provided URL

## Post-Deployment

### Verify Deployment

1. Check that the application loads correctly
2. Test key features:
   - Documentation pages load
   - Search functionality works
   - Any configured integrations are functional

### Monitoring

- Check Coolify logs for any runtime errors
- Monitor resource usage in Coolify dashboard
- Set up alerts for downtime if needed

## Troubleshooting

### Common Issues

1. **Build Failures**
   - Check that all required environment variables are set
   - Verify the Dockerfile path is correct
   - Check build logs for specific error messages

2. **Application Not Starting**
   - Verify PORT is set to 3000
   - Check that HOSTNAME is set to 0.0.0.0
   - Review application logs in Coolify

3. **Missing Dependencies**
   - The Dockerfile uses pnpm for package management
   - Ensure all workspace dependencies are properly installed

### Support

For E2B specific issues:
- Check the original repository: https://github.com/e2b-dev/e2b
- Review E2B documentation: https://e2b.dev/docs

For Coolify deployment issues:
- Consult Coolify documentation
- Check Coolify community forums

## Updates

To update your deployment:

1. Pull latest changes from upstream:
   ```bash
   git remote add upstream https://github.com/e2b-dev/e2b.git
   git fetch upstream
   git merge upstream/main
   git push origin main
   ```

2. If auto-deploy is enabled, Coolify will automatically redeploy
3. Otherwise, manually trigger a redeploy in Coolify dashboard

## Security Notes

- Keep all API keys and secrets secure
- Use Coolify's secret management for sensitive values
- Regularly update dependencies for security patches
- Enable HTTPS for production deployments