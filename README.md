## Handle Multiple Azure Deployments via OpenAI Proxy Server ([Docs](https://docs.litellm.ai/docs/routing))
Use [LiteLLM's](https://github.com/BerriAI/litellm) OpenAI proxy server to handle multiple azure deployments, behind 1 fixed endpoint.

1. Clone Repo 

```shell
git clone https://github.com/BerriAI/litellm.git
```

2. Add Azure/OpenAI deployments to `secrets_template.toml`

```python
[model."gpt-3.5-turbo"] # model name passed in /chat/completion call or `litellm --model gpt-3.5-turbo`
model_list = [{ # list of model deployments 
    "model_name": "gpt-3.5-turbo", # openai model name 
    "litellm_params": { # params for litellm completion/embedding call 
        "model": "azure/chatgpt-v-2", 
        "api_key": "my-azure-api-key-1",
        "api_version": "my-azure-api-version-1",
        "api_base": "my-azure-api-base-1"
    },
    "tpm": 240000,
    "rpm": 1800
}, {
    "model_name": "gpt-3.5-turbo", # openai model name 
    "litellm_params": { # params for litellm completion/embedding call 
        "model": "gpt-3.5-turbo", 
        "api_key": "sk-...",
    },
    "tpm": 1000000,
    "rpm": 9000
}]
```

3. Run with Docker Image
```shell
docker build -t litellm . && docker run -p 8000:8000 litellm

## OpenAI Compatible Endpoint at: http://0.0.0.0:8000
```

**replace openai base**

```python
OPENAI_API_HOST = "http://0.0.0.0:8000"
```

# Chatbot UI

## News

Chatbot UI 2.0 is out as an updated, hosted product!

Check out [Takeoff Chat](https://www.takeoffchat.com/).

Open source version coming soon!

## About

Chatbot UI is an open source chat UI for AI models.

See a [demo](https://twitter.com/mckaywrigley/status/1640380021423603713?s=46&t=AowqkodyK6B4JccSOxSPew).

![Chatbot UI](./public/screenshots/screenshot-0402023.jpg)

## Updates

Chatbot UI will be updated over time.

Expect frequent improvements.

**Next up:**

- [ ] Sharing
- [ ] "Bots"

## Deploy

**Vercel**

Host your own live version of Chatbot UI with Vercel.

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fmckaywrigley%2Fchatbot-ui)

**Docker**

Build locally:

```shell
docker build -t chatgpt-ui .
docker run -e OPENAI_API_KEY=xxxxxxxx -p 3000:3000 chatgpt-ui
```

Pull from ghcr:

```
docker run -e OPENAI_API_KEY=xxxxxxxx -p 3000:3000 ghcr.io/mckaywrigley/chatbot-ui:main
```

## Running Locally

**1. Clone Repo**

```bash
git clone https://github.com/mckaywrigley/chatbot-ui.git
```

**2. Install Dependencies**

```bash
npm i
```

**3. Provide OpenAI API Key**

Create a .env.local file in the root of the repo with your OpenAI API Key:

```bash
OPENAI_API_KEY=YOUR_KEY
```

> You can set `OPENAI_API_HOST` where access to the official OpenAI host is restricted or unavailable, allowing users to configure an alternative host for their specific needs.

> Additionally, if you have multiple OpenAI Organizations, you can set `OPENAI_ORGANIZATION` to specify one.

**4. Run App**

```bash
npm run dev
```

**5. Use It**

You should be able to start chatting.

## Configuration

When deploying the application, the following environment variables can be set:

| Environment Variable              | Default value                  | Description                                                                                                                               |
| --------------------------------- | ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| OPENAI_API_KEY                    |                                | The default API key used for authentication with OpenAI                                                                                   |
| OPENAI_API_HOST                   | `https://api.openai.com`       | The base url, for Azure use `https://<endpoint>.openai.azure.com`                                                                         |
| OPENAI_API_TYPE                   | `openai`                       | The API type, options are `openai` or `azure`                                                                                             |
| OPENAI_API_VERSION                | `2023-03-15-preview`           | Only applicable for Azure OpenAI                                                                                                          |
| AZURE_DEPLOYMENT_ID               |                                | Needed when Azure OpenAI, Ref [Azure OpenAI API](https://learn.microsoft.com/zh-cn/azure/cognitive-services/openai/reference#completions) |
| OPENAI_ORGANIZATION               |                                | Your OpenAI organization ID                                                                                                               |
| DEFAULT_MODEL                     | `gpt-3.5-turbo`                | The default model to use on new conversations, for Azure use `gpt-35-turbo`                                                               |
| NEXT_PUBLIC_DEFAULT_SYSTEM_PROMPT | [see here](utils/app/const.ts) | The default system prompt to use on new conversations                                                                                     |
| NEXT_PUBLIC_DEFAULT_TEMPERATURE   | 1                              | The default temperature to use on new conversations                                                                                       |
| GOOGLE_API_KEY                    |                                | See [Custom Search JSON API documentation][GCSE]                                                                                          |
| GOOGLE_CSE_ID                     |                                | See [Custom Search JSON API documentation][GCSE]                                                                                          |

If you do not provide an OpenAI API key with `OPENAI_API_KEY`, users will have to provide their own key.

If you don't have an OpenAI API key, you can get one [here](https://platform.openai.com/account/api-keys).

## Contact

If you have any questions, feel free to reach out to Mckay on [Twitter](https://twitter.com/mckaywrigley).

[GCSE]: https://developers.google.com/custom-search/v1/overview
