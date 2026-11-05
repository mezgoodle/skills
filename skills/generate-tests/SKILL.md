---
name: generate-tests
description: Generate unit and integration tests for backend services, APIs, and business logic adhering to pytest best practices and mocking conventions.
---

# Generate Tests

A standardized procedure for generating unit and integration tests for backend services, API endpoints, and utility functions following industry-standard `pytest` and async testing practices.

## Guidelines & Principles

- **Test-Driven Coverage:** Every new feature, endpoint, or service method must be accompanied by unit/integration tests.
- **Convention & Structure:**
  - Mirror source layout into the `tests/` directory:
    - Service: `app/services/<service_name>.py` -> `tests/services/test_<service_name>.py`
    - API Endpoint: `app/api/v1/endpoints/<router_name>.py` -> `tests/api/v1/test_<router_name>.py`
- **Naming Conventions:**
  - Test files: `test_<module_name>.py`
  - Test functions: `test_<function_or_method>_<scenario>_<expected_result>` (e.g., `test_get_item_success`, `test_create_user_duplicate_email_raises_conflict`).
- **Path Coverage:**
  - **Happy Path:** Expected successful execution (e.g. 200 OK, return value, expected side-effects).
  - **Edge Cases & Errors:** Negative inputs, validation errors, not found cases (404), unauthenticated/unauthorized access (401/403), or unexpected server errors.

---

## Mocking & Isolation

1. **External APIs & I/O:**
   - Always mock external network calls (third-party APIs, HTTP clients, cloud services, LLM providers) using `unittest.mock.AsyncMock`, `unittest.mock.patch`, or `pytest-mock` fixtures.
   - Never perform unmocked network calls against external live endpoints during unit test execution.
2. **Database Sessions:**
   - Use dedicated transactional test session fixtures (e.g. `db_session`) to ensure rollback or isolated in-memory/test DB state across test runs.
3. **Async Support:**
   - Explicitly annotate asynchronous test functions with `@pytest.mark.asyncio` (or configure `asyncio_mode = "auto"` via `pytest.ini` / `pyproject.toml`).

---

## Step-by-Step Procedure

1. **Analyze Target Code:**
   - Identify inputs, outputs, exceptions raised, dependencies, and external services used.
2. **Setup Fixtures:**
   - Determine reusable fixtures required (mock clients, dummy models, auth headers, database session).
3. **Implement Test Cases:**
   - Write clear Arrange-Act-Assert (AAA) blocks.
   - Assert status codes, response schemas, database state, and mock invocation counts (`mock.assert_awaited_once_with(...)`).
4. **Run Verification:**
   - Execute test suite locally:
     ```bash
     pytest tests/path/to/test_target.py -v
     ```
   - Check test coverage and ensure zero failures or warnings.
