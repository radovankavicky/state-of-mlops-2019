
<!-- .slide: data-background="images/network-background.jpg" class="background" -->

<h2>The Institute for Ethical AI & ML</h2>
<h3>The state of MLOps in 2019</h3>
<p>
  <br />
  <br />
    Alejandro Saucedo | <a href="">a@ethical.institute</a><br/><br/>
    Twitter: <a href="http://twitter.com/AxSaucedo">@AxSaucedo</a><br/>
  <br />
</p>

[NEXT]
<!-- .slide: data-background="images/parti.png" class="background smallquote" style="color: white" -->

## Small data science projects
![classification_large](images/ml-pipeline.png)
<!-- .element: style="background-color: white !important" -->

### Works relatively well


[NEXT]
<!-- .slide: data-background="images/parti.png" class="background smallquote" style="color: white" -->

# However
As our data science requirements grow...
## We face new issues

[NEXT]
<!-- .slide: data-background="images/parti.png" class="background smallquote" style="color: white" -->
#### Increasing complexity in flow of data
![classification_large](images/crontab.jpg)

[NEXT]
<!-- .slide: data-background="images/parti.png" class="background smallquote" style="color: white" -->
#### Each data scientist has their own set of tools

<div class="left-col">
    <ul>
        <li>Some ♥ Tensorflow</li>
        <li>Some ♥ R</li>
        <li>Some ♥ Spark</li>
    </ul>
</div>
<div class="right-col">
![classification_large](images/mlibs.jpg)
</div>

<br style="clear:both">
### Some ♥ all of them


[NEXT]
<!-- .slide: data-background="images/parti.png" class="background smallquote" style="color: white" -->

#### Serving models becomes increasinly harder
![classification_large](images/mlmodles.png)

[NEXT]
<!-- .slide: data-background="images/parti.png" class="background smallquote" style="color: white" -->
#### When stuff goes wrong it's hard to trace back
![classification_large](images/gitblame.jpg)


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

### As your technical functions grow...

![classification_large](images/mltemp1.png)

[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

### So should your infrastructure

![classification_large](images/mlops1.png)

[NEXT]
<!-- .slide: data-background="images/parti.png" class="background smallquote" style="color: white" -->
## It's challenging
![full_height](images/mleng-roles.png)


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

<h1 style="color: white !important">Awesome <font style="color:#00ffda">ML</font>Ops</h1>
<h4>Libraries to deploy, version, monitor & scale ML</h4>
![classification_large](images/mlops-now.png)
  <a style="color:#00ffda" href="https://bit.ly/awesome-mlops">bit.ly/awesome-mlops</a>
  <br />
  <br />


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

## Principles today
<br>

<div class="left-col" style="width: 40%; padding-top: 25px">
<ul>
    <li>Reproducibility</li>
    <li>Orchestration</li>
    <li>Explainability</li>
</ul>
</div>

<div class="right-col" style="width: 60%">
<img src="images/mlops2.png" alt="">
</div>

[NEXT SECTION]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->
# 2.1 Reproducibility
![classification_large](images/mltemp6.png)
### Model & data provenance


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

## Abstracting individual steps

![classification_large](images/mlstep.png)


<div style="float: left; width: 33%">
<h4>Data in </h4>

<pre><code class="code python hljs" style="font-size: 0.6em; line-height: 1em">
$ cat data-input.csv

>            Date    Open    High     Low   Close     Market Cap
> 1608 2013-04-28  135.30  135.98  132.10  134.21  1,500,520,000
> 1607 2013-04-29  134.44  147.49  134.00  144.54  1,491,160,000
> 1606 2013-04-30  144.00  146.93  134.05  139.00  1,597,780,000

</code></pre>
</div>

<div style="float: left; width: 33%">
<h4>Code / Config</h4>
<pre><code class="code python hljs" style="font-size: 0.6em; line-height: 1em">
$ cat feature-extractor.py

> def open_norm_feature_extractor(df):
>     feature = some_lib.get_open(df)
>     return feature


</code></pre>
</div>

<div style="float: left; width: 33%">
<h4>Data out</h4>
<pre><code class="code python hljs" style="font-size: 0.6em; line-height: 1em">
$ cat data-output.csv

>   Open 
>   0.57 
>   0.59 
>   0.47 

</code></pre>
</div>


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

<div style="position: absolute; width: 30%; right: 0; bottom: -5%">
![classification_large](images/versioning.jpg)
</div>
## Going one level higher

We can abstract our entire pipeline and data flows

![classification_large](images/mltemp5.png)

_note_
Similarly we can store the accuracy of the model
Together with all the parameters that were used to compute it


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

## Ones to watch

[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

## Data Version Control (DVC)


#### Add your data

```
dvc add images.zip
```

#### commit data input, model output and code

```
dvc run -d images.zip -o model.p ./cnn.py
```

#### Add repository location (here is s3)

```
dvc remote add myrepo s3://mybucket
```

#### Push to the location specified

```
dvc push
```

#### Check it out at <a href="https://dvc.org">dvc.org</a>

[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

## ModelDB

<div class="left-col">
<h4>Normal sklearn</h4>

<pre><code class="code python hljs" style="font-size: 1em; line-height: 1em">
def fit_and_predict(data):
    model.fit(data)
    preprocessor.transform(data)
    model.predict(data)

</code></pre>
</div>


<div class="right-col">
<h4>ModelDB-enabled</h4>

<pre><code class="code python hljs" style="font-size: 1em; line-height: 1em">
def fit_and_predict(data):
    model.fit_sync(data)
    preprocessor.transform_sync(data)
    model.predict_sync(data)

</code></pre>
</div>

![classification_large](images/modeldb-dashboard.png)

#### <a href="https://senselab.med.yale.edu/ModelDB/">senselab.med.yale.edu/ModelDB/</a>


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

## Pachyderm 
![classification_large](images/pachyderm-dashboard.png)

#### <a href="https://www.pachyderm.io/">www.pachyderm.io/</a>

[NEXT SECTION]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

# 2.2 Model Orchestration

![classification_large](images/mltemp3.png)

### Training & serving at scale


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

## Computational Resource allocation

Services with different computational requirements

With often complex computational graphs

We need to be able to allocate the right resources

<br>
### This is a hard problem


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

#### Usage vs Traditional Servers
![classification_large](images/arch3.png)

[source: <a href="https://algorithmia.com/">Algorithmia</a>]

[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

#### Usage vs Elastic Servers
![classification_large](images/arch2.png)

[source: <a href="https://algorithmia.com/">Algorithmia</a>]

[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

#### Usage vs Serverless

![classification_large](images/arch1.png)

[source: <a href="https://algorithmia.com/">Algorithmia</a>]

[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

#### Serverless vs MapReduce (Publications)

![classification_large](images/serverless-popularity.png)

[source: Arxiv Paper - <a href="https://arxiv.org/pdf/1812.03651.pdf">Serverless: One step fwd, two back</a>]


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

## Ones to watch

[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

![classification_large](images/seldon3.png)
## Seldon
![classification_large](images/seldon2.png)

#### <a href="https://seldon.io">seldon.io</a>

[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

## MLeap Serving

Diving one level deeper into serialization of models

![classification_large](images/mleapruntime.jpg)


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->
## DeepDetect

Unifying multiple external machine learning libraries on a single API

![classification_large](images/deepdetect.png)

#### <a href="https://www.deepdetect.com/">www.deepdetect.com/</a>


[NEXT SECTION]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->
# 2.3 Explainability
Tackling "black box model" situations

![classification_large](images/mlops2.png)

[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->
## Going beyond the algorithms 
Explainability through tools, process and domain expertise.
![classification_large](images/bias.png)

[Our talk on <a href="https://www.youtube.com/watch?v=GZpfBhQJ0H4">Explainability of Tensorflow Models</a>]


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

## Data assessment

<div class="left-col">
<br>
<ul>
    <li>Class imbalances</li>
    <li>Protected features</li>
    <li>Correlations</li>
    <li>Data representability</li>
</ul>
</div>
<div class="right-col">
<img src="images/class-imbalance.png" alt="">
</div>

[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

## Model assessment

<div class="left-col">
<img src="images/shap-values.png" style="background-color: white !important" alt="">
</div>
<div class="right-col">
<br>
<ul>
    <li>Feature importance</li>
    <li>Model specific methods</li>
    <li>Domain knowledge abstraction</li>
    <li>Model metrics analysis</li>
</ul>
</div>

[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

## Production monitoring


<div class="left-col">
<img src="images/roc-monitoring.png" alt="">
</div>
<div class="right-col">
<ul>
    <li>Evaluation of metrics</li>
    <li>Manual human review</li>
    <li>Monitoring of anomalies</li>
    <li>Setting thresholds for divergence</li>
</ul>
</div>



[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

# Ones to watch


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->

## ELI5
![classification_large](images/eli5-text.png)

#### <a href="https://github.com/TeamHG-Memex/eli5">github.com/TeamHG-Memex/eli5</a>


[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->
## SHAP

Unifying multiple model explainability techniques

![classification_large](images/minst-image-plot.png)
<!-- .element: style="background-color: white !important" -->

#### <a href="https://github.com/slundberg/shap">github.com/slundberg/shap</a>

[NEXT]
<!-- .slide: data-background="images/partistat.png" class="background smallquote" style="color: white" -->
## XAI
Analyse datasets, evaluate models and monitor production

![classification_large](images/xai-pr.png)

#### <a href="https://github.com/ethicalml/xai">github.com/ethicalml/xai</a>

[NEXT SECTION]
<!-- .slide: data-background="images/network-background.jpg" class="background smallest" -->

## Much more content


||||
|-|-|-|
|[🔍 Explainability](https://github.com/EthicalML/awesome-machine-learning-operations#1-explaining-black-box-models-and-datasets) |[🔏 Privacy](https://github.com/EthicalML/awesome-machine-learning-operations#2-privacy-preserving-machine-learning) | [📜 Versioning](https://github.com/EthicalML/awesome-machine-learning-operations#3-model-and-data-versioning)|
|[🏁 Orchestration](https://github.com/EthicalML/awesome-machine-learning-operations#4-model-deployment-and-orchestration-frameworks)|[🌀 FeaturEng](https://github.com/EthicalML/awesome-machine-learning-operations#5-feature-engineering-automation)|[🤖 AutoML](https://github.com/EthicalML/awesome-machine-learning-operations#6-neural-architecture-search)|
| [📓 Notebooks](https://github.com/EthicalML/awesome-machine-learning-operations#7-data-science-notebook-frameworks) | [📊 Visualisation](https://github.com/EthicalML/awesome-machine-learning-operations#8-industrial-strength-visualisation-libraries) | [🔠 NLP](https://github.com/EthicalML/awesome-machine-learning-operations#9-industrial-strength-nlp) |
| [🧵 ETL](https://github.com/EthicalML/awesome-machine-learning-operations#10-data-pipeline-etl-frameworks) | [🗞️ Storage](https://github.com/EthicalML/awesome-machine-learning-operations#11-data-storage-optimisation) | [📡 FaaS](https://github.com/EthicalML/awesome-machine-learning-operations#12-function-as-a-service-frameworks) |
| [🗺️ Computation](https://github.com/EthicalML/awesome-machine-learning-operations#13-computation-load-distribution-frameworks) | [📥 Serialisation](https://github.com/EthicalML/awesome-machine-learning-operations#14-model-serialisation-formats) | [🎁 Compiler](https://github.com/EthicalML/awesome-machine-learning-operations#15-compiler-optimisation-frameworks)  |
| [💸 CommercialML](https://github.com/EthicalML/awesome-machine-learning-operations#16-commercial-data-science-platforms) | [💰 CommercialETL](https://github.com/EthicalML/awesome-machine-learning-operations#17-commercial-etl-platforms)| |
<br>
### Check it out & add more libraries

[NEXT]
<!-- .slide: data-background="images/network-background.jpg" class="background" -->

<h2>The Institute for Ethical AI & ML</h2>
<h3>The state of MLops in 2019</h3>

<table class="bio-table" style="margin-top: -30px">
  <tr>
    <td style="float: left">
        <br>
        ![portrait](images/alejandro.jpg)
        <br>
        <font style="font-weight: bold; color: white">Alejandro Saucedo</font>
        <br>
        Twitter: <a href="https://twitter.com/axsaucedo">@AxSaucedo</a>
    </td>
    <td style="float: left; color: white; font-size: 0.7em;">

        <br>
        Chief Scientist
        <br>
        <a style="color: cyan" href="http://e-x.io">The Institute for Ethical AI & ML</a
        <br>
        <br>
        <br>
        <hr>
        <br>
        Head of Solutions Eng. & Sci.
        <br>
        <a style="color: cyan" href="http://eigentech.com">Eigen Technologies</a>
        <br>
        <br>
        Chief Technology Officer
        <br>
        <a style="color: cyan" href="#">Hack Partners</a>
        <br>
        <br>
        Software Engineer
        <br>
        <a style="color: cyan" href="#">Bloomberg LP.</a>

    </td>
  </tr>
  <tr>
  </tr>
</table>
  



