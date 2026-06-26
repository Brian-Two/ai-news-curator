# HABARI: The AI Intelligence Scout

![HABARI CLI Demo](assets/habari_cli_demo.png)

Daily AI/ML intelligence digest for technical founders and builders. Pulls from arXiv and Hacker News, filters for relevance, dedupes, and uses Gemini to summarize technical shifts into practical build opportunities.

## Setup

1. **Install Rust**: Ensure you have the Rust toolchain installed ([rustup.rs](https://rustup.rs)).
2. **Environment**: Create a `.env` file in the root directory:
   ```env
   GEMINI_API_KEY=your-key
   USER_PROFILE=Technical founder, builder, and student focused on AI systems, developer tools, and fast prototypes
   USER_PROFILE_PATH=profile/user_profile.md
   ```
   `USER_PROFILE` is optional, but improves how the Application Radar scores opportunities against your background. For a richer profile, set `USER_PROFILE_PATH` to a local Markdown file; this takes priority over `USER_PROFILE`.

## Usage

```bash
### Running the system
`cargo run`
```

## Automation with GitHub Actions

This project is configured to run automatically every day at 13:00 UTC (08:00 ET) via GitHub Actions. It will:
1.  Fetch and analyze the latest AI news.
2.  Send a premium HTML digest to your email.
3.  Archive raw data and update its internal "seen" database.
4.  Commit and push changes back to the repository.

### Setup GitHub Secrets
To enable automation, go to your repository **Settings > Secrets and variables > Actions** and add:
- `GEMINI_API_KEY`: Your Google Gemini API Key.
- `RESEND_API_KEY`: Your Resend API Key.
- `DESTINATION_EMAIL`: The email address where you want to receive the digest.
- `USER_PROFILE` (optional): A concise profile for Application Radar fit scoring during GitHub Actions runs.

Local `profile/` files are ignored by Git. If you want the scheduled GitHub Action to use the same rich profile, paste a shorter version into the `USER_PROFILE` secret instead of committing personal profile files.

# Build optimized release binary
cargo build --release
./target/release/ai-news-curator-rust

## How it works

1. Fetches latest papers from arXiv (cs.AI, cs.LG, cs.CL, cs.MA)
2. Fetches top HN posts filtered by AI keywords concurrently
3. Dedupes against SQLite store of seen items
4. Gemini summarizes the top items into a scannable digest with an Opportunity Radar for buildable wedges and an Application Radar for apply-to opportunities
5. Marks items as seen to avoid repeats

Data stored in `seen_items.db` (auto-created).
