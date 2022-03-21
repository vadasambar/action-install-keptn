# Keptn install actions
:warning: Work in progress :warning:
_________________

This repository contains GitHub Actions for installing and uninstalling Keptn inside a given kubernetes context.

## Actions

The following re-usable actions are available:

| Name                    | Filename                                     | Description                        | Inputs                    | Outputs     |
|-------------------------|----------------------------------------------|------------------------------------|---------------------------|-------------|
| Install Keptn     | `actions/install/action.yaml`                | Installs keptn in the current kubernets context    | See [Install Keptn (k3s)](#install-keptn-k3s) | See [Install Keptn (k3s)](#install-keptn-k3s)           |
| Uninstall Keptn         | `actions/uninstall/action.yaml`                     | Uninstalls keptn from the current kubernetes context | - | - |

### Install Keptn

**Inputs**:
* `KEPTN_VERSION`: The version of Keptn that should be installed (e.g. `0.13.1`)

**Outputs**:
* `KEPTN_ENDPOINT`: URI of the Keptn endpoint that can be used to communicate with Keptn
* `KEPTN_API_TOKEN`: A API token needed for the communication with Keptn

**Example Usage**:
```yaml
      - name: Install and start K3s
        run: curl -sfL https://get.k3s.io | sh -

      - name: Install Keptn
        id: install_keptn
        uses: keptn-sandbox/action-install-keptn/.github/actions/install@v1
        with:
          KEPTN_VERSION: "0.13.1"

      - name: Connect to keptn
        run: echo "connect to ${{ steps.install_keptn.KEPTN_ENDPOINT }} - ${{ steps.install_keptn KEPTN_API_TOKEN }}"
```

### Uninstall Keptn

**Example Usage**:
```yaml
      - name: Uninstall Keptn
        id: uninstall_keptn
        uses: keptn-sandbox/action-install-keptn/.github/actions/uninstall@v1
```
