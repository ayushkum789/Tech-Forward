# Quick Reference

## Mintlify Components

### Tips and Warnings

```markdown
<Tip>Helpful tip for users</Tip>

<Warning>Important warning message</Warning>

<Info>Additional information</Info>
```

### Cards

```markdown
<Card title="Feature Name" icon="icon-name" href="/link">
  Description text
</Card>
```

### Code Groups

```markdown
<CodeGroup>

\`\`\`javascript
// JavaScript example
\`\`\`

\`\`\`csharp
// C# example
\`\`\`

</CodeGroup>
```

### API Parameters

```markdown
<ParamField body="fieldName" type="string" required>
  Field description
</ParamField>
```

## Common CLI Commands

```bash
# Start dev server
mintlify dev

# Validate documentation
mintlify validate

# Check for broken links
mintlify broken-links

# Update CLI
mintlify update
```

## Configuration Reference

### mint.json Structure

```json
{
  "name": "Site Name",
  "api": {
    "baseUrl": "http://localhost:5065"
  },
  "navigation": [
    {
      "group": "Group Name",
      "pages": ["page1", "page2"]
    }
  ]
}
```
