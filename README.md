# Authenticating with Azure OpenAI models, using Entra ID (former Azure Active Directory).

Azure OpenAI service supports 2 authentication methods:
- _using API key_, when you authenticate with your Azure OpenAI endpoint's API key
- _using Entra ID_, when you authneticate with a temporary access token.

While API keys are simpler and easier to use, some clients may prefer the use of Entra ID bearer tokens because of internal security requirements.

In this repo I'll demo the use of the latest openai Python package v1.x, that was released in November 2023. And, particularly, on how to use the new **azure_ad_token_provider** parameter of **AzureOpenAI** class.

To use the latest version of *openai* python package, you can upgrade it wth the following pip command:
```
pip install --upgrade openai
```

## Table of contents:
- [Scenario 1: Authenticating with API Key](https://github.com/LazaUK/AOAI-EntraIDAuth-SDKv1/tree/main#scenario-1-authenticating-with-api-key)
- [Scenario 2: Authenticating with Entra ID - Interactive Login]()
- [Scenario 3: Authenticating with Entra ID - Service Principal]()

## Scenario 1: Authenticating with API Key
1. To use API key authentication, set API endpoint name, version and key, along with the Azure OpenAI deployment name to **OPENAI_API_BASE**, **OPENAI_API_VERSION**, **OPENAI_API_KEY** and **OPENAI_API_DEPLOY** environment variables.
![screenshot_1.1_environment](images/api_1_environment.png)
2. Now you can instantiate AzureOpenAI client and pass environment variable values to the relevant parameters.
``` Python
client = AzureOpenAI(
    azure_endpoint = os.getenv("OPENAI_API_BASE"),
    api_key = os.getenv("OPENAI_API_KEY"),
    api_version = os.getenv("OPENAI_API_VERSION")
)
```
3. Calling Chat Completions API will pass your API key to Azure OpenAI endpoint.
``` Python
response = client.chat.completions.create(
    model = os.getenv("OPENAI_API_DEPLOY"), # model = "Azure OpenAI deployment name".
    messages = [
        {"role": "system", "content": "You are a friendly chatbot"},
        {"role": "user", "content": "Choose a random planet and describe it to me in 3 sentences."}
    ]
)
```
4. And it should generate relevant completion.
``` JSON
Neptune is the eighth and farthest known planet from the Sun in our solar system. It is a giant planet composed primarily of hydrogen and helium, with traces of methane, giving it a striking blue appearance. Neptune has a dynamic atmosphere with the fastest winds in the solar system, reaching speeds of over 1,100 miles per hour, and a series of dark spots caused by storm activities.
```

## Scenario 2: Authenticating with Entra ID - Interactive Login
1. To use interactive authentication with Entra ID, import **InteractiveBrowserCredential** class **and get_bearer_token_provider** function from azure.identity package and instantiate your token provider
``` Python
token_provider = get_bearer_token_provider(
    InteractiveBrowserCredential(),
    "https://cognitiveservices.azure.com/.default"
)
```
2. Ensure that you have "**Cognitive Service OpenAI User**" role assigned to yourself on Azure OpenAI resource.
3. Now you can instantiate AzureOpenAI client and set **azure_ad_token_provider** parameter to your token provider from Step 2.1 above.
``` Python
client = AzureOpenAI(
    azure_endpoint = os.getenv("OPENAI_API_BASE"),
    azure_ad_token_provider = token_provider,
    api_version = os.getenv("OPENAI_API_VERSION")
)
```
4. Calling Chat Completions API will open a new browser window for you to login with your Azure account.
``` Python
response = client.chat.completions.create(
    model = os.getenv("OPENAI_API_DEPLOY"), # model = "Azure OpenAI deployment name".
    messages = [
        {"role": "system", "content": "You are a friendly chatbot"},
        {"role": "user", "content": "Choose a random flower and describe it to me in 3 sentences."}
    ]
)
```
5. 

## Scenario 3: Authenticating with Entra ID - Service Principal

