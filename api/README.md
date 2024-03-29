# Getting Started!

Welcome to Subconscious AI, where we are redefining the landscape of market research and product development through our revolutionary "Behaviour Change as a Service" model. Utilizing the cutting-edge capabilities of Large Language Models (LLMs), we empower businesses to conduct Causal Market Research at an unprecedented pace, ensuring higher quality and ethical standards that surpass existing methodologies. With our commitment to human-level reliability, we stand at the forefront of guiding businesses through the critical steps of Ideation, User Research, and Product Design. Subconscious AI is not just a tool; it's a game-changer in understanding and influencing consumer behavior.

To simulate a causal market research, our system employs the following steps:
Design a Causal Prompt pertaining to the task  -> Generate/Design attributes and their levels for the task -> Run the experiment
It is as simple as that! But before we start, remember to generate an API token for authorization purposes. So, 

### Step 1

```

conn = http.client.HTTPSConnection("auth.subconscious.ai")

username = "<username>"
password = "<password>"
audience = "https://dev-5qhuxyzkmd8cku6i.us.auth0.com/api/v2/"
client_id = "MR5gS0QSe3boMNAtnR0t1t2ctNpzSHsd"
client_secret = "c1GGYa9U7gbX5dvDR8pRHvmmY067InwLi0HYZMNXzKg9zn99RvNf13ibsDzT2jKV"

payload = f"grant_type=password&username={username}&password={password}&audience={audience}&scope=read:current_user&client_id={client_id}&client_secret={client_secret}"
headers = { 'content-type': "application/x-www-form-urlencoded" }

conn.request("POST", "/oauth/token", payload, headers)
res = conn.getresponse()
data = json.loads(res.read().decode("utf-8"))

```

Now, you can adjust your headers for all future calls to the API by adding the token we obtained above

```
access_token=data['access_token']
headers = {'Content-Type':'application/json',"Authorization": "Bearer %s" %access_token}

```

The next step involves defining our causal prompt, which will serve as the foundation for setting up our task. To assist you in crafting this prompt, we offer a helpful 'check-causality' feature!


### Step 2

```

prompt = "I would like to design a new electric car for the American markets"

response = requests.post("https://api.subconscious.ai/copilot/check-causality", headers=headers, params=params)
isCausal=json.loads(response.content)

if (not isCausal['is_causal']):
  prompt = isCausal['suggestions'][0]

```

After selecting the prompt, we progress to constructing our attributes and their corresponding levels, which define the scale of each attribute. For instance, consider a car: "Price" could be an essential attribute, with a range from $10,000 to $100,000. You have the flexibility to access a more detailed set of levels through our API, or you can opt to define these levels yourself!

### Step 3

```

data={
  "idea": prompt,
  "prompt_type": "product",
  "num_levels": 3,
  "max_length": 80,
  "num_attrs": 3,
  "model_type": "gpt4"
}

response = requests.post("https://api.subconscious.ai/levels", headers=headers, json=data)
levels=json.loads(response.content)


```

Finally, we can now run our experiment! But first, we need to setup the data to run the experiment.

### Step 4

```

exp_data={
  "number_of_attributes": 2,
  "number_of_levels": 3,
  "pre_cooked_attributes_and_levels_lookup": attributes,
  "where_preamble": "United States",
  "when_preamble": "2023",
  "experimentor_why_question_type": "generic",
  "experimentor_why_question_prompt": prompt,
  "number_of_respondents": 75,
  "number_of_tasks_per_respondent": 10,
  "model_type": "default",
  "levels_per_trait": 2,
  "null_levels": True,
  "max_length": 40,
  "add_price_and_brand_attributes": False,
  "hb_run_id": "",
  "paper_data": [],
  "is_private": False
}

response = requests.post("https://api.subconscious.ai/experiments", headers=headers, json=exp_data)
experiment=json.loads(response.content)

```

The provided snippet gives a WandB (Weights & Biases) ID and run name, enabling you to visualize the experiment run and view the result parameters. These details can be easily accessed in your code by utilizing our runs endpoint.

```

url = "https://api.subconscious.ai/runs/" + experiment['wandb_run_id']
response = requests.post(url, headers=headers)
metrics = json.loads(response.content)

```

That's it! You just ran your first experiment! 
You can further customize your experiment by consulting the Swagger UI documentation, accessible through the provided <link>. Should you have any additional questions or need further assistance, please feel free to contact us at <email>.
