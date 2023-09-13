# Example OpenAI API request

---

[Here is a video for this lesson](https://share.descript.com/view/6eAkjLEG9lB). You should not rely entirely on the video. PLEASE READ the lesson as you are going through the steps, since there are additional details in the text.

---

In order to interact with OpenAI's chat completions API, we must send HTTP requests that look like this:

```http
POST /v1/chat/completions HTTP/1.1
Host: api.openai.com
Authorization: Bearer OUR_SECRET_API_TOKEN_GOES_HERE
content-type: application/json
Content-Length: <calculated when request is sent>

{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "system",
      "content": "You are a helpful assistant who talks like Shakespeare."
    },
    {
      "role": "user",
      "content": "Hello! What are the best spots for pizza in Chicago?"
    }
  ]
}
```

- It's a `POST` request.
- The resource path is `/v1/chat/completions`.
- The host is `api.openai.com`.
- Our API token must be in an `Authorization` header after `"Bearer "`.
- The `Content-Type` header must be set to `application/json`.
- The body of the request must be a string containing JSON.
  - The JSON must have a key `"model"` with a string (e.g. "gpt-3.5-turbo" or "gpt-4")
  - The JSON must have a key `"messages"` with an array of hashes.
  - Each hash must have keys `"role"` and `"content"`.

To put together a request like this in Ruby using the `http.rb` gem:

```ruby
require "http"
require "json"

request_headers_hash = {
  "Authorization" => "Bearer #{ENV.fetch("OPENAI_API_KEY")}",
  "content-type" => "application/json"
}

request_body_hash = {
  "model" => "gpt-3.5-turbo",
  "messages" => [
    {
      "role" => "system",
      "content" => "You are a helpful assistant who talks like Shakespeare."
    },
    {
      "role" => "user",
      "content" => "Hello! What are the best spots for pizza in Chicago?"
    }
  ]
}

request_body_json = JSON.generate(request_body_hash)

raw_response = HTTP.headers(request_headers_hash).post(
  "https://api.openai.com/v1/chat/completions",
  :body => request_body_json
).to_s

parsed_response = JSON.parse(raw_response)

pp parsed_response
```
{: .repl #example_request title="Example request" points="1"}

You can add as many elements into the `messages` array as you like; that's how you keep the conversation going.
