# Rockwell Razors — Order Processor (Technical Test Part 1)

## Overview

erp-clients.ts -> line 30

When running the tests, the output showed No ERP mapping found for SKU: RW-LASTPAGE-SS. 
This SKU exists in the mock ERP but only on page 2. The fact that the handler couldn't find it pointed 
to a pagination issue — tracing back to erp-client.ts revealed the > 50 condition was stopping one page early, 
so when page 1 returned exactly 50 results the loop exited without ever fetching page 2.

handler.ts -> line 66

missing catch, this leaves skuMappings undefined, properties cannot be read

handler.ts -> line 112

missing await, handler returned before dynamodb update is finished
