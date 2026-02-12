# Subconscious AI API Documentation

> Note: For the canonical quickstart, use `api/README.md`. This document is an extended reference with larger payload examples.

## Welcome to Subconscious AI

Subconscious AI redefines market research and product development with our innovative "Behaviour Change as a Service" model. Powered by advanced Large Language Models (LLMs), we enable businesses to conduct **Causal Market Research** with unmatched speed, quality, and ethical standards. Our platform delivers human-level reliability, guiding you through **Ideation**, **User Research**, and **Product Design**. Subconscious AI is your partner in understanding and influencing consumer behavior.
## How It Works

To simulate a causal market research, our system employs three simple steps:

1. **Design a Causal Prompt** - Define your research question
2. **Generate Attributes & Levels** - Create product features and their variations
3. **Run the Experiment** - Launch your conjoint analysis
4. **Retrieve Results** - Retrieve and interpret your findings

It is as simple as that!

---

## Getting Started

### Step 1: Get Your API Token

Before you start, generate an API token for authorization:

1. Visit [https://app.subconscious.ai/settings](https://app.subconscious.ai/settings)
2. Generate and copy your access token
3. Use it in all your API requests

### Step 2: Setup Authentication

Add your token to the header of every API request:

```python
import httpx

headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
}

BASE_URL = "https://api.subconscious.ai"

# Create a client with extended timeout for API calls
client = httpx.Client(timeout=300.0)  # 5 minutes timeout
```

**Important:** Our API endpoints may take some time to process requests, especially for generating attributes and checking causality. Make sure to set appropriate timeouts in your HTTP client.

---

## API Endpoints

### 1. Check Causality

**Endpoint:** `POST /api/v2/copilot/causality`

The next step involves defining our causal prompt, which will serve as the foundation for setting up our task. To assist you in crafting this prompt, we offer a helpful 'check-causality' feature!

**Purpose:** Validates your research question to ensure it's suitable for causal market research.

**Request:**
```python
response = httpx.post(
    f"{BASE_URL}/api/v2/copilot/causality",
    headers=headers,
    json={
        "why_prompt": "There's been a lot of discussion about Samsung's recent innovations in display technology and camera systems, particularly with their Galaxy lineup. I'm interested in exploring Samsung's current smartphone offerings to understand how their latest models compare in terms of display quality, camera capabilities, and overall performance, especially since they've been positioning themselves as a premium alternative in the market."
    },
    timeout=300.0  # 5 minutes timeout
)
```

**Parameters:**
- `why_prompt` (string, required): Your research question or business objective

**Note:** This endpoint may take 1-2 minutes to process as it analyzes your research question.

---

### 2. Generate Product Attributes & Levels

**Endpoint:** `POST /api/v1/product-attributes-levels`

After selecting the prompt, we progress to constructing our attributes and their corresponding levels, which define the scale of each attribute. For instance, consider a car: "Price" could be an essential attribute, with a range from $10,000 to $100,000. You have the flexibility to access a more detailed set of levels through our API, or you can opt to define these levels yourself!

**Purpose:** Automatically generates relevant product features and their variations based on your research question.

**Request:**
```python
response = httpx.post(
    f"{BASE_URL}/api/v1/product-attributes-levels",
    headers=headers,
    json={
        "why_prompt": "There's been a lot of discussion about Samsung's recent innovations in display technology and camera systems, particularly with their Galaxy lineup. I'm interested in exploring Samsung's current smartphone offerings to understand how their latest models compare in terms of display quality, camera capabilities, and overall performance, especially since they've been positioning themselves as a premium alternative in the market.",
        "attribute_count": 8,
        "level_count": 5,
        "country": "USA"
    },
    timeout=300.0  # 5 minutes timeout
)
```

**Parameters:**
- `why_prompt` (string, required): Your research question
- `attribute_count` (integer, required,2-7): Number of product features to generate
- `level_count` (integer, required, 2-5): Number of variations per feature
- `country` (string, required): Target market for the research

**Note:** This endpoint may take 2-3 minutes to generate attributes and levels as it analyzes your market and research question.

---

### 3. Run Experiment

**Endpoint:** `POST /api/v1/experiments`

Finally, we can now run our experiment! But first, we need to setup the data to run the experiment.

[//]: # (TODO: use the output of the previous step to populate attributes and levels.)

**Purpose:** Launches a conjoint analysis experiment to measure customer preferences and understand what drives behavior change.

**Request:**
```python
response = httpx.post(
    f"{BASE_URL}/api/v1/experiments",
    headers=headers,
    json={
        "why_prompt": "There's been a lot of discussion about Samsung's recent innovations in display technology and camera systems, particularly with their Galaxy lineup. I'm interested in exploring Samsung's current smartphone offerings to understand how their latest models compare in terms of display quality, camera capabilities, and overall performance.",
        "country": "United States",
        "year": "2025",
        "pre_cooked_attributes_and_levels_lookup": [
            [
                "Price",
                ["$999.99", "$859.99", "$1099.99", "$1299.99", "$649.99"]
            ],
            [
                "Display",
                [
                    "6.4-inch Dynamic AMOLED 2X display with 120Hz refresh rate",
                    "6.2-inch Dynamic LTPO AMOLED 2X display with 120Hz refresh rate",
                    "6.7-inch Dynamic LTPO AMOLED 2X display with 120Hz refresh rate",
                    "6.7-inch Foldable Dynamic AMOLED 2X main display with 120Hz refresh rate",
                    "6.8-inch Dynamic LTPO AMOLED 2X display with 120Hz refresh rate"
                ]
            ],
            [
                "Processor",
                [
                    "Qualcomm Snapdragon 8 Gen 2",
                    "Qualcomm Snapdragon 8 Gen 3 for Galaxy"
                ]
            ],
            [
                "Rear Camera",
                [
                    "50MP main, 12MP ultrawide",
                    "50MP main, 12MP ultrawide, 10MP telephoto with 3x optical zoom",
                    "200MP main, 12MP ultrawide, 10MP telephoto with 3x zoom, 50MP telephoto with 5x zoom",
                    "50MP main, 12MP ultrawide, 8MP telephoto with 3x optical zoom"
                ]
            ],
            [
                "Front Camera",
                ["12MP", "10MP"]
            ],
            [
                "Battery",
                [
                    "4500mAh with 25W wired charging",
                    "4900mAh with 45W wired charging",
                    "5000mAh with 45W wired charging",
                    "4000mAh with 25W wired charging"
                ]
            ],
            [
                "RAM",
                ["8GB", "12GB"]
            ],
            [
                "Storage",
                ["256GB internal", "128GB internal"]
            ],
            [
                "Operating System",
                [
                    "Android 14 with One UI 6.1",
                    "Android 13 upgradable to Android 14 with One UI 6.1",
                    "Android 14 with One UI 6.1.1"
                ]
            ],
            [
                "Product",
                [
                    "Samsung (Galaxy S24)",
                    "Samsung (Galaxy Z Flip6)",
                    "Samsung (Galaxy S24 Ultra)",
                    "Samsung (Galaxy S23 FE)",
                    "Samsung (Galaxy S24+)"
                ]
            ]
        ],
        "realworld_products": [
            {
                "Product": "Samsung (Galaxy S25)",
                "Price": "$799",
                "Display": "6.2-inch Dynamic LTPO AMOLED 2X with 120Hz refresh rate",
                "Processor": "Qualcomm Snapdragon 8 Elite",
                "RAM": "12 GB",
                "Storage": "128 GB",
                "Battery": "4000 mAh with 25W fast charging",
                "Rear Camera": "50 MP main, 12 MP ultra-wide, 10 MP telephoto",
                "Front Camera": "12 MP",
                "OS": "Android 15 with One UI 7",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-s25/"
            },
            {
                "Product": "Samsung (Galaxy S25+)",
                "Price": "$999",
                "Display": "6.7-inch Dynamic LTPO AMOLED 2X with 120Hz refresh rate",
                "Processor": "Qualcomm Snapdragon 8 Elite",
                "RAM": "12 GB",
                "Storage": "256 GB",
                "Battery": "4900 mAh with 45W fast charging",
                "Rear Camera": "50 MP main, 12 MP ultra-wide, 10 MP telephoto",
                "Front Camera": "12 MP",
                "OS": "Android 15 with One UI 7",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-s25-plus/"
            },
            {
                "Product": "Samsung (Galaxy S25 Ultra)",
                "Price": "$1,299",
                "Display": "6.8-inch Dynamic LTPO AMOLED 2X with 120Hz refresh rate",
                "Processor": "Qualcomm Snapdragon 8 Elite",
                "RAM": "12 GB",
                "Storage": "256 GB",
                "Battery": "5000 mAh with 45W fast charging",
                "Rear Camera": "200 MP main, 12 MP ultra-wide, 50 MP periscope telephoto, 10 MP telephoto",
                "Front Camera": "12 MP",
                "OS": "Android 15 with One UI 7",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-s25-ultra/"
            },
            {
                "Product": "Samsung (Galaxy Z Flip6)",
                "Price": "$999",
                "Display": "6.7-inch Foldable Dynamic AMOLED 2X main screen with 120Hz, 3.4-inch Super AMOLED cover screen",
                "Processor": "Qualcomm Snapdragon 8 Gen 3",
                "RAM": "12 GB",
                "Storage": "256 GB",
                "Battery": "4000 mAh with 25W fast charging",
                "Rear Camera": "50 MP main, 12 MP ultra-wide",
                "Front Camera": "10 MP",
                "OS": "Android 14 with One UI 6.1.1",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-z-flip6/"
            },
            {
                "Product": "Samsung (Galaxy S24 FE)",
                "Price": "$649",
                "Display": "6.7-inch Dynamic AMOLED 2X with 120Hz refresh rate",
                "Processor": "Exynos 2400e",
                "RAM": "8 GB",
                "Storage": "128 GB",
                "Battery": "4700 mAh with 25W fast charging",
                "Rear Camera": "50 MP main, 12 MP ultra-wide, 8 MP telephoto",
                "Front Camera": "10 MP",
                "OS": "Android 14 with One UI 6.1",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-s24-fe/"
            }
        ]
    }
)
```

**Parameters:**
- `why_prompt` (string, required): Your research question
- `country` (string, required): Target market
- `year` (string, required): Current year for the research
- `pre_cooked_attributes_and_levels_lookup` (array, required): List of `[attribute_name, [levels]]` pairs that define your product features
- `realworld_products` (array, required): Actual products with their specifications to test in the experiment

**Response:**
```json
{
    "wandb_run_id": "abc123xyz",
    "wandb_run_name": "samsung-smartphones-experiment"
}
```
**Note:** Experiments take approximately 10 minutes to complete. Save the `wandb_run_id` and `wandb_run_name` to retrieve results.

### Alternative: Using LinkedIn-Based Personas

You can also run experiments using LinkedIn profiles as personas instead of traditional demographic targeting. This approach leverages real professional profiles to create more authentic respondent simulations.

**Request with LinkedIn Personas:**
```python
response = httpx.post(
    f"{BASE_URL}/api/v1/experiments",
    headers=headers,
    json={
        "add_neither_option": False,
        "concept_description": "",
        "concept_statements": [
            {
                "labels": [
                    "Strongly Disagree",
                    "Disagree",
                    "Neutral",
                    "Agree",
                    "Strongly Agree"
                ],
                "statement": "Would taste great"
            },
            {
                "labels": [
                    "Strongly Disagree",
                    "Disagree",
                    "Neutral",
                    "Agree",
                    "Strongly Agree"
                ],
                "statement": "Would have good texture"
            },
            {
                "labels": [
                    "Strongly Disagree",
                    "Disagree",
                    "Neutral",
                    "Agree",
                    "Strongly Agree"
                ],
                "statement": "Would be something I, or others in my family, look forward to drinking"
            }
        ],
        "country": "United States",
        "do_research": False,
        "experiment_type": "conjoint",
        "external_personas": [
            "https://www.linkedin.com/in/data-dawn",
            "https://sg.linkedin.com/in/dipayans",
            "https://www.linkedin.com/in/david-robinson-3584642a",
            "https://www.linkedin.com/in/rob-arthur-data-science",
            "https://www.linkedin.com/in/baileymjoseph",
            "https://in.linkedin.com/in/ajha16",
            "https://www.linkedin.com/in/martha-wood",
            "https://www.linkedin.com/in/johngilling",
            "https://www.linkedin.com/in/sean-fischer-079aa937",
            "https://www.linkedin.com/in/jesserowlands",
            "https://www.linkedin.com/in/dpatil"
        ],
        "hb_folder": "",
        "hb_run_id": "",
        "image_name1": "milk-image.png",
        "image_name2": "",
        "is_private": False,
        "mnp_model": True,
        "population_traits": {
            "political affiliation": [
                "republican",
                "democrat"
            ]
        },
        "pre_cooked_attributes_and_levels_lookup": [
            [
                "Price",
                [
                    "999.99 USD",
                    "859.99 USD",
                    "1099.99 USD",
                    "1299.99 USD",
                    "649.99 USD"
                ]
            ],
            [
                "Display",
                [
                    "6.4-inch Dynamic AMOLED 2X display with 120Hz refresh rate",
                    "6.2-inch Dynamic LTPO AMOLED 2X display with 120Hz refresh rate",
                    "6.7-inch Dynamic LTPO AMOLED 2X display with 120Hz refresh rate",
                    "6.7-inch Foldable Dynamic AMOLED 2X main display with 120Hz refresh rate",
                    "6.8-inch Dynamic LTPO AMOLED 2X display with 120Hz refresh rate"
                ]
            ],
            [
                "Processor",
                [
                    "Qualcomm Snapdragon 8 Gen 2",
                    "Qualcomm Snapdragon 8 Gen 3 for Galaxy"
                ]
            ],
            [
                "Rear Camera",
                [
                    "50MP main, 12MP ultrawide",
                    "50MP main, 12MP ultrawide, 10MP telephoto with 3x optical zoom",
                    "200MP main, 12MP ultrawide, 10MP telephoto with 3x zoom, 50MP telephoto with 5x zoom",
                    "50MP main, 12MP ultrawide, 8MP telephoto with 3x optical zoom"
                ]
            ],
            [
                "Front Camera",
                [
                    "12MP",
                    "10MP"
                ]
            ],
            [
                "Battery",
                [
                    "4500mAh with 25W wired charging",
                    "4900mAh with 45W wired charging",
                    "5000mAh with 45W wired charging",
                    "4000mAh with 25W wired charging"
                ]
            ],
            [
                "RAM",
                [
                    "8GB",
                    "12GB"
                ]
            ],
            [
                "Storage",
                [
                    "256GB internal",
                    "128GB internal"
                ]
            ],
            [
                "Operating System",
                [
                    "Android 14 with One UI 6.1",
                    "Android 13 upgradable to Android 14 with One UI 6.1",
                    "Android 14 with One UI 6.1.1"
                ]
            ],
            [
                "Product",
                [
                    "Samsung (Galaxy S24)",
                    "Samsung (Galaxy Z Flip6)",
                    "Samsung (Galaxy S24 Ultra)",
                    "Samsung (Galaxy S23 FE)",
                    "Samsung (Galaxy S24+)"
                ]
            ]
        ],
        "realworld_products": [
            {
                "Battery": "4000 mAh with 25W fast charging",
                "Display": "6.2-inch Dynamic LTPO AMOLED 2X with 120Hz refresh rate",
                "Front Camera": "12 MP",
                "OS": "Android 15 with One UI 7",
                "Price": "$799",
                "Processor": "Qualcomm Snapdragon 8 Elite",
                "Product": "Samsung (Galaxy S25)",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-s25/",
                "RAM": "12 GB",
                "Rear Camera": "50 MP main, 12 MP ultra-wide, 10 MP telephoto",
                "Storage": "128 GB"
            },
            {
                "Battery": "4900 mAh with 45W fast charging",
                "Display": "6.7-inch Dynamic LTPO AMOLED 2X with 120Hz refresh rate",
                "Front Camera": "12 MP",
                "OS": "Android 15 with One UI 7",
                "Price": "$999",
                "Processor": "Qualcomm Snapdragon 8 Elite",
                "Product": "Samsung (Galaxy S25+)",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-s25-plus/",
                "RAM": "12 GB",
                "Rear Camera": "50 MP main, 12 MP ultra-wide, 10 MP telephoto",
                "Storage": "256 GB"
            },
            {
                "Battery": "5000 mAh with 45W fast charging",
                "Display": "6.8-inch Dynamic LTPO AMOLED 2X with 120Hz refresh rate",
                "Front Camera": "12 MP",
                "OS": "Android 15 with One UI 7",
                "Price": "$1,299",
                "Processor": "Qualcomm Snapdragon 8 Elite",
                "Product": "Samsung (Galaxy S25 Ultra)",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-s25-ultra/",
                "RAM": "12 GB",
                "Rear Camera": "200 MP main, 12 MP ultra-wide, 50 MP periscope telephoto, 10 MP telephoto",
                "Storage": "256 GB"
            },
            {
                "Battery": "4000 mAh with 25W fast charging",
                "Display": "6.7-inch Foldable Dynamic AMOLED 2X main screen with 120Hz, 3.4-inch Super AMOLED cover screen",
                "Front Camera": "10 MP",
                "OS": "Android 14 with One UI 6.1.1",
                "Price": "$999",
                "Processor": "Qualcomm Snapdragon 8 Gen 3",
                "Product": "Samsung (Galaxy Z Flip6)",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-z-flip6/",
                "RAM": "12 GB",
                "Rear Camera": "50 MP main, 12 MP ultra-wide",
                "Storage": "256 GB"
            },
            {
                "Battery": "4700 mAh with 25W fast charging",
                "Display": "6.7-inch Dynamic AMOLED 2X with 120Hz refresh rate",
                "Front Camera": "10 MP",
                "OS": "Android 14 with One UI 6.1",
                "Price": "$649",
                "Processor": "Exynos 2400e",
                "Product": "Samsung (Galaxy S24 FE)",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-s24-fe/",
                "RAM": "8 GB",
                "Rear Camera": "50 MP main, 12 MP ultra-wide, 8 MP telephoto",
                "Storage": "128 GB"
            }
        ],
        "respondent_instruction_for_concept": "Please respond to the following statements based on your impressions of the product concept described above.",
        "response_type": "discrete",
        "target_population": {
            "age": [
                18,
                95
            ],
            "education_level": [
                "Less than high school",
                "High School but no diploma",
                "High School Diploma",
                "Some College",
                "Associates",
                "Bachelors",
                "Masters",
                "PhD"
            ],
            "gender": [
                "Male",
                "Female"
            ],
            "household_income": [
                0,
                417800
            ],
            "number_of_children": [
                "1",
                "2",
                "3",
                "4+"
            ],
            "racial_group": [
                "White",
                "African American",
                "American Indian or Alaska Native",
                "Asian or Pacific Islander",
                "Mixed race",
                "Other race"
            ]
        },
        "use_external_personas": True,
        "use_halton_draws": False,
        "why_prompt": "There's been a lot of discussion about Samsung's recent innovations in display technology and camera systems, particularly with their Galaxy lineup. I'm interested in exploring Samsung's current smartphone offerings to understand how their latest models compare in terms of display quality, camera capabilities, and overall performance, especially since they've been positioning themselves as a premium alternative in the market.",
        "year": "2025"
    }
)
```

**Key Parameters for LinkedIn Personas:**
- `use_external_personas` (boolean, required): Set to `true` to enable LinkedIn persona mode
- `external_personas` (array, required): List of LinkedIn profile URLs to use as personas
- `concept_statements` (array, required): Survey questions with response options
- `respondent_instruction_for_concept` (string, required): Instructions for respondents
- `response_type` (string, required): Type of response collection ("discrete" for multiple choice)
- `experiment_type` (string, required): Type of experiment ("conjoint" for choice-based conjoint)

**Benefits of LinkedIn Personas:**
- **Authentic Profiles**: Uses real professional backgrounds for more realistic responses
- **Diverse Perspectives**: Leverages varied professional experiences and expertise
- **Targeted Demographics**: Select personas based on specific professional backgrounds
- **Real-world Context**: Responses reflect actual professional decision-making patterns

## 4. Retrieve Results
**Endpoint:** `GET /api/v1/runs/artifact/{file_name}`

Retrieve the results of your conjoint analysis experiment.

**Purpose:** Fetches the analytics output for your experiment using the wandb_run_name from the experiment response.

**Request:**
```python
file_name = f"Analytics_output_{result['wandb_run_name']}"
response = client.get(
    f"{BASE_URL}/api/v1/runs/artifact/{file_name}",
    headers=headers,
    timeout=300.0
)
```

**Parameters:**

`file_name` (string, required): The analytics file name in the format `Analytics_output_{wandb_run_name}`.

**Response:**

A JSON object or file containing the experiment results, including customer preference data and insights from the conjoint analysis.
**Note:** Ensure the experiment has completed (typically 10 minutes) before attempting to retrieve results. Poll the endpoint if necessary.
---

## Complete Example Workflow

```python
import httpx
import time

# Configuration
BASE_URL = "https://api.dev.subconscious.ai"
headers = {
    "Authorization": "Bearer YOUR_API_TOKEN"
}
client = httpx.Client(timeout=600)

# Define our research question
why_prompt = "There's been a lot of discussion about Samsung's recent innovations in display technology and camera systems, particularly with their Galaxy lineup. I'm interested in exploring Samsung's current smartphone offerings to understand how their latest models compare in terms of display quality, camera capabilities, and overall performance, especially since they've been positioning themselves as a premium alternative in the market."

# Step 1: Validate your research question
print("Step 1: Checking causality...")
causality_response = client.post(
    f"{BASE_URL}/api/v2/copilot/causality",
    headers=headers,
    json={
        "why_prompt": why_prompt
    }
)
print("Causality validated:", causality_response.json())

# Step 2: Generate attributes and levels
print("\nStep 2: Generating attributes...")
attributes_response = client.post(
    f"{BASE_URL}/api/v1/product-attributes-levels",
    headers=headers,
    json={
        "why_prompt": why_prompt,
        "attribute_count": 8,
        "level_count": 5,
        "country": "USA"
    }
)
attributes_data = attributes_response.json()
print("Attributes generated:", attributes_data)

# Step 3: Run the experiment
print("\nStep 3: Running experiment...")
experiment_response = client.post(
    f"{BASE_URL}/api/v1/experiments",
    headers=headers,
    json={
        "why_prompt": why_prompt,
        "country": "United States",
        "year": "2025",
        "pre_cooked_attributes_and_levels_lookup": [
            [
                "Price",
                ["$999.99", "$859.99", "$1099.99", "$1299.99", "$649.99"]
            ],
            [
                "Display",
                [
                    "6.4-inch Dynamic AMOLED 2X display with 120Hz refresh rate",
                    "6.2-inch Dynamic LTPO AMOLED 2X display with 120Hz refresh rate",
                    "6.7-inch Dynamic LTPO AMOLED 2X display with 120Hz refresh rate",
                    "6.7-inch Foldable Dynamic AMOLED 2X main display with 120Hz refresh rate",
                    "6.8-inch Dynamic LTPO AMOLED 2X display with 120Hz refresh rate"
                ]
            ],
            [
                "Processor",
                [
                    "Qualcomm Snapdragon 8 Gen 2",
                    "Qualcomm Snapdragon 8 Gen 3 for Galaxy"
                ]
            ],
            [
                "Rear Camera",
                [
                    "50MP main, 12MP ultrawide",
                    "50MP main, 12MP ultrawide, 10MP telephoto with 3x optical zoom",
                    "200MP main, 12MP ultrawide, 10MP telephoto with 3x zoom, 50MP telephoto with 5x zoom",
                    "50MP main, 12MP ultrawide, 8MP telephoto with 3x optical zoom"
                ]
            ],
            [
                "Front Camera",
                ["12MP", "10MP"]
            ],
            [
                "Battery",
                [
                    "4500mAh with 25W wired charging",
                    "4900mAh with 45W wired charging",
                    "5000mAh with 45W wired charging",
                    "4000mAh with 25W wired charging"
                ]
            ],
            [
                "RAM",
                ["8GB", "12GB"]
            ],
            [
                "Storage",
                ["256GB internal", "128GB internal"]
            ]
        ],
        "realworld_products": [
            {
                "Product": "Samsung (Galaxy S25)",
                "Price": "$799",
                "Display": "6.2-inch Dynamic LTPO AMOLED 2X with 120Hz refresh rate",
                "Processor": "Qualcomm Snapdragon 8 Elite",
                "RAM": "12 GB",
                "Storage": "128 GB",
                "Battery": "4000 mAh with 25W fast charging",
                "Rear Camera": "50 MP main, 12 MP ultra-wide, 10 MP telephoto",
                "Front Camera": "12 MP",
                "OS": "Android 15 with One UI 7",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-s25/"
            },
            {
                "Product": "Samsung (Galaxy S25+)",
                "Price": "$999",
                "Display": "6.7-inch Dynamic LTPO AMOLED 2X with 120Hz refresh rate",
                "Processor": "Qualcomm Snapdragon 8 Elite",
                "RAM": "12 GB",
                "Storage": "256 GB",
                "Battery": "4900 mAh with 45W fast charging",
                "Rear Camera": "50 MP main, 12 MP ultra-wide, 10 MP telephoto",
                "Front Camera": "12 MP",
                "OS": "Android 15 with One UI 7",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-s25-plus/"
            },
            {
                "Product": "Samsung (Galaxy S25 Ultra)",
                "Price": "$1,299",
                "Display": "6.8-inch Dynamic LTPO AMOLED 2X with 120Hz refresh rate",
                "Processor": "Qualcomm Snapdragon 8 Elite",
                "RAM": "12 GB",
                "Storage": "256 GB",
                "Battery": "5000 mAh with 45W fast charging",
                "Rear Camera": "200 MP main, 12 MP ultra-wide, 50 MP periscope telephoto, 10 MP telephoto",
                "Front Camera": "12 MP",
                "OS": "Android 15 with One UI 7",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-s25-ultra/"
            },
            {
                "Product": "Samsung (Galaxy Z Flip6)",
                "Price": "$999",
                "Display": "6.7-inch Foldable Dynamic AMOLED 2X main screen with 120Hz, 3.4-inch Super AMOLED cover screen",
                "Processor": "Qualcomm Snapdragon 8 Gen 3",
                "RAM": "12 GB",
                "Storage": "256 GB",
                "Battery": "4000 mAh with 25W fast charging",
                "Rear Camera": "50 MP main, 12 MP ultra-wide",
                "Front Camera": "10 MP",
                "OS": "Android 14 with One UI 6.1.1",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-z-flip6/"
            },
            {
                "Product": "Samsung (Galaxy S24 FE)",
                "Price": "$649",
                "Display": "6.7-inch Dynamic AMOLED 2X with 120Hz refresh rate",
                "Processor": "Exynos 2400e",
                "RAM": "8 GB",
                "Storage": "128 GB",
                "Battery": "4700 mAh with 25W fast charging",
                "Rear Camera": "50 MP main, 12 MP ultra-wide, 8 MP telephoto",
                "Front Camera": "10 MP",
                "OS": "Android 14 with One UI 6.1",
                "Product Web link": "https://www.samsung.com/us/smartphones/galaxy-s24-fe/"
            }
        ]
    }
)
result = experiment_response.json()
print(f"\nExperiment started!")
print(f"WandB Run ID: {result['wandb_run_id']}")
print(f"Run Name: {result['wandb_run_name']}")
print("\nResults will be available in ~10 minutes.")

# Step 4: Retrieve experiment results
print("\nStep 4: Waiting for experiment results...")
time.sleep(600)  # Wait 10 minutes for the experiment to complete
file_name = f"Analytics_output_{result['wandb_run_name']}"
results_response = client.get(
    f"{BASE_URL}/api/v1/runs/artifact/{file_name}",
    headers=headers,
    timeout=300.0
)
print("Experiment results:", results_response.json())

# Close the client
client.close()
```

---

## Tips for Success

1. **Start with Causality Check**: Always validate your research question first to ensure quality results
2. **Be Specific**: Write clear, detailed research questions that include context about your target audience and objectives
3. **Define Clear Levels**: When creating attributes, ensure levels are distinct and meaningful for comparison
4. **Match Data Structure**: Ensure your `pre_cooked_attributes_and_levels_lookup` covers all attributes present in your `realworld_products`
5. **Be Patient**: Experiments need time to gather statistically significant results - plan accordingly

---

## What Makes Subconscious AI Different?

- **Speed**: Conduct causal market research at unprecedented pace
- **Quality**: Higher quality results with ethical standards that surpass existing methodologies
- **Human-Level Reliability**: AI-powered insights you can trust
- **Comprehensive**: Covers Ideation, User Research, and Product Design in one platform

---

## Need Help?

For additional support and detailed documentation, visit our resources or contact the Subconscious AI team.

**Happy researching! 🚀**
