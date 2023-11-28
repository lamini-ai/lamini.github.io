# POST `/v2/lamini/train/jobs/{job_id}/cancel`

Cancel a scheduled or running training job. You can also see these results by going to the `train` tab at [https://app.lamini.ai/train](https://app.lamini.ai/train)

## Request

**HTTP Method:** `POST`

**Path:** `https://api.lamini.ai/v2/lamini/train/jobs/{job_id}/cancel`

**Headers:**

- `Authorization: Bearer <LAMINI_API_KEY>`
- `Content-Type: application/json`

**Parameters:**

- `{job_id}` - The unique identifier of the training job to be cancelled.

## Response

**Body (JSON):**

- `message` - A message describing whether the job was successfully cancelled or not

```json
{ "message": "cancelled 418" }
```

## Example

This example cancels the training job with the ID `418`. The request is authenticated using the `LAMINI_API_KEY` bearer token.

### Request

```bash
curl --location --request POST 'https://api.lamini.ai/v2/lamini/train/jobs/418/cancel' \
--header 'Authorization: Bearer <LAMINI_API_KEY>' \
--header 'Content-Type: application/json'
```

### Response

```json
{
  "message": "cancelled 418"
}
```