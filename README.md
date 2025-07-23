# Podcast AI
An intelligent podcast generation system that fetches original tweets from X (Twitter) and transforms them into AI-generated podcast content. This system processes social media content to create engaging audio experiences.

# 🎯 Description
Podcast AI is a Rust-based backend service that:

Fetches original tweets from any public X (Twitter) user
Filters content to exclude replies, retweets, and quote tweets
Processes text for AI consumption by removing URLs and noise
Provides clean API endpoints for podcast generation workflows
Optimizes for AI models with structured, context-ready text output
Integrates with Alchemyst context processor for automated podcast generation
The system serves as the data pipeline for AI-powered podcast creation, transforming social media content into podcast-ready material.

# 🚀 Quick Start
Prerequisites
Rust 1.70+ installed (Install Rust)
Twitter Developer Account with Bearer Token
Alchemyst API Key for context processor integration
Git for cloning the repository
Installation
Clone the repository:
git clone <your-repository-url>
cd amplify
Navigate to backend directory:
cd backend
Set up environment variables:
# Create .env file with your API keys
echo "BEARER_TOKEN=your_actual_twitter_bearer_token_here" > .env
echo "ALCHEMYST_API_KEY=your_actual_alchemyst_api_key_here" >> .env
echo "ALCHEMYST_BASE_URL=https://api.alchemyst.ai" >> .env
⚠️ Important: Replace the placeholder values with your actual API keys

Install dependencies and build:
cargo build
Run the server:
cargo run
The server will start on http://127.0.0.1:8080 and display:

# 🖥️  Server running on port 8080
🛠️ Tech Stack
Backend
Rust - Systems programming language for performance and safety
Actix Web - High-performance web framework for Rust
Tokio - Asynchronous runtime for Rust
Reqwest - HTTP client for API requests
Serde - Serialization/deserialization framework
Regex - Text processing and URL cleaning
dotenvy - Environment variable management
External APIs
X (Twitter) API v2 - Tweet data retrieval
Alchemyst Context Processor - AI-powered content processing
Bearer Token Authentication - Secure API access
Data Format
JSON - API response format
Clean Text - Processed output for AI consumption
📡 API Endpoints
1. Get Original Tweets (Raw Data)
GET /tweets/original
Description: Fetches original tweets with full metadata and public metrics.

Query Parameters:

username (required): Twitter username without @ symbol
max (optional): Number of tweets (10-100, default: 20)
Example Request:

curl "http://127.0.0.1:8080/tweets/original?username=Rustix69&max=10"
Example Response:

[
  {
    "id": "1945690992981717364",
    "edit_history_tweet_ids": ["1945690992981717364"],
    "created_at": "2025-07-17T03:44:16.000Z",
    "text": "People who choose themselves always win no matter how bad the situation gets.",
    "public_metrics": {
      "retweet_count": 0,
      "reply_count": 0,
      "like_count": 5,
      "quote_count": 0,
      "bookmark_count": 1,
      "impression_count": 224
    }
  }
]
2. Get Processed Tweets (AI-Ready)
GET /tweets/processed
Description: Fetches and processes tweets into clean text format optimized for AI/Context APIs.

Query Parameters:

username (required): Twitter username without @ symbol
max (optional): Number of tweets (10-100, default: 20)
Example Request:

curl "http://127.0.0.1:8080/tweets/processed?username=Rustix69&max=10"
Example Response:

{
  "username": "Rustix69",
  "tweet_count": 10,
  "processed_text": "Here are the recent tweets from @Rustix69 to be made into a podcast:\n\nPeople who choose themselves always win no matter how bad the situation gets.\n\nWaiting for the NYC !!!\n\nLFG 🚀 Hope so Gold will respect my levels. Otherwise C gaye guru.\n\nWent from mom's little boy to her biggest disappointment. Will be turning 21 next month but it feels like nothing great has happened."
}
3. Context Addition (Automated Pipeline)
GET /tweets/context-addition
Description: Fetches tweets, processes them, and automatically sends to Alchemyst context processor for podcast generation.

Query Parameters:

username (required): Twitter username without @ symbol
max (optional): Number of tweets (10-100, default: 20)
user_id (optional): User identifier for context processor (default: "default_user")
Example Request:

curl "http://127.0.0.1:8080/tweets/context-addition?username=Rustix69&max=10&user_id=podcast_user_123"
Example Response:

{
  "success": true,
  "message": "Successfully processed 14 tweets from @Rustix69 and added to context processor. Context added successfully.",
  "username": "Rustix69",
  "tweet_count": 14,
  "context_added": true
}
🔧 Environment Configuration
Create a .env file in the backend/ directory:

# Twitter API Bearer Token (Required)
BEARER_TOKEN=your_twitter_bearer_token_here

# Alchemyst Context Processor (Required for context-addition endpoint)
ALCHEMYST_API_KEY=your_alchemyst_api_key_here
ALCHEMYST_BASE_URL=https://api.alchemyst.ai
Getting API Keys
Twitter Bearer Token
Go to Twitter Developer Portal
Create a new app or use existing app
Navigate to "Keys and Tokens"
Generate/copy your "Bearer Token"
Add it to your .env file
Alchemyst API Key
Sign up at Alchemyst AI
Navigate to your API settings
Generate or copy your API key
Add it to your .env file
🧪 Testing
Run Unit Tests
cargo test
Test with cURL
# Test processed endpoint (clean text output)
curl "http://127.0.0.1:8080/tweets/processed?username=elonmusk&max=5"

# Test original endpoint (full data)
curl "http://127.0.0.1:8080/tweets/original?username=elonmusk&max=5"

# Test context-addition endpoint (automated pipeline)
curl "http://127.0.0.1:8080/tweets/context-addition?username=elonmusk&max=5&user_id=test_user"
Test with Postman
Method: GET
URL: http://127.0.0.1:8080/tweets/context-addition
Params:
username: Rustix69
max: 10
user_id: podcast_user_123
📁 Project Structure
backend/
├── src/
│   ├── main.rs                    # Application entry point
│   └── api/
│       ├── mod.rs                 # API module declarations
│       ├── routes.rs              # Route configuration
│       ├── controllers/
│       │   ├── mod.rs
│       │   └── tweet_controller.rs # Tweet endpoint handlers
│       ├── services/
│       │   ├── mod.rs
│       │   └── tweet_service.rs    # Twitter + Alchemyst integration
│       └── models/
│           ├── mod.rs
│           └── tweet.rs           # Data models
├── Cargo.toml                     # Dependencies
├── Cargo.lock                     # Dependency lockfile
└── .env                          # Environment variables
🔄 Development Workflow
For Manual Processing:
Fetch tweets: Call /tweets/processed endpoint
Extract text: Get the processed_text field
Send to AI: Use the clean text with your Context API
Generate podcast: Process with your AI model
For Automated Pipeline:
Single call: Use /tweets/context-addition endpoint
Automatic processing: Tweets → Clean Text → Alchemyst Context Processor
Podcast ready: Context is added and ready for AI generation
Data Flow:
Twitter API → Rust Backend → Clean Text → Alchemyst Context Processor → Podcast Generation
⚡ Performance & Limits
Rate Limits: Twitter API allows 300 requests per 15-minute window
Max Tweets: 10-100 tweets per request
Response Time: Typically <2 seconds for 20 tweets
Concurrent Requests: Supported via Actix Web async handling
Context Processing: Real-time integration with Alchemyst
🛡️ Error Handling
The API returns appropriate HTTP status codes:

200 OK: Successful request
400 Bad Request: Invalid parameters or API errors
401 Unauthorized: Missing or invalid API keys
500 Internal Server Error: Server errors
Error Response Format:

{
  "error": "Missing ALCHEMYST_API_KEY"
}
🔮 Next Steps
 Twitter data fetching and processing
 Alchemyst context processor integration
 Add real-time podcast generation
 Create frontend interface
