# API Quickstart

This is the canonical quickstart for running your first Subconscious AI experiment.

## Contents
- [Step 1: Configure Access](#step-1-configure-access)
- [Step 2: Validate Prompt Causality](#step-2-validate-prompt-causality)
- [Step 3: Generate Attributes and Levels](#step-3-generate-attributes-and-levels)
- [Step 4: Run an Experiment](#step-4-run-an-experiment)
- [Step 5: Retrieve Results](#step-5-retrieve-results)

## API Versioning Notes

Current public routes include both `/api/v1` and `/api/v2` endpoints. Use the endpoint path shown in each example and confirm details in the API Playground: [https://api.subconscious.ai/docs#/](https://api.subconscious.ai/docs#/).

## Step 1: Configure Access

Generate an access token at [https://app.subconscious.ai/settings](https://app.subconscious.ai/settings), then pass it as a bearer token.

```python
import httpx

BASE_URL = "https://api.subconscious.ai"
TOKEN = "${SUBCONSCIOUS_TOKEN}"  # Set via environment variable or secret manager

headers = {
    "Authorization": f"Bearer {TOKEN}",
    "Content-Type": "application/json",
}

client = httpx.Client(timeout=300.0)
```

## Step 2: Validate Prompt Causality

Use `POST /api/v2/copilot/causality` to verify your research prompt.

```python
why_prompt = (
    "I want to understand which laptop features most influence purchase decisions "
    "for U.S. software engineers in 2026."
)

try:
    response = client.post(
        f"{BASE_URL}/api/v2/copilot/causality",
        headers=headers,
        json={"why_prompt": why_prompt},
    )
    response.raise_for_status()
    causality = response.json()
except httpx.HTTPStatusError as exc:
    raise RuntimeError(
        f"Causality check failed: {exc.response.status_code} {exc.response.text}"
    ) from exc
```

## Step 3: Generate Attributes and Levels

Use `POST /api/v1/product-attributes-levels` to generate candidate attributes.

```python
try:
    response = client.post(
        f"{BASE_URL}/api/v1/product-attributes-levels",
        headers=headers,
        json={
            "why_prompt": why_prompt,
            "attribute_count": 6,
            "level_count": 4,
            "country": "USA",
        },
    )
    response.raise_for_status()
    attributes = response.json().get("pre_cooked_attributes_and_levels_lookup", [])
except httpx.HTTPStatusError as exc:
    raise RuntimeError(
        f"Attribute generation failed: {exc.response.status_code} {exc.response.text}"
    ) from exc
```

## Step 4: Run an Experiment

Use `POST /api/v1/experiments` with the generated attributes.

```python
try:
    response = client.post(
        f"{BASE_URL}/api/v1/experiments",
        headers=headers,
        json={
            "why_prompt": why_prompt,
            "country": "United States",
            "year": "2026",
            "pre_cooked_attributes_and_levels_lookup": attributes,
            "number_of_respondents": 75,
            "number_of_tasks_per_respondent": 10,
        },
    )
    response.raise_for_status()
    result = response.json()
except httpx.HTTPStatusError as exc:
    raise RuntimeError(
        f"Experiment creation failed: {exc.response.status_code} {exc.response.text}"
    ) from exc
```

## Step 5: Retrieve Results

Use `GET /api/v1/runs/artifact/{file_name}` after experiment processing completes.

```python
wandb_run_name = result["wandb_run_name"]
file_name = f"Analytics_output_{wandb_run_name}"

try:
    response = client.get(
        f"{BASE_URL}/api/v1/runs/artifact/{file_name}",
        headers=headers,
    )
    response.raise_for_status()
    analytics = response.json()
except httpx.HTTPStatusError as exc:
    raise RuntimeError(
        f"Results retrieval failed: {exc.response.status_code} {exc.response.text}"
    ) from exc
```

## Troubleshooting

- `401 Unauthorized`: verify token validity and authorization header format.
- `422 Unprocessable Entity`: validate payload schema and required fields.
- Long-running requests: use client timeouts of at least 300 seconds for generation endpoints.

## Extended Reference

For a longer, end-to-end walkthrough (including persona-rich payloads), use `antler_hackathon.md`.
