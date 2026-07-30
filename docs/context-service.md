# Optional Context Service integration

Serverless Harness can use an external Context Service to allocate a workload-scoped sandbox pool
and workspace. This integration is optional. Without it, existing `/runs` requests and static
sandbox-pool configuration behave as before.

Set `CONTEXT_SERVICE_URL` on the Knative service to enable the workload lifecycle routes:

```text
POST   /workloads
GET    /workloads/{workloadId}
DELETE /workloads/{workloadId}
```

`POST /workloads` asks Context Service to allocate the pool. A subsequent `/runs` request can pass
the returned `workloadId`; Serverless Harness resolves it to the pool selector before leasing and
executing in a sandbox. Deleting the workload asks Context Service to release its resources.

If `CONTEXT_SERVICE_URL` is unset, the workload lifecycle routes return
`501 context_service_not_configured`. `/runs` requests that omit `workloadId` continue to use the
existing `KAGENTI_SANDBOX_POOL_SELECTOR` configuration.
