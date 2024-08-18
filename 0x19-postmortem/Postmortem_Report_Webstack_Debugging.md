# Postmortem: Web Application Outage Due to Memory Leak in Application Server

### Incident report for [504 error / Site Outage](https://github.com/oliversamuels/alx-system_engineering-devops/tree/master/0x17-web_stack_debugging_3)

## Issue Summary
### Duration of Outage:
Start: August 15, 2024, 10:00 AM UTC
End: August 15, 2024, 12:30 PM UTC

## Impact:
During the outage, the main web application experienced significant performance degradation, resulting in slow response times and complete unavailability for some users. Approximately 75% of active users were affected, experiencing timeouts or server errors when attempting to access the service.

## Root Cause:
The root cause was a memory leak in the application server, triggered by an inefficient handling of background tasks that gradually consumed all available memory, leading to system instability and eventual downtime.

## Timeline
- **10:05 AM UTC** - The issue was detected by automated monitoring systems, which triggered alerts due to unusually high memory usage and slow response times.
- **10:10 AM UTC** - Engineers began investigating the issue, initially suspecting a spike in traffic or a misconfigured server.
- **10:20 AM UTC** - The engineering team checked the network and database layers but found no abnormalities.
- **10:35 AM UTC** - Misleading assumption: the issue was thought to be related to a recent deployment, and the team initiated a rollback.
- **11:00 AM UTC** - Rollback completed, but the issue persisted. Memory usage continued to climb.
- **11:15 AM UTC** - The incident was escalated to the senior engineering team, who began a deeper investigation into the application server's performance metrics.
- **11:45 AM UTC** - The memory leak was identified as the root cause, traced to a background process handling user sessions inefficiently.
- **12:00 PM UTC** - The affected background task was disabled, and server memory was gradually restored.
- **12:30 PM UTC** - Full service was restored after memory stabilization.

## Root Cause and Resolution
### Root Cause:
The memory leak was caused by an inefficiently managed background task responsible for session handling. The task failed to release memory properly after processing, leading to a gradual buildup of memory consumption. As memory was exhausted, the application server became increasingly unresponsive until it could no longer handle requests.

### Resolution:
The issue was resolved by disabling the problematic background task, which immediately stopped the memory leak. The server's memory was gradually reclaimed, allowing the application to resume normal operation. A subsequent patch was developed to optimize the memory management in the background task, ensuring that it no longer leaks memory.

## Corrective and Preventative Measures
### Improvements:

* Implement more rigorous memory management practices in the background task code.
* Increase the frequency and sensitivity of monitoring for memory usage on all application servers.
* Establish a more robust rollback strategy that includes memory diagnostics.

### Tasks:

1 Patch Background Task Code: Refactor the session handling process to ensure proper memory deallocation.
2 Enhance Monitoring: Add detailed monitoring for memory usage, including alerts for gradual increases that could indicate leaks.
3 Conduct Post-Incident Review: Hold a detailed review session with the engineering team to learn from the incident and improve the response process.
4 Update Documentation: Document the incident and the resolution process for future reference, including the updated rollback procedures.
5 Load Testing: Perform load tests with simulated background tasks to ensure that similar issues do not arise in the future.