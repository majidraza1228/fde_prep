# Microsoft / Azure / GitHub Copilot — FDE Interview Prep

## Topics to Cover

- [ ] Azure OpenAI — enterprise deployments, data residency, private endpoints
- [ ] Managed Identity — no API keys in code for AKS-hosted agents
- [ ] Content Safety + Prompt Shields — prompt injection defense
- [ ] GitHub Actions + OIDC — zero-credential CI/CD pipelines
- [ ] GitHub Copilot — enterprise deployment, extensions, vs custom agents
- [ ] Copilot for Azure — AI-assisted cloud management

---

## Area: Azure OpenAI

When a customer says "data can't leave our environment" — Azure OpenAI with a private endpoint is the answer.

```python
from openai import AzureOpenAI
from azure.identity import DefaultAzureCredential, get_bearer_token_provider

client = AzureOpenAI(
    azure_endpoint="https://your-resource.openai.azure.com",
    api_version="2024-02-01",
    azure_ad_token_provider=get_bearer_token_provider(
        DefaultAzureCredential(),
        "https://cognitiveservices.azure.com/.default"
    )
)

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Analyze this portfolio..."}]
)
```

| Feature | Anthropic API | Azure OpenAI |
|---|---|---|
| Data residency | Anthropic's servers | Your Azure region |
| Private networking | No | Yes — private endpoints |
| Authentication | API key | Managed Identity |
| Compliance | SOC2 | SOC2, HIPAA, FedRAMP |

---

## Area: Managed Identity — No API Keys in Code

```python
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

credential = DefaultAzureCredential()  # pod authenticates automatically

secret_client = SecretClient(
    vault_url="https://your-vault.vault.azure.net",
    credential=credential
)
api_key = secret_client.get_secret("anthropic-api-key").value
```

**Interview answer:** "In AKS we use Managed Identity — the pod authenticates to Azure automatically. Secrets live in Key Vault, not in code or environment variables."

---

## Area: Content Safety + Prompt Shields

Run on every user input BEFORE it reaches your agent:

```python
def has_prompt_injection(user_input: str) -> bool:
    response = safety_client.detect_text_prompt_injection(
        {"user_prompt": user_input}
    )
    return response.user_prompt_attack_result.detected

if has_prompt_injection(user_input):
    return "Input rejected — prompt injection detected"

result = run_agent(user_input)
```

---

## Area: GitHub Actions + OIDC (No Secrets in GitHub)

```yaml
permissions:
  id-token: write
  contents: read

jobs:
  deploy:
    steps:
      - name: Login to Azure (no secrets needed)
        uses: azure/login@v1
        with:
          client-id: ${{ vars.AZURE_CLIENT_ID }}
          tenant-id: ${{ vars.AZURE_TENANT_ID }}
          subscription-id: ${{ vars.AZURE_SUBSCRIPTION_ID }}

      - name: Deploy to AKS
        run: kubectl apply -f k8s/agent-deployment.yaml
```

---

## Area: GitHub Copilot

| Scenario | Use |
|---|---|
| Developer productivity, code completion | GitHub Copilot |
| Custom business workflow | Claude/GPT agent |
| Customer already on GitHub Enterprise | Copilot Extension |
| Data must stay in customer VPC | Azure OpenAI + private endpoint |

### Copilot Extension Endpoint

```python
@app.route("/agent", methods=["POST"])
def handle_copilot_request():
    user_message = request.json["messages"][-1]["content"]
    result = run_agent(user_message)
    return jsonify({"messages": [{"role": "assistant", "content": result}]})
```

---

## Area 6 Score: No hire

| Strong | Gap |
|---|---|
| Right instinct on Key Vault | Couldn't deploy to Azure |
| — | Named wrong tool for prompt injection |
| — | No code for either answer |

**What to practice:** Write the Managed Identity + Key Vault code and Prompt Shields check from memory.

---

## Reference Material

| Topic | URL |
|---|---|
| Azure OpenAI Docs | https://learn.microsoft.com/en-us/azure/ai-services/openai |
| Managed Identity | https://learn.microsoft.com/en-us/entra/identity/managed-identities-azure-resources/overview |
| Azure Content Safety | https://learn.microsoft.com/en-us/azure/ai-services/content-safety |
| GitHub Copilot Docs | https://docs.github.com/en/copilot |
| Copilot Extensions | https://docs.github.com/en/copilot/building-copilot-extensions |
| GitHub Actions OIDC | https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/about-security-hardening-with-openid-connect |
