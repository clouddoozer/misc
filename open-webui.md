# Open-WebUI
This guide provides the steps to deploy Open-WebUI as docker containers with bundled Ollama 

## Install Open WebUI
1. Install and enable WSL - https://learn.microsoft.com/en-us/windows/wsl/setup/environment 
   Remember to enable Docker
2. Enable GPU support - https://learn.microsoft.com/en-us/windows/wsl/tutorials/gpu-compute
3. Install open-webui as docker containers - https://docs.openwebui.com/getting-started/#quick-start-with-docker--recommended
   Run the command to enable GPU support
``` bash 
 docker run -d -p 3000:8080 --gpus=all -v ollama:/root/.ollama -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:ollama
```
4. To update following installation, use
``` bash 
docker run --rm --volume /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --run-once open-webui
```

## Add Open-WebUI to Continue.dev in VS code
1. Verify that you can access Open-WebUI on http://127.0.0.1:3000 
2. Generate an API key _(User -> Settings -> Account -> API keys -> API Key )_
3. Add the below JSON to you continue config file

``` json
  "models": [
    {
      "title": "Ollama Remote - codellama:7b",
      "provider": "openai",
      "model": "codellama:7b",
      "completionOptions": {},
      "contextLength": 8192,
      "apiBase": "http://127.0.0.1:3000/ollama/v1",
      "apiKey" :"<API Key>"
    }
  ],
  "customCommands": [
    {
      "name": "test",
      "prompt": "{{{ input }}}\n\nWrite a comprehensive set of unit tests for the selected code. It should setup, run tests that check for correctness including important edge cases, and teardown. Ensure that the tests are complete and sophisticated. Give the tests just as chat output, don't edit any file.",
      "description": "Write unit tests for highlighted code"
    }
  ],
  "tabAutocompleteModel": {
    "title": "Ollama Remote Autocomplete - codellama:7b",
    "provider": "openai",
    "model": "codellama:7b",
    "completionOptions": {},
    "contextLength": 8192,
      "apiBase": "http://127.0.0.1:3000/ollama/v1",
      "apiKey" :"<API Key>"
  },
```

_replace the API key the one obtained from step 2_
4. models to be used can be found here: https://ollama.com/ 