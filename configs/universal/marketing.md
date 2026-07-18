# Marketing Guidance

When and how to position a project for commercial distribution.
Read this before starting marketing work on any project.

---

## When to Create a MARKETING.md

Create a MARKETING.md in the project root when the project has a **commercial viability signal**:

- Headed toward App Store or Google Play Store distribution
- Will have paid features or a subscription model
- Targeting a real audience beyond personal use

**Do NOT create a MARKETING.md for:**
- Internal tools and personal utilities
- CLI tools and developer scripts
- Hobby or experimental projects with no distribution intent

### Examples

| Project | MARKETING.md? | Reason |
|---------|---------------|--------|
| Mise | Yes | App Store bound, grocery ordering, real consumer audience |
| Kagami | Yes | Mobile app, public distribution planned |
| kiban | No | Personal framework, no external distribution |
| HakoBox | No | Hobby hardware project |

## Naming Is Step One

Before writing a MARKETING.md, the project needs a name. Follow the process
in [NAMING-GUIDE.md](../../NAMING-GUIDE.md) -- meaning-first ideation, cultural
checks, availability sweep, connotation check.

## Positioning Principles

- **Lead with the user problem, not the technology.** "Never forget an ingredient" beats "AI-powered recipe parsing."
- **One sentence that a stranger understands.** If you need jargon to explain it, rethink the angle.
- **Differentiate on the workflow, not the feature list.** What does the user's life look like AFTER using this?
- **Brand voice should feel like a person, not a company.** Confident, warm, direct.

---

## MARKETING.md Template

Copy this template into the root of any project that meets the commercial trigger above.
Fill in each section. Delete the bracketed guidance text.

````markdown
# [Project Name] -- Marketing Strategy

## Target Audience

[Who is this for? Be specific -- demographics, psychographics, pain points.
"Everyone" is not an audience.]

## Core Value Proposition

[1-2 sentences. What does this do for the user? Lead with the outcome, not the technology.]

## Competitive Landscape

[Who else solves this problem? What do they do well? Where do they fall short?
This is not a feature comparison -- it is understanding the market.]

## Differentiation

[What makes this different from the competition? Focus on the experience, not features.
"We use AI" is not a differentiator in 2026.]

## Brand Voice

[Tone, personality, key phrases. How should all communications sound?]

## Tagline

[One memorable line. Test it: would a stranger understand what the product does?]

## Distribution Channels

[Where will users find this? App Store, Play Store, web, social media, communities.]

## Channel Strategy

[How will you reach your audience? Content, partnerships, communities, paid acquisition.
Be specific -- "social media" is not a strategy.]
````
