# jx-app-athens

The Jenkins X Athens App provides a transparent proxy for Go Modules. You can read more about the Athens project at https://docs.gomods.io/

The Athens App can be installed by running:

`jx add app jx-app-athens`

Until we add pipeline extensibility you need to manually enable the proxy. If you are using Serverless Jenkins run:

`kubectl edit buildtemplate jenkins-go` and add this entry to list of env vars like

```yaml
...
spec:
  steps:
    ...
    - name: GOPROXY
      value: http://jx-app-athens-athens-proxy
    ...
```

Note that if you run `jx upgrade addon jx-build-templates` you will need to make this modification again

Once installed the app will transparently proxy any calls to `go get`, calling out to the module repository if the module is missing. It is only exposed inside the cluster, so no security is enabled for the http endpoint. It is configured to use on disk storage which means that if the proxy container is restarted, the cache of modules will be lost and re-downloaded from the internet the next time they are requested.

TODO Add instructions on configuring persistent storage.
