# Authenticating with Azure OpenAI models, using Entra ID (former Azure Active Directory).

Azure OpenAI service supports 2 authentication methods:
- _using API key_, when you authenticate with your Azure OpenAI endpoint's API key
- _using Entra ID_, when you authneticate with a temporary bearer token.

While API keys are simpler and easier to use, some clients may prefer the use of Entra ID bearer tokens because of security requirements.

In this repo I'll demo the use of the latest openai Python package v1.x, that was released in November 2023. And, particularly, on how to use the new **azure_ad_token_provider** parameter of **AzureOpenAI** class.

To use the latest version of *openai* python package, you can upgrade it wth the following pip command:
```
pip install --upgrade openai
```

## Table of contents:
- [Scenario 1: Authenticating with API Key]()
- [Scenario 2: Authenticating with Entra ID - Interactive Login]()
- [Scenario 3: Authenticating with Entra ID - Service Principal]()

## Scenario 1: Authenticating with API Key
1. To use API key authentication, set API endpoint name, version and key, along with the Azure OpenAI deployment name to **OPENAI_API_BASE**, **OPENAI_API_VERSION**, **OPENAI_API_KEY** and **OPENAI_API_DEPLOY** environment variables.
![screenshot_1.1_environment](images/api_1_environment.png)
2. 

## Scenario 2: Authenticating with Entra ID - Interactive Login


## Scenario 3: Authenticating with Entra ID - Service Principal

