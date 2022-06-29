# Keptn install actions

This repository contains GitHub Actions for installing and uninstalling Keptn inside a given kubernetes context.

### Install Keptn

**Inputs**:
* `KEPTN_VERSION`: The version of Keptn that should be installed (e.g. `0.13.1`)
* `HELM_VALUES`: Helm values using during installation passed a properly formatted multiline YAML string.  Defaults to 
```yaml
      control-plane:
        apiGatewayNginx:
          type: LoadBalancer
      continuous-delivery:
        enabled: true
``` 
* `KUBECONFIG`: The location of the kubernetes configuration file. Defaults to `$HOME/.kube/config`.
* `UNINSTALL`: Set to `true` if the Keptn instance should be removed from the kubernetes cluster

**Outputs**:
* `KEPTN_HTTP_ENDPOINT`: Endpoint (host(:port)) on which the api gateway of the installed Keptn is reachable. Could be empty if it was not possible to autodetect it.
* `KEPTN_API_URL`: HTTP URL of the Keptn API endpoint. Could be empty if it was not possible to determine a KEPTN_ENDPOINT.
* `KEPTN_API_TOKEN`: A API token needed for the communication with Keptn

If the `control-plane.apiGatewayNginx.type` value is not `LoadBalancer` or `NodePort` the action does not return an endpoint since there's no way to determine an address (host:port pair) that would expose the Keptn api gateway to the outside of the kubernetes cluster.

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
