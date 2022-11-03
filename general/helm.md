# Helm:

Helm arch (v3):

![image](https://user-images.githubusercontent.com/72389059/199489877-50d97cc5-ea83-473c-a978-c267d70e6717.png)


components:
  -  chart : a collection of files organized in a specific directory structure.
  -  configuration : configuration information related to a chart.
  -  release : a running instance of a chart with a specific config.
  -  library charts : enable support for common charts that we can use to define chart primitives or definitions. snippets of code that we can re-use across charts.

`Helm 3 releases are stored as Secrets by default in the namespace of the release directly.`

 Artifact Hub: distributed community chart repository
 
***Create a chart***

`helm create hello-world`
```
hello-world /
  Chart.yaml #file that contains the description of our chart
  values.yaml #file that contains the default values for our chart
  templates / #directory where Kubernetes resources are defined as templates
  charts / #optional directory that may contain sub-charts
  .helmignore #patterns to ignore when packaging
```

 Go template language ===>> Helm template language
 
 see the actual template:
 


 
{{}}: This is what is called a template directive.

***sample:***
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
 
```
``` 
  .X.Y
  ||  \-> Y :  look inside of it for an object called Name
  |\-> X : find the X object
  \--> . : top-most namespace for this scope
````


```
helm install [--debug --dry-run] full-coral ./mychart

helm get manifest full-coral # takes a release name and prints the Kubernetes resources 

helm uninstall full-coral

```


source:

https://www.baeldung.com/ops/kubernetes-helm

https://helm.sh/docs/chart_template_guide/getting_started/
