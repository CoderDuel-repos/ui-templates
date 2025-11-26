# CoderDuel UI Templates Library

**Autonomous, self-improving template system for AI-powered UI generation**

## 📋 Overview

This repository serves as a centralized library of reusable UI templates that power CoderDuel's AI generation pipeline. The system uses intelligent template matching to reduce costs by 72% while maintaining high-quality output.

## 🏗️ Architecture

```
User Request → Strands Agent → Confidence Analysis → Decision Tree
                                                           ↓
                                    ┌──────────────────────┼──────────────────────┐
                                    │                      │                      │
                              HIGH (≥0.85)           MEDIUM (0.60-0.84)      LOW (<0.60)
                                    │                      │                      │
                            Use Templates            Mix Templates          Generate New
                            + Customize              + Generate             (Claude 4.5)
                            (Nova Lite)              (Hybrid)                     ↓
                                    │                      │                Save to Library
                                    └──────────────────────┴──────────────────────┘
                                                           ↓
                                                  Return 6 Variations
```

## 📁 Repository Structure

```
ui-templates/
├── README.md                    # This file
├── templates/                   # Template library
│   ├── landing-page/           # 6 themes × 5 variants = 30 templates
│   │   ├── retro-nostalgic/
│   │   │   ├── variant-001.json
│   │   │   ├── variant-002.json
│   │   │   ├── variant-003.json
│   │   │   ├── variant-004.json
│   │   │   └── variant-005.json
│   │   ├── brutalist/
│   │   ├── organic-natural/
│   │   ├── maximalist-illustration/
│   │   ├── futuristic-tech/
│   │   └── storytelling-interactive/
│   ├── dashboard/              # 6 themes × 5 variants = 30 templates
│   ├── e-commerce/             # 6 themes × 5 variants = 30 templates
│   ├── saas-product/           # 6 themes × 5 variants = 30 templates
│   ├── social-feed/            # 6 themes × 5 variants = 30 templates
│   └── content-blog/           # 6 themes × 5 variants = 30 templates
└── metadata/
    ├── categories.json         # Category definitions and keywords
    ├── theme-mappings.json     # Theme styles and color schemes
    ├── usage-stats.json        # Analytics and cost tracking
    └── quality-scores.json     # User ratings and quality metrics
```

**Total Templates:** 180 (6 categories × 6 themes × 5 variants)

## 🎨 Categories

| Category | Description | Keywords |
|----------|-------------|----------|
| **Landing Page** | Marketing sites, hero sections, CTAs | landing, hero, marketing, product, cta |
| **Dashboard** | Admin panels, analytics, data viz | dashboard, admin, analytics, charts |
| **E-Commerce** | Product listings, carts, checkout | ecommerce, shop, product, cart |
| **SaaS Product** | SaaS apps, tools, productivity | saas, app, tool, workspace |
| **Social Feed** | Social networks, feeds, profiles | social, feed, posts, messaging |
| **Content/Blog** | Blogs, articles, media sites | blog, article, content, news |

## 🎭 Themes

| Theme | Style | Color Palette |
|-------|-------|---------------|
| **Retro & Nostalgic** | Y2K, 90s vibes, neon gradients | Pink, Purple, Neon |
| **Brutalist** | Raw, bold, minimal decoration | Black, Red, White |
| **Organic & Natural** | Earth tones, flowing shapes | Brown, Beige, Cream |
| **Maximalist Illustration** | Rich illustrations, vibrant | Red, Teal, Yellow |
| **Futuristic Tech** | Sleek, neon, dark themes | Cyan, Purple, Dark |
| **Storytelling Interactive** | Narrative-driven, immersive | Navy, Coral, Peach |

## 📄 Template Format

Each template is a JSON file with the following structure:

```json
{
  "id": "landing-page-retro-nostalgic-v001",
  "category": "landing-page",
  "theme": "Retro & Nostalgic",
  "variant": "001",
  "description": "Y2K hero with gradient buttons and pixel art accents",
  "keywords": ["landing", "hero", "marketing", "retro", "y2k", "gradient"],
  "placeholders": {
    "{{HERO_TITLE}}": "Welcome to Our Platform",
    "{{HERO_SUBTITLE}}": "Build amazing things together",
    "{{CTA_PRIMARY}}": "Get Started",
    "{{CTA_SECONDARY}}": "Learn More",
    "{{FEATURE_1_TITLE}}": "Fast & Reliable",
    "{{FEATURE_1_DESC}}": "Lightning-fast performance"
  },
  "code": "const HeroSection = () => { ... }",
  "generatedBy": "claude-sonnet-4-5",
  "createdAt": "2025-11-25T22:00:00Z",
  "usageCount": 0,
  "qualityScore": 0.95,
  "userRatings": []
}
```

## 🔄 Workflow

### 1. **User Generates UI**
- User enters app description in frontend
- Request sent to Strands orchestrator Lambda

### 2. **Intent Analysis**
- Nova Pro analyzes user prompt
- Determines category, keywords, confidence score

### 3. **Decision Tree**
- **HIGH confidence (≥0.85)**: Use 6 templates, customize with Nova Lite
- **MEDIUM confidence (0.60-0.84)**: Use 3 templates + generate 3 new
- **LOW confidence (<0.60)**: Generate all 6 new with Claude 4.5

### 4. **Template Customization**
- Nova Lite extracts values from user prompt
- Replaces placeholders in template code
- Returns customized variations

### 5. **Auto-Population**
- New generations saved to library
- Metadata updated with usage stats
- Quality scores tracked from user feedback

## 💰 Cost Analysis

### Current System (All Claude 4.5)
- 1,000 generations/month × 6 variations × 60,000 tokens
- **Cost:** $180/month

### New System (Template-First)
| Confidence | Traffic % | Strategy | Cost/Gen | Monthly |
|-----------|-----------|----------|----------|---------|
| HIGH (≥0.85) | 60% | Templates (Nova Lite) | $0.002 | $1.20 |
| MEDIUM | 25% | 50/50 Mix | $0.09 | $22.50 |
| LOW (<0.60) | 15% | All Claude 4.5 | $0.18 | $27.00 |

**Total:** $50.70/month  
**Savings:** $129.30/month (72%)

*As library grows, HIGH confidence % increases → costs decrease further*

## 📊 Metrics Tracked

- **Cache Hit Rate**: % of requests using templates
- **Confidence Distribution**: HIGH/MEDIUM/LOW breakdown
- **Cost Per Generation**: Average cost across all requests
- **Quality Scores**: User ratings per template
- **Usage Frequency**: Most/least used templates
- **Category Performance**: Success rate by category

## 🛠️ Lambda Functions

### 1. **template-generator**
- Contains Claude 4.5 generation logic
- Moved from local Next.js API
- Registered as Strands tool

### 2. **strands-orchestrator**
- Main decision-making agent
- Analyzes intent with Nova Pro
- Routes to appropriate strategy
- Invokes other Lambdas as needed

### 3. **template-customizer**
- Customizes templates with Nova Lite
- Replaces placeholders
- Returns ready-to-use code

### 4. **template-saver**
- Saves new generations to GitHub
- Updates metadata files
- Tracks usage statistics

## 🔐 Access Control

- **Repository:** Private (CoderDuel-repos/ui-templates)
- **GitHub Token:** Required for Lambda access
- **Permissions:** Read/Write for Lambdas, Read-only for frontend

## 🚀 Deployment

Templates are automatically deployed via GitHub Actions when:
- New templates are generated
- Metadata is updated
- Quality scores change

## 📈 Future Enhancements

- [ ] Semantic search using embeddings
- [ ] A/B testing for template variants
- [ ] Automatic quality scoring via AI
- [ ] Template versioning system
- [ ] Multi-language support
- [ ] Component-level templates (not just full pages)

## 🤝 Contributing

Templates are auto-generated by the AI pipeline. Manual contributions should follow the template format and be added via PR.

---

**Last Updated:** 2025-11-25  
**Total Templates:** 0 (growing automatically)  
**Maintained by:** CoderDuel AI Pipeline
