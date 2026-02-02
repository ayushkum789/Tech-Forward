# Getting Started

## Quick Start (5 minutes)

1. **Install Mintlify CLI**

   ```bash
   npm install -g mintlify
   ```

2. **Start Development Server**

   ```bash
   cd Movies-Api-Mintlify
   mintlify dev
   ```

3. **Open Browser**
   Navigate to http://localhost:3000

## Customization

### Update Branding

Edit `mint.json`:

```json
{
  "name": "Your API Name",
  "colors": {
    "primary": "#10b981"
  }
}
```

### Update API Base URL

```json
{
  "api": {
    "baseUrl": "http://localhost:5065"
  }
}
```

## Deployment

### Mintlify Cloud (Recommended)

1. Sign up at https://mintlify.com
2. Connect your Git repository
3. Configure custom domain (optional)
4. Deploy automatically on push

### Alternative Hosting

- **Vercel**: Connect GitHub repo
- **Netlify**: Deploy static site
- **GitHub Pages**: Build and deploy

## Best Practices

- Keep documentation up to date with API changes
- Add examples for all endpoints
- Include error handling documentation
- Test interactive playground regularly
