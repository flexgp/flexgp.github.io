<!DOCTYPE html>
<html>
  <head>
    <meta charset='utf-8'>
    <meta http-equiv="X-UA-Compatible" content="chrome=1">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
    <link href='https://fonts.googleapis.com/css?family=Architects+Daughter' rel='stylesheet' type='text/css'>
    <link rel="stylesheet" type="text/css" href="stylesheets/stylesheet.css" media="screen" />
    <link rel="stylesheet" type="text/css" href="stylesheets/pygment_trac.css" media="screen" />
    <link rel="stylesheet" type="text/css" href="stylesheets/print.css" media="print" />

    <!--[if lt IE 9]>
    <script src="//html5shiv.googlecode.com/svn/trunk/html5.js"></script>
    <![endif]-->

    <title>Flexgp.github.io by flexgp</title>
  </head>

  <body>
    <header>
      <div class="inner">
        <h1>Flexgp.github.io</h1>
        <h2>FlexGP Blog</h2>
        <a href="https://github.com/flexgp" class="button"><small>Follow me on</small>GitHub</a>
      </div>
    </header>

    <div id="content-wrapper">
      <div class="inner clearfix">
        <section id="main-content">
          <h1>
<a name="welcome-to-flexgp" class="anchor" href="#welcome-to-flexgp"><span class="octicon octicon-link"></span></a>Welcome to the FlexGP blog</h1>

<p>In this blog we will report the results obtained with the FlexGP data modeler for several datasets.</p>




<h1>
<a name="tutorial" class="anchor" href="#tutorial"><span class="octicon octicon-link"></span></a>Wine quality dataset</h1>

<p>This problem consists in modeling the quality of a given red or white wine given 11 features such as acidity, alcohol degree etc.</p>

<pre><code>$ java -jar flexgp.jar -train path_to_redWine_data -minutes 60 
$ java -jar flexgp.jar -train path_to_whiteWine_data -minutes 60 
</code></pre>

<p>At the end of both runs we measure the accuracy of the most accurate models and the fused Pareto Front Model:</p>

<h2>
<a name="tutorial" class="anchor" href="#tutorial"><span class="octicon octicon-link"></span></a>Red wine dataset</h2>

<pre><code>$ java -jar flexgp.jar -test path_to_NOx_data -integer false -scaled mostAccurate.txt 
$ java -jar flexgp.jar -test path_to_redWine_data -integer false -fused pareto.txt 
</code></pre>

<h2>
<a name="tutorial" class="anchor" href="#tutorial"><span class="octicon octicon-link"></span></a>White wine dataset</h2>

<pre><code>$ java -jar flexgp.jar -test path_to_NOx_data -integer false -scaled mostAccurate.txt 
$ java -jar flexgp.jar -test path_to_whiteWine_data -integer false -fused pareto.txt 
</code></pre>




<h1>
<a name="tutorial" class="anchor" href="#tutorial"><span class="octicon octicon-link"></span></a>Kaggle bond price dataset</h1>

Some preprocessing steps are required in this case to adapt the data to a format compatible with FlexGP. We first reduce the kaggle dataset available at by taking the first 200K lines:
<pre><code>$ head -n 200000 kaggle.data > reducedKaggle.data 
</code></pre>

We then remove all the lines containing NaN values:
<pre><code>$ less reducedKaggle.data | grep -v NaN > cleanKaggle.data
</code></pre>

The proposed challenge is to predict the bond price given in the third column. Note that columns 1 and 2 coresspond to id nominal values and this should be ignored. In FlexGP the targets need to be stored in the last column, therefore, we extract the targets and save them to a different file:
<pre><code>$ cut -d, -f3 cleanKaggle.data  > targets.data
</code></pre>

We then remove the first, second, and third columns of the dataset and paste the targets in the last column of the dataset:
<pre><code>$ cut --complement -d, -f1,2,3 cleanKaggle.data  > cleanColsKaggle.data
$ paste -d, cleanColsKaggle.data  targets.data > finalKaggle.data
</code></pre>

Finally, we employ FlexGP to model the data. In this case, the -cpp flag is employed, thus enabling the optimized C++ evaluation of candidate solutions. In the example below, 8 threads are used to speedup the process:
<pre><code>$ java -jar flexgp.jar -train path_to_kaggle_data -minutes 60 -cpp 8
</code></pre>

<p>At the end of the run we measure the accuracy of the most accurate model and the fused Pareto Front Model:</p>


<pre><code>$ java -jar flexgp.jar -test path_to_kaggle_data -integer false -scaled mostAccurate.txt 
$ java -jar flexgp.jar -test path_to_kaggle_data -integer false -fused pareto.txt 
</code></pre>




<h1>
<a name="tutorial" class="anchor" href="#tutorial"><span class="octicon octicon-link"></span></a>NOx Emissions dataset</h1>

The data is split into training and test set. The models obtained from the training set are then tested on the test set:
<pre><code>$ java -jar flexgp.jar -train path_to_NOx_train_data -minutes 60 
$ java -jar flexgp.jar -test path_to_NOx_test_data -integer false -scaled mostAccurate.txt 
$ java -jar flexgp.jar -test path_to_NOx_test_data -integer false -fused pareto.txt 
</code></pre>



<h1>
<a name="tutorial" class="anchor" href="#tutorial"><span class="octicon octicon-link"></span></a>Million Song Dataset dataset</h1>

The Million Song Dataset year prediction challenge is a regression problem in which the goal is to predict the year in which a given song was released. The dataset is composed of more than 500K songs, each described with a set of 500K features. 

The train/test strategy is repeated in this case. Note that the so-called producer effect has been taken into account to perform the data split. data is split into training and test set:
<pre><code>$ java -jar flexgp.jar -train path_to_msd_train_data -minutes 60 
$ java -jar flexgp.jar -test path_to_msd_test_data -integer false -scaled mostAccurate.txt 
$ java -jar flexgp.jar -test path_to_msd_test_data -integer false -fused pareto.txt 
</code></pre>














<h1>
<a name="support-or-contact" class="anchor" href="#support-or-contact"><span class="octicon octicon-link"></span></a>Support</h1>

<p>The <a href="index.html">FlexGP project</a> has been developed by the Any-scale Learning For All (ALFA) group at MIT. Please contact us at: <a href="mailto:flexgp@csail.mit.edu">flexgp@csail.mit.edu</a> </p>

<p>author: <a href="https://github.com/ignacioarnaldo" class="user-mention">@ignacioarnaldo</a></p>
        </section>

        <aside id="sidebar">


          <p>This page was generated by <a href="pages.github.com">GitHub Pages</a> using the Architect theme by <a href="https://twitter.com/jasonlong">Jason Long</a>.</p>
        </aside>
      </div>
    </div>

            <script type="text/javascript">
            var gaJsHost = (("https:" == document.location.protocol) ? "https://ssl." : "http://www.");
            document.write(unescape("%3Cscript src='" + gaJsHost + "google-analytics.com/ga.js' type='text/javascript'%3E%3C/script%3E"));
          </script>
          <script type="text/javascript">
            try {
              var pageTracker = _gat._getTracker("flexgp");
            pageTracker._trackPageview();
            } catch(err) {}
          </script>

  </body>
</html>
