---
layout: post-sidebar
title: Patent PDF API
permalink: /patent-pdf-api.html
show_sidebar: true
sidebar: toc
---

### Overview

The Patent PDF API allows for the download of PDF files for individual patent records based on the [Lens ID](https://support.lens.org/knowledge-base/the-lens-id/). Only patent records with [full text](https://link.lens.org/fHaai8x7XFk) have PDF files available, the API also allows the download of PDF files for patent family members if a PDF is not available for a given patent Lens Id. The API also provides an option to save the file with a custom file name.

### Usage Limits
The Trial Plan supports 5 requests per minute and 1,000 requests per month. The API can support higher rates and limits for use cases such as harvesting and real-time integration.

**Usage Endpoint:**
<pre>GET  https://api.lens.org/subscriptions/patent_image_api/usage</pre>


**Example Response:**
```json
[
	{
		"remaining": 998,
		"allowed": 1000,
		"frequency": "1 MONTH",
		"type": "REQUEST",
		"resetDate": "2026-11-20T03:05:15.287Z"
	},
	{
		"remaining": 4,
		"allowed": 5,
		"frequency": "1 MINUTE",
		"type": "REQUEST"
	}
]
```

> Note, only requests that return a PDF count toward your quota. Requests that return no PDF (for example, a 404 response) are not counted.


### Authentication
Include your API token with each request, using the `TOKEN` query parameter (shown below), or add the Authorization header: Bearer Token.

### Endpoint
<pre>GET  https://gateway.api.lens.org/images/patent/{lens_id}/pdf?token=TOKEN</pre>

Where `{lens_id}` is the Lens Identifier of a single patent record.

#### Optional: custom file name

Append a file name to set the name of the downloaded file:

<pre>GET  https://gateway.api.lens.org/images/patent/{lens_id}/pdf/{file_name.pdf}?token=TOKEN</pre>

Where `{file_name.pdf}` is the custom file name.

#### Optional: family fallback
If a record has no PDF, you can ask for a PDF from another member of its patent family.<br>Add `familyFallback=true`:

<pre>GET  https://gateway.api.lens.org/images/patent/056-313-321-891-799/pdf?token=TOKEN&familyFallback=true</pre>

In this example, the API returns the PDF for [050-702-013-216-303](https://www.lens.org/lens/patent/050-702-013-216-303/), a family member of the requested record. When this happens:

- The response includes the header `x-lens-family-fallback`.
- The file name is still the requested Lens ID ([056-313-321-891-799](https://www.lens.org/lens/patent/056-313-321-891-799)), not
  the ID of the family member.

If you store or cache files, check for this header and rename the file so it isn't mistaken for the requested record's own PDF, for example
`family_of_056-313-321-891-799.pdf`.



