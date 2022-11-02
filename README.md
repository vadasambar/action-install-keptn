# Keptn install actions

This repository contains GitHub Actions for installing and uninstalling Keptn inside a given kubernetes context.

### Compatibility Matrix

| Keptn Version* | [action-install-keptn releases](https://github.com/keptn-sandbox/action-install-keptn/releases) |
|:--------------:|:-----------------------------------------------------------------------------------------------:|
|   0.15, 0.16   |                            keptn-sandbox/action-install-keptn@v2.0.0                            |
|     0.17.0     |                            keptn-sandbox/action-install-keptn@v3.0.0                            |
|     0.19.x     |                            keptn-sandbox/action-install-keptn@v4.0.0                            |

### Install Keptn

**Inputs**:
* `KEPTN_VERSION`: The version of Keptn that should be installed (e.g. `0.13.1`)
* `HELM_VALUES`: Helm values used during installation passed as properly formatted multiline YAML string.  Defaults to 
```yaml
      apiGatewayNginx:
        type: LoadBalancer
``` 
* `KUBECONFIG`: The location of the kubernetes configuration file. Defaults to `$HOME/.kube/config`.
* `UNINSTALL`: Set to `true` if the Keptn instance should be removed from the kubernetes cluster
* `KEPTN_HELM_CHART_REPO`: Helm repository from which Keptn helm chart is retrieved. Defaults to `https://charts.keptn.sh`.

**Outputs**:
* `KEPTN_HTTP_ENDPOINT`: Endpoint (host(:port)) on which the api gateway of the installed Keptn is reachable. Could be empty if it was not possible to autodetect it.
* `KEPTN_API_URL`: HTTP URL of the Keptn API endpoint. Could be empty if it was not possible to determine a KEPTN_ENDPOINT.
* `KEPTN_API_TOKEN`: A API token needed for the communication with Keptn

If the `apiGatewayNginx.type` value is not `LoadBalancer` or `NodePort` the action does not return an endpoint since there's no way to determine an address (host:port pair) that would expose the Keptn api gateway to the outside of the kubernetes cluster.

In such cases please refer to the [relevant keptn documentation](https://keptn.sh/docs/0.16.x/operate/install/#access-options) for your keptn version.

**Example Usage**:
```yaml
      - name: Install and start K3s
        run: curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE="644" sh -

      - name: Install Keptn
        id: install_keptn
        uses: keptn-sandbox/action-install-keptn@main
        with:
          KEPTN_VERSION: "0.13.1"
          KUBECONFIG: /etc/rancher/k3s/k3s.yaml

      - name: Connect to keptn
        run: echo "connect to ${{ steps.install_keptn.KEPTN_ENDPOINT }} - ${{ steps.install_keptn KEPTN_API_TOKEN }}"
```

### Uninstall Keptn

**Example Usage**:
```yaml
      - name: Uninstall Keptn
        id: uninstall_keptn
        uses: keptn-sandbox/action-install-keptn@main
        with:
          UNINSTALL: true
```
