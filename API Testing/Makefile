.PHONY: help install run-api test test-verbose test-coverage test-payments test-auth test-rate-limit lint docs clean reset

# Default target
help: ## Show this help message
	@echo "Payment Processing API - Available Commands"
	@echo "============================================"
	@echo ""
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | \
		awk 'BEGIN {FS = ":.*?## "}; {printf "  \033[36m%-20s\033[0m %s\n", $$1, $$2}'
	@echo ""

# ---------------------------------------------------------------------------
# Setup
# ---------------------------------------------------------------------------

install: ## Install all dependencies
	pip install -r requirements.txt

# ---------------------------------------------------------------------------
# API Server
# ---------------------------------------------------------------------------

run-api: ## Start the mock API server on port 5000
	python -m api.app

run-api-debug: ## Start the API server with debug logging
	FLASK_DEBUG=1 python -m api.app

# ---------------------------------------------------------------------------
# Testing
# ---------------------------------------------------------------------------

test: ## Run all tests with pytest
	pytest tests/ -v --tb=short

test-verbose: ## Run all tests with verbose output
	pytest tests/ -v --tb=long -s

test-coverage: ## Run tests with coverage report
	pytest tests/ -v --cov=api --cov-report=term-missing --cov-report=html

test-payments: ## Run only payment tests
	pytest tests/test_payments.py -v --tb=short

test-auth: ## Run only authentication tests
	pytest tests/test_auth.py -v --tb=short

test-rate-limit: ## Run only rate limiting tests
	pytest tests/test_rate_limit.py -v --tb=short

test-runner: ## Run the standalone integration test runner (requires running API)
	python -m tests.test_runner

# ---------------------------------------------------------------------------
# Documentation
# ---------------------------------------------------------------------------

docs: ## Open the API documentation
	@echo "API Documentation: docs/API_DOCUMENTATION.md"
	@echo "Testing Guide:     docs/TESTING_GUIDE.md"
	@echo "Postman Guide:     docs/POSTMAN_GUIDE.md"
	@echo "OpenAPI Spec:      docs/api_spec.yaml"

docs-serve: ## Serve OpenAPI spec with Swagger UI (if swagger-ui is installed)
	@echo "Open docs/api_spec.yaml in https://editor.swagger.io"
	@echo "Or use: npx @redocly/cli preview-docs docs/api_spec.yaml"

# ---------------------------------------------------------------------------
# Utilities
# ---------------------------------------------------------------------------

lint: ## Run linting checks
	python -m py_compile api/app.py
	python -m py_compile api/auth.py
	python -m py_compile api/models.py
	python -m py_compile api/validators.py
	python -m py_compile api/rate_limiter.py
	python -m py_compile api/webhooks.py
	@echo "All modules compile successfully."

clean: ## Remove generated files and caches
	find . -type d -name __pycache__ -exec rm -rf {} + 2>/dev/null || true
	find . -type d -name .pytest_cache -exec rm -rf {} + 2>/dev/null || true
	rm -rf htmlcov/ .coverage coverage.xml
	@echo "Cleaned."

reset: ## Reset test data on a running API server
	curl -s -X POST http://localhost:5000/v1/test/reset \
		-H "Authorization: Bearer demo_key_BQokikJOvBiI2HlWgH4olfQ2" | python -m json.tool
