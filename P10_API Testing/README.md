# API Integration Testing Suite

A complete API integration testing framework built around a mock Payment Processing API. Designed for practicing and demonstrating professional API testing methodology including authentication, validation, rate limiting, webhooks, and automated test execution.

---

## Overview

This project provides:

- **A fully-featured mock Payment API** built with Flask, supporting 8+ endpoints with realistic behavior
- **Automated test suites** using both pytest (unit/integration) and a standalone test runner (black-box)
- **A Postman collection** with pre-built requests and test scripts for manual and automated testing
- **OpenAPI 3.0 specification** for formal API documentation
- **Comprehensive documentation** covering testing methodology, endpoint reference, and Postman usage

---

## Architecture

```
api-testing-suite/
|-- api/                        # Mock Payment API (Flask)
|   |-- app.py                  # Application factory and route definitions
|   |-- auth.py                 # API key validation and OAuth2 mock flow
|   |-- models.py               # Payment, Refund, Transaction data models
|   |-- validators.py           # Input validation with detailed error messages
|   |-- rate_limiter.py         # In-memory rate limiting middleware
|   |-- webhooks.py             # Webhook simulation endpoints
|   +-- requirements.txt        # API-specific dependencies
|
|-- tests/                      # Automated test suite
|   |-- conftest.py             # Pytest fixtures and configuration
|   |-- test_payments.py        # 34 payment endpoint test cases
|   |-- test_auth.py            # 23 authentication and authorization tests
|   |-- test_rate_limit.py      # 9 rate limiting tests
|   +-- test_runner.py          # Standalone integration test runner
|
|-- postman/                    # Postman artifacts
|   |-- payment_api_collection.json   # Full Postman collection (importable)
|   +-- environment.json              # Environment variables template
|
|-- docs/                       # Documentation
|   |-- api_spec.yaml           # OpenAPI 3.0 specification
|   |-- API_DOCUMENTATION.md    # Full endpoint reference
|   |-- TESTING_GUIDE.md        # How to write effective API tests
|   |-- POSTMAN_GUIDE.md        # Postman import and usage guide
|   +-- screenshots/            # Screenshot placeholders
|
|-- requirements.txt            # Project dependencies
|-- Makefile                    # Build and run commands
|-- .gitignore                  # Git ignore rules
+-- LICENSE                     # MIT License
```

---

## API Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|:---:|
| `GET` | `/v1/health` | Health check | No |
| `GET` | `/v1/api-info` | API metadata and version | No |
| `POST` | `/v1/payments` | Create a payment | Yes |
| `GET` | `/v1/payments` | List payments (paginated) | Yes |
| `GET` | `/v1/payments/{id}` | Get payment by ID | Yes |
| `GET` | `/v1/payments/{id}/status` | Check payment status | Yes |
| `POST` | `/v1/payments/{id}/cancel` | Cancel a pending payment | Yes |
| `POST` | `/v1/payments/{id}/refund` | Create a full or partial refund | Yes |
| `GET` | `/v1/oauth/authorize` | OAuth2 authorization | No |
| `POST` | `/v1/oauth/token` | OAuth2 token exchange | No |
| `POST` | `/v1/webhooks/endpoints` | Register webhook URL | Admin |
| `GET` | `/v1/webhooks/endpoints` | List webhook endpoints | Admin |
| `DELETE` | `/v1/webhooks/endpoints/{id}` | Delete webhook endpoint | Admin |
| `GET` | `/v1/webhooks/events` | List webhook events | Yes |
| `POST` | `/v1/webhooks/simulate` | Simulate a webhook event | Admin |
| `POST` | `/v1/test/reset` | Reset all test data | Admin |

---

## Setup

### Prerequisites

- Python 3.9 or later
- pip package manager

### Installation

```bash
# Navigate to the project directory
cd api-testing-suite

# Install dependencies
pip install -r requirements.txt

# Or using make
make install
```

### Start the API Server

```bash
python -m api.app

# Or using make
make run-api
```

The API will be available at `http://localhost:5000/v1`.

Verify the server is running:

```bash
curl http://localhost:5000/v1/health
```

---

## Test Execution

### Run All Tests with Pytest

```bash
pytest tests/ -v

# Or using make
make test
```

### Run Tests with Coverage

```bash
pytest tests/ --cov=api --cov-report=term-missing

# Or using make
make test-coverage
```

### Run Specific Test Modules

```bash
make test-payments      # Payment endpoint tests
make test-auth          # Authentication tests
make test-rate-limit    # Rate limiting tests
```

### Run the Standalone Integration Test Runner

This runner uses the `requests` library to test against a running API server:

```bash
# Terminal 1: Start the API
python -m api.app

# Terminal 2: Run integration tests
python -m tests.test_runner

# Or using make
make test-runner
```

---

## Authentication

### API Keys

Include an API key in every request to protected endpoints:

```bash
curl -H "Authorization: Bearer demo_key_4eC39HqLyjWDarjtT1zdp7dc" \
     http://localhost:5000/v1/payments
```

### Test API Keys

| Key | Role | Rate Limit |
|-----|------|------------|
| `demo_key_4eC39HqLyjWDarjtT1zdp7dc` | Standard (read/write) | 100/min |
| `demo_key_BQokikJOvBiI2HlWgH4olfQ2` | Admin (full access) | 500/min |
| `demo_key_readonly_9f8g7h6j5k4l3m2n` | Read-only | 50/min |
| `demo_key_expired_key_do_not_use` | Expired (inactive) | N/A |

---

## Postman Import Guide

### Import the Collection

1. Open Postman.
2. Click **Import** (top-left).
3. Select `postman/payment_api_collection.json`.
4. Click **Import**.

### Import the Environment

1. Click **Import** again.
2. Select `postman/environment.json`.
3. In the top-right dropdown, select **Payment API - Local Development**.

### Run the Collection

1. Right-click the collection and select **Run collection**.
2. Set iterations to 1 and delay to 100ms.
3. Click **Run**.

See `docs/POSTMAN_GUIDE.md` for detailed instructions.

---

## Simulated Payment Failures

The mock API simulates payment failures based on the amount:

| Amount Pattern | Failure Reason | HTTP Status |
|----------------|----------------|:-----------:|
| Ends in `13.00` | `card_declined` | 402 |
| Ends in `66.60` | `insufficient_funds` | 402 |
| Over 9000, ends in `00.00` | `amount_exceeds_limit` | 402 |

---

## Error Response Format

All errors follow a consistent structure:

```json
{
  "error": {
    "type": "validation_error",
    "message": "The request body contains invalid parameters.",
    "status": 422,
    "errors": [
      {
        "field": "amount",
        "code": "required",
        "message": "The 'amount' field is required."
      }
    ]
  }
}
```

### Error Types

| Type | HTTP Status | Description |
|------|:-----------:|-------------|
| `authentication_error` | 401/403 | Missing, invalid, or inactive API key |
| `authorization_error` | 403 | Insufficient permissions |
| `validation_error` | 422 | Invalid request parameters |
| `not_found` | 404 | Resource does not exist |
| `invalid_request` | 400/409 | Malformed request or state conflict |
| `rate_limit_error` | 429 | Too many requests |
| `method_not_allowed` | 405 | Wrong HTTP method |
| `internal_server_error` | 500 | Unexpected server error |

---

## Documentation

| Document | Description |
|----------|-------------|
| [API Documentation](docs/API_DOCUMENTATION.md) | Complete endpoint reference with request/response examples |
| [Testing Guide](docs/TESTING_GUIDE.md) | How to write effective API tests |
| [Postman Guide](docs/POSTMAN_GUIDE.md) | Importing and using the Postman collection |
| [OpenAPI Spec](docs/api_spec.yaml) | Machine-readable API specification (paste into [Swagger Editor](https://editor.swagger.io)) |

---

## Make Targets

```
make help              Show all available commands
make install           Install dependencies
make run-api           Start the API server
make test              Run all tests
make test-verbose      Run tests with verbose output
make test-coverage     Run tests with coverage report
make test-payments     Run payment tests only
make test-auth         Run authentication tests only
make test-rate-limit   Run rate limiting tests only
make test-runner       Run standalone integration tests
make lint              Check module syntax
make clean             Remove generated files
make reset             Reset test data on running server
make docs              Show documentation paths
```

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
