<div id="top"></div>

<br />
<div align="center">
  <h1 align="center">Grafana</h1>
</div>

### Variables

```ts
//instance
label_values(nodejs_version_info{app_kubernetes_io_instance=~"^lazada-api-wrapper-.*", app_kubernetes_io_instance!~".*testing|.*staging" }, kubernetes_pod_name)

//app_kubernetes_io_instance
label_values(up{app_kubernetes_io_instance=~"^lazada-api-wrapper-.*",app_kubernetes_io_instance!~".*testing|.*staging"}, app_kubernetes_io_instance)

//httpPath
label_values(http_total{app_kubernetes_io_instance=~"^lazada-api-wrapper-.*", app_kubernetes_io_instance!~".*testing|.*staging" }, path)
```

### Query

```ts
// Latency
histogram_quantile(0.95, sum(rate(http_duration_seconds_bucket{path=~"$httpPath", app_kubernetes_io_instance=~"$app_kubernetes_io_instance"}[5m])) by (le, path, app_kubernetes_io_instance))

// Status response
sum by (path, statusCode, app_kubernetes_io_instance) (increase(http_total{path=~"$httpPath", app_kubernetes_io_instance=~"$app_kubernetes_io_instance"}[5m]))

// Statistic
sum by (path, status, statusCode) (http_total{path=~"$httpPath", app_kubernetes_io_instance=~"$app_kubernetes_io_instance"})
```

  <p align="right">(<a href="#top">Back to top</a>)</p>
