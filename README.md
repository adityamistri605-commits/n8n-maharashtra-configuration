# Enterprise n8n Cloud Lead Generation Architecture

## Project Overview
This project processes 3,874 search queries combining 149 cities and 26 categories. It is **100% cloud-native**—designed specifically for modern n8n Cloud deployments, requiring zero local filesystem interactions.

## Cloud Architecture Highlights
1. **Dynamic Config Loading:** Reads JSON parameters securely from GitHub/Web endpoints instead of hardcoded nodes.
2. **State Checkpointing:** Leverages Google Sheets as a Key-Value Datastore (`Checkpoints` sheet) to record processed/failed `SearchIDs`. If n8n restarts, the workflow skips completed queries and seamlessly resumes.
3. **Queue Polling & Auto-Scale:** Utilizes the new `Loop` node logic with `Wait` states. HTTP Request nodes implement `retryOnFail: true` with automatic Exponential Backoff handling BrightData 429/50x limits natively.
4. **Cloud Exports:** Routes formatted data to Google Sheets appending instantly, or formats to `.xlsx`/`.csv` in memory and pushes securely to a specified Google Drive folder.

## Setup Instructions
1. Upload the provided JSON files (`workflow_constants.json`, `search_parameters.json`, etc.) to a secure web host or GitHub repo.
2. Set your environment variables (see `.env.example`). 
3. In Google Sheets, create three sheets named: **Leads**, **Checkpoints**, and **Logs**.
4. Import `workflow.json` into n8n.
5. Authenticate the Google nodes with your OAuth2 credentials.
6. Click Execute.