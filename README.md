# HubSpot Growth Engine

A project to earn HubSpot affiliate commissions through SEO content,
then automate content production using AWS cloud infrastructure.

**Author:** Valentine Sidi  
**Status:** Phase 1 — Content validation (live)  
**Stack (Phase 2+):** Python, AWS Lambda, Amazon Bedrock, DynamoDB, EventBridge, CloudWatch

---

## Project phases

### Phase 1 — Validate (current)
Publish manually written, SEO-focused blog posts to a GitHub Pages site.
Prove that organic search traffic converts to affiliate clicks before
building any automation.

- [x] GitHub Pages site live at valentinesidi.github.io
- [x] First post published: Best Free CRM for Small Business in 2026
- [x] FTC disclosure on all posts
- [x] HubSpot affiliate application submitted
- [ ] Google Search Console — submit site for indexing
- [ ] First affiliate click tracked

### Phase 2 — Semi-automate (upcoming)
A single Python script (runs locally, not in the cloud yet) that:
1. Takes a topic as input
2. Calls Amazon Bedrock (Claude Haiku) to generate a draft post
3. Pushes the draft to a staging GitHub repo via the GitHub API
4. Human reviews and approves before it goes to the live site

Why local first: validate AI output quality before paying for
cloud infrastructure. Catch problems cheaply.

### Phase 3 — Full AWS pipeline (future)
Once Phase 2 produces consistently good content and earns commissions,
automate the trigger and deploy everything to Lambda.

Full stack:
- EventBridge scheduled rule (daily trigger)
- AWS Lambda Python orchestrator
- Amazon Bedrock Claude Haiku (content generation)
- DynamoDB (published topics, deduplication, status tracking)
- SSM Parameter Store (secrets, never in code)
- CloudWatch (structured logging, failure alarms)
- GitHub API (publishes to GitHub Pages)

---

## Security principles (applied from day one)

- No credentials in code ever. .env in .gitignore, secrets in SSM for Lambda
- git-secrets installed to block accidental credential commits
- IAM least privilege — Lambda role scoped to specific resource ARNs only
- Fine-grained GitHub PAT scoped to one repo, Contents: Write only
- FTC disclosure hard-coded at infrastructure level, not left to AI output

---

## Folder structure

```
hubspot-growth-engine/
├── README.md
├── scripts/
│   └── generate_post.py      # Phase 2: local Bedrock + GitHub script
├── lambda/
│   └── handler.py            # Phase 3: Lambda function
├── prompts/
│   └── crm_post_template.txt # Bedrock prompt template
└── infra/
    └── iam_policy.json       # Least-privilege IAM policy
```

---

## Build checklist

- [x] Phase 1: live content site with first post
- [ ] Phase 2: local Python script — Bedrock to staging GitHub repo
- [ ] Phase 2: human review step before production publish
- [ ] Phase 3: Lambda + EventBridge scheduled trigger
- [ ] Phase 3: DynamoDB topic tracking with conditional writes
- [ ] Phase 3: SSM Parameter Store for all secrets
- [ ] Phase 3: CloudWatch structured logging and failure alarm
- [ ] Phase 3: DynamoDB Point-in-Time Recovery enabled
