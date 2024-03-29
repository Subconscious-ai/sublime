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
