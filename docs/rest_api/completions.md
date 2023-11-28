## Endpoint Documentation: `/v2/lamini/completions`

This endpoint allows you to make a POST request to complete a task or answer a question with a JSON output.

### Request

- HTTP Method: POST
- URL: `https://api.lamini.ai/v2/lamini/completions`
- Headers:
  - `Authorization: Bearer <LAMINI_API_KEY>`
  - `Content-Type: application/json`
- Example Body (JSON):


```json
{
    "id": "<YOUR_LLMENGINE_ID>",
    "model_name": "<YOUR_MODEL_NAME>",
    "in_value": {"question": "What is the hottest day of the year?"},
    "out_type": {"Answer": "string"},
    "prompt_template": "{input:question}",
    "stop_tokens": ["\n\n"],
}
```

#### Parameters:

-   `id`: `str`, an id which will allow you to iterate on finetuned models
-   `model_name`: `str`, the name of your base or finetuned model
-   `in_value`: `Dict[str, T]`, where `T` can be `str`, `int`, `float` or `bool`. Ex.
    ```
        {
            "question": "What is the hottest day of the year?",
            "question2": "2+2",
        }
    ```
-   `out_type`: `Dict[str, str]`. Type Schema of the output. Ex.
    ```
        {
            "Answer": "str",
            "Answer2": "int",
        }
    ```
    The valid types are `str`, `int`, `float`, and `bool`.
-   `prompt_template`: `str`, the template to use when passing the input to the model. For more information see [prompt templates](/deprecated/Concepts/prompt_templates).
-   `stop_tokens`: `list[str]`, a list of stop tokens to use. these are used in each field produced in the output.

### Response

If the web request is successful, you will see a response with an answer to the provided questions like below:

- Success Status Code: 200
- Body (JSON):
  - Output will be formatted as specified by the `out_type` argument passed in above. 
  ```json
     {
       "Answer": "The hottest day of the year varies depending on the location, but generally, it occurs during the summer months when the sun is closest to the Earth. In many regions, July or August tend to be the hottest months.",
       "Answer2": 4,
     }
  ```

Otherwise, the request will return an error code, and the response json will contain specific error details like invalid token or incompatible data.


### Example

#### Request

```bash
curl --location 'https://api.lamini.ai/v2/lamini/completions' \
--header 'Authorization: Bearer <LAMINI_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "id": "LaminiTest",
    "model_name": "meta-llama/Llama-2-7b-chat-hf",
    "in_value": {
        "question": "What is the hottest day of the year?",
        "question2": "What is for lunch?"
    },
    "out_type": {
        "Answer": "str",
        "Answer2": "str"
    }
}'
```

#### Response

Note the result is a hash, so the order of keys may be different from below.

```json
{
 "Answer":"The hottest day of the year varies depending on location.",
 "Answer2": "Lunch options depend on individual preferences.",
}
```