# AI Agency Lead Gen — n8n Automation

An end-to-end Twitter/X outreach automation system built for an AI agency internship project.
Scrapes targeted leads, filters them by quality, logs to Google Sheets, and sends 
AI-personalised DMs to follow-backs.

## What It Does
- Scrapes Twitter/X users by keyword (AI agency, marketing) via RapidAPI
- Filters leads: bio keywords + followers > 50 + not protected
- Logs 1,000+ leads to Google Sheets with username, bio, location, followers
- Auto-follows filtered leads with rate-limit safe delays
- Detects follow-backs via polling every 2 hours
- Sends AI-generated personalised DMs via OpenAI GPT-4o-mini
- Multi-step follow-up sequence for no-reply leads
- Daily report to Google Sheets

## Tech Stack
- n8n (workflow automation)
- RapidAPI — twitter241 by davethebeast (scraping)
- Twitter API v1.1 OAuth 1.0a (follow, DM)
- Google Sheets (lead database)
- OpenAI GPT-4o-mini (DM generation)

## Workflows
| File | Purpose |
|---|---|
| `lead-scraper.json` | Scrapes and filters leads into Google Sheets |
| `auto-follow.json` | Follows filtered leads with delays |
| `dm-on-followback.json` | Detects follow-backs and sends AI DMs |

## Setup
1. Import workflow JSONs into your n8n instance
2. Add credentials: RapidAPI key, Twitter OAuth1.0a, Google Sheets OAuth2, OpenAI API key
3. Set your Google Sheet name to `Leads_X` with columns:
   `username, name, bio, followers, location, verified, protected, followed, followed_at, dm_sent, dm_sent_at, timestamp`
4. Run `lead-scraper` first to populate leads
5. Then activate `auto-follow` and `dm-on-followback`

## Known Limitations
- Auto-follow via RapidAPI requires paid plan ($59/mo) — currently using Twitter v1.1 which requires Basic tier
- Instagram scraping dropped due to residential proxy costs
- OpenAI free tier limits DMs to ~5 per run

## Status
- [x] Lead scraping — 1,004 leads collected
- [x] Google Sheets logging
- [x] Auto-follow workflow (built, API tier limitation)
- [ ] DM on follow-back (in progress)
- [ ] Multi-step follow-up
- [ ] Daily report
- [ ] Event detection


## Future Improvements

With more time and a paid budget, the following improvements would significantly 
enhance the system:

**API Upgrades**
- Upgrade RapidAPI twitter241 to Pro plan ($59/mo) to unlock Follow and DM 
  endpoints — removes dependency on Twitter's restrictive free tier
- Allocate budget for Apify residential proxies to re-enable Instagram scraping, 
  doubling the lead pool

**Smarter Lead Scoring**
- Add an engagement rate filter (likes+replies/followers) instead of just follower 
  count — better quality signal
- Use OpenAI to score leads 1-10 based on bio relevance before adding to sheet

**DM Quality**
- Fine-tune GPT-4o-mini on high-converting outreach messages for better reply rates
- A/B test multiple DM variants and track which converts better in the daily report

**Infrastructure**
- Move from n8n cloud to self-hosted n8n to eliminate workflow execution limits
- Add a proper database (Supabase/PostgreSQL) instead of Google Sheets for 
  faster querying at scale

**Event Detection**
- Implement webhook-based triggers instead of polling to detect follow-backs 
  and replies in real-time rather than every 2 hours
