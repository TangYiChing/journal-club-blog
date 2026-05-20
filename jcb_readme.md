# Remove the old (real-names) issue
rm -rf content/issues/2026-05-19

# Step 1: Search for papers
python3 scripts/search_papers.py --topics protac --days 20 --top 8

# Step 2: Aristotle Panel Discussion
Go to Claude.ai, initiate panel-jcb SKILL, and upload candidates.md from step 1.

# Drop in corrected files
cp 2026-05-19-antibody-dialectic.json staging/
cp 2026-05-19-antibody-dialectic.md staging/

# Rebuild and publish
python3 scripts/new_issue.py --date 2026-05-19
npx quartz sync