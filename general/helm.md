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

source: https://www.baeldung.com/ops/kubernetes-helm
