# Financial News Sentiment Analysis Application

## Technical Documentation

*Version: 2.0.0*  
*Last Updated: May 9, 2025*

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Architecture](#2-architecture)
3. [File Structure and Components](#3-file-structure-and-components)
4. [Backend System](#4-backend-system)
5. [Frontend System](#5-frontend-system)
6. [Data Flow and Processing](#6-data-flow-and-processing)
7. [MLOps Implementation](#7-mlops-implementation)
8. [Security and Best Practices](#8-security-and-best-practices)
9. [Deployment Options](#9-deployment-options)
10. [Testing Framework](#10-testing-framework)
11. [Configuration Guide](#11-configuration-guide)
12. [API Reference](#12-api-reference)
13. [Future Development](#13-future-development)

---

## 1. Project Overview

### 1.1 Purpose

The Financial News Sentiment Analysis Application is a production-grade system designed to analyze sentiment in financial news for selected stock tickers. The application helps investors and financial analysts understand market sentiment for specific companies by classifying news as positive, negative, or neutral, enabling more informed investment decisions.

### 1.2 Key Features

- **Multi-source Financial News**: Integrates with NewsAPI.org with fallback to Excel-based historical data
- **Advanced Sentiment Analysis**: Rule-based sentiment analysis with MLOps capabilities
- **Interactive Dashboard**: Streamlit-based visualization of sentiment analysis results
- **Custom Date Selection**: Flexible date range selection, including custom calendar-based ranges
- **Data Export**: Export functionality for further analysis in external tools
- **Custom Text Analysis**: Analyze custom text input for financial sentiment
- **Enterprise-ready Architecture**: Containerization, CI/CD integration, and Kubernetes support

---

## 2. Architecture

### 2.1 System Architecture

The application follows a microservices architecture with two primary components:

```
┌────────────────┐       ┌───────────────────┐       ┌───────────────────┐
│                │       │                   │       │                   │
│  Web Browser   │◄─────►│  Streamlit        │◄─────►│  Backend API      │
│                │       │  Frontend (5000)  │       │  Server (8000)    │
└────────────────┘       └───────────────────┘       └───────────────────┘
                                                              │
                                                     ┌────────▼─────────┐
                                                     │                  │
                                                     │  News Sources    │
                                                     │  - NewsAPI.org   │
                                                     │  - Excel Data    │
                                                     │                  │
                                                     └──────────────────┘
```

### 2.2 Component Interactions

1. **User Interface Layer**: Streamlit frontend provides user interaction and visualization
2. **Service Layer**: Backend API handles data fetching, processing, and sentiment analysis
3. **Data Layer**: News data sources and sentiment model provide core functionality
4. **Deployment Layer**: Docker containers, CI/CD pipeline, and cloud deployment options

### 2.3 Technology Stack

- **Languages**: Python 3.11
- **Backend**: Custom HTTP server with RESTful API endpoints
- **Frontend**: Streamlit for interactive dashboard
- **Data Visualization**: Plotly for interactive charts
- **Data Processing**: Pandas for data manipulation
- **Containerization**: Docker with multi-stage builds
- **Orchestration**: Docker Compose, Kubernetes
- **CI/CD**: GitHub Actions
- **Cloud Deployment**: Render.com configuration

---

## 3. File Structure and Components

### 3.1 Root Directory

| File/Directory | Description |
|----------------|-------------|
| `run_backend.py` | Backend server entry point |
| `run_frontend.py` | Streamlit frontend entry point |
| `utils.py` | Shared utility functions |
| `backend.Dockerfile` | Docker configuration for backend service |
| `frontend.Dockerfile` | Docker configuration for frontend service |
| `docker-compose.yml` | Multi-container Docker configuration |
| `render.yaml` | Render.com deployment configuration |
| `.env.template` | Template for environment variables |
| `project_requirements.txt` | Python dependencies |
| `.github/workflows/ci-cd.yml` | GitHub Actions CI/CD workflow |
| `kubernetes/` | Kubernetes deployment configurations |
| `tests/` | Test suite |

### 3.2 Backend Components

| File/Directory | Description |
|----------------|-------------|
| `backend/app/api.py` | API endpoint definitions |
| `backend/app/news_api.py` | NewsAPI.org integration |
| `backend/app/news_scraper.py` | News source manager |
| `backend/app/sentiment.py` | Sentiment analysis model with MLOps |
| `backend/app/excel_news.py` | Excel data source for historical data |
| `backend/app/utils.py` | Backend utility functions |
| `backend/data/` | Data storage for Excel files and model registry |

### 3.3 Frontend Components

| File/Directory | Description |
|----------------|-------------|
| `frontend/app/dashboard.py` | Main Streamlit dashboard UI |
| `.streamlit/config.toml` | Streamlit configuration |

### 3.4 Deployment Components

| File/Directory | Description |
|----------------|-------------|
| `kubernetes/backend-deployment.yaml` | Backend Kubernetes deployment |
| `kubernetes/frontend-deployment.yaml` | Frontend Kubernetes deployment |
| `kubernetes/configmap.yaml` | Configuration data |
| `kubernetes/secrets.yaml` | Secret management template |
| `kubernetes/ingress.yaml` | Ingress configuration |

---

## 4. Backend System

### 4.1 Backend Entry Point (run_backend.py)

The `run_backend.py` file serves as the entry point for the backend server. It:

- Initializes the backend API server on port 8000
- Sets up API endpoints for news retrieval and sentiment analysis
- Configures CORS for cross-origin requests from the frontend
- Implements health check endpoint for monitoring

Key functions:
- `run_server()`: Starts the HTTP server
- `FinancialNewsHandler.do_GET()`: Handles GET requests
- `FinancialNewsHandler.do_POST()`: Handles POST requests

### 4.2 News Processing (backend/app/news_scraper.py)

Manages news retrieval from multiple sources:

- Integrates with NewsAPI.org as the primary source
- Falls back to Excel-based news data when necessary
- Implements error handling and rate limit management
- Provides consistent data formatting across sources

Key functions:
- `get_financial_news()`: Main function to retrieve news for a ticker
- `get_news_from_api()`: Retrieves news from NewsAPI.org
- `get_news_from_excel()`: Retrieves news from Excel files
- `is_api_key_valid()`: Validates NewsAPI key

### 4.3 Sentiment Analysis (backend/app/sentiment.py)

The MLOps-ready sentiment analysis engine:

- Implements rule-based sentiment classification
- Supports model versioning and configuration management
- Tracks performance metrics including latency and error rates
- Provides a framework for future ML model integration

Key components:
- `SentimentModel` class: Encapsulates model functionality
- `analyze_sentiment()`: Main function for text analysis
- Metrics tracking with JSON storage

### 4.4 API Endpoints (backend/app/api.py)

Defines the RESTful API interface:

| Endpoint | Method | Description | Parameters |
|----------|--------|-------------|------------|
| `/health` | GET | Health check | None |
| `/api/news` | GET | Get news with sentiment | `tickers`, `days`, `start_date`, `end_date`, `max_results` |
| `/api/sentiment_summary` | GET | Get sentiment summary | `tickers`, `days`, `start_date`, `end_date`, `max_results` |
| `/api/export` | GET | Export data | `tickers`, `days`, `start_date`, `end_date`, `max_results` |
| `/api/analyze_text` | POST | Analyze custom text | `text` in JSON body |

---

## 5. Frontend System

### 5.1 Frontend Entry Point (run_frontend.py)

Initializes the Streamlit dashboard:

- Configures the Streamlit application settings
- Sets up connection to the backend API
- Implements caching for performance optimization
- Manages the main application flow

Key functions:
- `main()`: Main application entry point
- `fetch_news()`: API client for news data
- `fetch_sentiment_summary()`: API client for sentiment summary
- `analyze_custom_text()`: Sends text for sentiment analysis

### 5.2 Dashboard UI (frontend/app/dashboard.py)

Implements the user interface:

- Sidebar controls for ticker selection and date range
- Sentiment overview section with key metrics
- Interactive visualizations for sentiment distribution
- News display with pagination and filtering
- Custom text analysis widget
- Data export functionality

Key functions:
- `display_sentiment_visualizations()`: Creates interactive charts
- `prepare_sentiment_data()`: Formats data for visualization

### 5.3 Streamlit Configuration (.streamlit/config.toml)

Configures Streamlit server behavior:

```toml
[server]
headless = true
address = "0.0.0.0"
port = 5000
```

---

## 6. Data Flow and Processing

### 6.1 Data Flow Overview

1. User selects stock tickers and date range in the frontend
2. Frontend makes API request to backend
3. Backend retrieves news from NewsAPI or Excel
4. Backend performs sentiment analysis on each news item
5. Results are returned to frontend for visualization
6. Frontend transforms data for visualization and display

### 6.2 News Data Sources

#### 6.2.1 NewsAPI.org
- Primary source for recent financial news
- Limited to last 30 days in free tier
- Configured via NEWS_API_KEY environment variable

#### 6.2.2 Excel Fallback
- Provides historical data beyond NewsAPI limits
- Located in `backend/data/demo_financial_news.xlsx`
- Contains 544 entries of financial news across major tickers

### 6.3 Sentiment Analysis Process

1. Text preprocessing removes special characters and normalizes text
2. Keyword matching against positive and negative financial terms
3. Sentiment classification based on keyword frequency
4. Confidence score calculation based on keyword strength
5. Result aggregation for visualization
6. Metrics tracking for model performance

### 6.4 Caching Strategy

- Frontend implements TTL-based caching for API responses
- Cache keys include tickers and time parameters for proper invalidation
- Cache busting through query parameters
- Performance optimization through intelligent refresh

---

## 7. MLOps Implementation

### 7.1 Model Architecture

The `SentimentModel` class in `backend/app/sentiment.py` provides a production-ready ML framework:

```python
class SentimentModel:
    def __init__(self):
        # Initialize with configuration from environment
        self.model_version = MODEL_VERSION
        self.model_path = os.environ.get("MODEL_PATH", "")
        self.registry_path = MODEL_REGISTRY
        
        # Load model configuration
        self.config = self._load_config()
        
        # Initialize metrics tracking
        self.request_count = 0
        self.error_count = 0
        self.latency_ms_sum = 0
```

### 7.2 Model Versioning

- Environment variable `MODEL_VERSION` controls active model version
- Model configuration stored in registry directory
- Support for loading different model versions dynamically

### 7.3 Metrics Collection

The model automatically tracks performance metrics:

- Request count and error count
- Average latency in milliseconds
- Error rate percentage
- Metrics saved in JSON format every 60 seconds

### 7.4 Model Registry

The `.model_registry` directory stores:

- Model configurations in JSON format
- Performance metrics with timestamps
- Runtime statistics for model monitoring

### 7.5 Future ML Integration

The system is designed for easy integration with advanced ML models:

- Standardized interface for model swapping
- Configuration-based model selection
- Error handling and fallback mechanisms

---

## 8. Security and Best Practices

### 8.1 Docker Security

The Dockerfiles implement security best practices:

- Non-root user (`appuser`) for container execution
- Proper file permissions and ownership
- Multi-stage builds for smaller image size
- Minimal base images to reduce attack surface

### 8.2 Secret Management

- Environment variables for sensitive configuration
- `.env.template` file for documentation
- Kubernetes Secrets for production deployment
- No hardcoded credentials in source code

### 8.3 Input Validation

- Parameter validation on all API endpoints
- Safe handling of user input
- Error handling for malformed requests
- Sanitization of data before processing

### 8.4 Error Handling

- Comprehensive try/except blocks
- Structured error logging
- Graceful degradation with fallback mechanisms
- Client-friendly error messages

### 8.5 Logging and Monitoring

- Structured JSON logging for machine readability
- Configurable log levels via environment variables
- Rotating file handlers for persistent logs
- Comprehensive error context for debugging

---

## 9. Deployment Options

### 9.1 Local Development

```bash
# Install dependencies
pip install -r project_requirements.txt

# Start backend server
python run_backend.py

# Start frontend (in separate terminal)
streamlit run run_frontend.py
```

### 9.2 Docker Deployment

```bash
# Build and start services
docker-compose up -d

# View logs
docker-compose logs -f
```

### 9.3 Render.com Deployment

The `render.yaml` file configures the Render.com deployment:

```yaml
services:
  - type: web
    name: financial-news-backend
    env: python
    buildCommand: pip install -r project_requirements.txt
    startCommand: python run_backend.py
    
  - type: web
    name: financial-news-frontend
    env: python
    buildCommand: pip install -r project_requirements.txt
    startCommand: streamlit run run_frontend.py --server.port $PORT
    envVars:
      - key: BACKEND_URL
        fromService:
          name: financial-news-backend
          type: web
          property: url
```

See `Render_Deployment_Guide.md` for step-by-step instructions.

### 9.4 Kubernetes Deployment

```bash
# Apply Kubernetes configurations
kubectl apply -f kubernetes/

# Verify deployment
kubectl get pods
```

---

## 10. Testing Framework

### 10.1 Test Structure

The `tests/` directory contains unit and integration tests:

- `test_sentiment.py`: Tests for sentiment analysis functionality
- `test_api.py`: Tests for API endpoints

### 10.2 Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=backend --cov=frontend
```

### 10.3 CI/CD Integration

The GitHub Actions workflow in `.github/workflows/ci-cd.yml` automatically:

1. Runs tests on every push and pull request
2. Generates coverage reports
3. Builds Docker images on successful tests
4. Deploys to Kubernetes on main branch changes

---

## 11. Configuration Guide

### 11.1 Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `NEWS_API_KEY` | None | API key for NewsAPI.org |
| `PORT` | 8000 (backend), 5000 (frontend) | Server ports |
| `LOG_LEVEL` | INFO | Logging verbosity |
| `LOG_FORMAT` | text | Log format (text or json) |
| `MODEL_VERSION` | rule-based-v1 | Sentiment model version |
| `MODEL_BATCH_SIZE` | 16 | Batch size for model inference |
| `MODEL_MAX_LENGTH` | 512 | Maximum text length for analysis |
| `BACKEND_URL` | http://localhost:8000 | URL for frontend to connect to backend |

### 11.2 Configuration Files

| File | Purpose |
|------|---------|
| `.env.template` | Template for environment variables |
| `.streamlit/config.toml` | Streamlit configuration |
| `docker-compose.yml` | Docker service configuration |
| `kubernetes/configmap.yaml` | Kubernetes configuration |

---

## 12. API Reference

### 12.1 `/api/news`

**Purpose**: Retrieve news with sentiment analysis

**Method**: GET

**Parameters**:
- `tickers` (required): Comma-separated list of stock tickers
- `days` (optional): Number of days to look back (default: 7)
- `start_date` (optional): Custom start date (YYYY-MM-DD)
- `end_date` (optional): Custom end date (YYYY-MM-DD)
- `max_results` (optional): Maximum results per ticker (default: 100)

**Response**:
```json
{
  "items": [
    {
      "title": "News headline",
      "publisher": "Publisher name",
      "link": "https://news-url.com",
      "published_date": "2025-05-01 12:00:00",
      "ticker": "AAPL",
      "sentiment": "positive",
      "score": 0.85
    },
    ...
  ]
}
```

### 12.2 `/api/sentiment_summary`

**Purpose**: Get sentiment summary statistics

**Method**: GET

**Parameters**: Same as `/api/news`

**Response**:
```json
{
  "AAPL": {
    "positive_count": 15,
    "negative_count": 5,
    "neutral_count": 10,
    "average_score": 0.65,
    "sentiment_distribution": {
      "positive": 0.5,
      "negative": 0.17,
      "neutral": 0.33
    }
  },
  ...
}
```

### 12.3 `/api/export`

**Purpose**: Export sentiment data in CSV format

**Method**: GET

**Parameters**: Same as `/api/news`

**Response**:
CSV formatted text with headers:
```
ticker,title,publisher,published_date,sentiment,score,link
```

### 12.4 `/api/analyze_text`

**Purpose**: Analyze custom text for sentiment

**Method**: POST

**Request Body**:
```json
{
  "text": "Text to analyze for sentiment"
}
```

**Response**:
```json
{
  "sentiment": "positive",
  "score": 0.78,
  "details": {
    "positive_count": 3,
    "negative_count": 1
  },
  "model_version": "rule-based-v1",
  "latency_ms": 12.45
}
```

---

## 13. Future Development

### 13.1 Planned Enhancements

- Integration with advanced NLP models (FinBERT)
- Real-time news streaming
- User authentication and personalization
- Advanced visualization options
- Mobile application

### 13.2 Technical Roadmap

1. **Q2 2025**: Implement FinBERT model integration
2. **Q3 2025**: Add real-time data streaming
3. **Q4 2025**: Develop predictive analytics features
4. **Q1 2026**: Release enterprise version with additional features

### 13.3 Contributing

Contributions are welcome! See `CONTRIBUTING.md` for guidelines on how to contribute to this project.