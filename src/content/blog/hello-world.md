---
title: "Zapier's Webhooks module won't send nested JSON — here's the fix"
description: "If you're using Zapier's POST action to send nested JSON to an API, it won't work. You need Custom Request. Here's what I learned."
pubDate: 2024-12-15
tags: ["zapier", "api", "webhook", "debugging"]
draft: false
---

I wasted an hour on this, so maybe I can save you the same.

## The problem

I had a Zapier automation that needed to send a webhook to an API that expected nested JSON in the request body. Something like:

```json
{
  "client": {
    "name": "Acme Corp",
    "contact": {
      "email": "jane@acme.com",
      "phone": "555-0100"
    }
  },
  "service": "tax-preparation"
}
```

I used Zapier's built-in **Webhooks by Zapier > POST** action. Set the payload type to JSON. Mapped all the fields. Looked fine in the editor.

But the receiving API kept rejecting it with a 400 error. The body it received was flat — all the nesting was gone.

## Why it happens

Zapier's standard POST action flattens your data. It doesn't support nested objects in the payload builder UI. Even if you think you're building nested JSON, you're not — Zapier serializes everything as top-level key-value pairs.

## The fix

Use **Webhooks by Zapier > Custom Request** instead. This gives you a raw body field where you can write actual JSON:

1. Set the method to POST
2. Set the URL
3. Add a header: `Content-Type: application/json`
4. In the **Body** field, write your JSON manually, using Zapier field references where needed

```json
{
  "client": {
    "name": "{{client_name}}",
    "contact": {
      "email": "{{client_email}}",
      "phone": "{{client_phone}}"
    }
  },
  "service": "{{service_type}}"
}
```

The Custom Request action sends the body exactly as you write it. Nested objects, arrays, whatever — it all works.

## One thing to watch out for

If any of your Zapier field values contain double quotes or special characters, your JSON will break. You might need to add a Code by Zapier step before the webhook to sanitize the values, or use `JSON.stringify()` in a code step to build the body programmatically.

That's it. Standard POST action: flat. Custom Request: actual JSON. The Zapier docs don't make this obvious.
