# Keptn install actions

This repository contains GitHub Actions for installing and uninstalling Keptn inside a given kubernetes context.

### Install Keptn

**Inputs**:
* `KEPTN_VERSION`: The version of Keptn that should be installed (e.g. `0.13.1`)
* `KEPTN_INSTALL_PARAMETERS`: Installation parameters that are used for installing Keptn. Defaults to `--endpoint-service-type=LoadBalancer --use-case=continuous-delivery`.
* `KUBECONFIG`: The location of the kubernetes configuration file. Defaults to `$HOME/.kube/config`.
* `UNINSTALL`: Set to `true` if the Keptn instance should be removed from the kubernetes cluster

**Outputs**:
* `KEPTN_ENDPOINT`: URI of the Keptn endpoint that can be used to communicate with Keptn
* `KEPTN_API_TOKEN`: A API token needed for the communication with Keptn

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
```
