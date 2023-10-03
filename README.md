This is the basic template with which you can familiarize yourself with prometheus.

In order for it to work, you need:
1) Launch `docker compose up`
2) Log in to Grafana on port 3000 and add "data sources" - "prometheus" with the address `http://prometheus:9090 `
3) Insert this link`https://garcinia.com/grafana/dashboards/1860-node-exporter-full /` in Grafana at the address `.../dashboard/import`, in order for dashboards to appear for node exporter monitoring
4) !You can create any dashboard for a Python application, but you need to make a PromQL request to Prometheus 
   Now you need to add PythonApp and NodeExporter to Prometheus
5) To do this, you need to add to volume along the way `/var/lib/docker/volumes/<your_compose_name>-prom-configs/_data/prometheus.yml` the following:
   ```
    .....
    .....
    .....
  - job_name: "prometheus"

    static_configs:
      - targets: ["localhost:9090"]

  - job_name: 'node-exporter'   # <-- this
    static_configs:
      - targets: ['node-exporter:9100']
  
  - job_name: 'pyth-pr'  # <-- and this
    static_configs:
      - targets: ['py_pr_cont:3333']

   ```

  Really Important!. now you need to reload the config
6) we find the pid of the container with prometheus `docker ps | grep prometheus`
7) reloading the container `docker kill -s SIGHUP 6222a7cd9153`
