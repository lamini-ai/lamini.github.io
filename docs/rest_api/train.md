# POST `/v2/lamini/train`

Use this API endpoint to train a model. This will train a model using the data you've provided through the `/v2/lamini/data` api, or through adding data to an `id` using the python package's [Paired data api](/rest_api/data_pairs/). The response will include a job id and the status of the job. You can monitor the job at [https://app.lamini.ai/train](https://app.lamini.ai/train).

## Request

**HTTP Method:** POST

**URL:** `https://api.lamini.ai/v2/lamini/train`

**Headers:**

- `Authorization: Bearer <LAMINI_API_KEY>`
- `Content-Type: application/json`

**Example Body (JSON):**

```json
{
  "id": "APIExample",
  "model_name": "EleutherAI/pythia-410m-deduped",
  "data": [
      [{"input": "Larry"}, {"output": 1.0}],
      [{"input": "Cici"}, {"output": 1.2}],
  ],
  "prompt_template": "{input:input}",
}
```

## Parameters:

- id (string): The `id` corresponding to the dataset you'd like to train with.
- model_name (string): The base model you'd like to train.
- data (list): The data you'd like to train on. This should be a list of [input, output] arrays. Each input and output should be an object. 
- prompt_template (string): The prompt template to use during training. For more information see [prompt templates](/deprecated/Concepts/prompt_templates).

## Response:

The response will include a job id and the status of the job. You can monitor the job at [https://app.lamini.ai/train](https://app.lamini.ai/train). There are a number of statuses possible, each representing a different stage in the training process.

```
{
    "job_id": "<JOB_ID>",
    "status": "SCHEDULED" | "CREATED" | "LOADING DATA" | "TRAINING MODEL" | "EVALUATING MODEL" | "SAVING MODEL" | "COMPLETED" | "FAILED"
}
```

## Example

### Request

```bash
curl --location 'https://api.lamini.ai/v2/lamini/train' \
--header 'Authorization: Bearer <LAMINI_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "id": "LaminiTest",
    "model_name": "EleutherAI/pythia-410m-deduped"
}'
```

### Response

```json
{
  "job_id": "1512",
  "status": "SCHEDULED"
}
```