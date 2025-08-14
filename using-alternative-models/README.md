## Install dependency
pip install litellm[proxy]

installing litellm[proxy] to get all the required dependencies

## Create litellm.yaml:
model_list:

  - model_name: gpt-oss-20b
    litellm_params:
      model: openai/gpt-oss-20b
      api_key: xxxx
      api_base: https://openrouter.ai/api/v1
  - model_name: qwen3-coder
    litellm_params:
      model: openrouter/qwen/qwen3-coder
      api_key: xxxxx
      api_base: https://openrouter.ai/api/v1


## Start the proxy:
litellm --config litellm.yaml

## Export Variables:

export ANTHROPIC_BASE_URL="http://localhost:4000"
export ANTHROPIC_AUTH_TOKEN="litellm_master"
export ANTHROPIC_MODEL="gpt-oss-20b"

## Code Generation
claude --model gpt-oss-20b "Write a Python REST API with Flask"