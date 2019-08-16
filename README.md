# Jupyter-OpenShift

This template deploys [JupyterHub](https://jupyter.org/hub) in [Rahti](https://rahti.csc.fi/), CSC's Kubernetes cluster. JupyterHub makes it easier to host custom Jupyter notebooks. The default deployment uses latest Jupyter Hub & Jupyter notebooks (Python 3.5 variant) OpenShift compatible image hosted at quay.io. Other OpenShift compatible Jupyter Hub and/or notebook images for the deployment as well. The template creates JupyterHub, JupyterHub Database pod and a PVC for storing JupyterHub DB. A new Jupyter Notebook instance is spawned up in form of pod for each new user session. Jupyter notebook instances created use ephemeral storage.

## Usage

### Using Rahti Web Interface
1. Create a new project with the "Create Project" button.
  * Add project description as your CSC project for ex. "csc_project: project_100123"
2. Select the new project by clicking its name under "My Projects".
3. Click the "Import YAML/JSON" button.
4. Upload JupyterHub.yaml or copy & paste its content.
5. Click Create, You can optionally check the checkbox "Save Template" which will make this template available for others with in your Rahti project.
6. Continue & provide [parameters](#parameters-to-be-supplied) values, finally click create.
7. Once deployment is successful, Click on "Application > Routes" to get URL of your deployed JupyterHub instance.

### Using OC Command Line tool
1. Download & Install OC CLI tool. [Instructions.](https://docs.okd.io/latest/cli_reference/get_started_cli.html)
2. Login to Rahti using the `oc login` command. You can find
   instructions in the [Rahti documentation](https://rahti.csc.fi/usage/cli/):

   ```bash
   oc login https://rahti.csc.fi:8443 --token=<hidden token from Rahti>
   ```
3. To begin with, you need to create your Rahti project, you could do so by using oc OC tool
 ```bash
oc new-project <your Rahti project name> --description="csc_project: <your CSC project name>"
for example:
oc new-project demo --description="csc_project: project_100123"
```
4. Deploy JupyterHub from deployment template, remember to supply value of [parameters](#parameters-to-be-supplied) for the deployment.  
```bash
oc new-app -f <path to your template> -p PARAM1=Value1 -p PARAM2=VALUE2 -p PARAM3=VALUE3
for example:
oc new-app -f Jupyter-OpenShift/Jupyter.yaml -p APPLICATION_NAME=skjupyter -p BUILDER_IMAGE=jupyterhub:3.1.0
```
## Parameters to be supplied

|Parameter|	Description|
|---------|------------|
|JupyterHub Name	| Name of your JupyterHub deployment |
|Application Name	| Name of deployed JupyterHub application|
|Jupyter Hub Image | OpenShift compatible Jupyter Hub image which should be used for deployment|
|Notebook Image	| OpenShift compatible Jupyter Notebook image which should be used for deployment|
|Jupyter Config |	Configuration file path for custom configurations. This [template](https://github.com/jupyter-on-openshift/jupyterhub-quickstart/blob/4987215df80d36d42f8a68f2c7b8a166e69d3d9b/jupyterhub_config.py) should be followed for custom configurations|
|Database Password|	Password for JupyterHub Database|
|Cookie Secret|	Cookie secret for application |
|Jupyter Hub Memory|	Amount of memory available to Jupyter Hub |
|Database Memory|	Amount of memory available to PostgreSQL DB (DB of Jupyter Hub) |
|Notebook Memory|	AAmount of memory available to each notebook instance |
