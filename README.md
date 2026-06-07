# Rockwell Razors — Order Processor (Technical Test Part 1)

## Overview

erp-clients.ts -> line 30

When running the tests, the output showed No ERP mapping found for SKU: RW-LASTPAGE-SS. 
This SKU exists in the mock ERP but only on page 2. The fact that the handler couldn't find it pointed 
to a pagination issue — tracing back to erp-client.ts revealed the > 50 condition was stopping one page early, 
so when page 1 returned exactly 50 results the loop exited without ever fetching page 2.

handler.ts -> line 66

noticed that the properties cannot be read, we can see that it's declared when the try is successful, but
if getSkuMappings() throws an error for any reason, SkuMapping will return undefined.

missing catch, this leaves skuMappings undefined, properties cannot be read

handler.ts -> line 112

the test has it set so the boolean is `true` after DynamoDB update finishes. After calling it the flag is still `false`,
meaning the handler was done before the update was completed

missing await, handler returned before dynamodb update is finished
