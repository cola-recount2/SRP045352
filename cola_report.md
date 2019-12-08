cola Report for Consensus Partitioning
==================

**Date**: 2019-12-05 04:23:52 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 14951    56
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:kmeans](#SD-kmeans)     |          2| 1.000|           1.000|       1.000|** |           |
|[CV:hclust](#CV-hclust)     |          2| 1.000|           1.000|       1.000|** |           |
|[CV:kmeans](#CV-kmeans)     |          2| 1.000|           1.000|       1.000|** |           |
|[CV:mclust](#CV-mclust)     |          4| 1.000|           0.974|       0.990|** |2          |
|[CV:NMF](#CV-NMF)           |          2| 1.000|           0.995|       0.998|** |           |
|[MAD:kmeans](#MAD-kmeans)   |          2| 1.000|           1.000|       1.000|** |           |
|[ATC:kmeans](#ATC-kmeans)   |          2| 1.000|           1.000|       1.000|** |           |
|[ATC:skmeans](#ATC-skmeans) |          4| 1.000|           0.940|       0.974|** |2          |
|[ATC:NMF](#ATC-NMF)         |          2| 1.000|           1.000|       1.000|** |           |
|[ATC:pam](#ATC-pam)         |          6| 0.987|           0.958|       0.980|** |2,4,5      |
|[MAD:skmeans](#MAD-skmeans) |          6| 0.970|           0.949|       0.966|** |2,3,5      |
|[SD:NMF](#SD-NMF)           |          6| 0.967|           0.940|       0.962|** |2,5        |
|[SD:skmeans](#SD-skmeans)   |          6| 0.966|           0.947|       0.966|** |2,3,4      |
|[MAD:NMF](#MAD-NMF)         |          6| 0.962|           0.932|       0.957|** |2,3,4      |
|[SD:hclust](#SD-hclust)     |          5| 0.954|           0.973|       0.986|** |2          |
|[MAD:mclust](#MAD-mclust)   |          6| 0.946|           0.870|       0.956|*  |2,3        |
|[ATC:hclust](#ATC-hclust)   |          6| 0.936|           0.912|       0.946|*  |2,3,4      |
|[MAD:pam](#MAD-pam)         |          6| 0.935|           0.892|       0.930|*  |2,3,5      |
|[SD:mclust](#SD-mclust)     |          6| 0.934|           0.917|       0.960|*  |2,4        |
|[SD:pam](#SD-pam)           |          6| 0.934|           0.906|       0.908|*  |2,5        |
|[CV:pam](#CV-pam)           |          5| 0.933|           0.918|       0.965|*  |2,4        |
|[ATC:mclust](#ATC-mclust)   |          6| 0.933|           0.884|       0.930|*  |2,3        |
|[MAD:hclust](#MAD-hclust)   |          5| 0.931|           0.912|       0.946|*  |2,4        |
|[CV:skmeans](#CV-skmeans)   |          6| 0.915|           0.889|       0.921|*  |2,3        |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2     1           1.000       1.000          0.457 0.544   0.544
#&gt; CV:NMF      2     1           0.995       0.998          0.458 0.544   0.544
#&gt; MAD:NMF     2     1           0.997       0.998          0.458 0.544   0.544
#&gt; ATC:NMF     2     1           1.000       1.000          0.457 0.544   0.544
#&gt; SD:skmeans  2     1           0.965       0.986          0.465 0.544   0.544
#&gt; CV:skmeans  2     1           0.973       0.989          0.466 0.532   0.532
#&gt; MAD:skmeans 2     1           0.992       0.997          0.476 0.523   0.523
#&gt; ATC:skmeans 2     1           0.976       0.991          0.471 0.532   0.532
#&gt; SD:mclust   2     1           1.000       1.000          0.457 0.544   0.544
#&gt; CV:mclust   2     1           1.000       1.000          0.457 0.544   0.544
#&gt; MAD:mclust  2     1           1.000       1.000          0.457 0.544   0.544
#&gt; ATC:mclust  2     1           1.000       1.000          0.457 0.544   0.544
#&gt; SD:kmeans   2     1           1.000       1.000          0.457 0.544   0.544
#&gt; CV:kmeans   2     1           1.000       1.000          0.457 0.544   0.544
#&gt; MAD:kmeans  2     1           1.000       1.000          0.457 0.544   0.544
#&gt; ATC:kmeans  2     1           1.000       1.000          0.457 0.544   0.544
#&gt; SD:pam      2     1           1.000       1.000          0.457 0.544   0.544
#&gt; CV:pam      2     1           1.000       1.000          0.457 0.544   0.544
#&gt; MAD:pam     2     1           1.000       1.000          0.457 0.544   0.544
#&gt; ATC:pam     2     1           1.000       1.000          0.457 0.544   0.544
#&gt; SD:hclust   2     1           1.000       1.000          0.457 0.544   0.544
#&gt; CV:hclust   2     1           1.000       1.000          0.457 0.544   0.544
#&gt; MAD:hclust  2     1           1.000       1.000          0.457 0.544   0.544
#&gt; ATC:hclust  2     1           1.000       1.000          0.457 0.544   0.544
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 0.830           0.923       0.951          0.424 0.805   0.642
#&gt; CV:NMF      3 0.664           0.844       0.817          0.381 0.791   0.615
#&gt; MAD:NMF     3 0.935           0.926       0.967          0.443 0.797   0.627
#&gt; ATC:NMF     3 0.803           0.831       0.925          0.350 0.836   0.699
#&gt; SD:skmeans  3 1.000           0.983       0.993          0.459 0.778   0.591
#&gt; CV:skmeans  3 1.000           0.989       0.991          0.450 0.769   0.575
#&gt; MAD:skmeans 3 1.000           0.976       0.992          0.425 0.757   0.553
#&gt; ATC:skmeans 3 0.840           0.846       0.928          0.317 0.802   0.633
#&gt; SD:mclust   3 0.779           0.922       0.878          0.248 0.849   0.723
#&gt; CV:mclust   3 0.772           0.933       0.874          0.282 0.836   0.699
#&gt; MAD:mclust  3 0.975           0.952       0.975          0.359 0.836   0.699
#&gt; ATC:mclust  3 1.000           0.991       0.995          0.363 0.836   0.699
#&gt; SD:kmeans   3 0.673           0.844       0.829          0.323 0.836   0.699
#&gt; CV:kmeans   3 0.778           0.953       0.918          0.306 0.836   0.699
#&gt; MAD:kmeans  3 0.661           0.756       0.754          0.349 1.000   1.000
#&gt; ATC:kmeans  3 0.749           0.957       0.906          0.331 0.814   0.658
#&gt; SD:pam      3 0.878           0.945       0.936          0.446 0.782   0.599
#&gt; CV:pam      3 0.778           0.969       0.936          0.312 0.836   0.699
#&gt; MAD:pam     3 0.976           0.946       0.978          0.477 0.779   0.594
#&gt; ATC:pam     3 0.836           0.876       0.929          0.404 0.836   0.699
#&gt; SD:hclust   3 0.825           0.932       0.964          0.188 0.934   0.878
#&gt; CV:hclust   3 0.624           0.681       0.843          0.334 0.914   0.842
#&gt; MAD:hclust  3 0.825           0.934       0.964          0.187 0.934   0.878
#&gt; ATC:hclust  3 1.000           0.996       0.998          0.360 0.836   0.699
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.869           0.846       0.881         0.0838 0.962   0.891
#&gt; CV:NMF      4 0.791           0.771       0.827         0.1442 0.864   0.637
#&gt; MAD:NMF     4 0.920           0.878       0.939         0.0876 0.938   0.819
#&gt; ATC:NMF     4 0.597           0.600       0.765         0.1131 0.896   0.746
#&gt; SD:skmeans  4 0.902           0.925       0.894         0.0817 0.942   0.818
#&gt; CV:skmeans  4 0.766           0.784       0.799         0.0935 0.842   0.573
#&gt; MAD:skmeans 4 0.874           0.596       0.770         0.0771 0.953   0.855
#&gt; ATC:skmeans 4 1.000           0.940       0.974         0.1422 0.921   0.781
#&gt; SD:mclust   4 0.936           0.935       0.965         0.1820 0.919   0.798
#&gt; CV:mclust   4 1.000           0.974       0.990         0.1550 0.942   0.846
#&gt; MAD:mclust  4 0.749           0.829       0.837         0.1076 0.943   0.850
#&gt; ATC:mclust  4 0.843           0.810       0.922         0.1501 0.905   0.749
#&gt; SD:kmeans   4 0.707           0.513       0.654         0.1627 0.896   0.726
#&gt; CV:kmeans   4 0.674           0.830       0.777         0.1434 0.987   0.966
#&gt; MAD:kmeans  4 0.718           0.418       0.679         0.1491 0.774   0.584
#&gt; ATC:kmeans  4 0.755           0.721       0.765         0.1597 0.877   0.668
#&gt; SD:pam      4 0.761           0.930       0.928         0.1009 0.942   0.820
#&gt; CV:pam      4 1.000           1.000       1.000         0.1323 0.942   0.846
#&gt; MAD:pam     4 0.853           0.936       0.946         0.0976 0.936   0.801
#&gt; ATC:pam     4 1.000           0.953       0.983         0.1632 0.873   0.667
#&gt; SD:hclust   4 0.825           0.927       0.961         0.1086 0.942   0.878
#&gt; CV:hclust   4 0.706           0.775       0.823         0.1393 0.896   0.779
#&gt; MAD:hclust  4 1.000           0.992       0.996         0.2115 0.865   0.717
#&gt; ATC:hclust  4 0.908           0.954       0.971         0.1461 0.914   0.774
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.915           0.929       0.949         0.0673 0.942   0.813
#&gt; CV:NMF      5 0.778           0.691       0.854         0.0922 0.920   0.721
#&gt; MAD:NMF     5 0.824           0.837       0.863         0.0641 0.926   0.742
#&gt; ATC:NMF     5 0.649           0.607       0.817         0.0766 0.755   0.408
#&gt; SD:skmeans  5 0.882           0.879       0.887         0.0685 0.964   0.862
#&gt; CV:skmeans  5 0.773           0.820       0.858         0.0799 0.942   0.774
#&gt; MAD:skmeans 5 0.969           0.932       0.952         0.0672 0.890   0.639
#&gt; ATC:skmeans 5 0.875           0.872       0.890         0.0674 0.921   0.739
#&gt; SD:mclust   5 0.825           0.807       0.885         0.1022 0.958   0.872
#&gt; CV:mclust   5 0.779           0.686       0.855         0.1409 0.905   0.703
#&gt; MAD:mclust  5 0.795           0.865       0.886         0.1328 0.875   0.614
#&gt; ATC:mclust  5 0.802           0.777       0.880         0.0292 1.000   1.000
#&gt; SD:kmeans   5 0.692           0.883       0.777         0.0809 0.825   0.465
#&gt; CV:kmeans   5 0.716           0.687       0.721         0.0945 0.833   0.561
#&gt; MAD:kmeans  5 0.692           0.879       0.785         0.0829 0.814   0.475
#&gt; ATC:kmeans  5 0.716           0.586       0.657         0.0841 0.832   0.479
#&gt; SD:pam      5 1.000           0.966       0.987         0.1009 0.896   0.634
#&gt; CV:pam      5 0.933           0.918       0.965         0.1815 0.858   0.572
#&gt; MAD:pam     5 0.982           0.946       0.979         0.0772 0.945   0.789
#&gt; ATC:pam     5 0.987           0.948       0.980         0.0780 0.942   0.769
#&gt; SD:hclust   5 0.954           0.973       0.986         0.2009 0.865   0.678
#&gt; CV:hclust   5 0.771           0.765       0.802         0.0882 0.856   0.621
#&gt; MAD:hclust  5 0.931           0.912       0.946         0.0627 0.955   0.867
#&gt; ATC:hclust  5 0.893           0.921       0.960         0.0111 0.995   0.982
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.967           0.940       0.962        0.06878 0.899   0.636
#&gt; CV:NMF      6 0.888           0.777       0.916        0.04301 0.897   0.592
#&gt; MAD:NMF     6 0.962           0.932       0.957        0.05385 0.937   0.737
#&gt; ATC:NMF     6 0.711           0.645       0.824        0.04779 0.930   0.750
#&gt; SD:skmeans  6 0.966           0.947       0.966        0.06529 0.942   0.742
#&gt; CV:skmeans  6 0.915           0.889       0.921        0.05715 0.931   0.673
#&gt; MAD:skmeans 6 0.970           0.949       0.966        0.07127 0.942   0.742
#&gt; ATC:skmeans 6 0.809           0.807       0.875        0.04198 0.986   0.938
#&gt; SD:mclust   6 0.934           0.917       0.960        0.10143 0.884   0.598
#&gt; CV:mclust   6 0.827           0.752       0.852        0.05859 0.866   0.506
#&gt; MAD:mclust  6 0.946           0.870       0.956        0.05060 0.970   0.853
#&gt; ATC:mclust  6 0.933           0.884       0.930        0.05752 0.902   0.681
#&gt; SD:kmeans   6 0.705           0.825       0.779        0.05447 0.950   0.756
#&gt; CV:kmeans   6 0.700           0.633       0.744        0.06333 0.931   0.710
#&gt; MAD:kmeans  6 0.782           0.846       0.798        0.04987 0.964   0.822
#&gt; ATC:kmeans  6 0.714           0.752       0.763        0.03995 0.874   0.535
#&gt; SD:pam      6 0.934           0.906       0.908        0.03106 0.953   0.785
#&gt; CV:pam      6 0.866           0.830       0.885        0.04684 0.939   0.705
#&gt; MAD:pam     6 0.935           0.892       0.930        0.03012 0.978   0.893
#&gt; ATC:pam     6 0.987           0.958       0.980        0.02942 0.973   0.865
#&gt; SD:hclust   6 0.954           0.956       0.982        0.00401 0.999   0.995
#&gt; CV:hclust   6 0.835           0.788       0.836        0.04377 0.987   0.945
#&gt; MAD:hclust  6 0.955           0.866       0.951        0.01738 0.999   0.996
#&gt; ATC:hclust  6 0.936           0.912       0.946        0.07927 0.932   0.764
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>



 
## Results for each method


---------------------------------------------------




### SD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000        0.45704 0.544   0.544
#> 3 3 0.825           0.932       0.964        0.18808 0.934   0.878
#> 4 4 0.825           0.927       0.961        0.10858 0.942   0.878
#> 5 5 0.954           0.973       0.986        0.20088 0.865   0.678
#> 6 6 0.954           0.956       0.982        0.00401 0.999   0.995
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3
#&gt; SRR1539207     1    0.46      0.785 0.796  0 0.204
#&gt; SRR1539208     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539211     3    0.46      0.869 0.204  0 0.796
#&gt; SRR1539210     3    0.00      0.733 0.000  0 1.000
#&gt; SRR1539209     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539212     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539214     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539213     1    0.46      0.785 0.796  0 0.204
#&gt; SRR1539215     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539216     1    0.46      0.785 0.796  0 0.204
#&gt; SRR1539217     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539218     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539220     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539219     1    0.46      0.785 0.796  0 0.204
#&gt; SRR1539221     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539223     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539224     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539222     1    0.46      0.785 0.796  0 0.204
#&gt; SRR1539225     1    0.46      0.785 0.796  0 0.204
#&gt; SRR1539227     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539226     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539228     1    0.46      0.785 0.796  0 0.204
#&gt; SRR1539229     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539232     1    0.46      0.785 0.796  0 0.204
#&gt; SRR1539230     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539231     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539234     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539233     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539235     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539236     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539237     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539238     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539239     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539242     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539240     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539241     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539243     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539244     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539245     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539246     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539247     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539248     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539249     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539250     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539251     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539253     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539252     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539255     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539254     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539256     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539257     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539258     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539259     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539260     1    0.00      0.941 1.000  0 0.000
#&gt; SRR1539262     2    0.00      1.000 0.000  1 0.000
#&gt; SRR1539261     3    0.46      0.869 0.204  0 0.796
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     1  0.3972      0.777 0.788 0.000 0.204 0.008
#&gt; SRR1539208     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539211     3  0.3791      0.828 0.200 0.000 0.796 0.004
#&gt; SRR1539210     3  0.0000      0.641 0.000 0.000 1.000 0.000
#&gt; SRR1539209     4  0.0469      1.000 0.000 0.012 0.000 0.988
#&gt; SRR1539212     4  0.0469      1.000 0.000 0.012 0.000 0.988
#&gt; SRR1539214     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539213     1  0.3972      0.777 0.788 0.000 0.204 0.008
#&gt; SRR1539215     4  0.0469      1.000 0.000 0.012 0.000 0.988
#&gt; SRR1539216     1  0.3972      0.777 0.788 0.000 0.204 0.008
#&gt; SRR1539217     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539218     4  0.0469      1.000 0.000 0.012 0.000 0.988
#&gt; SRR1539220     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539219     1  0.3972      0.777 0.788 0.000 0.204 0.008
#&gt; SRR1539221     4  0.0469      1.000 0.000 0.012 0.000 0.988
#&gt; SRR1539223     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539224     4  0.0469      1.000 0.000 0.012 0.000 0.988
#&gt; SRR1539222     1  0.3972      0.777 0.788 0.000 0.204 0.008
#&gt; SRR1539225     1  0.3972      0.777 0.788 0.000 0.204 0.008
#&gt; SRR1539227     4  0.0469      1.000 0.000 0.012 0.000 0.988
#&gt; SRR1539226     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539228     1  0.3972      0.777 0.788 0.000 0.204 0.008
#&gt; SRR1539229     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539232     1  0.3972      0.777 0.788 0.000 0.204 0.008
#&gt; SRR1539230     4  0.0469      1.000 0.000 0.012 0.000 0.988
#&gt; SRR1539231     4  0.0469      1.000 0.000 0.012 0.000 0.988
#&gt; SRR1539234     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539233     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539235     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539236     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539237     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539238     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539239     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539242     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539241     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539243     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539244     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539245     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539246     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539247     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539248     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539250     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539251     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539253     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539252     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539254     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539256     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539257     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539258     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539260     1  0.0000      0.939 1.000 0.000 0.000 0.000
#&gt; SRR1539262     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539261     3  0.3791      0.828 0.200 0.000 0.796 0.004
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3 p4    p5
#&gt; SRR1539207     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539208     1   0.238      0.885 0.872  0 0.000  0 0.128
#&gt; SRR1539211     5   0.000      0.888 0.000  0 0.000  0 1.000
#&gt; SRR1539210     5   0.314      0.727 0.000  0 0.204  0 0.796
#&gt; SRR1539209     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539212     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539214     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539213     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539215     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539216     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539217     1   0.161      0.932 0.928  0 0.000  0 0.072
#&gt; SRR1539218     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539220     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539219     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539221     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539223     1   0.161      0.932 0.928  0 0.000  0 0.072
#&gt; SRR1539224     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539222     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539225     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539227     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539226     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539228     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539229     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539232     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539230     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539231     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539234     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539233     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539235     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539236     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539237     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539238     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539239     1   0.213      0.904 0.892  0 0.000  0 0.108
#&gt; SRR1539242     1   0.213      0.904 0.892  0 0.000  0 0.108
#&gt; SRR1539240     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539241     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539243     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539244     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539245     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539246     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539247     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539248     1   0.213      0.904 0.892  0 0.000  0 0.108
#&gt; SRR1539249     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539250     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539251     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539253     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539252     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539255     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539254     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539256     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539257     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539258     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539259     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539260     1   0.000      0.976 1.000  0 0.000  0 0.000
#&gt; SRR1539262     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539261     5   0.000      0.888 0.000  0 0.000  0 1.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.1141      0.969 0.000  0 0.948 0.000 0.052 0.000
#&gt; SRR1539208     1  0.2135      0.883 0.872  0 0.000 0.000 0.000 0.128
#&gt; SRR1539211     6  0.0000      1.000 0.000  0 0.000 0.000 0.000 1.000
#&gt; SRR1539210     5  0.0865      0.000 0.000  0 0.000 0.000 0.964 0.036
#&gt; SRR1539209     4  0.0865      0.978 0.000  0 0.000 0.964 0.036 0.000
#&gt; SRR1539212     4  0.0865      0.978 0.000  0 0.000 0.964 0.036 0.000
#&gt; SRR1539214     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539213     3  0.0000      0.969 0.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1539215     4  0.0000      0.983 0.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1539216     3  0.1141      0.969 0.000  0 0.948 0.000 0.052 0.000
#&gt; SRR1539217     1  0.1444      0.930 0.928  0 0.000 0.000 0.000 0.072
#&gt; SRR1539218     4  0.0865      0.978 0.000  0 0.000 0.964 0.036 0.000
#&gt; SRR1539220     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539219     3  0.1141      0.969 0.000  0 0.948 0.000 0.052 0.000
#&gt; SRR1539221     4  0.0000      0.983 0.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1539223     1  0.1444      0.930 0.928  0 0.000 0.000 0.000 0.072
#&gt; SRR1539224     4  0.0865      0.978 0.000  0 0.000 0.964 0.036 0.000
#&gt; SRR1539222     3  0.1141      0.969 0.000  0 0.948 0.000 0.052 0.000
#&gt; SRR1539225     3  0.0000      0.969 0.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1539227     4  0.0000      0.983 0.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1539226     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539228     3  0.0000      0.969 0.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1539229     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539232     3  0.0000      0.969 0.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1539230     4  0.0000      0.983 0.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1539231     4  0.0000      0.983 0.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539233     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539235     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539236     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539238     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539239     1  0.1910      0.902 0.892  0 0.000 0.000 0.000 0.108
#&gt; SRR1539242     1  0.1910      0.902 0.892  0 0.000 0.000 0.000 0.108
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539241     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539244     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539245     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539247     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539248     1  0.1910      0.902 0.892  0 0.000 0.000 0.000 0.108
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539250     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539251     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539252     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539254     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539257     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539258     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539260     1  0.0000      0.976 1.000  0 0.000 0.000 0.000 0.000
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539261     6  0.0000      1.000 0.000  0 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.673           0.844       0.829         0.3234 0.836   0.699
#> 4 4 0.707           0.513       0.654         0.1627 0.896   0.726
#> 5 5 0.692           0.883       0.777         0.0809 0.825   0.465
#> 6 6 0.705           0.825       0.779         0.0545 0.950   0.756
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1539207     3  0.5465      0.993 0.288 0.000 0.712
#&gt; SRR1539208     1  0.0424      0.846 0.992 0.000 0.008
#&gt; SRR1539211     1  0.0747      0.839 0.984 0.000 0.016
#&gt; SRR1539210     3  0.5465      0.993 0.288 0.000 0.712
#&gt; SRR1539209     2  0.0424      0.867 0.000 0.992 0.008
#&gt; SRR1539212     2  0.0424      0.867 0.000 0.992 0.008
#&gt; SRR1539214     1  0.0000      0.849 1.000 0.000 0.000
#&gt; SRR1539213     3  0.5529      0.993 0.296 0.000 0.704
#&gt; SRR1539215     2  0.0000      0.869 0.000 1.000 0.000
#&gt; SRR1539216     3  0.5465      0.993 0.288 0.000 0.712
#&gt; SRR1539217     1  0.0237      0.848 0.996 0.000 0.004
#&gt; SRR1539218     2  0.0424      0.867 0.000 0.992 0.008
#&gt; SRR1539220     1  0.0000      0.849 1.000 0.000 0.000
#&gt; SRR1539219     3  0.5497      0.993 0.292 0.000 0.708
#&gt; SRR1539221     2  0.0000      0.869 0.000 1.000 0.000
#&gt; SRR1539223     1  0.0424      0.846 0.992 0.000 0.008
#&gt; SRR1539224     2  0.0424      0.867 0.000 0.992 0.008
#&gt; SRR1539222     3  0.5465      0.993 0.288 0.000 0.712
#&gt; SRR1539225     3  0.5529      0.993 0.296 0.000 0.704
#&gt; SRR1539227     2  0.0000      0.869 0.000 1.000 0.000
#&gt; SRR1539226     1  0.0000      0.849 1.000 0.000 0.000
#&gt; SRR1539228     3  0.5529      0.993 0.296 0.000 0.704
#&gt; SRR1539229     1  0.0000      0.849 1.000 0.000 0.000
#&gt; SRR1539232     3  0.5529      0.993 0.296 0.000 0.704
#&gt; SRR1539230     2  0.0000      0.869 0.000 1.000 0.000
#&gt; SRR1539231     2  0.0000      0.869 0.000 1.000 0.000
#&gt; SRR1539234     2  0.5397      0.884 0.000 0.720 0.280
#&gt; SRR1539233     1  0.0000      0.849 1.000 0.000 0.000
#&gt; SRR1539235     1  0.5058      0.646 0.756 0.000 0.244
#&gt; SRR1539236     1  0.0000      0.849 1.000 0.000 0.000
#&gt; SRR1539237     2  0.5397      0.884 0.000 0.720 0.280
#&gt; SRR1539238     1  0.5058      0.646 0.756 0.000 0.244
#&gt; SRR1539239     1  0.0237      0.848 0.996 0.000 0.004
#&gt; SRR1539242     1  0.0237      0.848 0.996 0.000 0.004
#&gt; SRR1539240     2  0.5397      0.884 0.000 0.720 0.280
#&gt; SRR1539241     1  0.5058      0.646 0.756 0.000 0.244
#&gt; SRR1539243     2  0.5397      0.884 0.000 0.720 0.280
#&gt; SRR1539244     1  0.5058      0.646 0.756 0.000 0.244
#&gt; SRR1539245     1  0.0000      0.849 1.000 0.000 0.000
#&gt; SRR1539246     2  0.5397      0.884 0.000 0.720 0.280
#&gt; SRR1539247     1  0.5058      0.646 0.756 0.000 0.244
#&gt; SRR1539248     1  0.0237      0.848 0.996 0.000 0.004
#&gt; SRR1539249     2  0.5397      0.884 0.000 0.720 0.280
#&gt; SRR1539250     1  0.5138      0.640 0.748 0.000 0.252
#&gt; SRR1539251     1  0.5138      0.640 0.748 0.000 0.252
#&gt; SRR1539253     2  0.5397      0.884 0.000 0.720 0.280
#&gt; SRR1539252     1  0.0000      0.849 1.000 0.000 0.000
#&gt; SRR1539255     1  0.0000      0.849 1.000 0.000 0.000
#&gt; SRR1539254     1  0.5058      0.646 0.756 0.000 0.244
#&gt; SRR1539256     2  0.5397      0.884 0.000 0.720 0.280
#&gt; SRR1539257     1  0.5058      0.646 0.756 0.000 0.244
#&gt; SRR1539258     1  0.0237      0.848 0.996 0.000 0.004
#&gt; SRR1539259     2  0.5397      0.884 0.000 0.720 0.280
#&gt; SRR1539260     1  0.5058      0.646 0.756 0.000 0.244
#&gt; SRR1539262     2  0.5465      0.881 0.000 0.712 0.288
#&gt; SRR1539261     1  0.0747      0.839 0.984 0.000 0.016
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3  0.5957      0.967 0.364 0.000 0.588 0.048
#&gt; SRR1539208     4  0.7771      0.879 0.328 0.000 0.252 0.420
#&gt; SRR1539211     4  0.7764      0.876 0.324 0.000 0.252 0.424
#&gt; SRR1539210     3  0.6446      0.910 0.328 0.000 0.584 0.088
#&gt; SRR1539209     2  0.5853      0.742 0.000 0.508 0.032 0.460
#&gt; SRR1539212     2  0.5853      0.742 0.000 0.508 0.032 0.460
#&gt; SRR1539214     1  0.7674     -0.561 0.460 0.000 0.260 0.280
#&gt; SRR1539213     3  0.4730      0.974 0.364 0.000 0.636 0.000
#&gt; SRR1539215     2  0.4999      0.743 0.000 0.508 0.000 0.492
#&gt; SRR1539216     3  0.5957      0.967 0.364 0.000 0.588 0.048
#&gt; SRR1539217     4  0.7824      0.839 0.348 0.000 0.260 0.392
#&gt; SRR1539218     2  0.5853      0.742 0.000 0.508 0.032 0.460
#&gt; SRR1539220     1  0.7674     -0.561 0.460 0.000 0.260 0.280
#&gt; SRR1539219     3  0.4730      0.974 0.364 0.000 0.636 0.000
#&gt; SRR1539221     2  0.4999      0.743 0.000 0.508 0.000 0.492
#&gt; SRR1539223     4  0.7847      0.878 0.328 0.000 0.276 0.396
#&gt; SRR1539224     2  0.5853      0.742 0.000 0.508 0.032 0.460
#&gt; SRR1539222     3  0.5957      0.967 0.364 0.000 0.588 0.048
#&gt; SRR1539225     3  0.4730      0.974 0.364 0.000 0.636 0.000
#&gt; SRR1539227     2  0.4999      0.743 0.000 0.508 0.000 0.492
#&gt; SRR1539226     1  0.7674     -0.561 0.460 0.000 0.260 0.280
#&gt; SRR1539228     3  0.4730      0.974 0.364 0.000 0.636 0.000
#&gt; SRR1539229     1  0.7674     -0.561 0.460 0.000 0.260 0.280
#&gt; SRR1539232     3  0.4730      0.974 0.364 0.000 0.636 0.000
#&gt; SRR1539230     2  0.4999      0.743 0.000 0.508 0.000 0.492
#&gt; SRR1539231     2  0.4999      0.743 0.000 0.508 0.000 0.492
#&gt; SRR1539234     2  0.0921      0.772 0.000 0.972 0.028 0.000
#&gt; SRR1539233     1  0.7674     -0.561 0.460 0.000 0.260 0.280
#&gt; SRR1539235     1  0.0000      0.485 1.000 0.000 0.000 0.000
#&gt; SRR1539236     1  0.7674     -0.561 0.460 0.000 0.260 0.280
#&gt; SRR1539237     2  0.0188      0.773 0.000 0.996 0.004 0.000
#&gt; SRR1539238     1  0.0000      0.485 1.000 0.000 0.000 0.000
#&gt; SRR1539239     4  0.7677      0.872 0.372 0.000 0.216 0.412
#&gt; SRR1539242     4  0.7677      0.872 0.372 0.000 0.216 0.412
#&gt; SRR1539240     2  0.0592      0.773 0.000 0.984 0.016 0.000
#&gt; SRR1539241     1  0.0000      0.485 1.000 0.000 0.000 0.000
#&gt; SRR1539243     2  0.0592      0.773 0.000 0.984 0.016 0.000
#&gt; SRR1539244     1  0.0000      0.485 1.000 0.000 0.000 0.000
#&gt; SRR1539245     1  0.7674     -0.561 0.460 0.000 0.260 0.280
#&gt; SRR1539246     2  0.0921      0.772 0.000 0.972 0.028 0.000
#&gt; SRR1539247     1  0.0000      0.485 1.000 0.000 0.000 0.000
#&gt; SRR1539248     4  0.7677      0.872 0.372 0.000 0.216 0.412
#&gt; SRR1539249     2  0.0188      0.773 0.000 0.996 0.004 0.000
#&gt; SRR1539250     1  0.1022      0.450 0.968 0.000 0.000 0.032
#&gt; SRR1539251     1  0.1022      0.450 0.968 0.000 0.000 0.032
#&gt; SRR1539253     2  0.0188      0.773 0.000 0.996 0.004 0.000
#&gt; SRR1539252     1  0.7674     -0.561 0.460 0.000 0.260 0.280
#&gt; SRR1539255     1  0.7824     -0.777 0.392 0.000 0.260 0.348
#&gt; SRR1539254     1  0.0000      0.485 1.000 0.000 0.000 0.000
#&gt; SRR1539256     2  0.0592      0.773 0.000 0.984 0.016 0.000
#&gt; SRR1539257     1  0.0000      0.485 1.000 0.000 0.000 0.000
#&gt; SRR1539258     1  0.7834     -0.825 0.372 0.000 0.260 0.368
#&gt; SRR1539259     2  0.0188      0.773 0.000 0.996 0.004 0.000
#&gt; SRR1539260     1  0.0000      0.485 1.000 0.000 0.000 0.000
#&gt; SRR1539262     2  0.0188      0.773 0.000 0.996 0.004 0.000
#&gt; SRR1539261     4  0.7764      0.876 0.324 0.000 0.252 0.424
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.4953      0.924 0.024 0.000 0.716 0.044 0.216
#&gt; SRR1539208     1  0.6360      0.609 0.576 0.000 0.060 0.300 0.064
#&gt; SRR1539211     1  0.6553      0.592 0.556 0.000 0.068 0.308 0.068
#&gt; SRR1539210     3  0.6604      0.693 0.024 0.000 0.564 0.196 0.216
#&gt; SRR1539209     4  0.6736      0.896 0.000 0.388 0.056 0.476 0.080
#&gt; SRR1539212     4  0.6736      0.896 0.000 0.388 0.056 0.476 0.080
#&gt; SRR1539214     1  0.2069      0.806 0.912 0.000 0.012 0.000 0.076
#&gt; SRR1539213     3  0.3745      0.937 0.024 0.000 0.780 0.000 0.196
#&gt; SRR1539215     4  0.4415      0.916 0.000 0.388 0.008 0.604 0.000
#&gt; SRR1539216     3  0.4953      0.924 0.024 0.000 0.716 0.044 0.216
#&gt; SRR1539217     1  0.2026      0.807 0.928 0.000 0.012 0.044 0.016
#&gt; SRR1539218     4  0.6736      0.896 0.000 0.388 0.056 0.476 0.080
#&gt; SRR1539220     1  0.2069      0.806 0.912 0.000 0.012 0.000 0.076
#&gt; SRR1539219     3  0.3745      0.937 0.024 0.000 0.780 0.000 0.196
#&gt; SRR1539221     4  0.4150      0.917 0.000 0.388 0.000 0.612 0.000
#&gt; SRR1539223     1  0.6273      0.621 0.596 0.000 0.060 0.280 0.064
#&gt; SRR1539224     4  0.6736      0.896 0.000 0.388 0.056 0.476 0.080
#&gt; SRR1539222     3  0.5415      0.904 0.024 0.000 0.688 0.076 0.212
#&gt; SRR1539225     3  0.3745      0.937 0.024 0.000 0.780 0.000 0.196
#&gt; SRR1539227     4  0.4150      0.917 0.000 0.388 0.000 0.612 0.000
#&gt; SRR1539226     1  0.2069      0.806 0.912 0.000 0.012 0.000 0.076
#&gt; SRR1539228     3  0.3745      0.937 0.024 0.000 0.780 0.000 0.196
#&gt; SRR1539229     1  0.2069      0.806 0.912 0.000 0.012 0.000 0.076
#&gt; SRR1539232     3  0.3745      0.937 0.024 0.000 0.780 0.000 0.196
#&gt; SRR1539230     4  0.4150      0.917 0.000 0.388 0.000 0.612 0.000
#&gt; SRR1539231     4  0.4150      0.917 0.000 0.388 0.000 0.612 0.000
#&gt; SRR1539234     2  0.1725      0.950 0.000 0.936 0.044 0.000 0.020
#&gt; SRR1539233     1  0.2069      0.806 0.912 0.000 0.012 0.000 0.076
#&gt; SRR1539235     5  0.3266      0.993 0.200 0.000 0.000 0.004 0.796
#&gt; SRR1539236     1  0.2069      0.805 0.912 0.000 0.012 0.000 0.076
#&gt; SRR1539237     2  0.0955      0.960 0.000 0.968 0.028 0.000 0.004
#&gt; SRR1539238     5  0.3266      0.993 0.200 0.000 0.000 0.004 0.796
#&gt; SRR1539239     1  0.2921      0.785 0.856 0.000 0.020 0.124 0.000
#&gt; SRR1539242     1  0.2921      0.785 0.856 0.000 0.020 0.124 0.000
#&gt; SRR1539240     2  0.1522      0.958 0.000 0.944 0.044 0.000 0.012
#&gt; SRR1539241     5  0.3266      0.993 0.200 0.000 0.000 0.004 0.796
#&gt; SRR1539243     2  0.1522      0.958 0.000 0.944 0.044 0.000 0.012
#&gt; SRR1539244     5  0.3266      0.993 0.200 0.000 0.000 0.004 0.796
#&gt; SRR1539245     1  0.1671      0.806 0.924 0.000 0.000 0.000 0.076
#&gt; SRR1539246     2  0.1386      0.951 0.000 0.952 0.032 0.000 0.016
#&gt; SRR1539247     5  0.3109      0.993 0.200 0.000 0.000 0.000 0.800
#&gt; SRR1539248     1  0.2921      0.785 0.856 0.000 0.020 0.124 0.000
#&gt; SRR1539249     2  0.0162      0.961 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1539250     5  0.2966      0.977 0.184 0.000 0.000 0.000 0.816
#&gt; SRR1539251     5  0.2966      0.977 0.184 0.000 0.000 0.000 0.816
#&gt; SRR1539253     2  0.0000      0.961 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539252     1  0.1671      0.806 0.924 0.000 0.000 0.000 0.076
#&gt; SRR1539255     1  0.1106      0.812 0.964 0.000 0.012 0.000 0.024
#&gt; SRR1539254     5  0.3109      0.993 0.200 0.000 0.000 0.000 0.800
#&gt; SRR1539256     2  0.1522      0.958 0.000 0.944 0.044 0.000 0.012
#&gt; SRR1539257     5  0.3109      0.993 0.200 0.000 0.000 0.000 0.800
#&gt; SRR1539258     1  0.1522      0.807 0.944 0.000 0.012 0.044 0.000
#&gt; SRR1539259     2  0.0162      0.960 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1539260     5  0.3109      0.993 0.200 0.000 0.000 0.000 0.800
#&gt; SRR1539262     2  0.0324      0.960 0.000 0.992 0.004 0.000 0.004
#&gt; SRR1539261     1  0.6430      0.603 0.568 0.000 0.060 0.304 0.068
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.5417      0.853 0.060 0.112 0.692 0.000 0.128 0.008
#&gt; SRR1539208     6  0.1807      0.643 0.020 0.000 0.000 0.000 0.060 0.920
#&gt; SRR1539211     6  0.2513      0.615 0.000 0.044 0.008 0.000 0.060 0.888
#&gt; SRR1539210     3  0.7970      0.575 0.080 0.216 0.400 0.000 0.072 0.232
#&gt; SRR1539209     4  0.3122      0.870 0.160 0.000 0.020 0.816 0.000 0.004
#&gt; SRR1539212     4  0.3331      0.865 0.160 0.000 0.020 0.808 0.000 0.012
#&gt; SRR1539214     1  0.6341      0.856 0.564 0.028 0.024 0.000 0.148 0.236
#&gt; SRR1539213     3  0.2178      0.884 0.000 0.000 0.868 0.000 0.132 0.000
#&gt; SRR1539215     4  0.0000      0.899 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539216     3  0.5417      0.853 0.060 0.112 0.692 0.000 0.128 0.008
#&gt; SRR1539217     1  0.6288      0.577 0.464 0.028 0.024 0.000 0.088 0.396
#&gt; SRR1539218     4  0.3017      0.870 0.164 0.000 0.020 0.816 0.000 0.000
#&gt; SRR1539220     1  0.6379      0.848 0.556 0.028 0.024 0.000 0.148 0.244
#&gt; SRR1539219     3  0.2135      0.885 0.000 0.000 0.872 0.000 0.128 0.000
#&gt; SRR1539221     4  0.0000      0.899 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539223     6  0.2960      0.617 0.056 0.008 0.008 0.000 0.060 0.868
#&gt; SRR1539224     4  0.3017      0.870 0.164 0.000 0.020 0.816 0.000 0.000
#&gt; SRR1539222     3  0.6199      0.813 0.080 0.172 0.616 0.000 0.120 0.012
#&gt; SRR1539225     3  0.2178      0.884 0.000 0.000 0.868 0.000 0.132 0.000
#&gt; SRR1539227     4  0.0000      0.899 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539226     1  0.6122      0.861 0.576 0.024 0.016 0.000 0.148 0.236
#&gt; SRR1539228     3  0.2178      0.884 0.000 0.000 0.868 0.000 0.132 0.000
#&gt; SRR1539229     1  0.6122      0.861 0.576 0.024 0.016 0.000 0.148 0.236
#&gt; SRR1539232     3  0.2431      0.884 0.008 0.000 0.860 0.000 0.132 0.000
#&gt; SRR1539230     4  0.0000      0.899 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539231     4  0.0000      0.899 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539234     2  0.5993      0.907 0.064 0.576 0.036 0.296 0.000 0.028
#&gt; SRR1539233     1  0.6122      0.861 0.576 0.024 0.016 0.000 0.148 0.236
#&gt; SRR1539235     5  0.0777      0.978 0.024 0.004 0.000 0.000 0.972 0.000
#&gt; SRR1539236     1  0.6438      0.808 0.556 0.044 0.016 0.000 0.148 0.236
#&gt; SRR1539237     2  0.5062      0.927 0.044 0.632 0.016 0.296 0.000 0.012
#&gt; SRR1539238     5  0.0922      0.979 0.024 0.004 0.000 0.000 0.968 0.004
#&gt; SRR1539239     6  0.6300      0.185 0.304 0.036 0.024 0.000 0.096 0.540
#&gt; SRR1539242     6  0.6300      0.185 0.304 0.036 0.024 0.000 0.096 0.540
#&gt; SRR1539240     2  0.5651      0.926 0.064 0.596 0.024 0.296 0.000 0.020
#&gt; SRR1539241     5  0.0922      0.979 0.024 0.004 0.000 0.000 0.968 0.004
#&gt; SRR1539243     2  0.5651      0.926 0.064 0.596 0.024 0.296 0.000 0.020
#&gt; SRR1539244     5  0.0777      0.978 0.024 0.004 0.000 0.000 0.972 0.000
#&gt; SRR1539245     1  0.5384      0.853 0.608 0.008 0.000 0.000 0.148 0.236
#&gt; SRR1539246     2  0.5438      0.909 0.048 0.612 0.024 0.296 0.000 0.020
#&gt; SRR1539247     5  0.0000      0.982 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539248     6  0.6300      0.185 0.304 0.036 0.024 0.000 0.096 0.540
#&gt; SRR1539249     2  0.3528      0.928 0.004 0.700 0.000 0.296 0.000 0.000
#&gt; SRR1539250     5  0.0837      0.970 0.004 0.004 0.000 0.000 0.972 0.020
#&gt; SRR1539251     5  0.0837      0.970 0.004 0.004 0.000 0.000 0.972 0.020
#&gt; SRR1539253     2  0.3528      0.928 0.004 0.700 0.000 0.296 0.000 0.000
#&gt; SRR1539252     1  0.5384      0.853 0.608 0.008 0.000 0.000 0.148 0.236
#&gt; SRR1539255     1  0.6340      0.775 0.556 0.044 0.016 0.000 0.120 0.264
#&gt; SRR1539254     5  0.0146      0.982 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1539256     2  0.5651      0.926 0.064 0.596 0.024 0.296 0.000 0.020
#&gt; SRR1539257     5  0.0000      0.982 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539258     1  0.6432      0.495 0.464 0.044 0.016 0.000 0.096 0.380
#&gt; SRR1539259     2  0.4004      0.923 0.012 0.684 0.004 0.296 0.000 0.004
#&gt; SRR1539260     5  0.0146      0.982 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1539262     2  0.4302      0.919 0.016 0.672 0.008 0.296 0.000 0.008
#&gt; SRR1539261     6  0.1781      0.639 0.000 0.008 0.008 0.000 0.060 0.924
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.965       0.986         0.4650 0.544   0.544
#> 3 3 1.000           0.983       0.993         0.4590 0.778   0.591
#> 4 4 0.902           0.925       0.894         0.0817 0.942   0.818
#> 5 5 0.882           0.879       0.887         0.0685 0.964   0.862
#> 6 6 0.966           0.947       0.966         0.0653 0.942   0.742
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1539207     1   0.000      0.978 1.000 0.000
#&gt; SRR1539208     1   0.000      0.978 1.000 0.000
#&gt; SRR1539211     1   0.961      0.394 0.616 0.384
#&gt; SRR1539210     1   0.000      0.978 1.000 0.000
#&gt; SRR1539209     2   0.000      1.000 0.000 1.000
#&gt; SRR1539212     2   0.000      1.000 0.000 1.000
#&gt; SRR1539214     1   0.000      0.978 1.000 0.000
#&gt; SRR1539213     1   0.000      0.978 1.000 0.000
#&gt; SRR1539215     2   0.000      1.000 0.000 1.000
#&gt; SRR1539216     1   0.000      0.978 1.000 0.000
#&gt; SRR1539217     1   0.000      0.978 1.000 0.000
#&gt; SRR1539218     2   0.000      1.000 0.000 1.000
#&gt; SRR1539220     1   0.000      0.978 1.000 0.000
#&gt; SRR1539219     1   0.000      0.978 1.000 0.000
#&gt; SRR1539221     2   0.000      1.000 0.000 1.000
#&gt; SRR1539223     1   0.000      0.978 1.000 0.000
#&gt; SRR1539224     2   0.000      1.000 0.000 1.000
#&gt; SRR1539222     1   0.000      0.978 1.000 0.000
#&gt; SRR1539225     1   0.000      0.978 1.000 0.000
#&gt; SRR1539227     2   0.000      1.000 0.000 1.000
#&gt; SRR1539226     1   0.000      0.978 1.000 0.000
#&gt; SRR1539228     1   0.000      0.978 1.000 0.000
#&gt; SRR1539229     1   0.000      0.978 1.000 0.000
#&gt; SRR1539232     1   0.000      0.978 1.000 0.000
#&gt; SRR1539230     2   0.000      1.000 0.000 1.000
#&gt; SRR1539231     2   0.000      1.000 0.000 1.000
#&gt; SRR1539234     2   0.000      1.000 0.000 1.000
#&gt; SRR1539233     1   0.000      0.978 1.000 0.000
#&gt; SRR1539235     1   0.000      0.978 1.000 0.000
#&gt; SRR1539236     1   0.000      0.978 1.000 0.000
#&gt; SRR1539237     2   0.000      1.000 0.000 1.000
#&gt; SRR1539238     1   0.000      0.978 1.000 0.000
#&gt; SRR1539239     1   0.000      0.978 1.000 0.000
#&gt; SRR1539242     1   0.000      0.978 1.000 0.000
#&gt; SRR1539240     2   0.000      1.000 0.000 1.000
#&gt; SRR1539241     1   0.000      0.978 1.000 0.000
#&gt; SRR1539243     2   0.000      1.000 0.000 1.000
#&gt; SRR1539244     1   0.000      0.978 1.000 0.000
#&gt; SRR1539245     1   0.000      0.978 1.000 0.000
#&gt; SRR1539246     2   0.000      1.000 0.000 1.000
#&gt; SRR1539247     1   0.000      0.978 1.000 0.000
#&gt; SRR1539248     1   0.000      0.978 1.000 0.000
#&gt; SRR1539249     2   0.000      1.000 0.000 1.000
#&gt; SRR1539250     1   0.000      0.978 1.000 0.000
#&gt; SRR1539251     1   0.000      0.978 1.000 0.000
#&gt; SRR1539253     2   0.000      1.000 0.000 1.000
#&gt; SRR1539252     1   0.000      0.978 1.000 0.000
#&gt; SRR1539255     1   0.000      0.978 1.000 0.000
#&gt; SRR1539254     1   0.000      0.978 1.000 0.000
#&gt; SRR1539256     2   0.000      1.000 0.000 1.000
#&gt; SRR1539257     1   0.000      0.978 1.000 0.000
#&gt; SRR1539258     1   0.000      0.978 1.000 0.000
#&gt; SRR1539259     2   0.000      1.000 0.000 1.000
#&gt; SRR1539260     1   0.000      0.978 1.000 0.000
#&gt; SRR1539262     2   0.000      1.000 0.000 1.000
#&gt; SRR1539261     1   0.961      0.394 0.616 0.384
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3
#&gt; SRR1539207     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539208     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539211     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539210     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539209     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539212     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539214     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539213     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539215     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539216     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539217     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539218     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539220     1   0.597      0.428 0.636  0 0.364
#&gt; SRR1539219     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539221     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539223     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539224     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539222     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539225     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539227     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539226     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539228     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539229     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539232     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539230     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539231     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539234     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539233     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539235     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539236     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539237     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539238     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539239     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539242     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539240     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539241     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539243     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539244     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539245     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539246     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539247     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539248     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539249     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539250     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539251     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539253     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539252     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539255     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539254     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539256     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539257     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539258     1   0.000      0.978 1.000  0 0.000
#&gt; SRR1539259     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539260     3   0.000      1.000 0.000  0 1.000
#&gt; SRR1539262     2   0.000      1.000 0.000  1 0.000
#&gt; SRR1539261     1   0.000      0.978 1.000  0 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3  0.0000      0.934 0.000 0.000 1.000 0.000
#&gt; SRR1539208     1  0.4406      0.785 0.700 0.000 0.000 0.300
#&gt; SRR1539211     1  0.4406      0.785 0.700 0.000 0.000 0.300
#&gt; SRR1539210     3  0.4304      0.560 0.000 0.000 0.716 0.284
#&gt; SRR1539209     2  0.2149      0.959 0.000 0.912 0.000 0.088
#&gt; SRR1539212     2  0.2149      0.959 0.000 0.912 0.000 0.088
#&gt; SRR1539214     1  0.0336      0.909 0.992 0.000 0.000 0.008
#&gt; SRR1539213     3  0.0000      0.934 0.000 0.000 1.000 0.000
#&gt; SRR1539215     2  0.2149      0.959 0.000 0.912 0.000 0.088
#&gt; SRR1539216     3  0.0000      0.934 0.000 0.000 1.000 0.000
#&gt; SRR1539217     1  0.3837      0.825 0.776 0.000 0.000 0.224
#&gt; SRR1539218     2  0.2149      0.959 0.000 0.912 0.000 0.088
#&gt; SRR1539220     1  0.4538      0.674 0.760 0.000 0.216 0.024
#&gt; SRR1539219     3  0.0000      0.934 0.000 0.000 1.000 0.000
#&gt; SRR1539221     2  0.2149      0.959 0.000 0.912 0.000 0.088
#&gt; SRR1539223     1  0.4406      0.785 0.700 0.000 0.000 0.300
#&gt; SRR1539224     2  0.2149      0.959 0.000 0.912 0.000 0.088
#&gt; SRR1539222     3  0.0000      0.934 0.000 0.000 1.000 0.000
#&gt; SRR1539225     3  0.0000      0.934 0.000 0.000 1.000 0.000
#&gt; SRR1539227     2  0.2149      0.959 0.000 0.912 0.000 0.088
#&gt; SRR1539226     1  0.0336      0.909 0.992 0.000 0.000 0.008
#&gt; SRR1539228     3  0.0000      0.934 0.000 0.000 1.000 0.000
#&gt; SRR1539229     1  0.0336      0.909 0.992 0.000 0.000 0.008
#&gt; SRR1539232     3  0.0000      0.934 0.000 0.000 1.000 0.000
#&gt; SRR1539230     2  0.2149      0.959 0.000 0.912 0.000 0.088
#&gt; SRR1539231     2  0.2149      0.959 0.000 0.912 0.000 0.088
#&gt; SRR1539234     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; SRR1539233     1  0.0336      0.909 0.992 0.000 0.000 0.008
#&gt; SRR1539235     4  0.4817      0.997 0.000 0.000 0.388 0.612
#&gt; SRR1539236     1  0.0336      0.909 0.992 0.000 0.000 0.008
#&gt; SRR1539237     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; SRR1539238     4  0.4817      0.997 0.000 0.000 0.388 0.612
#&gt; SRR1539239     1  0.0592      0.907 0.984 0.000 0.000 0.016
#&gt; SRR1539242     1  0.0592      0.907 0.984 0.000 0.000 0.016
#&gt; SRR1539240     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; SRR1539241     4  0.4817      0.997 0.000 0.000 0.388 0.612
#&gt; SRR1539243     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; SRR1539244     4  0.4817      0.997 0.000 0.000 0.388 0.612
#&gt; SRR1539245     1  0.0336      0.909 0.992 0.000 0.000 0.008
#&gt; SRR1539246     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; SRR1539247     4  0.4817      0.997 0.000 0.000 0.388 0.612
#&gt; SRR1539248     1  0.0592      0.907 0.984 0.000 0.000 0.016
#&gt; SRR1539249     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; SRR1539250     4  0.4843      0.986 0.000 0.000 0.396 0.604
#&gt; SRR1539251     4  0.4843      0.986 0.000 0.000 0.396 0.604
#&gt; SRR1539253     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; SRR1539252     1  0.0336      0.909 0.992 0.000 0.000 0.008
#&gt; SRR1539255     1  0.0336      0.909 0.992 0.000 0.000 0.008
#&gt; SRR1539254     4  0.4817      0.997 0.000 0.000 0.388 0.612
#&gt; SRR1539256     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; SRR1539257     4  0.4817      0.997 0.000 0.000 0.388 0.612
#&gt; SRR1539258     1  0.0469      0.907 0.988 0.000 0.000 0.012
#&gt; SRR1539259     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; SRR1539260     4  0.4817      0.997 0.000 0.000 0.388 0.612
#&gt; SRR1539262     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; SRR1539261     1  0.4406      0.785 0.700 0.000 0.000 0.300
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3   0.161      0.943 0.000 0.000 0.928 0.000 0.072
#&gt; SRR1539208     4   0.382      0.989 0.304 0.000 0.000 0.696 0.000
#&gt; SRR1539211     4   0.382      0.989 0.304 0.000 0.000 0.696 0.000
#&gt; SRR1539210     3   0.429      0.264 0.000 0.000 0.536 0.464 0.000
#&gt; SRR1539209     2   0.000      0.790 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539212     2   0.000      0.790 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539214     1   0.000      0.921 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539213     3   0.161      0.943 0.000 0.000 0.928 0.000 0.072
#&gt; SRR1539215     2   0.000      0.790 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539216     3   0.161      0.943 0.000 0.000 0.928 0.000 0.072
#&gt; SRR1539217     1   0.311      0.649 0.800 0.000 0.000 0.200 0.000
#&gt; SRR1539218     2   0.000      0.790 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539220     1   0.233      0.742 0.876 0.000 0.000 0.000 0.124
#&gt; SRR1539219     3   0.161      0.943 0.000 0.000 0.928 0.000 0.072
#&gt; SRR1539221     2   0.000      0.790 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539223     4   0.391      0.967 0.324 0.000 0.000 0.676 0.000
#&gt; SRR1539224     2   0.000      0.790 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539222     3   0.161      0.943 0.000 0.000 0.928 0.000 0.072
#&gt; SRR1539225     3   0.161      0.943 0.000 0.000 0.928 0.000 0.072
#&gt; SRR1539227     2   0.000      0.790 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539226     1   0.000      0.921 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539228     3   0.161      0.943 0.000 0.000 0.928 0.000 0.072
#&gt; SRR1539229     1   0.000      0.921 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539232     3   0.161      0.943 0.000 0.000 0.928 0.000 0.072
#&gt; SRR1539230     2   0.000      0.790 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539231     2   0.000      0.790 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539234     2   0.525      0.814 0.000 0.624 0.072 0.304 0.000
#&gt; SRR1539233     1   0.000      0.921 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539235     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539236     1   0.000      0.921 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539237     2   0.525      0.814 0.000 0.624 0.072 0.304 0.000
#&gt; SRR1539238     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539239     1   0.202      0.858 0.900 0.000 0.000 0.100 0.000
#&gt; SRR1539242     1   0.202      0.858 0.900 0.000 0.000 0.100 0.000
#&gt; SRR1539240     2   0.525      0.814 0.000 0.624 0.072 0.304 0.000
#&gt; SRR1539241     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539243     2   0.525      0.814 0.000 0.624 0.072 0.304 0.000
#&gt; SRR1539244     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539245     1   0.000      0.921 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539246     2   0.525      0.814 0.000 0.624 0.072 0.304 0.000
#&gt; SRR1539247     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539248     1   0.202      0.858 0.900 0.000 0.000 0.100 0.000
#&gt; SRR1539249     2   0.525      0.814 0.000 0.624 0.072 0.304 0.000
#&gt; SRR1539250     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539251     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539253     2   0.525      0.814 0.000 0.624 0.072 0.304 0.000
#&gt; SRR1539252     1   0.000      0.921 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539255     1   0.000      0.921 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539254     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539256     2   0.525      0.814 0.000 0.624 0.072 0.304 0.000
#&gt; SRR1539257     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539258     1   0.127      0.893 0.948 0.000 0.000 0.052 0.000
#&gt; SRR1539259     2   0.525      0.814 0.000 0.624 0.072 0.304 0.000
#&gt; SRR1539260     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539262     2   0.525      0.814 0.000 0.624 0.072 0.304 0.000
#&gt; SRR1539261     4   0.382      0.989 0.304 0.000 0.000 0.696 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.0146      0.941 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539208     6  0.0146      0.979 0.004 0.000 0.000 0.000 0.000 0.996
#&gt; SRR1539211     6  0.0146      0.979 0.004 0.000 0.000 0.000 0.000 0.996
#&gt; SRR1539210     3  0.3828      0.216 0.000 0.000 0.560 0.000 0.000 0.440
#&gt; SRR1539209     4  0.0935      0.998 0.000 0.032 0.000 0.964 0.000 0.004
#&gt; SRR1539212     4  0.0935      0.998 0.000 0.032 0.000 0.964 0.000 0.004
#&gt; SRR1539214     1  0.0260      0.920 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; SRR1539213     3  0.0291      0.941 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; SRR1539215     4  0.0790      0.998 0.000 0.032 0.000 0.968 0.000 0.000
#&gt; SRR1539216     3  0.0146      0.941 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539217     1  0.2772      0.809 0.816 0.000 0.000 0.004 0.000 0.180
#&gt; SRR1539218     4  0.0935      0.998 0.000 0.032 0.000 0.964 0.000 0.004
#&gt; SRR1539220     1  0.1462      0.882 0.936 0.000 0.000 0.008 0.056 0.000
#&gt; SRR1539219     3  0.0146      0.941 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539221     4  0.0790      0.998 0.000 0.032 0.000 0.968 0.000 0.000
#&gt; SRR1539223     6  0.1141      0.938 0.052 0.000 0.000 0.000 0.000 0.948
#&gt; SRR1539224     4  0.0935      0.998 0.000 0.032 0.000 0.964 0.000 0.004
#&gt; SRR1539222     3  0.0146      0.941 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539225     3  0.0291      0.941 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; SRR1539227     4  0.0790      0.998 0.000 0.032 0.000 0.968 0.000 0.000
#&gt; SRR1539226     1  0.0146      0.921 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; SRR1539228     3  0.0291      0.941 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; SRR1539229     1  0.0146      0.921 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; SRR1539232     3  0.0291      0.941 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; SRR1539230     4  0.0790      0.998 0.000 0.032 0.000 0.968 0.000 0.000
#&gt; SRR1539231     4  0.0790      0.998 0.000 0.032 0.000 0.968 0.000 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539233     1  0.0146      0.921 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; SRR1539235     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539236     1  0.0777      0.919 0.972 0.000 0.004 0.024 0.000 0.000
#&gt; SRR1539237     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539238     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539239     1  0.3376      0.822 0.792 0.000 0.004 0.024 0.000 0.180
#&gt; SRR1539242     1  0.3376      0.822 0.792 0.000 0.004 0.024 0.000 0.180
#&gt; SRR1539240     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539241     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539243     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539244     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539245     1  0.0000      0.922 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539246     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539247     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539248     1  0.3376      0.822 0.792 0.000 0.004 0.024 0.000 0.180
#&gt; SRR1539249     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539250     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539251     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539253     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539252     1  0.0260      0.921 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; SRR1539255     1  0.0777      0.919 0.972 0.000 0.004 0.024 0.000 0.000
#&gt; SRR1539254     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539256     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539258     1  0.2152      0.895 0.904 0.000 0.004 0.024 0.000 0.068
#&gt; SRR1539259     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539260     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539262     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539261     6  0.0146      0.979 0.004 0.000 0.000 0.000 0.000 0.996
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.878           0.945       0.936         0.4458 0.782   0.599
#> 4 4 0.761           0.930       0.928         0.1009 0.942   0.820
#> 5 5 1.000           0.966       0.987         0.1009 0.896   0.634
#> 6 6 0.934           0.906       0.908         0.0311 0.953   0.785
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 5
```

There is also optional best $k$ = 2 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1539207     3  0.0000      0.911 0.000 0.000 1.000
#&gt; SRR1539208     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539211     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539210     3  0.0424      0.908 0.008 0.000 0.992
#&gt; SRR1539209     2  0.0000      0.974 0.000 1.000 0.000
#&gt; SRR1539212     2  0.0000      0.974 0.000 1.000 0.000
#&gt; SRR1539214     1  0.2959      0.955 0.900 0.000 0.100
#&gt; SRR1539213     3  0.0000      0.911 0.000 0.000 1.000
#&gt; SRR1539215     2  0.0000      0.974 0.000 1.000 0.000
#&gt; SRR1539216     3  0.0000      0.911 0.000 0.000 1.000
#&gt; SRR1539217     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539218     2  0.0000      0.974 0.000 1.000 0.000
#&gt; SRR1539220     3  0.5926      0.524 0.356 0.000 0.644
#&gt; SRR1539219     3  0.0000      0.911 0.000 0.000 1.000
#&gt; SRR1539221     2  0.0000      0.974 0.000 1.000 0.000
#&gt; SRR1539223     3  0.5835      0.566 0.340 0.000 0.660
#&gt; SRR1539224     2  0.0000      0.974 0.000 1.000 0.000
#&gt; SRR1539222     3  0.0000      0.911 0.000 0.000 1.000
#&gt; SRR1539225     3  0.0000      0.911 0.000 0.000 1.000
#&gt; SRR1539227     2  0.0000      0.974 0.000 1.000 0.000
#&gt; SRR1539226     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539228     3  0.0000      0.911 0.000 0.000 1.000
#&gt; SRR1539229     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539232     3  0.0000      0.911 0.000 0.000 1.000
#&gt; SRR1539230     2  0.0000      0.974 0.000 1.000 0.000
#&gt; SRR1539231     2  0.0000      0.974 0.000 1.000 0.000
#&gt; SRR1539234     2  0.2165      0.976 0.064 0.936 0.000
#&gt; SRR1539233     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539235     3  0.2796      0.919 0.092 0.000 0.908
#&gt; SRR1539236     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539237     2  0.2165      0.976 0.064 0.936 0.000
#&gt; SRR1539238     3  0.2796      0.919 0.092 0.000 0.908
#&gt; SRR1539239     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539242     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539240     2  0.2165      0.976 0.064 0.936 0.000
#&gt; SRR1539241     3  0.2796      0.919 0.092 0.000 0.908
#&gt; SRR1539243     2  0.2165      0.976 0.064 0.936 0.000
#&gt; SRR1539244     3  0.2796      0.919 0.092 0.000 0.908
#&gt; SRR1539245     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539246     2  0.2165      0.976 0.064 0.936 0.000
#&gt; SRR1539247     3  0.2796      0.919 0.092 0.000 0.908
#&gt; SRR1539248     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539249     2  0.2165      0.976 0.064 0.936 0.000
#&gt; SRR1539250     3  0.2796      0.919 0.092 0.000 0.908
#&gt; SRR1539251     3  0.2796      0.919 0.092 0.000 0.908
#&gt; SRR1539253     2  0.2165      0.976 0.064 0.936 0.000
#&gt; SRR1539252     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539255     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539254     3  0.2796      0.919 0.092 0.000 0.908
#&gt; SRR1539256     2  0.2165      0.976 0.064 0.936 0.000
#&gt; SRR1539257     3  0.2796      0.919 0.092 0.000 0.908
#&gt; SRR1539258     1  0.2165      0.997 0.936 0.000 0.064
#&gt; SRR1539259     2  0.2165      0.976 0.064 0.936 0.000
#&gt; SRR1539260     3  0.2796      0.919 0.092 0.000 0.908
#&gt; SRR1539262     2  0.2165      0.976 0.064 0.936 0.000
#&gt; SRR1539261     1  0.2165      0.997 0.936 0.000 0.064
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3   0.234      0.843 0.000 0.000 0.900 0.100
#&gt; SRR1539208     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539211     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539210     3   0.267      0.841 0.008 0.000 0.892 0.100
#&gt; SRR1539209     4   0.234      0.989 0.000 0.100 0.000 0.900
#&gt; SRR1539212     4   0.336      0.908 0.000 0.176 0.000 0.824
#&gt; SRR1539214     1   0.112      0.955 0.964 0.000 0.036 0.000
#&gt; SRR1539213     3   0.234      0.843 0.000 0.000 0.900 0.100
#&gt; SRR1539215     4   0.234      0.989 0.000 0.100 0.000 0.900
#&gt; SRR1539216     3   0.234      0.843 0.000 0.000 0.900 0.100
#&gt; SRR1539217     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539218     4   0.234      0.989 0.000 0.100 0.000 0.900
#&gt; SRR1539220     3   0.491      0.483 0.420 0.000 0.580 0.000
#&gt; SRR1539219     3   0.234      0.843 0.000 0.000 0.900 0.100
#&gt; SRR1539221     4   0.234      0.989 0.000 0.100 0.000 0.900
#&gt; SRR1539223     3   0.480      0.568 0.384 0.000 0.616 0.000
#&gt; SRR1539224     4   0.234      0.989 0.000 0.100 0.000 0.900
#&gt; SRR1539222     3   0.234      0.843 0.000 0.000 0.900 0.100
#&gt; SRR1539225     3   0.234      0.843 0.000 0.000 0.900 0.100
#&gt; SRR1539227     4   0.234      0.989 0.000 0.100 0.000 0.900
#&gt; SRR1539226     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539228     3   0.234      0.843 0.000 0.000 0.900 0.100
#&gt; SRR1539229     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539232     3   0.234      0.843 0.000 0.000 0.900 0.100
#&gt; SRR1539230     4   0.234      0.989 0.000 0.100 0.000 0.900
#&gt; SRR1539231     4   0.234      0.989 0.000 0.100 0.000 0.900
#&gt; SRR1539234     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539233     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539235     3   0.276      0.868 0.128 0.000 0.872 0.000
#&gt; SRR1539236     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539237     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539238     3   0.276      0.868 0.128 0.000 0.872 0.000
#&gt; SRR1539239     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539242     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539240     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539241     3   0.276      0.868 0.128 0.000 0.872 0.000
#&gt; SRR1539243     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539244     3   0.276      0.868 0.128 0.000 0.872 0.000
#&gt; SRR1539245     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539246     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539247     3   0.276      0.868 0.128 0.000 0.872 0.000
#&gt; SRR1539248     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539249     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539250     3   0.276      0.868 0.128 0.000 0.872 0.000
#&gt; SRR1539251     3   0.276      0.868 0.128 0.000 0.872 0.000
#&gt; SRR1539253     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539252     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539255     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539254     3   0.276      0.868 0.128 0.000 0.872 0.000
#&gt; SRR1539256     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539257     3   0.276      0.868 0.128 0.000 0.872 0.000
#&gt; SRR1539258     1   0.000      0.997 1.000 0.000 0.000 0.000
#&gt; SRR1539259     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539260     3   0.276      0.868 0.128 0.000 0.872 0.000
#&gt; SRR1539262     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539261     1   0.000      0.997 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3    p4    p5
#&gt; SRR1539207     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539208     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539211     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539210     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539209     4   0.000      0.990 0.000 0.000  0 1.000 0.000
#&gt; SRR1539212     4   0.167      0.918 0.000 0.076  0 0.924 0.000
#&gt; SRR1539214     5   0.425      0.290 0.432 0.000  0 0.000 0.568
#&gt; SRR1539213     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539215     4   0.000      0.990 0.000 0.000  0 1.000 0.000
#&gt; SRR1539216     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539217     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539218     4   0.000      0.990 0.000 0.000  0 1.000 0.000
#&gt; SRR1539220     5   0.342      0.686 0.240 0.000  0 0.000 0.760
#&gt; SRR1539219     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539221     4   0.000      0.990 0.000 0.000  0 1.000 0.000
#&gt; SRR1539223     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539224     4   0.000      0.990 0.000 0.000  0 1.000 0.000
#&gt; SRR1539222     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539225     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539227     4   0.000      0.990 0.000 0.000  0 1.000 0.000
#&gt; SRR1539226     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539228     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539229     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539232     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539230     4   0.000      0.990 0.000 0.000  0 1.000 0.000
#&gt; SRR1539231     4   0.000      0.990 0.000 0.000  0 1.000 0.000
#&gt; SRR1539234     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539233     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539235     5   0.000      0.928 0.000 0.000  0 0.000 1.000
#&gt; SRR1539236     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539237     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539238     5   0.000      0.928 0.000 0.000  0 0.000 1.000
#&gt; SRR1539239     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539242     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539240     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539241     5   0.000      0.928 0.000 0.000  0 0.000 1.000
#&gt; SRR1539243     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539244     5   0.000      0.928 0.000 0.000  0 0.000 1.000
#&gt; SRR1539245     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539246     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539247     5   0.000      0.928 0.000 0.000  0 0.000 1.000
#&gt; SRR1539248     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539249     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539250     5   0.000      0.928 0.000 0.000  0 0.000 1.000
#&gt; SRR1539251     5   0.000      0.928 0.000 0.000  0 0.000 1.000
#&gt; SRR1539253     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539252     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539255     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539254     5   0.000      0.928 0.000 0.000  0 0.000 1.000
#&gt; SRR1539256     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539257     5   0.000      0.928 0.000 0.000  0 0.000 1.000
#&gt; SRR1539258     1   0.000      1.000 1.000 0.000  0 0.000 0.000
#&gt; SRR1539259     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539260     5   0.000      0.928 0.000 0.000  0 0.000 1.000
#&gt; SRR1539262     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539261     1   0.000      1.000 1.000 0.000  0 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2 p3    p4    p5    p6
#&gt; SRR1539207     3  0.0000      1.000 0.000  0  1 0.000 0.000 0.000
#&gt; SRR1539208     1  0.5258      0.723 0.596  0  0 0.252 0.152 0.000
#&gt; SRR1539211     1  0.3765      0.802 0.596  0  0 0.404 0.000 0.000
#&gt; SRR1539210     3  0.0000      1.000 0.000  0  1 0.000 0.000 0.000
#&gt; SRR1539209     6  0.0000      0.998 0.000  0  0 0.000 0.000 1.000
#&gt; SRR1539212     6  0.0000      0.998 0.000  0  0 0.000 0.000 1.000
#&gt; SRR1539214     1  0.2527      0.515 0.832  0  0 0.000 0.168 0.000
#&gt; SRR1539213     3  0.0000      1.000 0.000  0  1 0.000 0.000 0.000
#&gt; SRR1539215     4  0.3765      1.000 0.000  0  0 0.596 0.000 0.404
#&gt; SRR1539216     3  0.0000      1.000 0.000  0  1 0.000 0.000 0.000
#&gt; SRR1539217     1  0.3765      0.802 0.596  0  0 0.404 0.000 0.000
#&gt; SRR1539218     6  0.0146      0.993 0.000  0  0 0.004 0.000 0.996
#&gt; SRR1539220     1  0.3647      0.111 0.640  0  0 0.000 0.360 0.000
#&gt; SRR1539219     3  0.0000      1.000 0.000  0  1 0.000 0.000 0.000
#&gt; SRR1539221     4  0.3765      1.000 0.000  0  0 0.596 0.000 0.404
#&gt; SRR1539223     1  0.3765      0.802 0.596  0  0 0.404 0.000 0.000
#&gt; SRR1539224     6  0.0000      0.998 0.000  0  0 0.000 0.000 1.000
#&gt; SRR1539222     3  0.0000      1.000 0.000  0  1 0.000 0.000 0.000
#&gt; SRR1539225     3  0.0000      1.000 0.000  0  1 0.000 0.000 0.000
#&gt; SRR1539227     4  0.3765      1.000 0.000  0  0 0.596 0.000 0.404
#&gt; SRR1539226     1  0.0000      0.678 1.000  0  0 0.000 0.000 0.000
#&gt; SRR1539228     3  0.0000      1.000 0.000  0  1 0.000 0.000 0.000
#&gt; SRR1539229     1  0.0000      0.678 1.000  0  0 0.000 0.000 0.000
#&gt; SRR1539232     3  0.0000      1.000 0.000  0  1 0.000 0.000 0.000
#&gt; SRR1539230     4  0.3765      1.000 0.000  0  0 0.596 0.000 0.404
#&gt; SRR1539231     4  0.3765      1.000 0.000  0  0 0.596 0.000 0.404
#&gt; SRR1539234     2  0.0000      1.000 0.000  1  0 0.000 0.000 0.000
#&gt; SRR1539233     1  0.0000      0.678 1.000  0  0 0.000 0.000 0.000
#&gt; SRR1539235     5  0.0000      1.000 0.000  0  0 0.000 1.000 0.000
#&gt; SRR1539236     1  0.0146      0.680 0.996  0  0 0.004 0.000 0.000
#&gt; SRR1539237     2  0.0000      1.000 0.000  1  0 0.000 0.000 0.000
#&gt; SRR1539238     5  0.0000      1.000 0.000  0  0 0.000 1.000 0.000
#&gt; SRR1539239     1  0.3765      0.802 0.596  0  0 0.404 0.000 0.000
#&gt; SRR1539242     1  0.3765      0.802 0.596  0  0 0.404 0.000 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000  1  0 0.000 0.000 0.000
#&gt; SRR1539241     5  0.0000      1.000 0.000  0  0 0.000 1.000 0.000
#&gt; SRR1539243     2  0.0000      1.000 0.000  1  0 0.000 0.000 0.000
#&gt; SRR1539244     5  0.0000      1.000 0.000  0  0 0.000 1.000 0.000
#&gt; SRR1539245     1  0.0000      0.678 1.000  0  0 0.000 0.000 0.000
#&gt; SRR1539246     2  0.0000      1.000 0.000  1  0 0.000 0.000 0.000
#&gt; SRR1539247     5  0.0000      1.000 0.000  0  0 0.000 1.000 0.000
#&gt; SRR1539248     1  0.3765      0.802 0.596  0  0 0.404 0.000 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000  1  0 0.000 0.000 0.000
#&gt; SRR1539250     5  0.0000      1.000 0.000  0  0 0.000 1.000 0.000
#&gt; SRR1539251     5  0.0000      1.000 0.000  0  0 0.000 1.000 0.000
#&gt; SRR1539253     2  0.0000      1.000 0.000  1  0 0.000 0.000 0.000
#&gt; SRR1539252     1  0.3765      0.802 0.596  0  0 0.404 0.000 0.000
#&gt; SRR1539255     1  0.3765      0.802 0.596  0  0 0.404 0.000 0.000
#&gt; SRR1539254     5  0.0000      1.000 0.000  0  0 0.000 1.000 0.000
#&gt; SRR1539256     2  0.0000      1.000 0.000  1  0 0.000 0.000 0.000
#&gt; SRR1539257     5  0.0000      1.000 0.000  0  0 0.000 1.000 0.000
#&gt; SRR1539258     1  0.3765      0.802 0.596  0  0 0.404 0.000 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000  1  0 0.000 0.000 0.000
#&gt; SRR1539260     5  0.0000      1.000 0.000  0  0 0.000 1.000 0.000
#&gt; SRR1539262     2  0.0000      1.000 0.000  1  0 0.000 0.000 0.000
#&gt; SRR1539261     1  0.3765      0.802 0.596  0  0 0.404 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:mclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000          0.457 0.544   0.544
#> 3 3 0.779           0.922       0.878          0.248 0.849   0.723
#> 4 4 0.936           0.935       0.965          0.182 0.919   0.798
#> 5 5 0.825           0.807       0.885          0.102 0.958   0.872
#> 6 6 0.934           0.917       0.960          0.101 0.884   0.598
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 4
```

There is also optional best $k$ = 2 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1539207     3  0.6204      0.986 0.424 0.000 0.576
#&gt; SRR1539208     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539211     1  0.1411      0.932 0.964 0.000 0.036
#&gt; SRR1539210     3  0.6126      0.958 0.400 0.000 0.600
#&gt; SRR1539209     2  0.0000      0.859 0.000 1.000 0.000
#&gt; SRR1539212     2  0.0592      0.854 0.000 0.988 0.012
#&gt; SRR1539214     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539213     3  0.6215      0.986 0.428 0.000 0.572
#&gt; SRR1539215     2  0.0000      0.859 0.000 1.000 0.000
#&gt; SRR1539216     3  0.6204      0.986 0.424 0.000 0.576
#&gt; SRR1539217     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539218     2  0.0000      0.859 0.000 1.000 0.000
#&gt; SRR1539220     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539219     3  0.6215      0.986 0.428 0.000 0.572
#&gt; SRR1539221     2  0.0000      0.859 0.000 1.000 0.000
#&gt; SRR1539223     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539224     2  0.0000      0.859 0.000 1.000 0.000
#&gt; SRR1539222     3  0.6168      0.974 0.412 0.000 0.588
#&gt; SRR1539225     3  0.6215      0.986 0.428 0.000 0.572
#&gt; SRR1539227     2  0.0000      0.859 0.000 1.000 0.000
#&gt; SRR1539226     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539228     3  0.6225      0.981 0.432 0.000 0.568
#&gt; SRR1539229     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539232     1  0.4555      0.474 0.800 0.000 0.200
#&gt; SRR1539230     2  0.0000      0.859 0.000 1.000 0.000
#&gt; SRR1539231     2  0.0000      0.859 0.000 1.000 0.000
#&gt; SRR1539234     2  0.0000      0.859 0.000 1.000 0.000
#&gt; SRR1539233     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539235     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539236     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539237     2  0.6126      0.795 0.000 0.600 0.400
#&gt; SRR1539238     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539239     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539242     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539240     2  0.6126      0.795 0.000 0.600 0.400
#&gt; SRR1539241     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539243     2  0.6126      0.795 0.000 0.600 0.400
#&gt; SRR1539244     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539245     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539246     2  0.6126      0.795 0.000 0.600 0.400
#&gt; SRR1539247     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539248     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539249     2  0.6126      0.795 0.000 0.600 0.400
#&gt; SRR1539250     1  0.0237      0.978 0.996 0.000 0.004
#&gt; SRR1539251     1  0.0424      0.973 0.992 0.000 0.008
#&gt; SRR1539253     2  0.6126      0.795 0.000 0.600 0.400
#&gt; SRR1539252     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539255     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539254     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539256     2  0.6126      0.795 0.000 0.600 0.400
#&gt; SRR1539257     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539258     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539259     2  0.4291      0.842 0.000 0.820 0.180
#&gt; SRR1539260     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539262     2  0.4291      0.842 0.000 0.820 0.180
#&gt; SRR1539261     1  0.0747      0.961 0.984 0.000 0.016
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3  0.2216      0.848 0.092 0.000 0.908 0.000
#&gt; SRR1539208     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539211     1  0.3486      0.852 0.864 0.000 0.092 0.044
#&gt; SRR1539210     3  0.1022      0.888 0.000 0.000 0.968 0.032
#&gt; SRR1539209     4  0.3123      0.856 0.000 0.156 0.000 0.844
#&gt; SRR1539212     2  0.4643      0.440 0.000 0.656 0.000 0.344
#&gt; SRR1539214     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539213     3  0.0000      0.902 0.000 0.000 1.000 0.000
#&gt; SRR1539215     4  0.1302      0.982 0.000 0.044 0.000 0.956
#&gt; SRR1539216     3  0.0000      0.902 0.000 0.000 1.000 0.000
#&gt; SRR1539217     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539218     4  0.1302      0.982 0.000 0.044 0.000 0.956
#&gt; SRR1539220     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539219     3  0.0000      0.902 0.000 0.000 1.000 0.000
#&gt; SRR1539221     4  0.1302      0.982 0.000 0.044 0.000 0.956
#&gt; SRR1539223     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539224     4  0.1302      0.982 0.000 0.044 0.000 0.956
#&gt; SRR1539222     3  0.0000      0.902 0.000 0.000 1.000 0.000
#&gt; SRR1539225     3  0.0000      0.902 0.000 0.000 1.000 0.000
#&gt; SRR1539227     4  0.1302      0.982 0.000 0.044 0.000 0.956
#&gt; SRR1539226     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539228     3  0.2281      0.845 0.096 0.000 0.904 0.000
#&gt; SRR1539229     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539232     3  0.4585      0.547 0.332 0.000 0.668 0.000
#&gt; SRR1539230     4  0.1302      0.982 0.000 0.044 0.000 0.956
#&gt; SRR1539231     4  0.1302      0.982 0.000 0.044 0.000 0.956
#&gt; SRR1539234     2  0.2011      0.882 0.000 0.920 0.000 0.080
#&gt; SRR1539233     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539235     1  0.0592      0.976 0.984 0.000 0.016 0.000
#&gt; SRR1539236     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539237     2  0.0000      0.954 0.000 1.000 0.000 0.000
#&gt; SRR1539238     1  0.0592      0.976 0.984 0.000 0.016 0.000
#&gt; SRR1539239     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539242     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539240     2  0.0000      0.954 0.000 1.000 0.000 0.000
#&gt; SRR1539241     1  0.0592      0.976 0.984 0.000 0.016 0.000
#&gt; SRR1539243     2  0.0000      0.954 0.000 1.000 0.000 0.000
#&gt; SRR1539244     1  0.1557      0.946 0.944 0.000 0.056 0.000
#&gt; SRR1539245     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539246     2  0.0000      0.954 0.000 1.000 0.000 0.000
#&gt; SRR1539247     1  0.0592      0.976 0.984 0.000 0.016 0.000
#&gt; SRR1539248     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539249     2  0.0000      0.954 0.000 1.000 0.000 0.000
#&gt; SRR1539250     1  0.1022      0.965 0.968 0.000 0.032 0.000
#&gt; SRR1539251     1  0.1022      0.965 0.968 0.000 0.032 0.000
#&gt; SRR1539253     2  0.0000      0.954 0.000 1.000 0.000 0.000
#&gt; SRR1539252     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539254     1  0.0592      0.976 0.984 0.000 0.016 0.000
#&gt; SRR1539256     2  0.0000      0.954 0.000 1.000 0.000 0.000
#&gt; SRR1539257     1  0.0592      0.976 0.984 0.000 0.016 0.000
#&gt; SRR1539258     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1539259     2  0.0000      0.954 0.000 1.000 0.000 0.000
#&gt; SRR1539260     1  0.0592      0.976 0.984 0.000 0.016 0.000
#&gt; SRR1539262     2  0.0000      0.954 0.000 1.000 0.000 0.000
#&gt; SRR1539261     1  0.3486      0.852 0.864 0.000 0.092 0.044
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.0162      0.912 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1539208     1  0.0000      0.759 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539211     5  0.4210      0.588 0.412 0.000 0.000 0.000 0.588
#&gt; SRR1539210     3  0.1965      0.842 0.000 0.000 0.904 0.000 0.096
#&gt; SRR1539209     4  0.6059      0.783 0.000 0.120 0.000 0.468 0.412
#&gt; SRR1539212     5  0.5143     -0.285 0.000 0.368 0.000 0.048 0.584
#&gt; SRR1539214     1  0.2929      0.755 0.820 0.000 0.000 0.180 0.000
#&gt; SRR1539213     3  0.0000      0.914 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539215     4  0.4126      0.970 0.000 0.000 0.000 0.620 0.380
#&gt; SRR1539216     3  0.0000      0.914 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539217     1  0.0290      0.760 0.992 0.000 0.000 0.008 0.000
#&gt; SRR1539218     4  0.4150      0.967 0.000 0.000 0.000 0.612 0.388
#&gt; SRR1539220     1  0.3003      0.754 0.812 0.000 0.000 0.188 0.000
#&gt; SRR1539219     3  0.0000      0.914 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539221     4  0.4126      0.970 0.000 0.000 0.000 0.620 0.380
#&gt; SRR1539223     1  0.2929      0.755 0.820 0.000 0.000 0.180 0.000
#&gt; SRR1539224     4  0.4150      0.967 0.000 0.000 0.000 0.612 0.388
#&gt; SRR1539222     3  0.0000      0.914 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539225     3  0.0000      0.914 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539227     4  0.4126      0.970 0.000 0.000 0.000 0.620 0.380
#&gt; SRR1539226     1  0.0000      0.759 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539228     3  0.0510      0.906 0.000 0.000 0.984 0.000 0.016
#&gt; SRR1539229     1  0.0000      0.759 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539232     3  0.6026      0.299 0.228 0.000 0.580 0.192 0.000
#&gt; SRR1539230     4  0.4126      0.970 0.000 0.000 0.000 0.620 0.380
#&gt; SRR1539231     4  0.4126      0.970 0.000 0.000 0.000 0.620 0.380
#&gt; SRR1539234     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539233     1  0.0000      0.759 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539235     1  0.4126      0.714 0.620 0.000 0.000 0.380 0.000
#&gt; SRR1539236     1  0.0000      0.759 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539237     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539238     1  0.4126      0.714 0.620 0.000 0.000 0.380 0.000
#&gt; SRR1539239     1  0.0162      0.756 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1539242     1  0.0000      0.759 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539240     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539241     1  0.4126      0.714 0.620 0.000 0.000 0.380 0.000
#&gt; SRR1539243     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539244     1  0.4757      0.693 0.596 0.000 0.000 0.380 0.024
#&gt; SRR1539245     1  0.0000      0.759 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539246     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539247     1  0.4126      0.714 0.620 0.000 0.000 0.380 0.000
#&gt; SRR1539248     1  0.0404      0.747 0.988 0.000 0.000 0.000 0.012
#&gt; SRR1539249     2  0.0290      0.992 0.000 0.992 0.000 0.000 0.008
#&gt; SRR1539250     1  0.4126      0.714 0.620 0.000 0.000 0.380 0.000
#&gt; SRR1539251     1  0.4126      0.714 0.620 0.000 0.000 0.380 0.000
#&gt; SRR1539253     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539252     1  0.0000      0.759 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0000      0.759 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539254     1  0.4126      0.714 0.620 0.000 0.000 0.380 0.000
#&gt; SRR1539256     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539257     1  0.4126      0.714 0.620 0.000 0.000 0.380 0.000
#&gt; SRR1539258     1  0.0000      0.759 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539259     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539260     1  0.4126      0.714 0.620 0.000 0.000 0.380 0.000
#&gt; SRR1539262     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539261     5  0.4210      0.588 0.412 0.000 0.000 0.000 0.588
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.0000      0.967 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539208     1  0.0146      0.938 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1539211     6  0.2416      1.000 0.156 0.000 0.000 0.000 0.000 0.844
#&gt; SRR1539210     3  0.0632      0.950 0.000 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1539209     4  0.2655      0.868 0.000 0.008 0.000 0.848 0.004 0.140
#&gt; SRR1539212     4  0.4456      0.725 0.000 0.132 0.000 0.724 0.004 0.140
#&gt; SRR1539214     5  0.2883      0.718 0.212 0.000 0.000 0.000 0.788 0.000
#&gt; SRR1539213     3  0.0000      0.967 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539215     4  0.0000      0.940 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539216     3  0.0000      0.967 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539217     1  0.3797      0.164 0.580 0.000 0.000 0.000 0.420 0.000
#&gt; SRR1539218     4  0.1075      0.929 0.000 0.000 0.000 0.952 0.000 0.048
#&gt; SRR1539220     5  0.2823      0.729 0.204 0.000 0.000 0.000 0.796 0.000
#&gt; SRR1539219     3  0.0000      0.967 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539221     4  0.0000      0.940 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539223     5  0.2854      0.724 0.208 0.000 0.000 0.000 0.792 0.000
#&gt; SRR1539224     4  0.1075      0.929 0.000 0.000 0.000 0.952 0.000 0.048
#&gt; SRR1539222     3  0.0000      0.967 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539225     3  0.0000      0.967 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539227     4  0.0000      0.940 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539226     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539228     3  0.0000      0.967 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539229     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539232     3  0.2697      0.728 0.000 0.000 0.812 0.000 0.188 0.000
#&gt; SRR1539230     4  0.0000      0.940 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539231     4  0.0000      0.940 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539234     2  0.0935      0.961 0.000 0.964 0.000 0.004 0.000 0.032
#&gt; SRR1539233     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539235     5  0.0146      0.923 0.004 0.000 0.000 0.000 0.996 0.000
#&gt; SRR1539236     1  0.0146      0.938 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; SRR1539237     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539238     5  0.0363      0.923 0.012 0.000 0.000 0.000 0.988 0.000
#&gt; SRR1539239     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539242     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539240     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539241     5  0.0146      0.923 0.004 0.000 0.000 0.000 0.996 0.000
#&gt; SRR1539243     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539244     5  0.0363      0.923 0.012 0.000 0.000 0.000 0.988 0.000
#&gt; SRR1539245     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539246     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539247     5  0.0146      0.923 0.004 0.000 0.000 0.000 0.996 0.000
#&gt; SRR1539248     1  0.0146      0.938 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1539249     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539250     5  0.0291      0.922 0.004 0.000 0.004 0.000 0.992 0.000
#&gt; SRR1539251     5  0.0291      0.922 0.004 0.000 0.004 0.000 0.992 0.000
#&gt; SRR1539253     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539252     1  0.0260      0.933 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; SRR1539255     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539254     5  0.0363      0.923 0.012 0.000 0.000 0.000 0.988 0.000
#&gt; SRR1539256     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.0146      0.923 0.004 0.000 0.000 0.000 0.996 0.000
#&gt; SRR1539258     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539259     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539260     5  0.0363      0.923 0.012 0.000 0.000 0.000 0.988 0.000
#&gt; SRR1539262     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539261     6  0.2416      1.000 0.156 0.000 0.000 0.000 0.000 0.844
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.830           0.923       0.951         0.4239 0.805   0.642
#> 4 4 0.869           0.846       0.881         0.0838 0.962   0.891
#> 5 5 0.915           0.929       0.949         0.0673 0.942   0.813
#> 6 6 0.967           0.940       0.962         0.0688 0.899   0.636
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 5
```

There is also optional best $k$ = 2 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1539207     3  0.0000      0.965 0.000 0.000 1.000
#&gt; SRR1539208     1  0.1529      0.904 0.960 0.000 0.040
#&gt; SRR1539211     1  0.0000      0.893 1.000 0.000 0.000
#&gt; SRR1539210     3  0.0000      0.965 0.000 0.000 1.000
#&gt; SRR1539209     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539212     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539214     1  0.3619      0.898 0.864 0.000 0.136
#&gt; SRR1539213     3  0.0000      0.965 0.000 0.000 1.000
#&gt; SRR1539215     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539216     3  0.0000      0.965 0.000 0.000 1.000
#&gt; SRR1539217     1  0.3551      0.899 0.868 0.000 0.132
#&gt; SRR1539218     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539220     1  0.3879      0.891 0.848 0.000 0.152
#&gt; SRR1539219     3  0.0000      0.965 0.000 0.000 1.000
#&gt; SRR1539221     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539223     1  0.3619      0.898 0.864 0.000 0.136
#&gt; SRR1539224     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539222     3  0.0000      0.965 0.000 0.000 1.000
#&gt; SRR1539225     3  0.0000      0.965 0.000 0.000 1.000
#&gt; SRR1539227     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539226     1  0.3267      0.902 0.884 0.000 0.116
#&gt; SRR1539228     3  0.0000      0.965 0.000 0.000 1.000
#&gt; SRR1539229     1  0.1964      0.905 0.944 0.000 0.056
#&gt; SRR1539232     3  0.0000      0.965 0.000 0.000 1.000
#&gt; SRR1539230     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539231     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539233     1  0.0592      0.898 0.988 0.000 0.012
#&gt; SRR1539235     1  0.3941      0.888 0.844 0.000 0.156
#&gt; SRR1539236     1  0.0747      0.900 0.984 0.000 0.016
#&gt; SRR1539237     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539238     1  0.4002      0.886 0.840 0.000 0.160
#&gt; SRR1539239     1  0.0000      0.893 1.000 0.000 0.000
#&gt; SRR1539242     1  0.0000      0.893 1.000 0.000 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539241     1  0.3879      0.891 0.848 0.000 0.152
#&gt; SRR1539243     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539244     1  0.4842      0.821 0.776 0.000 0.224
#&gt; SRR1539245     1  0.1031      0.902 0.976 0.000 0.024
#&gt; SRR1539246     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539247     3  0.5678      0.408 0.316 0.000 0.684
#&gt; SRR1539248     1  0.0000      0.893 1.000 0.000 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539250     3  0.0000      0.965 0.000 0.000 1.000
#&gt; SRR1539251     3  0.0000      0.965 0.000 0.000 1.000
#&gt; SRR1539253     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539252     1  0.3116      0.903 0.892 0.000 0.108
#&gt; SRR1539255     1  0.0892      0.901 0.980 0.000 0.020
#&gt; SRR1539254     1  0.4002      0.885 0.840 0.000 0.160
#&gt; SRR1539256     2  0.0424      0.993 0.008 0.992 0.000
#&gt; SRR1539257     1  0.6291      0.310 0.532 0.000 0.468
#&gt; SRR1539258     1  0.0592      0.898 0.988 0.000 0.012
#&gt; SRR1539259     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539260     1  0.4504      0.854 0.804 0.000 0.196
#&gt; SRR1539262     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1539261     1  0.0000      0.893 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3  0.0000     0.9881 0.000 0.000 1.000 0.000
#&gt; SRR1539208     1  0.0188     0.9243 0.996 0.000 0.000 0.004
#&gt; SRR1539211     1  0.0804     0.9161 0.980 0.000 0.012 0.008
#&gt; SRR1539210     3  0.0000     0.9881 0.000 0.000 1.000 0.000
#&gt; SRR1539209     2  0.4948     0.7533 0.000 0.560 0.000 0.440
#&gt; SRR1539212     2  0.4948     0.7533 0.000 0.560 0.000 0.440
#&gt; SRR1539214     1  0.2760     0.7958 0.872 0.000 0.000 0.128
#&gt; SRR1539213     3  0.0000     0.9881 0.000 0.000 1.000 0.000
#&gt; SRR1539215     2  0.4948     0.7533 0.000 0.560 0.000 0.440
#&gt; SRR1539216     3  0.0000     0.9881 0.000 0.000 1.000 0.000
#&gt; SRR1539217     1  0.0336     0.9237 0.992 0.000 0.008 0.000
#&gt; SRR1539218     2  0.4948     0.7533 0.000 0.560 0.000 0.440
#&gt; SRR1539220     1  0.1305     0.9110 0.960 0.000 0.004 0.036
#&gt; SRR1539219     3  0.0000     0.9881 0.000 0.000 1.000 0.000
#&gt; SRR1539221     2  0.4948     0.7533 0.000 0.560 0.000 0.440
#&gt; SRR1539223     1  0.0469     0.9213 0.988 0.000 0.012 0.000
#&gt; SRR1539224     2  0.4948     0.7533 0.000 0.560 0.000 0.440
#&gt; SRR1539222     3  0.0000     0.9881 0.000 0.000 1.000 0.000
#&gt; SRR1539225     3  0.0000     0.9881 0.000 0.000 1.000 0.000
#&gt; SRR1539227     2  0.4948     0.7533 0.000 0.560 0.000 0.440
#&gt; SRR1539226     1  0.0707     0.9213 0.980 0.000 0.000 0.020
#&gt; SRR1539228     3  0.0000     0.9881 0.000 0.000 1.000 0.000
#&gt; SRR1539229     1  0.0921     0.9181 0.972 0.000 0.000 0.028
#&gt; SRR1539232     3  0.1109     0.9617 0.004 0.000 0.968 0.028
#&gt; SRR1539230     2  0.4948     0.7533 0.000 0.560 0.000 0.440
#&gt; SRR1539231     2  0.4948     0.7533 0.000 0.560 0.000 0.440
#&gt; SRR1539234     2  0.0188     0.7811 0.000 0.996 0.000 0.004
#&gt; SRR1539233     1  0.0817     0.9197 0.976 0.000 0.000 0.024
#&gt; SRR1539235     1  0.4837     0.0577 0.648 0.000 0.004 0.348
#&gt; SRR1539236     1  0.0188     0.9247 0.996 0.000 0.000 0.004
#&gt; SRR1539237     2  0.0000     0.7810 0.000 1.000 0.000 0.000
#&gt; SRR1539238     1  0.3450     0.7362 0.836 0.000 0.008 0.156
#&gt; SRR1539239     1  0.0188     0.9243 0.996 0.000 0.000 0.004
#&gt; SRR1539242     1  0.0188     0.9243 0.996 0.000 0.000 0.004
#&gt; SRR1539240     2  0.0000     0.7810 0.000 1.000 0.000 0.000
#&gt; SRR1539241     1  0.2773     0.8159 0.880 0.000 0.004 0.116
#&gt; SRR1539243     2  0.0188     0.7788 0.000 0.996 0.000 0.004
#&gt; SRR1539244     4  0.5815     0.6801 0.428 0.000 0.032 0.540
#&gt; SRR1539245     1  0.0817     0.9203 0.976 0.000 0.000 0.024
#&gt; SRR1539246     2  0.0000     0.7810 0.000 1.000 0.000 0.000
#&gt; SRR1539247     4  0.7390     0.7219 0.252 0.000 0.228 0.520
#&gt; SRR1539248     1  0.0188     0.9243 0.996 0.000 0.000 0.004
#&gt; SRR1539249     2  0.0000     0.7810 0.000 1.000 0.000 0.000
#&gt; SRR1539250     3  0.1042     0.9629 0.020 0.000 0.972 0.008
#&gt; SRR1539251     3  0.1042     0.9629 0.020 0.000 0.972 0.008
#&gt; SRR1539253     2  0.0000     0.7810 0.000 1.000 0.000 0.000
#&gt; SRR1539252     1  0.0000     0.9247 1.000 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0000     0.9247 1.000 0.000 0.000 0.000
#&gt; SRR1539254     1  0.1584     0.9039 0.952 0.000 0.012 0.036
#&gt; SRR1539256     2  0.0000     0.7810 0.000 1.000 0.000 0.000
#&gt; SRR1539257     4  0.6887     0.8008 0.356 0.000 0.116 0.528
#&gt; SRR1539258     1  0.0188     0.9243 0.996 0.000 0.000 0.004
#&gt; SRR1539259     2  0.0000     0.7810 0.000 1.000 0.000 0.000
#&gt; SRR1539260     1  0.2759     0.8474 0.904 0.000 0.044 0.052
#&gt; SRR1539262     2  0.0188     0.7788 0.000 0.996 0.000 0.004
#&gt; SRR1539261     1  0.0592     0.9165 0.984 0.000 0.000 0.016
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.0000      0.971 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539208     1  0.1478      0.904 0.936 0.000 0.000 0.000 0.064
#&gt; SRR1539211     1  0.0609      0.923 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1539210     3  0.0671      0.955 0.004 0.000 0.980 0.000 0.016
#&gt; SRR1539209     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539212     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539214     1  0.2127      0.879 0.892 0.000 0.000 0.000 0.108
#&gt; SRR1539213     3  0.0000      0.971 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539215     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539216     3  0.0000      0.971 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539217     1  0.0000      0.927 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539218     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539220     1  0.0898      0.926 0.972 0.000 0.008 0.000 0.020
#&gt; SRR1539219     3  0.0000      0.971 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539221     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539223     1  0.0404      0.924 0.988 0.000 0.000 0.000 0.012
#&gt; SRR1539224     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539222     3  0.0000      0.971 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539225     3  0.0000      0.971 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539227     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539226     1  0.0671      0.927 0.980 0.004 0.000 0.000 0.016
#&gt; SRR1539228     3  0.0000      0.971 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539229     1  0.0963      0.923 0.964 0.000 0.000 0.000 0.036
#&gt; SRR1539232     3  0.0451      0.964 0.000 0.004 0.988 0.000 0.008
#&gt; SRR1539230     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539231     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539234     2  0.1270      0.999 0.000 0.948 0.000 0.052 0.000
#&gt; SRR1539233     1  0.1270      0.917 0.948 0.000 0.000 0.000 0.052
#&gt; SRR1539235     1  0.4350      0.421 0.588 0.000 0.004 0.000 0.408
#&gt; SRR1539236     1  0.0609      0.926 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1539237     2  0.1270      0.999 0.000 0.948 0.000 0.052 0.000
#&gt; SRR1539238     1  0.3582      0.761 0.768 0.000 0.008 0.000 0.224
#&gt; SRR1539239     1  0.0162      0.927 0.996 0.004 0.000 0.000 0.000
#&gt; SRR1539242     1  0.0162      0.927 0.996 0.004 0.000 0.000 0.000
#&gt; SRR1539240     2  0.1270      0.999 0.000 0.948 0.000 0.052 0.000
#&gt; SRR1539241     1  0.3039      0.807 0.808 0.000 0.000 0.000 0.192
#&gt; SRR1539243     2  0.1197      0.996 0.000 0.952 0.000 0.048 0.000
#&gt; SRR1539244     5  0.1018      0.748 0.016 0.000 0.016 0.000 0.968
#&gt; SRR1539245     1  0.1285      0.922 0.956 0.004 0.004 0.000 0.036
#&gt; SRR1539246     2  0.1270      0.999 0.000 0.948 0.000 0.052 0.000
#&gt; SRR1539247     5  0.4552      0.760 0.040 0.000 0.264 0.000 0.696
#&gt; SRR1539248     1  0.0000      0.927 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539249     2  0.1270      0.999 0.000 0.948 0.000 0.052 0.000
#&gt; SRR1539250     3  0.2069      0.877 0.012 0.000 0.912 0.000 0.076
#&gt; SRR1539251     3  0.2069      0.877 0.012 0.000 0.912 0.000 0.076
#&gt; SRR1539253     2  0.1270      0.999 0.000 0.948 0.000 0.052 0.000
#&gt; SRR1539252     1  0.0162      0.927 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1539255     1  0.0162      0.927 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1539254     1  0.2674      0.849 0.856 0.000 0.004 0.000 0.140
#&gt; SRR1539256     2  0.1197      0.996 0.000 0.952 0.000 0.048 0.000
#&gt; SRR1539257     5  0.4627      0.811 0.080 0.000 0.188 0.000 0.732
#&gt; SRR1539258     1  0.0162      0.927 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1539259     2  0.1270      0.999 0.000 0.948 0.000 0.052 0.000
#&gt; SRR1539260     1  0.3714      0.806 0.812 0.000 0.056 0.000 0.132
#&gt; SRR1539262     2  0.1270      0.999 0.000 0.948 0.000 0.052 0.000
#&gt; SRR1539261     1  0.0404      0.924 0.988 0.000 0.000 0.000 0.012
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3    p4    p5 p6
#&gt; SRR1539207     3  0.0146      0.981 0.000  0 0.996 0.000 0.000 NA
#&gt; SRR1539208     5  0.4561      0.105 0.428  0 0.000 0.000 0.536 NA
#&gt; SRR1539211     1  0.3014      0.860 0.832  0 0.000 0.000 0.132 NA
#&gt; SRR1539210     3  0.1285      0.942 0.000  0 0.944 0.000 0.052 NA
#&gt; SRR1539209     4  0.0146      0.998 0.000  0 0.000 0.996 0.000 NA
#&gt; SRR1539212     4  0.0146      0.998 0.000  0 0.000 0.996 0.000 NA
#&gt; SRR1539214     1  0.1719      0.923 0.924  0 0.016 0.000 0.000 NA
#&gt; SRR1539213     3  0.0458      0.980 0.000  0 0.984 0.000 0.000 NA
#&gt; SRR1539215     4  0.0146      0.998 0.000  0 0.000 0.996 0.000 NA
#&gt; SRR1539216     3  0.0146      0.981 0.000  0 0.996 0.000 0.000 NA
#&gt; SRR1539217     1  0.0146      0.950 0.996  0 0.004 0.000 0.000 NA
#&gt; SRR1539218     4  0.0000      0.999 0.000  0 0.000 1.000 0.000 NA
#&gt; SRR1539220     1  0.2000      0.918 0.916  0 0.048 0.000 0.004 NA
#&gt; SRR1539219     3  0.0000      0.982 0.000  0 1.000 0.000 0.000 NA
#&gt; SRR1539221     4  0.0000      0.999 0.000  0 0.000 1.000 0.000 NA
#&gt; SRR1539223     1  0.2277      0.916 0.892  0 0.000 0.000 0.076 NA
#&gt; SRR1539224     4  0.0000      0.999 0.000  0 0.000 1.000 0.000 NA
#&gt; SRR1539222     3  0.0291      0.980 0.000  0 0.992 0.000 0.004 NA
#&gt; SRR1539225     3  0.0458      0.980 0.000  0 0.984 0.000 0.000 NA
#&gt; SRR1539227     4  0.0000      0.999 0.000  0 0.000 1.000 0.000 NA
#&gt; SRR1539226     1  0.0717      0.945 0.976  0 0.008 0.000 0.000 NA
#&gt; SRR1539228     3  0.0458      0.980 0.000  0 0.984 0.000 0.000 NA
#&gt; SRR1539229     1  0.0260      0.950 0.992  0 0.000 0.000 0.000 NA
#&gt; SRR1539232     3  0.1141      0.965 0.000  0 0.948 0.000 0.000 NA
#&gt; SRR1539230     4  0.0000      0.999 0.000  0 0.000 1.000 0.000 NA
#&gt; SRR1539231     4  0.0000      0.999 0.000  0 0.000 1.000 0.000 NA
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 NA
#&gt; SRR1539233     1  0.0260      0.951 0.992  0 0.000 0.000 0.008 NA
#&gt; SRR1539235     5  0.1349      0.895 0.004  0 0.000 0.000 0.940 NA
#&gt; SRR1539236     1  0.0363      0.951 0.988  0 0.000 0.000 0.012 NA
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 NA
#&gt; SRR1539238     5  0.0551      0.903 0.004  0 0.004 0.000 0.984 NA
#&gt; SRR1539239     1  0.1391      0.939 0.944  0 0.000 0.000 0.040 NA
#&gt; SRR1539242     1  0.1829      0.929 0.920  0 0.000 0.000 0.056 NA
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 NA
#&gt; SRR1539241     5  0.0291      0.903 0.004  0 0.004 0.000 0.992 NA
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 NA
#&gt; SRR1539244     5  0.2442      0.854 0.004  0 0.000 0.000 0.852 NA
#&gt; SRR1539245     1  0.0713      0.945 0.972  0 0.000 0.000 0.000 NA
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 NA
#&gt; SRR1539247     5  0.1788      0.888 0.004  0 0.004 0.000 0.916 NA
#&gt; SRR1539248     1  0.2046      0.923 0.908  0 0.000 0.000 0.060 NA
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 NA
#&gt; SRR1539250     5  0.1492      0.888 0.000  0 0.036 0.000 0.940 NA
#&gt; SRR1539251     5  0.1572      0.886 0.000  0 0.036 0.000 0.936 NA
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 NA
#&gt; SRR1539252     1  0.0146      0.950 0.996  0 0.000 0.000 0.000 NA
#&gt; SRR1539255     1  0.0000      0.950 1.000  0 0.000 0.000 0.000 NA
#&gt; SRR1539254     5  0.1003      0.898 0.004  0 0.004 0.000 0.964 NA
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 NA
#&gt; SRR1539257     5  0.1843      0.887 0.004  0 0.004 0.000 0.912 NA
#&gt; SRR1539258     1  0.0146      0.950 0.996  0 0.000 0.000 0.000 NA
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 NA
#&gt; SRR1539260     5  0.0653      0.902 0.004  0 0.004 0.000 0.980 NA
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 NA
#&gt; SRR1539261     1  0.2509      0.903 0.876  0 0.000 0.000 0.088 NA
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.624           0.681       0.843         0.3339 0.914   0.842
#> 4 4 0.706           0.775       0.823         0.1393 0.896   0.779
#> 5 5 0.771           0.765       0.802         0.0882 0.856   0.621
#> 6 6 0.835           0.788       0.836         0.0438 0.987   0.945
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1  p2    p3
#&gt; SRR1539207     1  0.6192     -0.216 0.580 0.0 0.420
#&gt; SRR1539208     1  0.0237      0.691 0.996 0.0 0.004
#&gt; SRR1539211     1  0.0237      0.691 0.996 0.0 0.004
#&gt; SRR1539210     1  0.6192     -0.216 0.580 0.0 0.420
#&gt; SRR1539209     2  0.4555      0.903 0.000 0.8 0.200
#&gt; SRR1539212     2  0.4555      0.903 0.000 0.8 0.200
#&gt; SRR1539214     1  0.4291      0.683 0.820 0.0 0.180
#&gt; SRR1539213     3  0.4605      1.000 0.204 0.0 0.796
#&gt; SRR1539215     2  0.4555      0.903 0.000 0.8 0.200
#&gt; SRR1539216     1  0.6192     -0.216 0.580 0.0 0.420
#&gt; SRR1539217     1  0.0000      0.692 1.000 0.0 0.000
#&gt; SRR1539218     2  0.4555      0.903 0.000 0.8 0.200
#&gt; SRR1539220     1  0.4291      0.683 0.820 0.0 0.180
#&gt; SRR1539219     1  0.6192     -0.228 0.580 0.0 0.420
#&gt; SRR1539221     2  0.4555      0.903 0.000 0.8 0.200
#&gt; SRR1539223     1  0.0000      0.692 1.000 0.0 0.000
#&gt; SRR1539224     2  0.4555      0.903 0.000 0.8 0.200
#&gt; SRR1539222     1  0.6192     -0.216 0.580 0.0 0.420
#&gt; SRR1539225     3  0.4605      1.000 0.204 0.0 0.796
#&gt; SRR1539227     2  0.4555      0.903 0.000 0.8 0.200
#&gt; SRR1539226     1  0.4504      0.672 0.804 0.0 0.196
#&gt; SRR1539228     3  0.4605      1.000 0.204 0.0 0.796
#&gt; SRR1539229     1  0.4555      0.669 0.800 0.0 0.200
#&gt; SRR1539232     3  0.4605      1.000 0.204 0.0 0.796
#&gt; SRR1539230     2  0.4555      0.903 0.000 0.8 0.200
#&gt; SRR1539231     2  0.4555      0.903 0.000 0.8 0.200
#&gt; SRR1539234     2  0.0000      0.914 0.000 1.0 0.000
#&gt; SRR1539233     1  0.4555      0.669 0.800 0.0 0.200
#&gt; SRR1539235     1  0.6045      0.391 0.620 0.0 0.380
#&gt; SRR1539236     1  0.6045      0.391 0.620 0.0 0.380
#&gt; SRR1539237     2  0.0000      0.914 0.000 1.0 0.000
#&gt; SRR1539238     1  0.3619      0.710 0.864 0.0 0.136
#&gt; SRR1539239     1  0.0237      0.691 0.996 0.0 0.004
#&gt; SRR1539242     1  0.0237      0.691 0.996 0.0 0.004
#&gt; SRR1539240     2  0.0000      0.914 0.000 1.0 0.000
#&gt; SRR1539241     1  0.3619      0.710 0.864 0.0 0.136
#&gt; SRR1539243     2  0.0000      0.914 0.000 1.0 0.000
#&gt; SRR1539244     1  0.6045      0.391 0.620 0.0 0.380
#&gt; SRR1539245     1  0.6045      0.391 0.620 0.0 0.380
#&gt; SRR1539246     2  0.0000      0.914 0.000 1.0 0.000
#&gt; SRR1539247     1  0.3619      0.710 0.864 0.0 0.136
#&gt; SRR1539248     1  0.0237      0.691 0.996 0.0 0.004
#&gt; SRR1539249     2  0.0000      0.914 0.000 1.0 0.000
#&gt; SRR1539250     1  0.2711      0.712 0.912 0.0 0.088
#&gt; SRR1539251     1  0.2711      0.712 0.912 0.0 0.088
#&gt; SRR1539253     2  0.0000      0.914 0.000 1.0 0.000
#&gt; SRR1539252     1  0.3816      0.701 0.852 0.0 0.148
#&gt; SRR1539255     1  0.6045      0.391 0.620 0.0 0.380
#&gt; SRR1539254     1  0.3619      0.710 0.864 0.0 0.136
#&gt; SRR1539256     2  0.0000      0.914 0.000 1.0 0.000
#&gt; SRR1539257     1  0.3619      0.710 0.864 0.0 0.136
#&gt; SRR1539258     1  0.0000      0.692 1.000 0.0 0.000
#&gt; SRR1539259     2  0.0000      0.914 0.000 1.0 0.000
#&gt; SRR1539260     1  0.3619      0.710 0.864 0.0 0.136
#&gt; SRR1539262     2  0.0000      0.914 0.000 1.0 0.000
#&gt; SRR1539261     1  0.0237      0.691 0.996 0.0 0.004
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1   p2    p3 p4
#&gt; SRR1539207     3  0.0000      0.821 0.000 0.00 1.000 NA
#&gt; SRR1539208     1  0.3688      0.775 0.792 0.00 0.000 NA
#&gt; SRR1539211     1  0.3688      0.775 0.792 0.00 0.000 NA
#&gt; SRR1539210     3  0.0000      0.821 0.000 0.00 1.000 NA
#&gt; SRR1539209     2  0.0000      0.755 0.000 1.00 0.000 NA
#&gt; SRR1539212     2  0.0000      0.755 0.000 1.00 0.000 NA
#&gt; SRR1539214     1  0.1940      0.820 0.924 0.00 0.000 NA
#&gt; SRR1539213     3  0.6501      0.771 0.116 0.00 0.616 NA
#&gt; SRR1539215     2  0.0000      0.755 0.000 1.00 0.000 NA
#&gt; SRR1539216     3  0.0000      0.821 0.000 0.00 1.000 NA
#&gt; SRR1539217     1  0.3528      0.783 0.808 0.00 0.000 NA
#&gt; SRR1539218     2  0.0000      0.755 0.000 1.00 0.000 NA
#&gt; SRR1539220     1  0.1940      0.820 0.924 0.00 0.000 NA
#&gt; SRR1539219     3  0.0376      0.822 0.004 0.00 0.992 NA
#&gt; SRR1539221     2  0.0000      0.755 0.000 1.00 0.000 NA
#&gt; SRR1539223     1  0.3528      0.783 0.808 0.00 0.000 NA
#&gt; SRR1539224     2  0.0000      0.755 0.000 1.00 0.000 NA
#&gt; SRR1539222     3  0.0000      0.821 0.000 0.00 1.000 NA
#&gt; SRR1539225     3  0.6501      0.771 0.116 0.00 0.616 NA
#&gt; SRR1539227     2  0.0000      0.755 0.000 1.00 0.000 NA
#&gt; SRR1539226     1  0.2216      0.815 0.908 0.00 0.000 NA
#&gt; SRR1539228     3  0.6501      0.771 0.116 0.00 0.616 NA
#&gt; SRR1539229     1  0.2408      0.810 0.896 0.00 0.000 NA
#&gt; SRR1539232     3  0.6501      0.771 0.116 0.00 0.616 NA
#&gt; SRR1539230     2  0.0000      0.755 0.000 1.00 0.000 NA
#&gt; SRR1539231     2  0.0000      0.755 0.000 1.00 0.000 NA
#&gt; SRR1539234     2  0.4948      0.783 0.000 0.56 0.000 NA
#&gt; SRR1539233     1  0.2408      0.810 0.896 0.00 0.000 NA
#&gt; SRR1539235     1  0.4679      0.600 0.648 0.00 0.000 NA
#&gt; SRR1539236     1  0.4679      0.600 0.648 0.00 0.000 NA
#&gt; SRR1539237     2  0.4948      0.783 0.000 0.56 0.000 NA
#&gt; SRR1539238     1  0.0921      0.834 0.972 0.00 0.000 NA
#&gt; SRR1539239     1  0.3610      0.780 0.800 0.00 0.000 NA
#&gt; SRR1539242     1  0.3610      0.780 0.800 0.00 0.000 NA
#&gt; SRR1539240     2  0.4948      0.783 0.000 0.56 0.000 NA
#&gt; SRR1539241     1  0.0921      0.834 0.972 0.00 0.000 NA
#&gt; SRR1539243     2  0.4948      0.783 0.000 0.56 0.000 NA
#&gt; SRR1539244     1  0.4679      0.600 0.648 0.00 0.000 NA
#&gt; SRR1539245     1  0.4679      0.600 0.648 0.00 0.000 NA
#&gt; SRR1539246     2  0.4948      0.783 0.000 0.56 0.000 NA
#&gt; SRR1539247     1  0.0921      0.834 0.972 0.00 0.000 NA
#&gt; SRR1539248     1  0.3649      0.778 0.796 0.00 0.000 NA
#&gt; SRR1539249     2  0.4948      0.783 0.000 0.56 0.000 NA
#&gt; SRR1539250     1  0.0817      0.830 0.976 0.00 0.000 NA
#&gt; SRR1539251     1  0.0817      0.830 0.976 0.00 0.000 NA
#&gt; SRR1539253     2  0.4948      0.783 0.000 0.56 0.000 NA
#&gt; SRR1539252     1  0.1792      0.829 0.932 0.00 0.000 NA
#&gt; SRR1539255     1  0.4679      0.600 0.648 0.00 0.000 NA
#&gt; SRR1539254     1  0.0921      0.834 0.972 0.00 0.000 NA
#&gt; SRR1539256     2  0.4948      0.783 0.000 0.56 0.000 NA
#&gt; SRR1539257     1  0.0921      0.834 0.972 0.00 0.000 NA
#&gt; SRR1539258     1  0.3528      0.783 0.808 0.00 0.000 NA
#&gt; SRR1539259     2  0.4948      0.783 0.000 0.56 0.000 NA
#&gt; SRR1539260     1  0.0921      0.834 0.972 0.00 0.000 NA
#&gt; SRR1539262     2  0.4948      0.783 0.000 0.56 0.000 NA
#&gt; SRR1539261     1  0.3688      0.775 0.792 0.00 0.000 NA
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.4138      0.838 0.000 0.384 0.616 0.000 0.000
#&gt; SRR1539208     1  0.0000      0.659 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539211     1  0.0000      0.659 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539210     3  0.4150      0.837 0.000 0.388 0.612 0.000 0.000
#&gt; SRR1539209     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539212     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539214     1  0.4088      0.574 0.632 0.000 0.000 0.000 0.368
#&gt; SRR1539213     3  0.0609      0.795 0.000 0.000 0.980 0.000 0.020
#&gt; SRR1539215     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539216     3  0.4138      0.838 0.000 0.384 0.616 0.000 0.000
#&gt; SRR1539217     1  0.0510      0.667 0.984 0.000 0.000 0.000 0.016
#&gt; SRR1539218     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539220     1  0.4088      0.574 0.632 0.000 0.000 0.000 0.368
#&gt; SRR1539219     3  0.4101      0.839 0.000 0.372 0.628 0.000 0.000
#&gt; SRR1539221     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539223     1  0.0510      0.667 0.984 0.000 0.000 0.000 0.016
#&gt; SRR1539224     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539222     3  0.4138      0.838 0.000 0.384 0.616 0.000 0.000
#&gt; SRR1539225     3  0.0609      0.795 0.000 0.000 0.980 0.000 0.020
#&gt; SRR1539227     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539226     1  0.4138      0.557 0.616 0.000 0.000 0.000 0.384
#&gt; SRR1539228     3  0.0609      0.795 0.000 0.000 0.980 0.000 0.020
#&gt; SRR1539229     1  0.4201      0.525 0.592 0.000 0.000 0.000 0.408
#&gt; SRR1539232     3  0.0609      0.795 0.000 0.000 0.980 0.000 0.020
#&gt; SRR1539230     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539231     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539234     2  0.4150      1.000 0.000 0.612 0.000 0.388 0.000
#&gt; SRR1539233     1  0.4201      0.525 0.592 0.000 0.000 0.000 0.408
#&gt; SRR1539235     5  0.0000      0.751 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539236     5  0.0000      0.751 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539237     2  0.4150      1.000 0.000 0.612 0.000 0.388 0.000
#&gt; SRR1539238     1  0.4227      0.543 0.580 0.000 0.000 0.000 0.420
#&gt; SRR1539239     1  0.0290      0.664 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1539242     1  0.0290      0.664 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1539240     2  0.4150      1.000 0.000 0.612 0.000 0.388 0.000
#&gt; SRR1539241     1  0.4227      0.543 0.580 0.000 0.000 0.000 0.420
#&gt; SRR1539243     2  0.4150      1.000 0.000 0.612 0.000 0.388 0.000
#&gt; SRR1539244     5  0.1121      0.751 0.044 0.000 0.000 0.000 0.956
#&gt; SRR1539245     5  0.2561      0.716 0.144 0.000 0.000 0.000 0.856
#&gt; SRR1539246     2  0.4150      1.000 0.000 0.612 0.000 0.388 0.000
#&gt; SRR1539247     1  0.4227      0.543 0.580 0.000 0.000 0.000 0.420
#&gt; SRR1539248     1  0.0162      0.662 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1539249     2  0.4150      1.000 0.000 0.612 0.000 0.388 0.000
#&gt; SRR1539250     1  0.4015      0.604 0.652 0.000 0.000 0.000 0.348
#&gt; SRR1539251     1  0.4015      0.604 0.652 0.000 0.000 0.000 0.348
#&gt; SRR1539253     2  0.4150      1.000 0.000 0.612 0.000 0.388 0.000
#&gt; SRR1539252     1  0.3932      0.607 0.672 0.000 0.000 0.000 0.328
#&gt; SRR1539255     5  0.2561      0.716 0.144 0.000 0.000 0.000 0.856
#&gt; SRR1539254     5  0.4307     -0.459 0.496 0.000 0.000 0.000 0.504
#&gt; SRR1539256     2  0.4150      1.000 0.000 0.612 0.000 0.388 0.000
#&gt; SRR1539257     1  0.4227      0.543 0.580 0.000 0.000 0.000 0.420
#&gt; SRR1539258     1  0.0510      0.667 0.984 0.000 0.000 0.000 0.016
#&gt; SRR1539259     2  0.4150      1.000 0.000 0.612 0.000 0.388 0.000
#&gt; SRR1539260     1  0.4227      0.543 0.580 0.000 0.000 0.000 0.420
#&gt; SRR1539262     2  0.4150      1.000 0.000 0.612 0.000 0.388 0.000
#&gt; SRR1539261     1  0.0000      0.659 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3 p4    p5    p6
#&gt; SRR1539207     3  0.0000      0.981 0.000  0 1.000  0 0.000 0.000
#&gt; SRR1539208     1  0.0000      0.657 1.000  0 0.000  0 0.000 0.000
#&gt; SRR1539211     1  0.0000      0.657 1.000  0 0.000  0 0.000 0.000
#&gt; SRR1539210     3  0.0363      0.972 0.000  0 0.988  0 0.000 0.012
#&gt; SRR1539209     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539212     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539214     1  0.3672      0.570 0.632  0 0.000  0 0.368 0.000
#&gt; SRR1539213     6  0.2883      1.000 0.000  0 0.212  0 0.000 0.788
#&gt; SRR1539215     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539216     3  0.0146      0.982 0.000  0 0.996  0 0.000 0.004
#&gt; SRR1539217     1  0.0458      0.664 0.984  0 0.000  0 0.016 0.000
#&gt; SRR1539218     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539220     1  0.3672      0.570 0.632  0 0.000  0 0.368 0.000
#&gt; SRR1539219     3  0.0937      0.948 0.000  0 0.960  0 0.000 0.040
#&gt; SRR1539221     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539223     1  0.0458      0.664 0.984  0 0.000  0 0.016 0.000
#&gt; SRR1539224     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539222     3  0.0146      0.982 0.000  0 0.996  0 0.000 0.004
#&gt; SRR1539225     6  0.2883      1.000 0.000  0 0.212  0 0.000 0.788
#&gt; SRR1539227     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539226     1  0.3717      0.554 0.616  0 0.000  0 0.384 0.000
#&gt; SRR1539228     6  0.2883      1.000 0.000  0 0.212  0 0.000 0.788
#&gt; SRR1539229     1  0.3774      0.523 0.592  0 0.000  0 0.408 0.000
#&gt; SRR1539232     6  0.2883      1.000 0.000  0 0.212  0 0.000 0.788
#&gt; SRR1539230     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539231     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539233     1  0.3774      0.523 0.592  0 0.000  0 0.408 0.000
#&gt; SRR1539235     5  0.2793      0.722 0.000  0 0.000  0 0.800 0.200
#&gt; SRR1539236     5  0.2793      0.722 0.000  0 0.000  0 0.800 0.200
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539238     1  0.3838      0.536 0.552  0 0.000  0 0.448 0.000
#&gt; SRR1539239     1  0.0260      0.662 0.992  0 0.000  0 0.008 0.000
#&gt; SRR1539242     1  0.0260      0.662 0.992  0 0.000  0 0.008 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539241     1  0.3838      0.536 0.552  0 0.000  0 0.448 0.000
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539244     5  0.0000      0.699 0.000  0 0.000  0 1.000 0.000
#&gt; SRR1539245     5  0.2135      0.708 0.128  0 0.000  0 0.872 0.000
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539247     1  0.3838      0.536 0.552  0 0.000  0 0.448 0.000
#&gt; SRR1539248     1  0.0146      0.659 0.996  0 0.000  0 0.004 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539250     1  0.3695      0.593 0.624  0 0.000  0 0.376 0.000
#&gt; SRR1539251     1  0.3695      0.593 0.624  0 0.000  0 0.376 0.000
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539252     1  0.3531      0.601 0.672  0 0.000  0 0.328 0.000
#&gt; SRR1539255     5  0.2135      0.708 0.128  0 0.000  0 0.872 0.000
#&gt; SRR1539254     5  0.3847     -0.446 0.456  0 0.000  0 0.544 0.000
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539257     1  0.3838      0.536 0.552  0 0.000  0 0.448 0.000
#&gt; SRR1539258     1  0.0458      0.664 0.984  0 0.000  0 0.016 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539260     1  0.3838      0.536 0.552  0 0.000  0 0.448 0.000
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539261     1  0.0000      0.657 1.000  0 0.000  0 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.778           0.953       0.918         0.3063 0.836   0.699
#> 4 4 0.674           0.830       0.777         0.1434 0.987   0.966
#> 5 5 0.716           0.687       0.721         0.0945 0.833   0.561
#> 6 6 0.700           0.633       0.744         0.0633 0.931   0.710
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1539207     3  0.5016      1.000 0.240 0.000 0.760
#&gt; SRR1539208     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539211     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539210     3  0.5016      1.000 0.240 0.000 0.760
#&gt; SRR1539209     2  0.4974      0.887 0.000 0.764 0.236
#&gt; SRR1539212     2  0.4974      0.887 0.000 0.764 0.236
#&gt; SRR1539214     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539213     3  0.5016      1.000 0.240 0.000 0.760
#&gt; SRR1539215     2  0.4974      0.887 0.000 0.764 0.236
#&gt; SRR1539216     3  0.5016      1.000 0.240 0.000 0.760
#&gt; SRR1539217     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539218     2  0.4974      0.887 0.000 0.764 0.236
#&gt; SRR1539220     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539219     3  0.5016      1.000 0.240 0.000 0.760
#&gt; SRR1539221     2  0.4974      0.887 0.000 0.764 0.236
#&gt; SRR1539223     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539224     2  0.4974      0.887 0.000 0.764 0.236
#&gt; SRR1539222     3  0.5016      1.000 0.240 0.000 0.760
#&gt; SRR1539225     3  0.5016      1.000 0.240 0.000 0.760
#&gt; SRR1539227     2  0.4974      0.887 0.000 0.764 0.236
#&gt; SRR1539226     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539228     3  0.5016      1.000 0.240 0.000 0.760
#&gt; SRR1539229     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539232     3  0.5016      1.000 0.240 0.000 0.760
#&gt; SRR1539230     2  0.4974      0.887 0.000 0.764 0.236
#&gt; SRR1539231     2  0.4974      0.887 0.000 0.764 0.236
#&gt; SRR1539234     2  0.0000      0.899 0.000 1.000 0.000
#&gt; SRR1539233     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539235     1  0.1411      0.969 0.964 0.000 0.036
#&gt; SRR1539236     1  0.0237      0.981 0.996 0.000 0.004
#&gt; SRR1539237     2  0.0000      0.899 0.000 1.000 0.000
#&gt; SRR1539238     1  0.1289      0.970 0.968 0.000 0.032
#&gt; SRR1539239     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539242     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539240     2  0.0000      0.899 0.000 1.000 0.000
#&gt; SRR1539241     1  0.1289      0.970 0.968 0.000 0.032
#&gt; SRR1539243     2  0.0000      0.899 0.000 1.000 0.000
#&gt; SRR1539244     1  0.1289      0.970 0.968 0.000 0.032
#&gt; SRR1539245     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539246     2  0.0000      0.899 0.000 1.000 0.000
#&gt; SRR1539247     1  0.1289      0.970 0.968 0.000 0.032
#&gt; SRR1539248     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539249     2  0.0000      0.899 0.000 1.000 0.000
#&gt; SRR1539250     1  0.1289      0.970 0.968 0.000 0.032
#&gt; SRR1539251     1  0.1289      0.970 0.968 0.000 0.032
#&gt; SRR1539253     2  0.0000      0.899 0.000 1.000 0.000
#&gt; SRR1539252     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539255     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539254     1  0.1163      0.972 0.972 0.000 0.028
#&gt; SRR1539256     2  0.0000      0.899 0.000 1.000 0.000
#&gt; SRR1539257     1  0.1289      0.970 0.968 0.000 0.032
#&gt; SRR1539258     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1539259     2  0.0000      0.899 0.000 1.000 0.000
#&gt; SRR1539260     1  0.1289      0.970 0.968 0.000 0.032
#&gt; SRR1539262     2  0.0000      0.899 0.000 1.000 0.000
#&gt; SRR1539261     1  0.0000      0.984 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     4  0.6568      0.944 0.080 0.000 0.408 0.512
#&gt; SRR1539208     1  0.3219      0.834 0.836 0.000 0.000 0.164
#&gt; SRR1539211     1  0.3219      0.834 0.836 0.000 0.000 0.164
#&gt; SRR1539210     4  0.6546      0.842 0.080 0.000 0.396 0.524
#&gt; SRR1539209     2  0.0817      0.733 0.000 0.976 0.000 0.024
#&gt; SRR1539212     2  0.0817      0.733 0.000 0.976 0.000 0.024
#&gt; SRR1539214     1  0.0592      0.872 0.984 0.000 0.000 0.016
#&gt; SRR1539213     3  0.6580      1.000 0.080 0.000 0.504 0.416
#&gt; SRR1539215     2  0.0336      0.733 0.000 0.992 0.000 0.008
#&gt; SRR1539216     4  0.6568      0.944 0.080 0.000 0.408 0.512
#&gt; SRR1539217     1  0.3074      0.836 0.848 0.000 0.000 0.152
#&gt; SRR1539218     2  0.0592      0.733 0.000 0.984 0.000 0.016
#&gt; SRR1539220     1  0.0000      0.872 1.000 0.000 0.000 0.000
#&gt; SRR1539219     3  0.6580      1.000 0.080 0.000 0.504 0.416
#&gt; SRR1539221     2  0.0000      0.733 0.000 1.000 0.000 0.000
#&gt; SRR1539223     1  0.3074      0.836 0.848 0.000 0.000 0.152
#&gt; SRR1539224     2  0.0817      0.733 0.000 0.976 0.000 0.024
#&gt; SRR1539222     4  0.6568      0.944 0.080 0.000 0.408 0.512
#&gt; SRR1539225     3  0.6580      1.000 0.080 0.000 0.504 0.416
#&gt; SRR1539227     2  0.0188      0.733 0.000 0.996 0.000 0.004
#&gt; SRR1539226     1  0.1022      0.871 0.968 0.000 0.000 0.032
#&gt; SRR1539228     3  0.6580      1.000 0.080 0.000 0.504 0.416
#&gt; SRR1539229     1  0.1118      0.871 0.964 0.000 0.000 0.036
#&gt; SRR1539232     3  0.6580      1.000 0.080 0.000 0.504 0.416
#&gt; SRR1539230     2  0.0188      0.733 0.000 0.996 0.000 0.004
#&gt; SRR1539231     2  0.0188      0.733 0.000 0.996 0.000 0.004
#&gt; SRR1539234     2  0.5168      0.761 0.000 0.504 0.492 0.004
#&gt; SRR1539233     1  0.1118      0.871 0.964 0.000 0.000 0.036
#&gt; SRR1539235     1  0.4164      0.785 0.736 0.000 0.000 0.264
#&gt; SRR1539236     1  0.3311      0.827 0.828 0.000 0.000 0.172
#&gt; SRR1539237     2  0.5693      0.760 0.000 0.504 0.472 0.024
#&gt; SRR1539238     1  0.2921      0.847 0.860 0.000 0.000 0.140
#&gt; SRR1539239     1  0.3569      0.830 0.804 0.000 0.000 0.196
#&gt; SRR1539242     1  0.3569      0.830 0.804 0.000 0.000 0.196
#&gt; SRR1539240     2  0.5407      0.761 0.000 0.504 0.484 0.012
#&gt; SRR1539241     1  0.2921      0.847 0.860 0.000 0.000 0.140
#&gt; SRR1539243     2  0.5407      0.761 0.000 0.504 0.484 0.012
#&gt; SRR1539244     1  0.3975      0.797 0.760 0.000 0.000 0.240
#&gt; SRR1539245     1  0.3024      0.837 0.852 0.000 0.000 0.148
#&gt; SRR1539246     2  0.5000      0.761 0.000 0.504 0.496 0.000
#&gt; SRR1539247     1  0.2760      0.848 0.872 0.000 0.000 0.128
#&gt; SRR1539248     1  0.3172      0.837 0.840 0.000 0.000 0.160
#&gt; SRR1539249     2  0.5693      0.760 0.000 0.504 0.472 0.024
#&gt; SRR1539250     1  0.2704      0.851 0.876 0.000 0.000 0.124
#&gt; SRR1539251     1  0.2704      0.851 0.876 0.000 0.000 0.124
#&gt; SRR1539253     2  0.5693      0.760 0.000 0.504 0.472 0.024
#&gt; SRR1539252     1  0.1302      0.871 0.956 0.000 0.000 0.044
#&gt; SRR1539255     1  0.2868      0.844 0.864 0.000 0.000 0.136
#&gt; SRR1539254     1  0.2921      0.845 0.860 0.000 0.000 0.140
#&gt; SRR1539256     2  0.5000      0.761 0.000 0.504 0.496 0.000
#&gt; SRR1539257     1  0.2760      0.848 0.872 0.000 0.000 0.128
#&gt; SRR1539258     1  0.3311      0.836 0.828 0.000 0.000 0.172
#&gt; SRR1539259     2  0.5693      0.760 0.000 0.504 0.472 0.024
#&gt; SRR1539260     1  0.2760      0.848 0.872 0.000 0.000 0.128
#&gt; SRR1539262     2  0.5693      0.760 0.000 0.504 0.472 0.024
#&gt; SRR1539261     1  0.3356      0.831 0.824 0.000 0.000 0.176
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.4074      0.932 0.052 0.092 0.820 0.000 0.036
#&gt; SRR1539208     1  0.5344      0.790 0.500 0.052 0.000 0.000 0.448
#&gt; SRR1539211     1  0.5344      0.790 0.500 0.052 0.000 0.000 0.448
#&gt; SRR1539210     3  0.4414      0.920 0.060 0.108 0.796 0.000 0.036
#&gt; SRR1539209     4  0.2179      0.920 0.112 0.000 0.000 0.888 0.000
#&gt; SRR1539212     4  0.2179      0.920 0.112 0.000 0.000 0.888 0.000
#&gt; SRR1539214     5  0.4795      0.373 0.224 0.072 0.000 0.000 0.704
#&gt; SRR1539213     3  0.0963      0.947 0.000 0.000 0.964 0.000 0.036
#&gt; SRR1539215     4  0.0703      0.935 0.024 0.000 0.000 0.976 0.000
#&gt; SRR1539216     3  0.4074      0.932 0.052 0.092 0.820 0.000 0.036
#&gt; SRR1539217     5  0.5376     -0.693 0.424 0.056 0.000 0.000 0.520
#&gt; SRR1539218     4  0.2011      0.924 0.088 0.000 0.004 0.908 0.000
#&gt; SRR1539220     5  0.4313      0.371 0.172 0.068 0.000 0.000 0.760
#&gt; SRR1539219     3  0.0963      0.947 0.000 0.000 0.964 0.000 0.036
#&gt; SRR1539221     4  0.0162      0.937 0.004 0.000 0.000 0.996 0.000
#&gt; SRR1539223     5  0.5431     -0.693 0.424 0.060 0.000 0.000 0.516
#&gt; SRR1539224     4  0.2233      0.922 0.104 0.000 0.004 0.892 0.000
#&gt; SRR1539222     3  0.4074      0.932 0.052 0.092 0.820 0.000 0.036
#&gt; SRR1539225     3  0.0963      0.947 0.000 0.000 0.964 0.000 0.036
#&gt; SRR1539227     4  0.0162      0.937 0.004 0.000 0.000 0.996 0.000
#&gt; SRR1539226     5  0.4755      0.358 0.244 0.060 0.000 0.000 0.696
#&gt; SRR1539228     3  0.0963      0.947 0.000 0.000 0.964 0.000 0.036
#&gt; SRR1539229     5  0.4755      0.367 0.244 0.060 0.000 0.000 0.696
#&gt; SRR1539232     3  0.0963      0.947 0.000 0.000 0.964 0.000 0.036
#&gt; SRR1539230     4  0.0162      0.937 0.000 0.000 0.004 0.996 0.000
#&gt; SRR1539231     4  0.0162      0.937 0.000 0.000 0.004 0.996 0.000
#&gt; SRR1539234     2  0.4749      0.945 0.020 0.620 0.004 0.356 0.000
#&gt; SRR1539233     5  0.4755      0.367 0.244 0.060 0.000 0.000 0.696
#&gt; SRR1539235     5  0.5426      0.392 0.160 0.160 0.004 0.000 0.676
#&gt; SRR1539236     5  0.6491      0.219 0.336 0.200 0.000 0.000 0.464
#&gt; SRR1539237     2  0.5649      0.919 0.060 0.572 0.012 0.356 0.000
#&gt; SRR1539238     5  0.1686      0.543 0.028 0.020 0.008 0.000 0.944
#&gt; SRR1539239     1  0.4470      0.811 0.616 0.012 0.000 0.000 0.372
#&gt; SRR1539242     1  0.4470      0.811 0.616 0.012 0.000 0.000 0.372
#&gt; SRR1539240     2  0.5307      0.933 0.044 0.592 0.008 0.356 0.000
#&gt; SRR1539241     5  0.1686      0.543 0.028 0.020 0.008 0.000 0.944
#&gt; SRR1539243     2  0.5370      0.932 0.048 0.588 0.008 0.356 0.000
#&gt; SRR1539244     5  0.4446      0.470 0.116 0.100 0.008 0.000 0.776
#&gt; SRR1539245     5  0.6068      0.263 0.328 0.140 0.000 0.000 0.532
#&gt; SRR1539246     2  0.4347      0.948 0.004 0.636 0.004 0.356 0.000
#&gt; SRR1539247     5  0.0579      0.555 0.000 0.008 0.008 0.000 0.984
#&gt; SRR1539248     1  0.4718      0.841 0.540 0.016 0.000 0.000 0.444
#&gt; SRR1539249     2  0.5274      0.941 0.036 0.596 0.012 0.356 0.000
#&gt; SRR1539250     5  0.2178      0.522 0.024 0.048 0.008 0.000 0.920
#&gt; SRR1539251     5  0.2178      0.522 0.024 0.048 0.008 0.000 0.920
#&gt; SRR1539253     2  0.5274      0.941 0.036 0.596 0.012 0.356 0.000
#&gt; SRR1539252     5  0.5008      0.245 0.300 0.056 0.000 0.000 0.644
#&gt; SRR1539255     5  0.5996      0.149 0.388 0.116 0.000 0.000 0.496
#&gt; SRR1539254     5  0.1299      0.553 0.020 0.012 0.008 0.000 0.960
#&gt; SRR1539256     2  0.4347      0.948 0.004 0.636 0.004 0.356 0.000
#&gt; SRR1539257     5  0.0451      0.555 0.000 0.004 0.008 0.000 0.988
#&gt; SRR1539258     1  0.4201      0.816 0.592 0.000 0.000 0.000 0.408
#&gt; SRR1539259     2  0.5274      0.941 0.036 0.596 0.012 0.356 0.000
#&gt; SRR1539260     5  0.0579      0.555 0.000 0.008 0.008 0.000 0.984
#&gt; SRR1539262     2  0.5274      0.941 0.036 0.596 0.012 0.356 0.000
#&gt; SRR1539261     1  0.4867      0.843 0.544 0.024 0.000 0.000 0.432
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.3337     0.8628 0.000 0.000 0.736 0.000 0.004 0.260
#&gt; SRR1539208     1  0.4704     0.6989 0.660 0.000 0.000 0.012 0.272 0.056
#&gt; SRR1539211     1  0.4628     0.6999 0.668 0.000 0.000 0.012 0.268 0.052
#&gt; SRR1539210     3  0.4139     0.8350 0.008 0.000 0.688 0.016 0.004 0.284
#&gt; SRR1539209     4  0.5871     0.8749 0.008 0.280 0.000 0.520 0.000 0.192
#&gt; SRR1539212     4  0.5871     0.8749 0.008 0.280 0.000 0.520 0.000 0.192
#&gt; SRR1539214     5  0.6649    -0.0776 0.224 0.000 0.000 0.080 0.512 0.184
#&gt; SRR1539213     3  0.0146     0.8933 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539215     4  0.4488     0.8948 0.004 0.280 0.000 0.664 0.000 0.052
#&gt; SRR1539216     3  0.3337     0.8628 0.000 0.000 0.736 0.000 0.004 0.260
#&gt; SRR1539217     1  0.4962     0.6547 0.608 0.000 0.000 0.012 0.320 0.060
#&gt; SRR1539218     4  0.5657     0.8853 0.008 0.280 0.004 0.568 0.000 0.140
#&gt; SRR1539220     5  0.6247     0.0252 0.188 0.000 0.000 0.068 0.572 0.172
#&gt; SRR1539219     3  0.0405     0.8934 0.000 0.000 0.988 0.000 0.004 0.008
#&gt; SRR1539221     4  0.3693     0.8974 0.008 0.280 0.004 0.708 0.000 0.000
#&gt; SRR1539223     1  0.5156     0.6516 0.592 0.000 0.000 0.012 0.320 0.076
#&gt; SRR1539224     4  0.5883     0.8804 0.008 0.280 0.004 0.536 0.000 0.172
#&gt; SRR1539222     3  0.3337     0.8628 0.000 0.000 0.736 0.000 0.004 0.260
#&gt; SRR1539225     3  0.0146     0.8933 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539227     4  0.3693     0.8938 0.008 0.280 0.000 0.708 0.000 0.004
#&gt; SRR1539226     5  0.6702    -0.1263 0.252 0.000 0.000 0.076 0.492 0.180
#&gt; SRR1539228     3  0.0146     0.8933 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539229     5  0.6724    -0.1363 0.252 0.000 0.000 0.076 0.488 0.184
#&gt; SRR1539232     3  0.0291     0.8931 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; SRR1539230     4  0.4107     0.8938 0.028 0.280 0.000 0.688 0.000 0.004
#&gt; SRR1539231     4  0.4107     0.8938 0.028 0.280 0.000 0.688 0.000 0.004
#&gt; SRR1539234     2  0.1088     0.9288 0.024 0.960 0.000 0.000 0.000 0.016
#&gt; SRR1539233     5  0.6724    -0.1363 0.252 0.000 0.000 0.076 0.488 0.184
#&gt; SRR1539235     5  0.6074    -0.0250 0.052 0.000 0.000 0.156 0.580 0.212
#&gt; SRR1539236     6  0.7694     0.6594 0.208 0.000 0.000 0.248 0.248 0.296
#&gt; SRR1539237     2  0.2724     0.8741 0.052 0.864 0.000 0.000 0.000 0.084
#&gt; SRR1539238     5  0.1377     0.5334 0.004 0.000 0.004 0.024 0.952 0.016
#&gt; SRR1539239     1  0.4624     0.6457 0.724 0.000 0.000 0.064 0.180 0.032
#&gt; SRR1539242     1  0.4624     0.6457 0.724 0.000 0.000 0.064 0.180 0.032
#&gt; SRR1539240     2  0.1549     0.9162 0.020 0.936 0.000 0.000 0.000 0.044
#&gt; SRR1539241     5  0.1377     0.5334 0.004 0.000 0.004 0.024 0.952 0.016
#&gt; SRR1539243     2  0.1616     0.9157 0.020 0.932 0.000 0.000 0.000 0.048
#&gt; SRR1539244     5  0.4982     0.2226 0.048 0.000 0.004 0.076 0.716 0.156
#&gt; SRR1539245     6  0.7460     0.6387 0.280 0.000 0.000 0.124 0.292 0.304
#&gt; SRR1539246     2  0.0806     0.9306 0.020 0.972 0.000 0.000 0.000 0.008
#&gt; SRR1539247     5  0.0291     0.5475 0.000 0.000 0.004 0.004 0.992 0.000
#&gt; SRR1539248     1  0.2883     0.7154 0.788 0.000 0.000 0.000 0.212 0.000
#&gt; SRR1539249     2  0.1461     0.9253 0.044 0.940 0.000 0.000 0.000 0.016
#&gt; SRR1539250     5  0.2001     0.5250 0.028 0.000 0.004 0.012 0.924 0.032
#&gt; SRR1539251     5  0.2001     0.5250 0.028 0.000 0.004 0.012 0.924 0.032
#&gt; SRR1539253     2  0.1461     0.9253 0.044 0.940 0.000 0.000 0.000 0.016
#&gt; SRR1539252     5  0.6892    -0.3290 0.328 0.000 0.000 0.080 0.420 0.172
#&gt; SRR1539255     1  0.7405    -0.7284 0.344 0.000 0.000 0.120 0.280 0.256
#&gt; SRR1539254     5  0.0924     0.5363 0.008 0.000 0.004 0.008 0.972 0.008
#&gt; SRR1539256     2  0.0909     0.9302 0.020 0.968 0.000 0.000 0.000 0.012
#&gt; SRR1539257     5  0.0146     0.5475 0.000 0.000 0.004 0.000 0.996 0.000
#&gt; SRR1539258     1  0.4214     0.6744 0.740 0.000 0.000 0.032 0.200 0.028
#&gt; SRR1539259     2  0.1461     0.9253 0.044 0.940 0.000 0.000 0.000 0.016
#&gt; SRR1539260     5  0.0291     0.5475 0.000 0.000 0.004 0.004 0.992 0.000
#&gt; SRR1539262     2  0.1461     0.9253 0.044 0.940 0.000 0.000 0.000 0.016
#&gt; SRR1539261     1  0.3374     0.7137 0.772 0.000 0.000 0.000 0.208 0.020
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.973       0.989         0.4656 0.532   0.532
#> 3 3 1.000           0.989       0.991         0.4501 0.769   0.575
#> 4 4 0.766           0.784       0.799         0.0935 0.842   0.573
#> 5 5 0.773           0.820       0.858         0.0799 0.942   0.774
#> 6 6 0.915           0.889       0.921         0.0571 0.931   0.673
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1539207     1   0.000      0.994 1.000 0.000
#&gt; SRR1539208     1   0.000      0.994 1.000 0.000
#&gt; SRR1539211     1   0.714      0.748 0.804 0.196
#&gt; SRR1539210     1   0.000      0.994 1.000 0.000
#&gt; SRR1539209     2   0.000      0.979 0.000 1.000
#&gt; SRR1539212     2   0.000      0.979 0.000 1.000
#&gt; SRR1539214     1   0.000      0.994 1.000 0.000
#&gt; SRR1539213     1   0.000      0.994 1.000 0.000
#&gt; SRR1539215     2   0.000      0.979 0.000 1.000
#&gt; SRR1539216     1   0.000      0.994 1.000 0.000
#&gt; SRR1539217     1   0.000      0.994 1.000 0.000
#&gt; SRR1539218     2   0.000      0.979 0.000 1.000
#&gt; SRR1539220     1   0.000      0.994 1.000 0.000
#&gt; SRR1539219     1   0.000      0.994 1.000 0.000
#&gt; SRR1539221     2   0.000      0.979 0.000 1.000
#&gt; SRR1539223     1   0.000      0.994 1.000 0.000
#&gt; SRR1539224     2   0.000      0.979 0.000 1.000
#&gt; SRR1539222     1   0.000      0.994 1.000 0.000
#&gt; SRR1539225     1   0.000      0.994 1.000 0.000
#&gt; SRR1539227     2   0.000      0.979 0.000 1.000
#&gt; SRR1539226     1   0.000      0.994 1.000 0.000
#&gt; SRR1539228     1   0.000      0.994 1.000 0.000
#&gt; SRR1539229     1   0.000      0.994 1.000 0.000
#&gt; SRR1539232     1   0.000      0.994 1.000 0.000
#&gt; SRR1539230     2   0.000      0.979 0.000 1.000
#&gt; SRR1539231     2   0.000      0.979 0.000 1.000
#&gt; SRR1539234     2   0.000      0.979 0.000 1.000
#&gt; SRR1539233     1   0.000      0.994 1.000 0.000
#&gt; SRR1539235     1   0.000      0.994 1.000 0.000
#&gt; SRR1539236     1   0.000      0.994 1.000 0.000
#&gt; SRR1539237     2   0.000      0.979 0.000 1.000
#&gt; SRR1539238     1   0.000      0.994 1.000 0.000
#&gt; SRR1539239     1   0.000      0.994 1.000 0.000
#&gt; SRR1539242     1   0.000      0.994 1.000 0.000
#&gt; SRR1539240     2   0.000      0.979 0.000 1.000
#&gt; SRR1539241     1   0.000      0.994 1.000 0.000
#&gt; SRR1539243     2   0.000      0.979 0.000 1.000
#&gt; SRR1539244     1   0.000      0.994 1.000 0.000
#&gt; SRR1539245     1   0.000      0.994 1.000 0.000
#&gt; SRR1539246     2   0.000      0.979 0.000 1.000
#&gt; SRR1539247     1   0.000      0.994 1.000 0.000
#&gt; SRR1539248     1   0.000      0.994 1.000 0.000
#&gt; SRR1539249     2   0.000      0.979 0.000 1.000
#&gt; SRR1539250     1   0.000      0.994 1.000 0.000
#&gt; SRR1539251     1   0.000      0.994 1.000 0.000
#&gt; SRR1539253     2   0.000      0.979 0.000 1.000
#&gt; SRR1539252     1   0.000      0.994 1.000 0.000
#&gt; SRR1539255     1   0.000      0.994 1.000 0.000
#&gt; SRR1539254     1   0.000      0.994 1.000 0.000
#&gt; SRR1539256     2   0.000      0.979 0.000 1.000
#&gt; SRR1539257     1   0.000      0.994 1.000 0.000
#&gt; SRR1539258     1   0.000      0.994 1.000 0.000
#&gt; SRR1539259     2   0.000      0.979 0.000 1.000
#&gt; SRR1539260     1   0.000      0.994 1.000 0.000
#&gt; SRR1539262     2   0.000      0.979 0.000 1.000
#&gt; SRR1539261     2   0.966      0.344 0.392 0.608
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3
#&gt; SRR1539207     3  0.0000      0.993 0.000  0 1.000
#&gt; SRR1539208     1  0.0000      0.979 1.000  0 0.000
#&gt; SRR1539211     1  0.0000      0.979 1.000  0 0.000
#&gt; SRR1539210     3  0.0000      0.993 0.000  0 1.000
#&gt; SRR1539209     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539212     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539214     1  0.2261      0.948 0.932  0 0.068
#&gt; SRR1539213     3  0.0000      0.993 0.000  0 1.000
#&gt; SRR1539215     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539216     3  0.0000      0.993 0.000  0 1.000
#&gt; SRR1539217     1  0.0000      0.979 1.000  0 0.000
#&gt; SRR1539218     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539220     3  0.2261      0.931 0.068  0 0.932
#&gt; SRR1539219     3  0.0000      0.993 0.000  0 1.000
#&gt; SRR1539221     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539223     1  0.1753      0.954 0.952  0 0.048
#&gt; SRR1539224     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539222     3  0.0000      0.993 0.000  0 1.000
#&gt; SRR1539225     3  0.0000      0.993 0.000  0 1.000
#&gt; SRR1539227     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539226     1  0.1289      0.978 0.968  0 0.032
#&gt; SRR1539228     3  0.0000      0.993 0.000  0 1.000
#&gt; SRR1539229     1  0.1289      0.978 0.968  0 0.032
#&gt; SRR1539232     3  0.0000      0.993 0.000  0 1.000
#&gt; SRR1539230     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539231     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539233     1  0.1289      0.978 0.968  0 0.032
#&gt; SRR1539235     3  0.0424      0.992 0.008  0 0.992
#&gt; SRR1539236     1  0.1289      0.978 0.968  0 0.032
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539238     3  0.0424      0.992 0.008  0 0.992
#&gt; SRR1539239     1  0.0000      0.979 1.000  0 0.000
#&gt; SRR1539242     1  0.0000      0.979 1.000  0 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539241     3  0.0424      0.992 0.008  0 0.992
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539244     3  0.0424      0.992 0.008  0 0.992
#&gt; SRR1539245     1  0.1289      0.978 0.968  0 0.032
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539247     3  0.0424      0.992 0.008  0 0.992
#&gt; SRR1539248     1  0.0000      0.979 1.000  0 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539250     3  0.0237      0.993 0.004  0 0.996
#&gt; SRR1539251     3  0.0237      0.993 0.004  0 0.996
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539252     1  0.1289      0.978 0.968  0 0.032
#&gt; SRR1539255     1  0.1289      0.978 0.968  0 0.032
#&gt; SRR1539254     3  0.0424      0.992 0.008  0 0.992
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539257     3  0.0424      0.992 0.008  0 0.992
#&gt; SRR1539258     1  0.0000      0.979 1.000  0 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539260     3  0.0424      0.992 0.008  0 0.992
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539261     1  0.0000      0.979 1.000  0 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; SRR1539208     4  0.1118      0.946 0.036 0.000 0.000 0.964
#&gt; SRR1539211     4  0.0921      0.953 0.028 0.000 0.000 0.972
#&gt; SRR1539210     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; SRR1539209     2  0.0000      0.842 0.000 1.000 0.000 0.000
#&gt; SRR1539212     2  0.0000      0.842 0.000 1.000 0.000 0.000
#&gt; SRR1539214     1  0.5698      0.512 0.608 0.000 0.036 0.356
#&gt; SRR1539213     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; SRR1539215     2  0.0000      0.842 0.000 1.000 0.000 0.000
#&gt; SRR1539216     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; SRR1539217     4  0.0921      0.953 0.028 0.000 0.000 0.972
#&gt; SRR1539218     2  0.0000      0.842 0.000 1.000 0.000 0.000
#&gt; SRR1539220     1  0.6701      0.586 0.564 0.000 0.328 0.108
#&gt; SRR1539219     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; SRR1539221     2  0.0000      0.842 0.000 1.000 0.000 0.000
#&gt; SRR1539223     4  0.2578      0.871 0.036 0.000 0.052 0.912
#&gt; SRR1539224     2  0.0000      0.842 0.000 1.000 0.000 0.000
#&gt; SRR1539222     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; SRR1539225     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; SRR1539227     2  0.0000      0.842 0.000 1.000 0.000 0.000
#&gt; SRR1539226     1  0.5080      0.483 0.576 0.000 0.004 0.420
#&gt; SRR1539228     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; SRR1539229     1  0.5080      0.483 0.576 0.000 0.004 0.420
#&gt; SRR1539232     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; SRR1539230     2  0.0000      0.842 0.000 1.000 0.000 0.000
#&gt; SRR1539231     2  0.0000      0.842 0.000 1.000 0.000 0.000
#&gt; SRR1539234     2  0.4500      0.859 0.316 0.684 0.000 0.000
#&gt; SRR1539233     1  0.5080      0.483 0.576 0.000 0.004 0.420
#&gt; SRR1539235     1  0.4917      0.569 0.656 0.000 0.336 0.008
#&gt; SRR1539236     1  0.5070      0.486 0.580 0.000 0.004 0.416
#&gt; SRR1539237     2  0.4500      0.859 0.316 0.684 0.000 0.000
#&gt; SRR1539238     1  0.5138      0.529 0.600 0.000 0.392 0.008
#&gt; SRR1539239     4  0.0817      0.951 0.024 0.000 0.000 0.976
#&gt; SRR1539242     4  0.0817      0.951 0.024 0.000 0.000 0.976
#&gt; SRR1539240     2  0.4500      0.859 0.316 0.684 0.000 0.000
#&gt; SRR1539241     1  0.5138      0.529 0.600 0.000 0.392 0.008
#&gt; SRR1539243     2  0.4500      0.859 0.316 0.684 0.000 0.000
#&gt; SRR1539244     1  0.4917      0.569 0.656 0.000 0.336 0.008
#&gt; SRR1539245     1  0.5080      0.483 0.576 0.000 0.004 0.420
#&gt; SRR1539246     2  0.4500      0.859 0.316 0.684 0.000 0.000
#&gt; SRR1539247     1  0.5150      0.524 0.596 0.000 0.396 0.008
#&gt; SRR1539248     4  0.0000      0.956 0.000 0.000 0.000 1.000
#&gt; SRR1539249     2  0.4500      0.859 0.316 0.684 0.000 0.000
#&gt; SRR1539250     3  0.2675      0.873 0.100 0.000 0.892 0.008
#&gt; SRR1539251     3  0.2675      0.873 0.100 0.000 0.892 0.008
#&gt; SRR1539253     2  0.4500      0.859 0.316 0.684 0.000 0.000
#&gt; SRR1539252     1  0.5097      0.470 0.568 0.000 0.004 0.428
#&gt; SRR1539255     1  0.5088      0.477 0.572 0.000 0.004 0.424
#&gt; SRR1539254     1  0.5138      0.529 0.600 0.000 0.392 0.008
#&gt; SRR1539256     2  0.4500      0.859 0.316 0.684 0.000 0.000
#&gt; SRR1539257     1  0.5150      0.524 0.596 0.000 0.396 0.008
#&gt; SRR1539258     4  0.0817      0.951 0.024 0.000 0.000 0.976
#&gt; SRR1539259     2  0.4500      0.859 0.316 0.684 0.000 0.000
#&gt; SRR1539260     1  0.5150      0.524 0.596 0.000 0.396 0.008
#&gt; SRR1539262     2  0.4500      0.859 0.316 0.684 0.000 0.000
#&gt; SRR1539261     4  0.0000      0.956 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.0000      0.903 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539208     1  0.3160      0.797 0.808 0.000 0.000 0.004 0.188
#&gt; SRR1539211     1  0.2930      0.817 0.832 0.000 0.000 0.004 0.164
#&gt; SRR1539210     3  0.0162      0.899 0.004 0.000 0.996 0.000 0.000
#&gt; SRR1539209     4  0.2891      1.000 0.000 0.176 0.000 0.824 0.000
#&gt; SRR1539212     4  0.2891      1.000 0.000 0.176 0.000 0.824 0.000
#&gt; SRR1539214     5  0.5373      0.625 0.128 0.000 0.008 0.176 0.688
#&gt; SRR1539213     3  0.0000      0.903 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539215     4  0.2891      1.000 0.000 0.176 0.000 0.824 0.000
#&gt; SRR1539216     3  0.0000      0.903 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539217     1  0.3053      0.814 0.828 0.000 0.000 0.008 0.164
#&gt; SRR1539218     4  0.2891      1.000 0.000 0.176 0.000 0.824 0.000
#&gt; SRR1539220     5  0.5783      0.652 0.076 0.000 0.088 0.136 0.700
#&gt; SRR1539219     3  0.0000      0.903 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539221     4  0.2891      1.000 0.000 0.176 0.000 0.824 0.000
#&gt; SRR1539223     1  0.3864      0.779 0.784 0.000 0.020 0.008 0.188
#&gt; SRR1539224     4  0.2891      1.000 0.000 0.176 0.000 0.824 0.000
#&gt; SRR1539222     3  0.0000      0.903 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539225     3  0.0000      0.903 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539227     4  0.2891      1.000 0.000 0.176 0.000 0.824 0.000
#&gt; SRR1539226     5  0.6034      0.593 0.256 0.000 0.000 0.172 0.572
#&gt; SRR1539228     3  0.0000      0.903 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539229     5  0.6034      0.593 0.256 0.000 0.000 0.172 0.572
#&gt; SRR1539232     3  0.0000      0.903 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539230     4  0.2891      1.000 0.000 0.176 0.000 0.824 0.000
#&gt; SRR1539231     4  0.2891      1.000 0.000 0.176 0.000 0.824 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539233     5  0.6034      0.593 0.256 0.000 0.000 0.172 0.572
#&gt; SRR1539235     5  0.0963      0.682 0.000 0.000 0.036 0.000 0.964
#&gt; SRR1539236     5  0.6178      0.554 0.296 0.000 0.000 0.168 0.536
#&gt; SRR1539237     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539238     5  0.1908      0.678 0.000 0.000 0.092 0.000 0.908
#&gt; SRR1539239     1  0.1915      0.838 0.928 0.000 0.000 0.040 0.032
#&gt; SRR1539242     1  0.1915      0.838 0.928 0.000 0.000 0.040 0.032
#&gt; SRR1539240     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539241     5  0.1908      0.678 0.000 0.000 0.092 0.000 0.908
#&gt; SRR1539243     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539244     5  0.1357      0.685 0.000 0.000 0.048 0.004 0.948
#&gt; SRR1539245     5  0.6221      0.556 0.300 0.000 0.000 0.172 0.528
#&gt; SRR1539246     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539247     5  0.2127      0.671 0.000 0.000 0.108 0.000 0.892
#&gt; SRR1539248     1  0.0162      0.851 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1539249     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539250     3  0.4557      0.378 0.004 0.000 0.552 0.004 0.440
#&gt; SRR1539251     3  0.4557      0.378 0.004 0.000 0.552 0.004 0.440
#&gt; SRR1539253     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539252     5  0.6326      0.516 0.336 0.000 0.000 0.172 0.492
#&gt; SRR1539255     5  0.6290      0.526 0.332 0.000 0.000 0.168 0.500
#&gt; SRR1539254     5  0.1965      0.678 0.000 0.000 0.096 0.000 0.904
#&gt; SRR1539256     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.2074      0.674 0.000 0.000 0.104 0.000 0.896
#&gt; SRR1539258     1  0.1725      0.832 0.936 0.000 0.000 0.044 0.020
#&gt; SRR1539259     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539260     5  0.2127      0.671 0.000 0.000 0.108 0.000 0.892
#&gt; SRR1539262     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539261     1  0.0000      0.852 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.0146      0.999 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539208     1  0.3459      0.718 0.832 0.000 0.004 0.024 0.036 0.104
#&gt; SRR1539211     1  0.3159      0.721 0.844 0.000 0.004 0.024 0.016 0.112
#&gt; SRR1539210     3  0.0146      0.991 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539209     4  0.1204      1.000 0.000 0.056 0.000 0.944 0.000 0.000
#&gt; SRR1539212     4  0.1204      1.000 0.000 0.056 0.000 0.944 0.000 0.000
#&gt; SRR1539214     6  0.2039      0.799 0.016 0.000 0.000 0.004 0.072 0.908
#&gt; SRR1539213     3  0.0146      0.999 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539215     4  0.1204      1.000 0.000 0.056 0.000 0.944 0.000 0.000
#&gt; SRR1539216     3  0.0146      0.999 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539217     1  0.4882      0.619 0.684 0.000 0.004 0.028 0.052 0.232
#&gt; SRR1539218     4  0.1204      1.000 0.000 0.056 0.000 0.944 0.000 0.000
#&gt; SRR1539220     6  0.4540      0.562 0.016 0.000 0.040 0.004 0.244 0.696
#&gt; SRR1539219     3  0.0146      0.999 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539221     4  0.1204      1.000 0.000 0.056 0.000 0.944 0.000 0.000
#&gt; SRR1539223     1  0.5090      0.616 0.680 0.000 0.012 0.028 0.056 0.224
#&gt; SRR1539224     4  0.1204      1.000 0.000 0.056 0.000 0.944 0.000 0.000
#&gt; SRR1539222     3  0.0146      0.999 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539225     3  0.0146      0.999 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539227     4  0.1204      1.000 0.000 0.056 0.000 0.944 0.000 0.000
#&gt; SRR1539226     6  0.1411      0.818 0.004 0.000 0.000 0.000 0.060 0.936
#&gt; SRR1539228     3  0.0146      0.999 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539229     6  0.1349      0.819 0.004 0.000 0.000 0.000 0.056 0.940
#&gt; SRR1539232     3  0.0146      0.999 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539230     4  0.1204      1.000 0.000 0.056 0.000 0.944 0.000 0.000
#&gt; SRR1539231     4  0.1204      1.000 0.000 0.056 0.000 0.944 0.000 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539233     6  0.1285      0.820 0.004 0.000 0.000 0.000 0.052 0.944
#&gt; SRR1539235     5  0.1555      0.916 0.004 0.000 0.000 0.004 0.932 0.060
#&gt; SRR1539236     6  0.4128      0.700 0.164 0.000 0.000 0.028 0.044 0.764
#&gt; SRR1539237     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539238     5  0.0865      0.933 0.000 0.000 0.000 0.000 0.964 0.036
#&gt; SRR1539239     1  0.3688      0.589 0.724 0.000 0.000 0.020 0.000 0.256
#&gt; SRR1539242     1  0.3688      0.589 0.724 0.000 0.000 0.020 0.000 0.256
#&gt; SRR1539240     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539241     5  0.0865      0.933 0.000 0.000 0.000 0.000 0.964 0.036
#&gt; SRR1539243     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539244     5  0.1152      0.924 0.000 0.000 0.000 0.004 0.952 0.044
#&gt; SRR1539245     6  0.2844      0.784 0.112 0.000 0.000 0.012 0.020 0.856
#&gt; SRR1539246     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539247     5  0.0777      0.933 0.000 0.000 0.004 0.000 0.972 0.024
#&gt; SRR1539248     1  0.0790      0.732 0.968 0.000 0.000 0.000 0.000 0.032
#&gt; SRR1539249     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539250     5  0.3379      0.826 0.020 0.000 0.132 0.004 0.824 0.020
#&gt; SRR1539251     5  0.3379      0.826 0.020 0.000 0.132 0.004 0.824 0.020
#&gt; SRR1539253     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539252     6  0.2742      0.786 0.128 0.000 0.000 0.008 0.012 0.852
#&gt; SRR1539255     6  0.3757      0.717 0.180 0.000 0.000 0.024 0.020 0.776
#&gt; SRR1539254     5  0.0777      0.933 0.000 0.000 0.004 0.000 0.972 0.024
#&gt; SRR1539256     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.0935      0.930 0.000 0.000 0.004 0.000 0.964 0.032
#&gt; SRR1539258     1  0.3695      0.574 0.712 0.000 0.000 0.016 0.000 0.272
#&gt; SRR1539259     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539260     5  0.0777      0.933 0.000 0.000 0.004 0.000 0.972 0.024
#&gt; SRR1539262     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539261     1  0.0713      0.732 0.972 0.000 0.000 0.000 0.000 0.028
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.778           0.969       0.936         0.3118 0.836   0.699
#> 4 4 1.000           1.000       1.000         0.1323 0.942   0.846
#> 5 5 0.933           0.918       0.965         0.1815 0.858   0.572
#> 6 6 0.866           0.830       0.885         0.0468 0.939   0.705
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 4
```

There is also optional best $k$ = 2 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette  p1  p2  p3
#&gt; SRR1539207     3   0.455      1.000 0.2 0.0 0.8
#&gt; SRR1539208     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539211     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539210     3   0.455      1.000 0.2 0.0 0.8
#&gt; SRR1539209     2   0.455      0.904 0.0 0.8 0.2
#&gt; SRR1539212     2   0.455      0.904 0.0 0.8 0.2
#&gt; SRR1539214     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539213     3   0.455      1.000 0.2 0.0 0.8
#&gt; SRR1539215     2   0.455      0.904 0.0 0.8 0.2
#&gt; SRR1539216     3   0.455      1.000 0.2 0.0 0.8
#&gt; SRR1539217     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539218     2   0.455      0.904 0.0 0.8 0.2
#&gt; SRR1539220     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539219     3   0.455      1.000 0.2 0.0 0.8
#&gt; SRR1539221     2   0.455      0.904 0.0 0.8 0.2
#&gt; SRR1539223     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539224     2   0.455      0.904 0.0 0.8 0.2
#&gt; SRR1539222     3   0.455      1.000 0.2 0.0 0.8
#&gt; SRR1539225     3   0.455      1.000 0.2 0.0 0.8
#&gt; SRR1539227     2   0.455      0.904 0.0 0.8 0.2
#&gt; SRR1539226     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539228     3   0.455      1.000 0.2 0.0 0.8
#&gt; SRR1539229     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539232     3   0.455      1.000 0.2 0.0 0.8
#&gt; SRR1539230     2   0.455      0.904 0.0 0.8 0.2
#&gt; SRR1539231     2   0.455      0.904 0.0 0.8 0.2
#&gt; SRR1539234     2   0.000      0.914 0.0 1.0 0.0
#&gt; SRR1539233     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539235     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539236     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539237     2   0.000      0.914 0.0 1.0 0.0
#&gt; SRR1539238     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539239     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539242     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539240     2   0.000      0.914 0.0 1.0 0.0
#&gt; SRR1539241     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539243     2   0.000      0.914 0.0 1.0 0.0
#&gt; SRR1539244     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539245     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539246     2   0.000      0.914 0.0 1.0 0.0
#&gt; SRR1539247     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539248     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539249     2   0.000      0.914 0.0 1.0 0.0
#&gt; SRR1539250     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539251     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539253     2   0.000      0.914 0.0 1.0 0.0
#&gt; SRR1539252     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539255     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539254     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539256     2   0.000      0.914 0.0 1.0 0.0
#&gt; SRR1539257     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539258     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539259     2   0.000      0.914 0.0 1.0 0.0
#&gt; SRR1539260     1   0.000      1.000 1.0 0.0 0.0
#&gt; SRR1539262     2   0.000      0.914 0.0 1.0 0.0
#&gt; SRR1539261     1   0.000      1.000 1.0 0.0 0.0
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3 p4
#&gt; SRR1539207     3       0          1  0  0  1  0
#&gt; SRR1539208     1       0          1  1  0  0  0
#&gt; SRR1539211     1       0          1  1  0  0  0
#&gt; SRR1539210     3       0          1  0  0  1  0
#&gt; SRR1539209     4       0          1  0  0  0  1
#&gt; SRR1539212     4       0          1  0  0  0  1
#&gt; SRR1539214     1       0          1  1  0  0  0
#&gt; SRR1539213     3       0          1  0  0  1  0
#&gt; SRR1539215     4       0          1  0  0  0  1
#&gt; SRR1539216     3       0          1  0  0  1  0
#&gt; SRR1539217     1       0          1  1  0  0  0
#&gt; SRR1539218     4       0          1  0  0  0  1
#&gt; SRR1539220     1       0          1  1  0  0  0
#&gt; SRR1539219     3       0          1  0  0  1  0
#&gt; SRR1539221     4       0          1  0  0  0  1
#&gt; SRR1539223     1       0          1  1  0  0  0
#&gt; SRR1539224     4       0          1  0  0  0  1
#&gt; SRR1539222     3       0          1  0  0  1  0
#&gt; SRR1539225     3       0          1  0  0  1  0
#&gt; SRR1539227     4       0          1  0  0  0  1
#&gt; SRR1539226     1       0          1  1  0  0  0
#&gt; SRR1539228     3       0          1  0  0  1  0
#&gt; SRR1539229     1       0          1  1  0  0  0
#&gt; SRR1539232     3       0          1  0  0  1  0
#&gt; SRR1539230     4       0          1  0  0  0  1
#&gt; SRR1539231     4       0          1  0  0  0  1
#&gt; SRR1539234     2       0          1  0  1  0  0
#&gt; SRR1539233     1       0          1  1  0  0  0
#&gt; SRR1539235     1       0          1  1  0  0  0
#&gt; SRR1539236     1       0          1  1  0  0  0
#&gt; SRR1539237     2       0          1  0  1  0  0
#&gt; SRR1539238     1       0          1  1  0  0  0
#&gt; SRR1539239     1       0          1  1  0  0  0
#&gt; SRR1539242     1       0          1  1  0  0  0
#&gt; SRR1539240     2       0          1  0  1  0  0
#&gt; SRR1539241     1       0          1  1  0  0  0
#&gt; SRR1539243     2       0          1  0  1  0  0
#&gt; SRR1539244     1       0          1  1  0  0  0
#&gt; SRR1539245     1       0          1  1  0  0  0
#&gt; SRR1539246     2       0          1  0  1  0  0
#&gt; SRR1539247     1       0          1  1  0  0  0
#&gt; SRR1539248     1       0          1  1  0  0  0
#&gt; SRR1539249     2       0          1  0  1  0  0
#&gt; SRR1539250     1       0          1  1  0  0  0
#&gt; SRR1539251     1       0          1  1  0  0  0
#&gt; SRR1539253     2       0          1  0  1  0  0
#&gt; SRR1539252     1       0          1  1  0  0  0
#&gt; SRR1539255     1       0          1  1  0  0  0
#&gt; SRR1539254     1       0          1  1  0  0  0
#&gt; SRR1539256     2       0          1  0  1  0  0
#&gt; SRR1539257     1       0          1  1  0  0  0
#&gt; SRR1539258     1       0          1  1  0  0  0
#&gt; SRR1539259     2       0          1  0  1  0  0
#&gt; SRR1539260     1       0          1  1  0  0  0
#&gt; SRR1539262     2       0          1  0  1  0  0
#&gt; SRR1539261     1       0          1  1  0  0  0
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3 p4    p5
#&gt; SRR1539207     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539208     5   0.191      0.826 0.092  0 0.000  0 0.908
#&gt; SRR1539211     5   0.377      0.602 0.296  0 0.000  0 0.704
#&gt; SRR1539210     5   0.402      0.444 0.000  0 0.348  0 0.652
#&gt; SRR1539209     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539212     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539214     1   0.242      0.832 0.868  0 0.000  0 0.132
#&gt; SRR1539213     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539215     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539216     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539217     1   0.300      0.748 0.812  0 0.000  0 0.188
#&gt; SRR1539218     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539220     5   0.366      0.595 0.276  0 0.000  0 0.724
#&gt; SRR1539219     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539221     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539223     5   0.265      0.780 0.152  0 0.000  0 0.848
#&gt; SRR1539224     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539222     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539225     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539227     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539226     1   0.000      0.972 1.000  0 0.000  0 0.000
#&gt; SRR1539228     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539229     1   0.000      0.972 1.000  0 0.000  0 0.000
#&gt; SRR1539232     3   0.000      1.000 0.000  0 1.000  0 0.000
#&gt; SRR1539230     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539231     4   0.000      1.000 0.000  0 0.000  1 0.000
#&gt; SRR1539234     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539233     1   0.000      0.972 1.000  0 0.000  0 0.000
#&gt; SRR1539235     5   0.000      0.868 0.000  0 0.000  0 1.000
#&gt; SRR1539236     1   0.000      0.972 1.000  0 0.000  0 0.000
#&gt; SRR1539237     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539238     5   0.000      0.868 0.000  0 0.000  0 1.000
#&gt; SRR1539239     1   0.000      0.972 1.000  0 0.000  0 0.000
#&gt; SRR1539242     1   0.000      0.972 1.000  0 0.000  0 0.000
#&gt; SRR1539240     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539241     5   0.000      0.868 0.000  0 0.000  0 1.000
#&gt; SRR1539243     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539244     5   0.429      0.115 0.464  0 0.000  0 0.536
#&gt; SRR1539245     1   0.000      0.972 1.000  0 0.000  0 0.000
#&gt; SRR1539246     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539247     5   0.000      0.868 0.000  0 0.000  0 1.000
#&gt; SRR1539248     1   0.000      0.972 1.000  0 0.000  0 0.000
#&gt; SRR1539249     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539250     5   0.000      0.868 0.000  0 0.000  0 1.000
#&gt; SRR1539251     5   0.000      0.868 0.000  0 0.000  0 1.000
#&gt; SRR1539253     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539252     1   0.000      0.972 1.000  0 0.000  0 0.000
#&gt; SRR1539255     1   0.000      0.972 1.000  0 0.000  0 0.000
#&gt; SRR1539254     5   0.000      0.868 0.000  0 0.000  0 1.000
#&gt; SRR1539256     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539257     5   0.000      0.868 0.000  0 0.000  0 1.000
#&gt; SRR1539258     1   0.000      0.972 1.000  0 0.000  0 0.000
#&gt; SRR1539259     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539260     5   0.000      0.868 0.000  0 0.000  0 1.000
#&gt; SRR1539262     2   0.000      1.000 0.000  1 0.000  0 0.000
#&gt; SRR1539261     1   0.000      0.972 1.000  0 0.000  0 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.0000      1.000 0.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1539208     6  0.1524      0.627 0.008  0 0.000 0.000 0.060 0.932
#&gt; SRR1539211     6  0.3168      0.607 0.116  0 0.000 0.000 0.056 0.828
#&gt; SRR1539210     6  0.4737      0.498 0.000  0 0.192 0.000 0.132 0.676
#&gt; SRR1539209     4  0.0146      0.997 0.000  0 0.000 0.996 0.004 0.000
#&gt; SRR1539212     4  0.0146      0.997 0.000  0 0.000 0.996 0.004 0.000
#&gt; SRR1539214     1  0.4117      0.625 0.716  0 0.000 0.000 0.056 0.228
#&gt; SRR1539213     3  0.0000      1.000 0.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1539215     4  0.0146      0.997 0.000  0 0.000 0.996 0.004 0.000
#&gt; SRR1539216     3  0.0000      1.000 0.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1539217     6  0.1957      0.625 0.112  0 0.000 0.000 0.000 0.888
#&gt; SRR1539218     4  0.0000      0.997 0.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1539220     6  0.2812      0.595 0.048  0 0.000 0.000 0.096 0.856
#&gt; SRR1539219     3  0.0000      1.000 0.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1539221     4  0.0000      0.997 0.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1539223     6  0.1219      0.631 0.004  0 0.000 0.000 0.048 0.948
#&gt; SRR1539224     4  0.0146      0.997 0.000  0 0.000 0.996 0.004 0.000
#&gt; SRR1539222     3  0.0000      1.000 0.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1539225     3  0.0000      1.000 0.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1539227     4  0.0146      0.997 0.000  0 0.000 0.996 0.004 0.000
#&gt; SRR1539226     1  0.1204      0.863 0.944  0 0.000 0.000 0.000 0.056
#&gt; SRR1539228     3  0.0000      1.000 0.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1539229     1  0.1204      0.863 0.944  0 0.000 0.000 0.000 0.056
#&gt; SRR1539232     3  0.0000      1.000 0.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1539230     4  0.0146      0.997 0.000  0 0.000 0.996 0.004 0.000
#&gt; SRR1539231     4  0.0146      0.997 0.000  0 0.000 0.996 0.004 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539233     1  0.1204      0.863 0.944  0 0.000 0.000 0.000 0.056
#&gt; SRR1539235     5  0.0547      0.572 0.000  0 0.000 0.000 0.980 0.020
#&gt; SRR1539236     1  0.4118      0.618 0.660  0 0.000 0.000 0.312 0.028
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539238     5  0.3756      0.656 0.000  0 0.000 0.000 0.600 0.400
#&gt; SRR1539239     1  0.3416      0.773 0.804  0 0.000 0.000 0.056 0.140
#&gt; SRR1539242     1  0.1957      0.816 0.888  0 0.000 0.000 0.000 0.112
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539241     5  0.3789      0.674 0.000  0 0.000 0.000 0.584 0.416
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539244     5  0.4281      0.562 0.136  0 0.000 0.000 0.732 0.132
#&gt; SRR1539245     1  0.0146      0.868 0.996  0 0.000 0.000 0.000 0.004
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539247     5  0.3515      0.772 0.000  0 0.000 0.000 0.676 0.324
#&gt; SRR1539248     1  0.2996      0.697 0.772  0 0.000 0.000 0.000 0.228
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539250     6  0.3309      0.364 0.000  0 0.000 0.000 0.280 0.720
#&gt; SRR1539251     6  0.3515      0.240 0.000  0 0.000 0.000 0.324 0.676
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539252     1  0.0146      0.868 0.996  0 0.000 0.000 0.000 0.004
#&gt; SRR1539255     1  0.0260      0.868 0.992  0 0.000 0.000 0.000 0.008
#&gt; SRR1539254     5  0.3515      0.772 0.000  0 0.000 0.000 0.676 0.324
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.3515      0.772 0.000  0 0.000 0.000 0.676 0.324
#&gt; SRR1539258     1  0.0260      0.868 0.992  0 0.000 0.000 0.000 0.008
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539260     5  0.3515      0.772 0.000  0 0.000 0.000 0.676 0.324
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1539261     6  0.3765      0.210 0.404  0 0.000 0.000 0.000 0.596
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.772           0.933       0.874         0.2815 0.836   0.699
#> 4 4 1.000           0.974       0.990         0.1550 0.942   0.846
#> 5 5 0.779           0.686       0.855         0.1409 0.905   0.703
#> 6 6 0.827           0.752       0.852         0.0586 0.866   0.506
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1539207     3  0.7401      0.990 0.340 0.048 0.612
#&gt; SRR1539208     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539211     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539210     3  0.7401      0.990 0.340 0.048 0.612
#&gt; SRR1539209     2  0.0000      0.818 0.000 1.000 0.000
#&gt; SRR1539212     2  0.0000      0.818 0.000 1.000 0.000
#&gt; SRR1539214     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539213     3  0.7401      0.990 0.340 0.048 0.612
#&gt; SRR1539215     2  0.0000      0.818 0.000 1.000 0.000
#&gt; SRR1539216     3  0.7401      0.990 0.340 0.048 0.612
#&gt; SRR1539217     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539218     2  0.0000      0.818 0.000 1.000 0.000
#&gt; SRR1539220     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539219     3  0.7401      0.990 0.340 0.048 0.612
#&gt; SRR1539221     2  0.0000      0.818 0.000 1.000 0.000
#&gt; SRR1539223     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539224     2  0.0000      0.818 0.000 1.000 0.000
#&gt; SRR1539222     3  0.7401      0.990 0.340 0.048 0.612
#&gt; SRR1539225     3  0.7401      0.990 0.340 0.048 0.612
#&gt; SRR1539227     2  0.0000      0.818 0.000 1.000 0.000
#&gt; SRR1539226     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539228     3  0.7401      0.990 0.340 0.048 0.612
#&gt; SRR1539229     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539232     3  0.6079      0.912 0.388 0.000 0.612
#&gt; SRR1539230     2  0.0000      0.818 0.000 1.000 0.000
#&gt; SRR1539231     2  0.0000      0.818 0.000 1.000 0.000
#&gt; SRR1539234     2  0.4974      0.827 0.000 0.764 0.236
#&gt; SRR1539233     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539235     1  0.0424      0.988 0.992 0.000 0.008
#&gt; SRR1539236     1  0.0592      0.983 0.988 0.000 0.012
#&gt; SRR1539237     2  0.6079      0.826 0.000 0.612 0.388
#&gt; SRR1539238     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539239     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539242     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539240     2  0.6079      0.826 0.000 0.612 0.388
#&gt; SRR1539241     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539243     2  0.6079      0.826 0.000 0.612 0.388
#&gt; SRR1539244     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539245     1  0.0424      0.988 0.992 0.000 0.008
#&gt; SRR1539246     2  0.6079      0.826 0.000 0.612 0.388
#&gt; SRR1539247     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539248     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539249     2  0.6079      0.826 0.000 0.612 0.388
#&gt; SRR1539250     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539251     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539253     2  0.6079      0.826 0.000 0.612 0.388
#&gt; SRR1539252     1  0.0237      0.992 0.996 0.000 0.004
#&gt; SRR1539255     1  0.0592      0.983 0.988 0.000 0.012
#&gt; SRR1539254     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539256     2  0.6079      0.826 0.000 0.612 0.388
#&gt; SRR1539257     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539258     1  0.0424      0.988 0.992 0.000 0.008
#&gt; SRR1539259     2  0.6079      0.826 0.000 0.612 0.388
#&gt; SRR1539260     1  0.0000      0.996 1.000 0.000 0.000
#&gt; SRR1539262     2  0.6079      0.826 0.000 0.612 0.388
#&gt; SRR1539261     1  0.0747      0.977 0.984 0.000 0.016
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1   p2    p3    p4
#&gt; SRR1539207     3   0.000      0.966 0.000 0.00 1.000 0.000
#&gt; SRR1539208     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539211     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539210     3   0.000      0.966 0.000 0.00 1.000 0.000
#&gt; SRR1539209     4   0.000      1.000 0.000 0.00 0.000 1.000
#&gt; SRR1539212     4   0.000      1.000 0.000 0.00 0.000 1.000
#&gt; SRR1539214     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539213     3   0.000      0.966 0.000 0.00 1.000 0.000
#&gt; SRR1539215     4   0.000      1.000 0.000 0.00 0.000 1.000
#&gt; SRR1539216     3   0.000      0.966 0.000 0.00 1.000 0.000
#&gt; SRR1539217     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539218     4   0.000      1.000 0.000 0.00 0.000 1.000
#&gt; SRR1539220     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539219     3   0.000      0.966 0.000 0.00 1.000 0.000
#&gt; SRR1539221     4   0.000      1.000 0.000 0.00 0.000 1.000
#&gt; SRR1539223     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539224     4   0.000      1.000 0.000 0.00 0.000 1.000
#&gt; SRR1539222     3   0.000      0.966 0.000 0.00 1.000 0.000
#&gt; SRR1539225     3   0.000      0.966 0.000 0.00 1.000 0.000
#&gt; SRR1539227     4   0.000      1.000 0.000 0.00 0.000 1.000
#&gt; SRR1539226     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539228     3   0.000      0.966 0.000 0.00 1.000 0.000
#&gt; SRR1539229     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539232     3   0.349      0.716 0.188 0.00 0.812 0.000
#&gt; SRR1539230     4   0.000      1.000 0.000 0.00 0.000 1.000
#&gt; SRR1539231     4   0.000      1.000 0.000 0.00 0.000 1.000
#&gt; SRR1539234     2   0.519      0.456 0.000 0.64 0.016 0.344
#&gt; SRR1539233     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539235     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539236     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539237     2   0.000      0.960 0.000 1.00 0.000 0.000
#&gt; SRR1539238     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539239     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539242     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539240     2   0.000      0.960 0.000 1.00 0.000 0.000
#&gt; SRR1539241     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539243     2   0.000      0.960 0.000 1.00 0.000 0.000
#&gt; SRR1539244     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539245     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539246     2   0.000      0.960 0.000 1.00 0.000 0.000
#&gt; SRR1539247     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539248     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539249     2   0.000      0.960 0.000 1.00 0.000 0.000
#&gt; SRR1539250     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539251     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539253     2   0.000      0.960 0.000 1.00 0.000 0.000
#&gt; SRR1539252     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539255     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539254     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539256     2   0.000      0.960 0.000 1.00 0.000 0.000
#&gt; SRR1539257     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539258     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539259     2   0.000      0.960 0.000 1.00 0.000 0.000
#&gt; SRR1539260     1   0.000      1.000 1.000 0.00 0.000 0.000
#&gt; SRR1539262     2   0.000      0.960 0.000 1.00 0.000 0.000
#&gt; SRR1539261     1   0.000      1.000 1.000 0.00 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.0000    0.98939 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539208     5  0.1965    0.63995 0.096 0.000 0.000 0.000 0.904
#&gt; SRR1539211     5  0.3039    0.50301 0.192 0.000 0.000 0.000 0.808
#&gt; SRR1539210     3  0.0000    0.98939 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539209     4  0.3074    0.84048 0.000 0.196 0.000 0.804 0.000
#&gt; SRR1539212     4  0.3074    0.84048 0.000 0.196 0.000 0.804 0.000
#&gt; SRR1539214     5  0.0963    0.65969 0.036 0.000 0.000 0.000 0.964
#&gt; SRR1539213     3  0.0000    0.98939 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539215     4  0.0000    0.89029 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539216     3  0.0000    0.98939 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539217     5  0.0510    0.67300 0.016 0.000 0.000 0.000 0.984
#&gt; SRR1539218     4  0.3074    0.84048 0.000 0.196 0.000 0.804 0.000
#&gt; SRR1539220     5  0.1270    0.66105 0.052 0.000 0.000 0.000 0.948
#&gt; SRR1539219     3  0.0000    0.98939 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539221     4  0.0000    0.89029 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539223     5  0.1851    0.64347 0.088 0.000 0.000 0.000 0.912
#&gt; SRR1539224     4  0.3074    0.84048 0.000 0.196 0.000 0.804 0.000
#&gt; SRR1539222     3  0.0000    0.98939 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539225     3  0.0000    0.98939 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539227     4  0.0000    0.89029 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539226     5  0.2471    0.55526 0.136 0.000 0.000 0.000 0.864
#&gt; SRR1539228     3  0.0000    0.98939 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539229     5  0.3966    0.21060 0.336 0.000 0.000 0.000 0.664
#&gt; SRR1539232     3  0.1544    0.91288 0.000 0.000 0.932 0.000 0.068
#&gt; SRR1539230     4  0.0000    0.89029 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539231     4  0.0000    0.89029 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539234     2  0.3874    0.67938 0.008 0.776 0.016 0.200 0.000
#&gt; SRR1539233     5  0.4161    0.00966 0.392 0.000 0.000 0.000 0.608
#&gt; SRR1539235     5  0.4307   -0.39220 0.496 0.000 0.000 0.000 0.504
#&gt; SRR1539236     1  0.3730    0.68935 0.712 0.000 0.000 0.000 0.288
#&gt; SRR1539237     2  0.0000    0.97235 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539238     5  0.0162    0.67435 0.004 0.000 0.000 0.000 0.996
#&gt; SRR1539239     1  0.4305    0.51018 0.512 0.000 0.000 0.000 0.488
#&gt; SRR1539242     5  0.4306   -0.58903 0.492 0.000 0.000 0.000 0.508
#&gt; SRR1539240     2  0.0000    0.97235 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539241     5  0.0609    0.67330 0.020 0.000 0.000 0.000 0.980
#&gt; SRR1539243     2  0.0000    0.97235 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539244     5  0.3913    0.23479 0.324 0.000 0.000 0.000 0.676
#&gt; SRR1539245     1  0.3774    0.68737 0.704 0.000 0.000 0.000 0.296
#&gt; SRR1539246     2  0.0000    0.97235 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539247     5  0.0290    0.67301 0.008 0.000 0.000 0.000 0.992
#&gt; SRR1539248     1  0.4304    0.49647 0.516 0.000 0.000 0.000 0.484
#&gt; SRR1539249     2  0.0000    0.97235 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539250     5  0.3707    0.45478 0.284 0.000 0.000 0.000 0.716
#&gt; SRR1539251     5  0.3707    0.45478 0.284 0.000 0.000 0.000 0.716
#&gt; SRR1539253     2  0.0000    0.97235 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539252     1  0.4126    0.62087 0.620 0.000 0.000 0.000 0.380
#&gt; SRR1539255     1  0.3752    0.69184 0.708 0.000 0.000 0.000 0.292
#&gt; SRR1539254     5  0.1270    0.66250 0.052 0.000 0.000 0.000 0.948
#&gt; SRR1539256     2  0.0000    0.97235 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.0162    0.67435 0.004 0.000 0.000 0.000 0.996
#&gt; SRR1539258     5  0.4307   -0.58541 0.496 0.000 0.000 0.000 0.504
#&gt; SRR1539259     2  0.0000    0.97235 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539260     5  0.0162    0.67435 0.004 0.000 0.000 0.000 0.996
#&gt; SRR1539262     2  0.0000    0.97235 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539261     1  0.4302    0.49080 0.520 0.000 0.000 0.000 0.480
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.0000      0.976 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539208     1  0.2260      0.709 0.860 0.000 0.000 0.000 0.140 0.000
#&gt; SRR1539211     1  0.3558      0.589 0.760 0.000 0.000 0.000 0.212 0.028
#&gt; SRR1539210     3  0.0000      0.976 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539209     4  0.0725      0.986 0.000 0.012 0.000 0.976 0.000 0.012
#&gt; SRR1539212     4  0.0725      0.986 0.000 0.012 0.000 0.976 0.000 0.012
#&gt; SRR1539214     5  0.3747      0.607 0.396 0.000 0.000 0.000 0.604 0.000
#&gt; SRR1539213     3  0.0363      0.973 0.000 0.000 0.988 0.000 0.000 0.012
#&gt; SRR1539215     4  0.0000      0.989 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539216     3  0.0000      0.976 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539217     1  0.1327      0.690 0.936 0.000 0.000 0.000 0.064 0.000
#&gt; SRR1539218     4  0.0725      0.986 0.000 0.012 0.000 0.976 0.000 0.012
#&gt; SRR1539220     5  0.3774      0.602 0.408 0.000 0.000 0.000 0.592 0.000
#&gt; SRR1539219     3  0.0000      0.976 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539221     4  0.0000      0.989 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539223     1  0.1075      0.723 0.952 0.000 0.000 0.000 0.048 0.000
#&gt; SRR1539224     4  0.0725      0.986 0.000 0.012 0.000 0.976 0.000 0.012
#&gt; SRR1539222     3  0.0000      0.976 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539225     3  0.0363      0.973 0.000 0.000 0.988 0.000 0.000 0.012
#&gt; SRR1539227     4  0.0000      0.989 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539226     5  0.5723      0.350 0.408 0.000 0.000 0.000 0.428 0.164
#&gt; SRR1539228     3  0.0363      0.973 0.000 0.000 0.988 0.000 0.000 0.012
#&gt; SRR1539229     6  0.4764      0.673 0.384 0.000 0.000 0.000 0.056 0.560
#&gt; SRR1539232     3  0.2844      0.813 0.104 0.000 0.860 0.000 0.016 0.020
#&gt; SRR1539230     4  0.0000      0.989 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539231     4  0.0000      0.989 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539234     2  0.3912      0.652 0.000 0.648 0.000 0.012 0.000 0.340
#&gt; SRR1539233     6  0.4766      0.675 0.316 0.000 0.000 0.000 0.072 0.612
#&gt; SRR1539235     5  0.3655      0.514 0.112 0.000 0.000 0.000 0.792 0.096
#&gt; SRR1539236     5  0.3998     -0.368 0.004 0.000 0.000 0.000 0.504 0.492
#&gt; SRR1539237     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539238     5  0.3881      0.603 0.396 0.000 0.000 0.000 0.600 0.004
#&gt; SRR1539239     5  0.1176      0.519 0.020 0.000 0.000 0.000 0.956 0.024
#&gt; SRR1539242     5  0.1320      0.519 0.016 0.000 0.000 0.000 0.948 0.036
#&gt; SRR1539240     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539241     5  0.3747      0.607 0.396 0.000 0.000 0.000 0.604 0.000
#&gt; SRR1539243     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539244     6  0.4720      0.668 0.388 0.000 0.000 0.000 0.052 0.560
#&gt; SRR1539245     6  0.4064      0.365 0.016 0.000 0.000 0.000 0.360 0.624
#&gt; SRR1539246     2  0.0363      0.960 0.000 0.988 0.000 0.000 0.000 0.012
#&gt; SRR1539247     5  0.3923      0.596 0.416 0.000 0.000 0.000 0.580 0.004
#&gt; SRR1539248     5  0.1700      0.518 0.048 0.000 0.000 0.000 0.928 0.024
#&gt; SRR1539249     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539250     1  0.0993      0.730 0.964 0.000 0.000 0.000 0.024 0.012
#&gt; SRR1539251     1  0.0993      0.730 0.964 0.000 0.000 0.000 0.024 0.012
#&gt; SRR1539253     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539252     5  0.2448      0.543 0.064 0.000 0.000 0.000 0.884 0.052
#&gt; SRR1539255     5  0.2362      0.440 0.004 0.000 0.000 0.000 0.860 0.136
#&gt; SRR1539254     5  0.3789      0.598 0.416 0.000 0.000 0.000 0.584 0.000
#&gt; SRR1539256     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.3747      0.607 0.396 0.000 0.000 0.000 0.604 0.000
#&gt; SRR1539258     5  0.0790      0.533 0.032 0.000 0.000 0.000 0.968 0.000
#&gt; SRR1539259     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539260     5  0.3782      0.598 0.412 0.000 0.000 0.000 0.588 0.000
#&gt; SRR1539262     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539261     1  0.4066      0.322 0.596 0.000 0.000 0.000 0.392 0.012
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.995       0.998         0.4583 0.544   0.544
#> 3 3 0.664           0.844       0.817         0.3811 0.791   0.615
#> 4 4 0.791           0.771       0.827         0.1442 0.864   0.637
#> 5 5 0.778           0.691       0.854         0.0922 0.920   0.721
#> 6 6 0.888           0.777       0.916         0.0430 0.897   0.592
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1539207     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539208     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539211     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539210     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539209     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539212     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539214     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539213     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539215     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539216     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539217     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539218     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539220     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539219     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539221     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539223     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539224     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539222     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539225     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539227     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539226     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539228     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539229     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539232     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539230     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539231     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539234     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539233     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539235     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539236     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539237     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539238     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539239     1  0.0376      0.993 0.996 0.004
#&gt; SRR1539242     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539241     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539243     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539244     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539245     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539246     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539247     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539248     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539250     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539251     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539253     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539252     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539255     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539254     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539256     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539257     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539258     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539260     1  0.0000      0.997 1.000 0.000
#&gt; SRR1539262     2  0.0000      1.000 0.000 1.000
#&gt; SRR1539261     1  0.5178      0.869 0.884 0.116
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1539207     3   0.000     0.8818 0.000 0.000 1.000
#&gt; SRR1539208     1   0.493     0.9336 0.768 0.000 0.232
#&gt; SRR1539211     1   0.493     0.9336 0.768 0.000 0.232
#&gt; SRR1539210     3   0.000     0.8818 0.000 0.000 1.000
#&gt; SRR1539209     2   0.000     0.8950 0.000 1.000 0.000
#&gt; SRR1539212     2   0.000     0.8950 0.000 1.000 0.000
#&gt; SRR1539214     1   0.497     0.9311 0.764 0.000 0.236
#&gt; SRR1539213     3   0.000     0.8818 0.000 0.000 1.000
#&gt; SRR1539215     2   0.000     0.8950 0.000 1.000 0.000
#&gt; SRR1539216     3   0.000     0.8818 0.000 0.000 1.000
#&gt; SRR1539217     1   0.506     0.9242 0.756 0.000 0.244
#&gt; SRR1539218     2   0.000     0.8950 0.000 1.000 0.000
#&gt; SRR1539220     3   0.617    -0.0855 0.412 0.000 0.588
#&gt; SRR1539219     3   0.000     0.8818 0.000 0.000 1.000
#&gt; SRR1539221     2   0.000     0.8950 0.000 1.000 0.000
#&gt; SRR1539223     3   0.484     0.5682 0.224 0.000 0.776
#&gt; SRR1539224     2   0.000     0.8950 0.000 1.000 0.000
#&gt; SRR1539222     3   0.000     0.8818 0.000 0.000 1.000
#&gt; SRR1539225     3   0.000     0.8818 0.000 0.000 1.000
#&gt; SRR1539227     2   0.000     0.8950 0.000 1.000 0.000
#&gt; SRR1539226     1   0.493     0.9336 0.768 0.000 0.232
#&gt; SRR1539228     3   0.000     0.8818 0.000 0.000 1.000
#&gt; SRR1539229     1   0.493     0.9336 0.768 0.000 0.232
#&gt; SRR1539232     3   0.000     0.8818 0.000 0.000 1.000
#&gt; SRR1539230     2   0.000     0.8950 0.000 1.000 0.000
#&gt; SRR1539231     2   0.000     0.8950 0.000 1.000 0.000
#&gt; SRR1539234     2   0.440     0.9023 0.188 0.812 0.000
#&gt; SRR1539233     1   0.493     0.9336 0.768 0.000 0.232
#&gt; SRR1539235     1   0.493     0.9336 0.768 0.000 0.232
#&gt; SRR1539236     1   0.493     0.9336 0.768 0.000 0.232
#&gt; SRR1539237     2   0.493     0.9020 0.232 0.768 0.000
#&gt; SRR1539238     1   0.497     0.9311 0.764 0.000 0.236
#&gt; SRR1539239     1   0.312     0.8097 0.892 0.000 0.108
#&gt; SRR1539242     1   0.319     0.8148 0.888 0.000 0.112
#&gt; SRR1539240     2   0.493     0.9020 0.232 0.768 0.000
#&gt; SRR1539241     1   0.506     0.9240 0.756 0.000 0.244
#&gt; SRR1539243     2   0.493     0.9020 0.232 0.768 0.000
#&gt; SRR1539244     1   0.493     0.9336 0.768 0.000 0.232
#&gt; SRR1539245     1   0.493     0.9336 0.768 0.000 0.232
#&gt; SRR1539246     2   0.493     0.9020 0.232 0.768 0.000
#&gt; SRR1539247     3   0.630    -0.3713 0.484 0.000 0.516
#&gt; SRR1539248     1   0.348     0.8337 0.872 0.000 0.128
#&gt; SRR1539249     2   0.493     0.9020 0.232 0.768 0.000
#&gt; SRR1539250     3   0.000     0.8818 0.000 0.000 1.000
#&gt; SRR1539251     3   0.000     0.8818 0.000 0.000 1.000
#&gt; SRR1539253     2   0.493     0.9020 0.232 0.768 0.000
#&gt; SRR1539252     1   0.493     0.9336 0.768 0.000 0.232
#&gt; SRR1539255     1   0.489     0.9312 0.772 0.000 0.228
#&gt; SRR1539254     1   0.493     0.9336 0.768 0.000 0.232
#&gt; SRR1539256     2   0.493     0.9020 0.232 0.768 0.000
#&gt; SRR1539257     1   0.628     0.4919 0.540 0.000 0.460
#&gt; SRR1539258     1   0.418     0.8804 0.828 0.000 0.172
#&gt; SRR1539259     2   0.493     0.9020 0.232 0.768 0.000
#&gt; SRR1539260     1   0.546     0.8694 0.712 0.000 0.288
#&gt; SRR1539262     2   0.493     0.9020 0.232 0.768 0.000
#&gt; SRR1539261     1   0.207     0.7413 0.940 0.000 0.060
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3  0.0000      0.980 0.000 0.000 1.000 0.000
#&gt; SRR1539208     1  0.0921      0.836 0.972 0.000 0.000 0.028
#&gt; SRR1539211     1  0.0336      0.839 0.992 0.000 0.000 0.008
#&gt; SRR1539210     3  0.0188      0.977 0.000 0.000 0.996 0.004
#&gt; SRR1539209     4  0.4992      0.600 0.000 0.476 0.000 0.524
#&gt; SRR1539212     4  0.4992      0.600 0.000 0.476 0.000 0.524
#&gt; SRR1539214     1  0.4855      0.705 0.644 0.000 0.004 0.352
#&gt; SRR1539213     3  0.0000      0.980 0.000 0.000 1.000 0.000
#&gt; SRR1539215     4  0.4992      0.600 0.000 0.476 0.000 0.524
#&gt; SRR1539216     3  0.0000      0.980 0.000 0.000 1.000 0.000
#&gt; SRR1539217     1  0.0707      0.837 0.980 0.000 0.020 0.000
#&gt; SRR1539218     4  0.4992      0.600 0.000 0.476 0.000 0.524
#&gt; SRR1539220     1  0.7716      0.426 0.396 0.000 0.224 0.380
#&gt; SRR1539219     3  0.0000      0.980 0.000 0.000 1.000 0.000
#&gt; SRR1539221     4  0.4992      0.600 0.000 0.476 0.000 0.524
#&gt; SRR1539223     1  0.4790      0.423 0.620 0.000 0.380 0.000
#&gt; SRR1539224     4  0.4992      0.600 0.000 0.476 0.000 0.524
#&gt; SRR1539222     3  0.0000      0.980 0.000 0.000 1.000 0.000
#&gt; SRR1539225     3  0.0000      0.980 0.000 0.000 1.000 0.000
#&gt; SRR1539227     4  0.4992      0.600 0.000 0.476 0.000 0.524
#&gt; SRR1539226     1  0.4843      0.675 0.604 0.000 0.000 0.396
#&gt; SRR1539228     3  0.0000      0.980 0.000 0.000 1.000 0.000
#&gt; SRR1539229     1  0.4941      0.644 0.564 0.000 0.000 0.436
#&gt; SRR1539232     3  0.3610      0.790 0.000 0.000 0.800 0.200
#&gt; SRR1539230     4  0.4992      0.600 0.000 0.476 0.000 0.524
#&gt; SRR1539231     4  0.4992      0.600 0.000 0.476 0.000 0.524
#&gt; SRR1539234     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539233     1  0.4134      0.763 0.740 0.000 0.000 0.260
#&gt; SRR1539235     1  0.2530      0.833 0.888 0.000 0.000 0.112
#&gt; SRR1539236     1  0.0817      0.837 0.976 0.000 0.000 0.024
#&gt; SRR1539237     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539238     1  0.2530      0.833 0.888 0.000 0.000 0.112
#&gt; SRR1539239     1  0.0000      0.840 1.000 0.000 0.000 0.000
#&gt; SRR1539242     1  0.0000      0.840 1.000 0.000 0.000 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539241     1  0.2469      0.833 0.892 0.000 0.000 0.108
#&gt; SRR1539243     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539244     1  0.4989      0.627 0.528 0.000 0.000 0.472
#&gt; SRR1539245     1  0.4961      0.633 0.552 0.000 0.000 0.448
#&gt; SRR1539246     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539247     4  0.7400     -0.492 0.360 0.000 0.172 0.468
#&gt; SRR1539248     1  0.0000      0.840 1.000 0.000 0.000 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539250     3  0.0000      0.980 0.000 0.000 1.000 0.000
#&gt; SRR1539251     3  0.0188      0.977 0.000 0.000 0.996 0.004
#&gt; SRR1539253     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539252     1  0.0000      0.840 1.000 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0000      0.840 1.000 0.000 0.000 0.000
#&gt; SRR1539254     1  0.2408      0.834 0.896 0.000 0.000 0.104
#&gt; SRR1539256     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539257     4  0.6965     -0.566 0.428 0.000 0.112 0.460
#&gt; SRR1539258     1  0.0000      0.840 1.000 0.000 0.000 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539260     1  0.3545      0.815 0.828 0.000 0.008 0.164
#&gt; SRR1539262     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; SRR1539261     1  0.0000      0.840 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.0162     0.9598 0.000 0.004 0.996 0.000 0.000
#&gt; SRR1539208     1  0.4251     0.3709 0.672 0.012 0.000 0.000 0.316
#&gt; SRR1539211     1  0.1478     0.6310 0.936 0.000 0.000 0.000 0.064
#&gt; SRR1539210     3  0.0865     0.9496 0.000 0.004 0.972 0.000 0.024
#&gt; SRR1539209     4  0.0000     1.0000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539212     4  0.0000     1.0000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539214     1  0.5180     0.2351 0.644 0.060 0.004 0.000 0.292
#&gt; SRR1539213     3  0.0000     0.9608 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539215     4  0.0000     1.0000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539216     3  0.0000     0.9608 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539217     1  0.0162     0.6563 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1539218     4  0.0000     1.0000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539220     1  0.7895    -0.1824 0.392 0.092 0.196 0.000 0.320
#&gt; SRR1539219     3  0.0000     0.9608 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539221     4  0.0000     1.0000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539223     1  0.3098     0.5378 0.836 0.000 0.148 0.000 0.016
#&gt; SRR1539224     4  0.0000     1.0000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539222     3  0.0162     0.9598 0.000 0.004 0.996 0.000 0.000
#&gt; SRR1539225     3  0.0000     0.9608 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539227     4  0.0000     1.0000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539226     1  0.5357    -0.0985 0.520 0.044 0.004 0.000 0.432
#&gt; SRR1539228     3  0.0000     0.9608 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539229     5  0.5786     0.2463 0.380 0.096 0.000 0.000 0.524
#&gt; SRR1539232     3  0.3865     0.7768 0.000 0.092 0.808 0.000 0.100
#&gt; SRR1539230     4  0.0000     1.0000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539231     4  0.0000     1.0000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539234     2  0.2561     0.9983 0.000 0.856 0.000 0.144 0.000
#&gt; SRR1539233     1  0.4854     0.2422 0.648 0.044 0.000 0.000 0.308
#&gt; SRR1539235     5  0.4738    -0.1528 0.464 0.016 0.000 0.000 0.520
#&gt; SRR1539236     1  0.4371     0.2963 0.644 0.012 0.000 0.000 0.344
#&gt; SRR1539237     2  0.2471     0.9907 0.000 0.864 0.000 0.136 0.000
#&gt; SRR1539238     1  0.4826     0.0874 0.508 0.020 0.000 0.000 0.472
#&gt; SRR1539239     1  0.0000     0.6574 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539242     1  0.0000     0.6574 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539240     2  0.2561     0.9983 0.000 0.856 0.000 0.144 0.000
#&gt; SRR1539241     1  0.4658     0.0746 0.504 0.012 0.000 0.000 0.484
#&gt; SRR1539243     2  0.2561     0.9983 0.000 0.856 0.000 0.144 0.000
#&gt; SRR1539244     5  0.1981     0.5500 0.048 0.028 0.000 0.000 0.924
#&gt; SRR1539245     5  0.5534     0.3441 0.300 0.096 0.000 0.000 0.604
#&gt; SRR1539246     2  0.2561     0.9983 0.000 0.856 0.000 0.144 0.000
#&gt; SRR1539247     5  0.1399     0.5442 0.020 0.000 0.028 0.000 0.952
#&gt; SRR1539248     1  0.0000     0.6574 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539249     2  0.2561     0.9983 0.000 0.856 0.000 0.144 0.000
#&gt; SRR1539250     3  0.1478     0.9236 0.000 0.000 0.936 0.000 0.064
#&gt; SRR1539251     3  0.1965     0.8927 0.000 0.000 0.904 0.000 0.096
#&gt; SRR1539253     2  0.2561     0.9983 0.000 0.856 0.000 0.144 0.000
#&gt; SRR1539252     1  0.0000     0.6574 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0000     0.6574 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539254     1  0.4655     0.0629 0.512 0.012 0.000 0.000 0.476
#&gt; SRR1539256     2  0.2516     0.9949 0.000 0.860 0.000 0.140 0.000
#&gt; SRR1539257     5  0.3706     0.5370 0.020 0.076 0.064 0.000 0.840
#&gt; SRR1539258     1  0.0000     0.6574 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539259     2  0.2561     0.9983 0.000 0.856 0.000 0.144 0.000
#&gt; SRR1539260     5  0.4905    -0.1208 0.464 0.008 0.012 0.000 0.516
#&gt; SRR1539262     2  0.2561     0.9983 0.000 0.856 0.000 0.144 0.000
#&gt; SRR1539261     1  0.0510     0.6497 0.984 0.000 0.000 0.000 0.016
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3 p4    p5    p6
#&gt; SRR1539207     3  0.0146      0.961 0.000  0 0.996  0 0.000 0.004
#&gt; SRR1539208     5  0.4756      0.591 0.128  0 0.000  0 0.672 0.200
#&gt; SRR1539211     1  0.3388      0.414 0.792  0 0.000  0 0.036 0.172
#&gt; SRR1539210     3  0.0291      0.958 0.000  0 0.992  0 0.004 0.004
#&gt; SRR1539209     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539212     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539214     1  0.2191      0.630 0.876  0 0.004  0 0.000 0.120
#&gt; SRR1539213     3  0.0000      0.962 0.000  0 1.000  0 0.000 0.000
#&gt; SRR1539215     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539216     3  0.0000      0.962 0.000  0 1.000  0 0.000 0.000
#&gt; SRR1539217     1  0.0000      0.783 1.000  0 0.000  0 0.000 0.000
#&gt; SRR1539218     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539220     1  0.5933     -0.379 0.460  0 0.288  0 0.000 0.252
#&gt; SRR1539219     3  0.0000      0.962 0.000  0 1.000  0 0.000 0.000
#&gt; SRR1539221     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539223     1  0.0865      0.742 0.964  0 0.036  0 0.000 0.000
#&gt; SRR1539224     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539222     3  0.0146      0.961 0.000  0 0.996  0 0.000 0.004
#&gt; SRR1539225     3  0.0000      0.962 0.000  0 1.000  0 0.000 0.000
#&gt; SRR1539227     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539226     1  0.1814      0.667 0.900  0 0.000  0 0.000 0.100
#&gt; SRR1539228     3  0.0000      0.962 0.000  0 1.000  0 0.000 0.000
#&gt; SRR1539229     1  0.3727     -0.477 0.612  0 0.000  0 0.000 0.388
#&gt; SRR1539232     3  0.3428      0.633 0.000  0 0.696  0 0.000 0.304
#&gt; SRR1539230     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539231     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539233     1  0.2320      0.602 0.864  0 0.000  0 0.004 0.132
#&gt; SRR1539235     5  0.2092      0.753 0.000  0 0.000  0 0.876 0.124
#&gt; SRR1539236     5  0.4614      0.615 0.108  0 0.000  0 0.684 0.208
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539238     5  0.2378      0.742 0.000  0 0.000  0 0.848 0.152
#&gt; SRR1539239     1  0.0000      0.783 1.000  0 0.000  0 0.000 0.000
#&gt; SRR1539242     1  0.0000      0.783 1.000  0 0.000  0 0.000 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539241     5  0.0632      0.773 0.000  0 0.000  0 0.976 0.024
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539244     5  0.3499      0.602 0.000  0 0.000  0 0.680 0.320
#&gt; SRR1539245     6  0.4080      0.000 0.456  0 0.000  0 0.008 0.536
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539247     5  0.0260      0.772 0.000  0 0.000  0 0.992 0.008
#&gt; SRR1539248     1  0.0000      0.783 1.000  0 0.000  0 0.000 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539250     5  0.3937      0.319 0.000  0 0.424  0 0.572 0.004
#&gt; SRR1539251     5  0.3872      0.390 0.000  0 0.392  0 0.604 0.004
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539252     1  0.0000      0.783 1.000  0 0.000  0 0.000 0.000
#&gt; SRR1539255     1  0.0000      0.783 1.000  0 0.000  0 0.000 0.000
#&gt; SRR1539254     5  0.0000      0.773 0.000  0 0.000  0 1.000 0.000
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539257     5  0.3371      0.623 0.000  0 0.000  0 0.708 0.292
#&gt; SRR1539258     1  0.0000      0.783 1.000  0 0.000  0 0.000 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539260     5  0.0146      0.773 0.000  0 0.000  0 0.996 0.004
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; SRR1539261     1  0.0000      0.783 1.000  0 0.000  0 0.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.825           0.934       0.964         0.1872 0.934   0.878
#> 4 4 1.000           0.992       0.996         0.2115 0.865   0.717
#> 5 5 0.931           0.912       0.946         0.0627 0.955   0.867
#> 6 6 0.955           0.866       0.951         0.0174 0.999   0.996
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 4
```

There is also optional best $k$ = 2 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette  p1 p2  p3
#&gt; SRR1539207     1   0.455      0.791 0.8  0 0.2
#&gt; SRR1539208     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539211     3   0.455      0.872 0.2  0 0.8
#&gt; SRR1539210     3   0.000      0.738 0.0  0 1.0
#&gt; SRR1539209     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539212     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539214     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539213     1   0.455      0.791 0.8  0 0.2
#&gt; SRR1539215     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539216     1   0.455      0.791 0.8  0 0.2
#&gt; SRR1539217     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539218     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539220     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539219     1   0.455      0.791 0.8  0 0.2
#&gt; SRR1539221     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539223     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539224     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539222     1   0.455      0.791 0.8  0 0.2
#&gt; SRR1539225     1   0.455      0.791 0.8  0 0.2
#&gt; SRR1539227     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539226     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539228     1   0.455      0.791 0.8  0 0.2
#&gt; SRR1539229     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539232     1   0.455      0.791 0.8  0 0.2
#&gt; SRR1539230     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539231     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539234     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539233     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539235     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539236     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539237     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539238     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539239     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539242     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539240     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539241     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539243     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539244     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539245     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539246     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539247     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539248     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539249     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539250     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539251     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539253     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539252     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539255     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539254     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539256     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539257     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539258     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539259     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539260     1   0.000      0.942 1.0  0 0.0
#&gt; SRR1539262     2   0.000      1.000 0.0  1 0.0
#&gt; SRR1539261     3   0.455      0.872 0.2  0 0.8
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2  p3  p4
#&gt; SRR1539207     3   0.000      1.000  0  0 1.0 0.0
#&gt; SRR1539208     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539211     4   0.000      0.897  0  0 0.0 1.0
#&gt; SRR1539210     4   0.361      0.750  0  0 0.2 0.8
#&gt; SRR1539209     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539212     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539214     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539213     3   0.000      1.000  0  0 1.0 0.0
#&gt; SRR1539215     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539216     3   0.000      1.000  0  0 1.0 0.0
#&gt; SRR1539217     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539218     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539220     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539219     3   0.000      1.000  0  0 1.0 0.0
#&gt; SRR1539221     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539223     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539224     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539222     3   0.000      1.000  0  0 1.0 0.0
#&gt; SRR1539225     3   0.000      1.000  0  0 1.0 0.0
#&gt; SRR1539227     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539226     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539228     3   0.000      1.000  0  0 1.0 0.0
#&gt; SRR1539229     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539232     3   0.000      1.000  0  0 1.0 0.0
#&gt; SRR1539230     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539231     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539234     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539233     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539235     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539236     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539237     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539238     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539239     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539242     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539240     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539241     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539243     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539244     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539245     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539246     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539247     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539248     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539249     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539250     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539251     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539253     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539252     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539255     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539254     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539256     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539257     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539258     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539259     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539260     1   0.000      1.000  1  0 0.0 0.0
#&gt; SRR1539262     2   0.000      1.000  0  1 0.0 0.0
#&gt; SRR1539261     4   0.000      0.897  0  0 0.0 1.0
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4  p5
#&gt; SRR1539207     3  0.0404      0.994  0 0.000 0.988 0.012 0.0
#&gt; SRR1539208     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539211     5  0.0000      0.941  0 0.000 0.000 0.000 1.0
#&gt; SRR1539210     5  0.3109      0.879  0 0.000 0.000 0.200 0.8
#&gt; SRR1539209     4  0.3109      0.881  0 0.200 0.000 0.800 0.0
#&gt; SRR1539212     4  0.3109      0.881  0 0.200 0.000 0.800 0.0
#&gt; SRR1539214     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539213     3  0.0000      0.994  0 0.000 1.000 0.000 0.0
#&gt; SRR1539215     2  0.3752      0.599  0 0.708 0.000 0.292 0.0
#&gt; SRR1539216     3  0.0404      0.994  0 0.000 0.988 0.012 0.0
#&gt; SRR1539217     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539218     4  0.3109      0.881  0 0.200 0.000 0.800 0.0
#&gt; SRR1539220     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539219     3  0.0404      0.994  0 0.000 0.988 0.012 0.0
#&gt; SRR1539221     2  0.3752      0.599  0 0.708 0.000 0.292 0.0
#&gt; SRR1539223     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539224     4  0.3109      0.881  0 0.200 0.000 0.800 0.0
#&gt; SRR1539222     3  0.0404      0.994  0 0.000 0.988 0.012 0.0
#&gt; SRR1539225     3  0.0000      0.994  0 0.000 1.000 0.000 0.0
#&gt; SRR1539227     2  0.3752      0.599  0 0.708 0.000 0.292 0.0
#&gt; SRR1539226     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539228     3  0.0000      0.994  0 0.000 1.000 0.000 0.0
#&gt; SRR1539229     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539232     3  0.0000      0.994  0 0.000 1.000 0.000 0.0
#&gt; SRR1539230     2  0.3752      0.599  0 0.708 0.000 0.292 0.0
#&gt; SRR1539231     2  0.3752      0.599  0 0.708 0.000 0.292 0.0
#&gt; SRR1539234     2  0.0162      0.822  0 0.996 0.000 0.004 0.0
#&gt; SRR1539233     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539235     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539236     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539237     2  0.0000      0.824  0 1.000 0.000 0.000 0.0
#&gt; SRR1539238     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539239     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539242     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539240     2  0.0000      0.824  0 1.000 0.000 0.000 0.0
#&gt; SRR1539241     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539243     2  0.0000      0.824  0 1.000 0.000 0.000 0.0
#&gt; SRR1539244     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539245     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539246     2  0.0000      0.824  0 1.000 0.000 0.000 0.0
#&gt; SRR1539247     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539248     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539249     2  0.0000      0.824  0 1.000 0.000 0.000 0.0
#&gt; SRR1539250     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539251     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539253     2  0.0000      0.824  0 1.000 0.000 0.000 0.0
#&gt; SRR1539252     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539255     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539254     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539256     2  0.0000      0.824  0 1.000 0.000 0.000 0.0
#&gt; SRR1539257     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539258     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539259     2  0.0000      0.824  0 1.000 0.000 0.000 0.0
#&gt; SRR1539260     1  0.0000      1.000  1 0.000 0.000 0.000 0.0
#&gt; SRR1539262     4  0.4306      0.416  0 0.492 0.000 0.508 0.0
#&gt; SRR1539261     5  0.0000      0.941  0 0.000 0.000 0.000 1.0
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4 p5    p6
#&gt; SRR1539207     3  0.0363      0.993  0 0.000 0.988 0.000  0 0.012
#&gt; SRR1539208     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539211     5  0.0000      1.000  0 0.000 0.000 0.000  1 0.000
#&gt; SRR1539210     6  0.0000      0.000  0 0.000 0.000 0.000  0 1.000
#&gt; SRR1539209     4  0.0000      0.806  0 0.000 0.000 1.000  0 0.000
#&gt; SRR1539212     4  0.0000      0.806  0 0.000 0.000 1.000  0 0.000
#&gt; SRR1539214     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539213     3  0.0000      0.993  0 0.000 1.000 0.000  0 0.000
#&gt; SRR1539215     2  0.3838      0.446  0 0.552 0.000 0.448  0 0.000
#&gt; SRR1539216     3  0.0363      0.993  0 0.000 0.988 0.000  0 0.012
#&gt; SRR1539217     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539218     4  0.0000      0.806  0 0.000 0.000 1.000  0 0.000
#&gt; SRR1539220     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539219     3  0.0363      0.993  0 0.000 0.988 0.000  0 0.012
#&gt; SRR1539221     2  0.3838      0.446  0 0.552 0.000 0.448  0 0.000
#&gt; SRR1539223     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539224     4  0.0000      0.806  0 0.000 0.000 1.000  0 0.000
#&gt; SRR1539222     3  0.0363      0.993  0 0.000 0.988 0.000  0 0.012
#&gt; SRR1539225     3  0.0000      0.993  0 0.000 1.000 0.000  0 0.000
#&gt; SRR1539227     2  0.3838      0.446  0 0.552 0.000 0.448  0 0.000
#&gt; SRR1539226     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539228     3  0.0000      0.993  0 0.000 1.000 0.000  0 0.000
#&gt; SRR1539229     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539232     3  0.0000      0.993  0 0.000 1.000 0.000  0 0.000
#&gt; SRR1539230     2  0.3838      0.446  0 0.552 0.000 0.448  0 0.000
#&gt; SRR1539231     2  0.3838      0.446  0 0.552 0.000 0.448  0 0.000
#&gt; SRR1539234     2  0.0458      0.769  0 0.984 0.000 0.016  0 0.000
#&gt; SRR1539233     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539235     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539236     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539237     2  0.0000      0.776  0 1.000 0.000 0.000  0 0.000
#&gt; SRR1539238     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539239     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539242     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539240     2  0.0000      0.776  0 1.000 0.000 0.000  0 0.000
#&gt; SRR1539241     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539243     2  0.0000      0.776  0 1.000 0.000 0.000  0 0.000
#&gt; SRR1539244     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539245     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539246     2  0.0000      0.776  0 1.000 0.000 0.000  0 0.000
#&gt; SRR1539247     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539248     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539249     2  0.0000      0.776  0 1.000 0.000 0.000  0 0.000
#&gt; SRR1539250     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539251     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539253     2  0.0000      0.776  0 1.000 0.000 0.000  0 0.000
#&gt; SRR1539252     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539255     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539254     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539256     2  0.0000      0.776  0 1.000 0.000 0.000  0 0.000
#&gt; SRR1539257     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539258     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539259     2  0.0000      0.776  0 1.000 0.000 0.000  0 0.000
#&gt; SRR1539260     1  0.0000      1.000  1 0.000 0.000 0.000  0 0.000
#&gt; SRR1539262     4  0.3838      0.131  0 0.448 0.000 0.552  0 0.000
#&gt; SRR1539261     5  0.0000      1.000  0 0.000 0.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.661           0.756       0.754         0.3491 1.000   1.000
#> 4 4 0.718           0.418       0.679         0.1491 0.774   0.584
#> 5 5 0.692           0.879       0.785         0.0829 0.814   0.475
#> 6 6 0.782           0.846       0.798         0.0499 0.964   0.822
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1   p2    p3
#&gt; SRR1539207     1   0.628      0.475 0.540 0.00 0.460
#&gt; SRR1539208     1   0.579      0.745 0.668 0.00 0.332
#&gt; SRR1539211     1   0.579      0.745 0.668 0.00 0.332
#&gt; SRR1539210     1   0.628      0.475 0.540 0.00 0.460
#&gt; SRR1539209     2   0.480      0.910 0.000 0.78 0.220
#&gt; SRR1539212     2   0.480      0.910 0.000 0.78 0.220
#&gt; SRR1539214     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539213     1   0.626      0.480 0.552 0.00 0.448
#&gt; SRR1539215     2   0.480      0.910 0.000 0.78 0.220
#&gt; SRR1539216     1   0.628      0.475 0.540 0.00 0.460
#&gt; SRR1539217     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539218     2   0.480      0.910 0.000 0.78 0.220
#&gt; SRR1539220     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539219     1   0.628      0.475 0.540 0.00 0.460
#&gt; SRR1539221     2   0.480      0.910 0.000 0.78 0.220
#&gt; SRR1539223     1   0.579      0.745 0.668 0.00 0.332
#&gt; SRR1539224     2   0.480      0.910 0.000 0.78 0.220
#&gt; SRR1539222     1   0.628      0.475 0.540 0.00 0.460
#&gt; SRR1539225     1   0.626      0.480 0.552 0.00 0.448
#&gt; SRR1539227     2   0.480      0.910 0.000 0.78 0.220
#&gt; SRR1539226     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539228     1   0.626      0.480 0.552 0.00 0.448
#&gt; SRR1539229     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539232     1   0.626      0.480 0.552 0.00 0.448
#&gt; SRR1539230     2   0.480      0.910 0.000 0.78 0.220
#&gt; SRR1539231     2   0.480      0.910 0.000 0.78 0.220
#&gt; SRR1539234     2   0.000      0.919 0.000 1.00 0.000
#&gt; SRR1539233     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539235     1   0.000      0.723 1.000 0.00 0.000
#&gt; SRR1539236     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539237     2   0.000      0.919 0.000 1.00 0.000
#&gt; SRR1539238     1   0.000      0.723 1.000 0.00 0.000
#&gt; SRR1539239     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539242     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539240     2   0.000      0.919 0.000 1.00 0.000
#&gt; SRR1539241     1   0.000      0.723 1.000 0.00 0.000
#&gt; SRR1539243     2   0.000      0.919 0.000 1.00 0.000
#&gt; SRR1539244     1   0.000      0.723 1.000 0.00 0.000
#&gt; SRR1539245     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539246     2   0.000      0.919 0.000 1.00 0.000
#&gt; SRR1539247     1   0.000      0.723 1.000 0.00 0.000
#&gt; SRR1539248     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539249     2   0.000      0.919 0.000 1.00 0.000
#&gt; SRR1539250     1   0.116      0.715 0.972 0.00 0.028
#&gt; SRR1539251     1   0.116      0.715 0.972 0.00 0.028
#&gt; SRR1539253     2   0.000      0.919 0.000 1.00 0.000
#&gt; SRR1539252     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539255     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539254     1   0.000      0.723 1.000 0.00 0.000
#&gt; SRR1539256     2   0.000      0.919 0.000 1.00 0.000
#&gt; SRR1539257     1   0.000      0.723 1.000 0.00 0.000
#&gt; SRR1539258     1   0.571      0.748 0.680 0.00 0.320
#&gt; SRR1539259     2   0.000      0.919 0.000 1.00 0.000
#&gt; SRR1539260     1   0.000      0.723 1.000 0.00 0.000
#&gt; SRR1539262     2   0.000      0.919 0.000 1.00 0.000
#&gt; SRR1539261     1   0.579      0.745 0.668 0.00 0.332
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3  0.2984      0.945 0.028 0.000 0.888 0.084
#&gt; SRR1539208     4  0.5396      1.000 0.464 0.000 0.012 0.524
#&gt; SRR1539211     4  0.5396      1.000 0.464 0.000 0.012 0.524
#&gt; SRR1539210     3  0.4225      0.876 0.024 0.000 0.792 0.184
#&gt; SRR1539209     2  0.1406      0.799 0.000 0.960 0.016 0.024
#&gt; SRR1539212     2  0.1406      0.799 0.000 0.960 0.016 0.024
#&gt; SRR1539214     1  0.4643     -0.513 0.656 0.000 0.000 0.344
#&gt; SRR1539213     3  0.1118      0.956 0.036 0.000 0.964 0.000
#&gt; SRR1539215     2  0.0000      0.807 0.000 1.000 0.000 0.000
#&gt; SRR1539216     3  0.2984      0.945 0.028 0.000 0.888 0.084
#&gt; SRR1539217     1  0.4830     -0.623 0.608 0.000 0.000 0.392
#&gt; SRR1539218     2  0.1406      0.799 0.000 0.960 0.016 0.024
#&gt; SRR1539220     1  0.4643     -0.513 0.656 0.000 0.000 0.344
#&gt; SRR1539219     3  0.0921      0.954 0.028 0.000 0.972 0.000
#&gt; SRR1539221     2  0.0000      0.807 0.000 1.000 0.000 0.000
#&gt; SRR1539223     4  0.5396      1.000 0.464 0.000 0.012 0.524
#&gt; SRR1539224     2  0.1406      0.799 0.000 0.960 0.016 0.024
#&gt; SRR1539222     3  0.2949      0.942 0.024 0.000 0.888 0.088
#&gt; SRR1539225     3  0.1118      0.956 0.036 0.000 0.964 0.000
#&gt; SRR1539227     2  0.0000      0.807 0.000 1.000 0.000 0.000
#&gt; SRR1539226     1  0.4643     -0.513 0.656 0.000 0.000 0.344
#&gt; SRR1539228     3  0.1118      0.956 0.036 0.000 0.964 0.000
#&gt; SRR1539229     1  0.4643     -0.513 0.656 0.000 0.000 0.344
#&gt; SRR1539232     3  0.1118      0.956 0.036 0.000 0.964 0.000
#&gt; SRR1539230     2  0.0000      0.807 0.000 1.000 0.000 0.000
#&gt; SRR1539231     2  0.0000      0.807 0.000 1.000 0.000 0.000
#&gt; SRR1539234     2  0.4889      0.830 0.000 0.636 0.004 0.360
#&gt; SRR1539233     1  0.4643     -0.513 0.656 0.000 0.000 0.344
#&gt; SRR1539235     1  0.4382      0.336 0.704 0.000 0.296 0.000
#&gt; SRR1539236     1  0.4643     -0.513 0.656 0.000 0.000 0.344
#&gt; SRR1539237     2  0.4730      0.830 0.000 0.636 0.000 0.364
#&gt; SRR1539238     1  0.4382      0.336 0.704 0.000 0.296 0.000
#&gt; SRR1539239     1  0.4916     -0.680 0.576 0.000 0.000 0.424
#&gt; SRR1539242     1  0.4916     -0.680 0.576 0.000 0.000 0.424
#&gt; SRR1539240     2  0.4730      0.830 0.000 0.636 0.000 0.364
#&gt; SRR1539241     1  0.4382      0.336 0.704 0.000 0.296 0.000
#&gt; SRR1539243     2  0.4730      0.830 0.000 0.636 0.000 0.364
#&gt; SRR1539244     1  0.4382      0.336 0.704 0.000 0.296 0.000
#&gt; SRR1539245     1  0.4643     -0.513 0.656 0.000 0.000 0.344
#&gt; SRR1539246     2  0.5007      0.830 0.000 0.636 0.008 0.356
#&gt; SRR1539247     1  0.4382      0.336 0.704 0.000 0.296 0.000
#&gt; SRR1539248     1  0.4916     -0.680 0.576 0.000 0.000 0.424
#&gt; SRR1539249     2  0.4889      0.830 0.000 0.636 0.004 0.360
#&gt; SRR1539250     1  0.4868      0.313 0.684 0.000 0.304 0.012
#&gt; SRR1539251     1  0.4868      0.313 0.684 0.000 0.304 0.012
#&gt; SRR1539253     2  0.4889      0.830 0.000 0.636 0.004 0.360
#&gt; SRR1539252     1  0.4643     -0.513 0.656 0.000 0.000 0.344
#&gt; SRR1539255     1  0.4697     -0.544 0.644 0.000 0.000 0.356
#&gt; SRR1539254     1  0.4382      0.336 0.704 0.000 0.296 0.000
#&gt; SRR1539256     2  0.4730      0.830 0.000 0.636 0.000 0.364
#&gt; SRR1539257     1  0.4382      0.336 0.704 0.000 0.296 0.000
#&gt; SRR1539258     1  0.4790     -0.588 0.620 0.000 0.000 0.380
#&gt; SRR1539259     2  0.4889      0.830 0.000 0.636 0.004 0.360
#&gt; SRR1539260     1  0.4382      0.336 0.704 0.000 0.296 0.000
#&gt; SRR1539262     2  0.4978      0.824 0.000 0.612 0.004 0.384
#&gt; SRR1539261     4  0.5396      1.000 0.464 0.000 0.012 0.524
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.2850      0.893 0.016 0.000 0.880 0.088 0.016
#&gt; SRR1539208     1  0.6749      0.608 0.512 0.000 0.020 0.292 0.176
#&gt; SRR1539211     1  0.6789      0.600 0.504 0.000 0.020 0.296 0.180
#&gt; SRR1539210     3  0.5656      0.647 0.000 0.000 0.624 0.236 0.140
#&gt; SRR1539209     4  0.6007      0.859 0.000 0.396 0.000 0.488 0.116
#&gt; SRR1539212     4  0.6007      0.859 0.000 0.396 0.000 0.488 0.116
#&gt; SRR1539214     1  0.0880      0.841 0.968 0.000 0.000 0.000 0.032
#&gt; SRR1539213     3  0.0609      0.914 0.020 0.000 0.980 0.000 0.000
#&gt; SRR1539215     4  0.4273      0.883 0.000 0.448 0.000 0.552 0.000
#&gt; SRR1539216     3  0.2850      0.893 0.016 0.000 0.880 0.088 0.016
#&gt; SRR1539217     1  0.2514      0.834 0.896 0.000 0.000 0.060 0.044
#&gt; SRR1539218     4  0.6007      0.859 0.000 0.396 0.000 0.488 0.116
#&gt; SRR1539220     1  0.0880      0.841 0.968 0.000 0.000 0.000 0.032
#&gt; SRR1539219     3  0.0510      0.913 0.016 0.000 0.984 0.000 0.000
#&gt; SRR1539221     4  0.4273      0.883 0.000 0.448 0.000 0.552 0.000
#&gt; SRR1539223     1  0.6749      0.608 0.512 0.000 0.020 0.292 0.176
#&gt; SRR1539224     4  0.6007      0.859 0.000 0.396 0.000 0.488 0.116
#&gt; SRR1539222     3  0.2707      0.876 0.000 0.000 0.876 0.100 0.024
#&gt; SRR1539225     3  0.0609      0.914 0.020 0.000 0.980 0.000 0.000
#&gt; SRR1539227     4  0.4273      0.883 0.000 0.448 0.000 0.552 0.000
#&gt; SRR1539226     1  0.0880      0.841 0.968 0.000 0.000 0.000 0.032
#&gt; SRR1539228     3  0.0609      0.914 0.020 0.000 0.980 0.000 0.000
#&gt; SRR1539229     1  0.0880      0.841 0.968 0.000 0.000 0.000 0.032
#&gt; SRR1539232     3  0.0609      0.914 0.020 0.000 0.980 0.000 0.000
#&gt; SRR1539230     4  0.4273      0.883 0.000 0.448 0.000 0.552 0.000
#&gt; SRR1539231     4  0.4273      0.883 0.000 0.448 0.000 0.552 0.000
#&gt; SRR1539234     2  0.0609      0.961 0.000 0.980 0.000 0.000 0.020
#&gt; SRR1539233     1  0.0880      0.841 0.968 0.000 0.000 0.000 0.032
#&gt; SRR1539235     5  0.5687      0.987 0.164 0.000 0.208 0.000 0.628
#&gt; SRR1539236     1  0.0703      0.841 0.976 0.000 0.000 0.000 0.024
#&gt; SRR1539237     2  0.0290      0.972 0.000 0.992 0.000 0.000 0.008
#&gt; SRR1539238     5  0.5687      0.987 0.164 0.000 0.208 0.000 0.628
#&gt; SRR1539239     1  0.3035      0.818 0.856 0.000 0.000 0.112 0.032
#&gt; SRR1539242     1  0.3035      0.818 0.856 0.000 0.000 0.112 0.032
#&gt; SRR1539240     2  0.0000      0.972 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539241     5  0.5687      0.987 0.164 0.000 0.208 0.000 0.628
#&gt; SRR1539243     2  0.0000      0.972 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539244     5  0.5687      0.987 0.164 0.000 0.208 0.000 0.628
#&gt; SRR1539245     1  0.0609      0.841 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1539246     2  0.0880      0.962 0.000 0.968 0.000 0.000 0.032
#&gt; SRR1539247     5  0.5687      0.987 0.164 0.000 0.208 0.000 0.628
#&gt; SRR1539248     1  0.3035      0.818 0.856 0.000 0.000 0.112 0.032
#&gt; SRR1539249     2  0.0703      0.970 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1539250     5  0.5691      0.945 0.132 0.000 0.212 0.008 0.648
#&gt; SRR1539251     5  0.5691      0.945 0.132 0.000 0.212 0.008 0.648
#&gt; SRR1539253     2  0.0609      0.971 0.000 0.980 0.000 0.000 0.020
#&gt; SRR1539252     1  0.0404      0.843 0.988 0.000 0.000 0.000 0.012
#&gt; SRR1539255     1  0.0162      0.844 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1539254     5  0.5687      0.987 0.164 0.000 0.208 0.000 0.628
#&gt; SRR1539256     2  0.0000      0.972 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.5687      0.987 0.164 0.000 0.208 0.000 0.628
#&gt; SRR1539258     1  0.1809      0.836 0.928 0.000 0.000 0.060 0.012
#&gt; SRR1539259     2  0.0609      0.971 0.000 0.980 0.000 0.000 0.020
#&gt; SRR1539260     5  0.5687      0.987 0.164 0.000 0.208 0.000 0.628
#&gt; SRR1539262     2  0.1872      0.895 0.000 0.928 0.000 0.052 0.020
#&gt; SRR1539261     1  0.6749      0.608 0.512 0.000 0.020 0.292 0.176
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.5239      0.845 0.008 0.000 0.712 0.100 0.112 0.068
#&gt; SRR1539208     6  0.3244      0.964 0.268 0.000 0.000 0.000 0.000 0.732
#&gt; SRR1539211     6  0.4455      0.946 0.264 0.000 0.008 0.048 0.000 0.680
#&gt; SRR1539210     3  0.7004      0.423 0.000 0.000 0.348 0.272 0.060 0.320
#&gt; SRR1539209     4  0.6950      0.779 0.000 0.352 0.056 0.424 0.016 0.152
#&gt; SRR1539212     4  0.6950      0.779 0.000 0.352 0.056 0.424 0.016 0.152
#&gt; SRR1539214     1  0.1180      0.804 0.960 0.000 0.008 0.024 0.004 0.004
#&gt; SRR1539213     3  0.2212      0.878 0.008 0.000 0.880 0.000 0.112 0.000
#&gt; SRR1539215     4  0.3899      0.816 0.000 0.404 0.004 0.592 0.000 0.000
#&gt; SRR1539216     3  0.5239      0.845 0.008 0.000 0.712 0.100 0.112 0.068
#&gt; SRR1539217     1  0.3648      0.666 0.776 0.000 0.012 0.024 0.000 0.188
#&gt; SRR1539218     4  0.6950      0.779 0.000 0.352 0.056 0.424 0.016 0.152
#&gt; SRR1539220     1  0.1764      0.789 0.936 0.000 0.012 0.024 0.004 0.024
#&gt; SRR1539219     3  0.2212      0.878 0.008 0.000 0.880 0.000 0.112 0.000
#&gt; SRR1539221     4  0.3765      0.817 0.000 0.404 0.000 0.596 0.000 0.000
#&gt; SRR1539223     6  0.3628      0.959 0.268 0.000 0.008 0.004 0.000 0.720
#&gt; SRR1539224     4  0.6950      0.779 0.000 0.352 0.056 0.424 0.016 0.152
#&gt; SRR1539222     3  0.6126      0.771 0.000 0.000 0.584 0.224 0.108 0.084
#&gt; SRR1539225     3  0.2212      0.878 0.008 0.000 0.880 0.000 0.112 0.000
#&gt; SRR1539227     4  0.3765      0.817 0.000 0.404 0.000 0.596 0.000 0.000
#&gt; SRR1539226     1  0.0837      0.808 0.972 0.000 0.004 0.020 0.004 0.000
#&gt; SRR1539228     3  0.2212      0.878 0.008 0.000 0.880 0.000 0.112 0.000
#&gt; SRR1539229     1  0.0837      0.808 0.972 0.000 0.004 0.020 0.004 0.000
#&gt; SRR1539232     3  0.2355      0.878 0.008 0.000 0.876 0.000 0.112 0.004
#&gt; SRR1539230     4  0.3765      0.817 0.000 0.404 0.000 0.596 0.000 0.000
#&gt; SRR1539231     4  0.3765      0.817 0.000 0.404 0.000 0.596 0.000 0.000
#&gt; SRR1539234     2  0.1562      0.930 0.000 0.940 0.004 0.000 0.024 0.032
#&gt; SRR1539233     1  0.0837      0.808 0.972 0.000 0.004 0.020 0.004 0.000
#&gt; SRR1539235     5  0.1141      0.995 0.052 0.000 0.000 0.000 0.948 0.000
#&gt; SRR1539236     1  0.1477      0.795 0.940 0.000 0.008 0.048 0.004 0.000
#&gt; SRR1539237     2  0.0717      0.938 0.000 0.976 0.016 0.000 0.000 0.008
#&gt; SRR1539238     5  0.1141      0.995 0.052 0.000 0.000 0.000 0.948 0.000
#&gt; SRR1539239     1  0.4668      0.446 0.652 0.000 0.008 0.056 0.000 0.284
#&gt; SRR1539242     1  0.4668      0.446 0.652 0.000 0.008 0.056 0.000 0.284
#&gt; SRR1539240     2  0.1074      0.939 0.000 0.960 0.000 0.000 0.012 0.028
#&gt; SRR1539241     5  0.1141      0.995 0.052 0.000 0.000 0.000 0.948 0.000
#&gt; SRR1539243     2  0.1074      0.939 0.000 0.960 0.000 0.000 0.012 0.028
#&gt; SRR1539244     5  0.1141      0.995 0.052 0.000 0.000 0.000 0.948 0.000
#&gt; SRR1539245     1  0.0146      0.809 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; SRR1539246     2  0.1777      0.934 0.000 0.932 0.012 0.000 0.032 0.024
#&gt; SRR1539247     5  0.1141      0.995 0.052 0.000 0.000 0.000 0.948 0.000
#&gt; SRR1539248     1  0.4668      0.446 0.652 0.000 0.008 0.056 0.000 0.284
#&gt; SRR1539249     2  0.1138      0.934 0.000 0.960 0.024 0.000 0.012 0.004
#&gt; SRR1539250     5  0.1765      0.978 0.052 0.000 0.000 0.000 0.924 0.024
#&gt; SRR1539251     5  0.1765      0.978 0.052 0.000 0.000 0.000 0.924 0.024
#&gt; SRR1539253     2  0.0891      0.934 0.000 0.968 0.024 0.000 0.008 0.000
#&gt; SRR1539252     1  0.0000      0.810 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539255     1  0.1462      0.791 0.936 0.000 0.008 0.056 0.000 0.000
#&gt; SRR1539254     5  0.1141      0.995 0.052 0.000 0.000 0.000 0.948 0.000
#&gt; SRR1539256     2  0.1074      0.939 0.000 0.960 0.000 0.000 0.012 0.028
#&gt; SRR1539257     5  0.1141      0.995 0.052 0.000 0.000 0.000 0.948 0.000
#&gt; SRR1539258     1  0.3816      0.666 0.780 0.000 0.008 0.056 0.000 0.156
#&gt; SRR1539259     2  0.1074      0.931 0.000 0.960 0.028 0.000 0.012 0.000
#&gt; SRR1539260     5  0.1141      0.995 0.052 0.000 0.000 0.000 0.948 0.000
#&gt; SRR1539262     2  0.2202      0.868 0.000 0.908 0.028 0.052 0.012 0.000
#&gt; SRR1539261     6  0.3971      0.962 0.268 0.000 0.004 0.024 0.000 0.704
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.992       0.997         0.4760 0.523   0.523
#> 3 3 1.000           0.976       0.992         0.4251 0.757   0.553
#> 4 4 0.874           0.596       0.770         0.0771 0.953   0.855
#> 5 5 0.969           0.932       0.952         0.0672 0.890   0.639
#> 6 6 0.970           0.949       0.966         0.0713 0.942   0.742
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 5
```

There is also optional best $k$ = 2 3 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1539207     1   0.000      1.000 1.000 0.000
#&gt; SRR1539208     1   0.000      1.000 1.000 0.000
#&gt; SRR1539211     2   0.697      0.768 0.188 0.812
#&gt; SRR1539210     1   0.000      1.000 1.000 0.000
#&gt; SRR1539209     2   0.000      0.991 0.000 1.000
#&gt; SRR1539212     2   0.000      0.991 0.000 1.000
#&gt; SRR1539214     1   0.000      1.000 1.000 0.000
#&gt; SRR1539213     1   0.000      1.000 1.000 0.000
#&gt; SRR1539215     2   0.000      0.991 0.000 1.000
#&gt; SRR1539216     1   0.000      1.000 1.000 0.000
#&gt; SRR1539217     1   0.000      1.000 1.000 0.000
#&gt; SRR1539218     2   0.000      0.991 0.000 1.000
#&gt; SRR1539220     1   0.000      1.000 1.000 0.000
#&gt; SRR1539219     1   0.000      1.000 1.000 0.000
#&gt; SRR1539221     2   0.000      0.991 0.000 1.000
#&gt; SRR1539223     1   0.000      1.000 1.000 0.000
#&gt; SRR1539224     2   0.000      0.991 0.000 1.000
#&gt; SRR1539222     1   0.000      1.000 1.000 0.000
#&gt; SRR1539225     1   0.000      1.000 1.000 0.000
#&gt; SRR1539227     2   0.000      0.991 0.000 1.000
#&gt; SRR1539226     1   0.000      1.000 1.000 0.000
#&gt; SRR1539228     1   0.000      1.000 1.000 0.000
#&gt; SRR1539229     1   0.000      1.000 1.000 0.000
#&gt; SRR1539232     1   0.000      1.000 1.000 0.000
#&gt; SRR1539230     2   0.000      0.991 0.000 1.000
#&gt; SRR1539231     2   0.000      0.991 0.000 1.000
#&gt; SRR1539234     2   0.000      0.991 0.000 1.000
#&gt; SRR1539233     1   0.000      1.000 1.000 0.000
#&gt; SRR1539235     1   0.000      1.000 1.000 0.000
#&gt; SRR1539236     1   0.000      1.000 1.000 0.000
#&gt; SRR1539237     2   0.000      0.991 0.000 1.000
#&gt; SRR1539238     1   0.000      1.000 1.000 0.000
#&gt; SRR1539239     1   0.000      1.000 1.000 0.000
#&gt; SRR1539242     1   0.000      1.000 1.000 0.000
#&gt; SRR1539240     2   0.000      0.991 0.000 1.000
#&gt; SRR1539241     1   0.000      1.000 1.000 0.000
#&gt; SRR1539243     2   0.000      0.991 0.000 1.000
#&gt; SRR1539244     1   0.000      1.000 1.000 0.000
#&gt; SRR1539245     1   0.000      1.000 1.000 0.000
#&gt; SRR1539246     2   0.000      0.991 0.000 1.000
#&gt; SRR1539247     1   0.000      1.000 1.000 0.000
#&gt; SRR1539248     1   0.000      1.000 1.000 0.000
#&gt; SRR1539249     2   0.000      0.991 0.000 1.000
#&gt; SRR1539250     1   0.000      1.000 1.000 0.000
#&gt; SRR1539251     1   0.000      1.000 1.000 0.000
#&gt; SRR1539253     2   0.000      0.991 0.000 1.000
#&gt; SRR1539252     1   0.000      1.000 1.000 0.000
#&gt; SRR1539255     1   0.000      1.000 1.000 0.000
#&gt; SRR1539254     1   0.000      1.000 1.000 0.000
#&gt; SRR1539256     2   0.000      0.991 0.000 1.000
#&gt; SRR1539257     1   0.000      1.000 1.000 0.000
#&gt; SRR1539258     1   0.000      1.000 1.000 0.000
#&gt; SRR1539259     2   0.000      0.991 0.000 1.000
#&gt; SRR1539260     1   0.000      1.000 1.000 0.000
#&gt; SRR1539262     2   0.000      0.991 0.000 1.000
#&gt; SRR1539261     2   0.000      0.991 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1 p2   p3
#&gt; SRR1539207     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539208     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539211     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539210     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539209     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539212     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539214     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539213     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539215     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539216     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539217     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539218     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539220     1   0.628      0.148 0.54  0 0.46
#&gt; SRR1539219     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539221     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539223     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539224     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539222     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539225     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539227     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539226     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539228     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539229     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539232     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539230     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539231     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539234     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539233     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539235     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539236     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539237     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539238     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539239     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539242     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539240     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539241     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539243     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539244     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539245     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539246     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539247     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539248     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539249     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539250     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539251     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539253     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539252     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539255     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539254     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539256     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539257     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539258     1   0.000      0.973 1.00  0 0.00
#&gt; SRR1539259     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539260     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539262     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539261     1   0.000      0.973 1.00  0 0.00
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3  0.0000     0.7097 0.000 0.000 1.000 0.000
#&gt; SRR1539208     1  0.0000     0.3082 1.000 0.000 0.000 0.000
#&gt; SRR1539211     1  0.0000     0.3082 1.000 0.000 0.000 0.000
#&gt; SRR1539210     3  0.4992     0.1473 0.476 0.000 0.524 0.000
#&gt; SRR1539209     2  0.0336     0.9965 0.000 0.992 0.000 0.008
#&gt; SRR1539212     2  0.0336     0.9965 0.000 0.992 0.000 0.008
#&gt; SRR1539214     1  0.5000    -1.0000 0.500 0.000 0.000 0.500
#&gt; SRR1539213     3  0.0000     0.7097 0.000 0.000 1.000 0.000
#&gt; SRR1539215     2  0.0336     0.9965 0.000 0.992 0.000 0.008
#&gt; SRR1539216     3  0.0000     0.7097 0.000 0.000 1.000 0.000
#&gt; SRR1539217     1  0.4431    -0.2345 0.696 0.000 0.000 0.304
#&gt; SRR1539218     2  0.0336     0.9965 0.000 0.992 0.000 0.008
#&gt; SRR1539220     1  0.7617     0.0121 0.452 0.000 0.332 0.216
#&gt; SRR1539219     3  0.0000     0.7097 0.000 0.000 1.000 0.000
#&gt; SRR1539221     2  0.0336     0.9965 0.000 0.992 0.000 0.008
#&gt; SRR1539223     1  0.0000     0.3082 1.000 0.000 0.000 0.000
#&gt; SRR1539224     2  0.0336     0.9965 0.000 0.992 0.000 0.008
#&gt; SRR1539222     3  0.0000     0.7097 0.000 0.000 1.000 0.000
#&gt; SRR1539225     3  0.0000     0.7097 0.000 0.000 1.000 0.000
#&gt; SRR1539227     2  0.0336     0.9965 0.000 0.992 0.000 0.008
#&gt; SRR1539226     4  0.5000     1.0000 0.500 0.000 0.000 0.500
#&gt; SRR1539228     3  0.0000     0.7097 0.000 0.000 1.000 0.000
#&gt; SRR1539229     4  0.5000     1.0000 0.500 0.000 0.000 0.500
#&gt; SRR1539232     3  0.0000     0.7097 0.000 0.000 1.000 0.000
#&gt; SRR1539230     2  0.0336     0.9965 0.000 0.992 0.000 0.008
#&gt; SRR1539231     2  0.0336     0.9965 0.000 0.992 0.000 0.008
#&gt; SRR1539234     2  0.0000     0.9968 0.000 1.000 0.000 0.000
#&gt; SRR1539233     4  0.5000     1.0000 0.500 0.000 0.000 0.500
#&gt; SRR1539235     3  0.4999     0.7564 0.000 0.000 0.508 0.492
#&gt; SRR1539236     4  0.5000     1.0000 0.500 0.000 0.000 0.500
#&gt; SRR1539237     2  0.0000     0.9968 0.000 1.000 0.000 0.000
#&gt; SRR1539238     3  0.4999     0.7564 0.000 0.000 0.508 0.492
#&gt; SRR1539239     1  0.5000    -0.9832 0.504 0.000 0.000 0.496
#&gt; SRR1539242     1  0.5000    -0.9832 0.504 0.000 0.000 0.496
#&gt; SRR1539240     2  0.0000     0.9968 0.000 1.000 0.000 0.000
#&gt; SRR1539241     3  0.4999     0.7564 0.000 0.000 0.508 0.492
#&gt; SRR1539243     2  0.0000     0.9968 0.000 1.000 0.000 0.000
#&gt; SRR1539244     3  0.4999     0.7564 0.000 0.000 0.508 0.492
#&gt; SRR1539245     1  0.5000    -1.0000 0.500 0.000 0.000 0.500
#&gt; SRR1539246     2  0.0000     0.9968 0.000 1.000 0.000 0.000
#&gt; SRR1539247     3  0.4999     0.7564 0.000 0.000 0.508 0.492
#&gt; SRR1539248     1  0.5000    -0.9832 0.504 0.000 0.000 0.496
#&gt; SRR1539249     2  0.0000     0.9968 0.000 1.000 0.000 0.000
#&gt; SRR1539250     3  0.4999     0.7564 0.000 0.000 0.508 0.492
#&gt; SRR1539251     3  0.4999     0.7564 0.000 0.000 0.508 0.492
#&gt; SRR1539253     2  0.0000     0.9968 0.000 1.000 0.000 0.000
#&gt; SRR1539252     4  0.5000     1.0000 0.500 0.000 0.000 0.500
#&gt; SRR1539255     4  0.5000     1.0000 0.500 0.000 0.000 0.500
#&gt; SRR1539254     3  0.4999     0.7564 0.000 0.000 0.508 0.492
#&gt; SRR1539256     2  0.0000     0.9968 0.000 1.000 0.000 0.000
#&gt; SRR1539257     3  0.4999     0.7564 0.000 0.000 0.508 0.492
#&gt; SRR1539258     1  0.5000    -1.0000 0.500 0.000 0.000 0.500
#&gt; SRR1539259     2  0.0000     0.9968 0.000 1.000 0.000 0.000
#&gt; SRR1539260     3  0.4999     0.7564 0.000 0.000 0.508 0.492
#&gt; SRR1539262     2  0.0000     0.9968 0.000 1.000 0.000 0.000
#&gt; SRR1539261     1  0.0000     0.3082 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1   p2    p3    p4    p5
#&gt; SRR1539207     3  0.1197      0.954 0.000 0.00 0.952 0.000 0.048
#&gt; SRR1539208     4  0.0963      0.995 0.036 0.00 0.000 0.964 0.000
#&gt; SRR1539211     4  0.0963      0.995 0.036 0.00 0.000 0.964 0.000
#&gt; SRR1539210     3  0.4211      0.446 0.000 0.00 0.636 0.360 0.004
#&gt; SRR1539209     2  0.2074      0.959 0.000 0.92 0.044 0.036 0.000
#&gt; SRR1539212     2  0.2074      0.959 0.000 0.92 0.044 0.036 0.000
#&gt; SRR1539214     1  0.0000      0.912 1.000 0.00 0.000 0.000 0.000
#&gt; SRR1539213     3  0.1197      0.954 0.000 0.00 0.952 0.000 0.048
#&gt; SRR1539215     2  0.2074      0.959 0.000 0.92 0.044 0.036 0.000
#&gt; SRR1539216     3  0.1197      0.954 0.000 0.00 0.952 0.000 0.048
#&gt; SRR1539217     1  0.4074      0.442 0.636 0.00 0.000 0.364 0.000
#&gt; SRR1539218     2  0.2074      0.959 0.000 0.92 0.044 0.036 0.000
#&gt; SRR1539220     1  0.1792      0.841 0.916 0.00 0.000 0.000 0.084
#&gt; SRR1539219     3  0.1197      0.954 0.000 0.00 0.952 0.000 0.048
#&gt; SRR1539221     2  0.2074      0.959 0.000 0.92 0.044 0.036 0.000
#&gt; SRR1539223     4  0.1197      0.985 0.048 0.00 0.000 0.952 0.000
#&gt; SRR1539224     2  0.2074      0.959 0.000 0.92 0.044 0.036 0.000
#&gt; SRR1539222     3  0.1197      0.954 0.000 0.00 0.952 0.000 0.048
#&gt; SRR1539225     3  0.1197      0.954 0.000 0.00 0.952 0.000 0.048
#&gt; SRR1539227     2  0.2074      0.959 0.000 0.92 0.044 0.036 0.000
#&gt; SRR1539226     1  0.0000      0.912 1.000 0.00 0.000 0.000 0.000
#&gt; SRR1539228     3  0.1197      0.954 0.000 0.00 0.952 0.000 0.048
#&gt; SRR1539229     1  0.0000      0.912 1.000 0.00 0.000 0.000 0.000
#&gt; SRR1539232     3  0.1197      0.954 0.000 0.00 0.952 0.000 0.048
#&gt; SRR1539230     2  0.2074      0.959 0.000 0.92 0.044 0.036 0.000
#&gt; SRR1539231     2  0.2074      0.959 0.000 0.92 0.044 0.036 0.000
#&gt; SRR1539234     2  0.0000      0.963 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1539233     1  0.0000      0.912 1.000 0.00 0.000 0.000 0.000
#&gt; SRR1539235     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000
#&gt; SRR1539236     1  0.0162      0.911 0.996 0.00 0.004 0.000 0.000
#&gt; SRR1539237     2  0.0000      0.963 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1539238     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000
#&gt; SRR1539239     1  0.3048      0.809 0.820 0.00 0.004 0.176 0.000
#&gt; SRR1539242     1  0.3048      0.809 0.820 0.00 0.004 0.176 0.000
#&gt; SRR1539240     2  0.0000      0.963 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1539241     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000
#&gt; SRR1539243     2  0.0000      0.963 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1539244     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000
#&gt; SRR1539245     1  0.0000      0.912 1.000 0.00 0.000 0.000 0.000
#&gt; SRR1539246     2  0.0000      0.963 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1539247     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000
#&gt; SRR1539248     1  0.3048      0.809 0.820 0.00 0.004 0.176 0.000
#&gt; SRR1539249     2  0.0000      0.963 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1539250     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000
#&gt; SRR1539251     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000
#&gt; SRR1539253     2  0.0000      0.963 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1539252     1  0.0000      0.912 1.000 0.00 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0162      0.911 0.996 0.00 0.004 0.000 0.000
#&gt; SRR1539254     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000
#&gt; SRR1539256     2  0.0000      0.963 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1539257     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000
#&gt; SRR1539258     1  0.1502      0.888 0.940 0.00 0.004 0.056 0.000
#&gt; SRR1539259     2  0.0000      0.963 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1539260     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000
#&gt; SRR1539262     2  0.0000      0.963 0.000 1.00 0.000 0.000 0.000
#&gt; SRR1539261     4  0.0963      0.995 0.036 0.00 0.000 0.964 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.0000      0.949 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539208     6  0.0000      0.991 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539211     6  0.0000      0.991 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539210     3  0.3737      0.362 0.000 0.000 0.608 0.000 0.000 0.392
#&gt; SRR1539209     4  0.1075      1.000 0.000 0.048 0.000 0.952 0.000 0.000
#&gt; SRR1539212     4  0.1075      1.000 0.000 0.048 0.000 0.952 0.000 0.000
#&gt; SRR1539214     1  0.0547      0.917 0.980 0.000 0.000 0.020 0.000 0.000
#&gt; SRR1539213     3  0.0000      0.949 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539215     4  0.1075      1.000 0.000 0.048 0.000 0.952 0.000 0.000
#&gt; SRR1539216     3  0.0000      0.949 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539217     1  0.3969      0.551 0.668 0.000 0.000 0.020 0.000 0.312
#&gt; SRR1539218     4  0.1075      1.000 0.000 0.048 0.000 0.952 0.000 0.000
#&gt; SRR1539220     1  0.1549      0.890 0.936 0.000 0.000 0.020 0.044 0.000
#&gt; SRR1539219     3  0.0000      0.949 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539221     4  0.1075      1.000 0.000 0.048 0.000 0.952 0.000 0.000
#&gt; SRR1539223     6  0.0692      0.972 0.020 0.000 0.000 0.004 0.000 0.976
#&gt; SRR1539224     4  0.1075      1.000 0.000 0.048 0.000 0.952 0.000 0.000
#&gt; SRR1539222     3  0.0000      0.949 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539225     3  0.0000      0.949 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539227     4  0.1075      1.000 0.000 0.048 0.000 0.952 0.000 0.000
#&gt; SRR1539226     1  0.0458      0.918 0.984 0.000 0.000 0.016 0.000 0.000
#&gt; SRR1539228     3  0.0000      0.949 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539229     1  0.0458      0.918 0.984 0.000 0.000 0.016 0.000 0.000
#&gt; SRR1539232     3  0.0000      0.949 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539230     4  0.1075      1.000 0.000 0.048 0.000 0.952 0.000 0.000
#&gt; SRR1539231     4  0.1075      1.000 0.000 0.048 0.000 0.952 0.000 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539233     1  0.0458      0.918 0.984 0.000 0.000 0.016 0.000 0.000
#&gt; SRR1539235     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539236     1  0.0632      0.917 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; SRR1539237     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539238     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539239     1  0.2949      0.842 0.832 0.000 0.000 0.028 0.000 0.140
#&gt; SRR1539242     1  0.2949      0.842 0.832 0.000 0.000 0.028 0.000 0.140
#&gt; SRR1539240     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539241     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539243     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539244     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539245     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539246     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539247     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539248     1  0.2949      0.842 0.832 0.000 0.000 0.028 0.000 0.140
#&gt; SRR1539249     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539250     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539251     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539253     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539252     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0632      0.917 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; SRR1539254     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539256     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539258     1  0.1498      0.906 0.940 0.000 0.000 0.028 0.000 0.032
#&gt; SRR1539259     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539260     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539262     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539261     6  0.0000      0.991 0.000 0.000 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.976           0.946       0.978         0.4768 0.779   0.594
#> 4 4 0.853           0.936       0.946         0.0976 0.936   0.801
#> 5 5 0.982           0.946       0.979         0.0772 0.945   0.789
#> 6 6 0.935           0.892       0.930         0.0301 0.978   0.893
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 5
```

There is also optional best $k$ = 2 3 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3
#&gt; SRR1539207     3  0.0000      0.985 0.000  0 1.000
#&gt; SRR1539208     1  0.4399      0.749 0.812  0 0.188
#&gt; SRR1539211     1  0.0000      0.936 1.000  0 0.000
#&gt; SRR1539210     3  0.0000      0.985 0.000  0 1.000
#&gt; SRR1539209     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539212     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539214     1  0.6026      0.394 0.624  0 0.376
#&gt; SRR1539213     3  0.0000      0.985 0.000  0 1.000
#&gt; SRR1539215     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539216     3  0.0000      0.985 0.000  0 1.000
#&gt; SRR1539217     1  0.0000      0.936 1.000  0 0.000
#&gt; SRR1539218     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539220     3  0.4796      0.690 0.220  0 0.780
#&gt; SRR1539219     3  0.0000      0.985 0.000  0 1.000
#&gt; SRR1539221     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539223     1  0.6154      0.331 0.592  0 0.408
#&gt; SRR1539224     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539222     3  0.0000      0.985 0.000  0 1.000
#&gt; SRR1539225     3  0.0000      0.985 0.000  0 1.000
#&gt; SRR1539227     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539226     1  0.0000      0.936 1.000  0 0.000
#&gt; SRR1539228     3  0.0000      0.985 0.000  0 1.000
#&gt; SRR1539229     1  0.0000      0.936 1.000  0 0.000
#&gt; SRR1539232     3  0.0000      0.985 0.000  0 1.000
#&gt; SRR1539230     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539231     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539233     1  0.0000      0.936 1.000  0 0.000
#&gt; SRR1539235     3  0.0237      0.985 0.004  0 0.996
#&gt; SRR1539236     1  0.0000      0.936 1.000  0 0.000
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539238     3  0.0237      0.985 0.004  0 0.996
#&gt; SRR1539239     1  0.0000      0.936 1.000  0 0.000
#&gt; SRR1539242     1  0.0000      0.936 1.000  0 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539241     3  0.0237      0.985 0.004  0 0.996
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539244     3  0.0237      0.985 0.004  0 0.996
#&gt; SRR1539245     1  0.0000      0.936 1.000  0 0.000
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539247     3  0.0237      0.985 0.004  0 0.996
#&gt; SRR1539248     1  0.0000      0.936 1.000  0 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539250     3  0.0237      0.985 0.004  0 0.996
#&gt; SRR1539251     3  0.0237      0.985 0.004  0 0.996
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539252     1  0.0000      0.936 1.000  0 0.000
#&gt; SRR1539255     1  0.0000      0.936 1.000  0 0.000
#&gt; SRR1539254     3  0.0237      0.985 0.004  0 0.996
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539257     3  0.0237      0.985 0.004  0 0.996
#&gt; SRR1539258     1  0.0000      0.936 1.000  0 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539260     3  0.0237      0.985 0.004  0 0.996
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539261     1  0.0000      0.936 1.000  0 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1539208     1  0.4780      0.711 0.788 0.000 0.116 0.096
#&gt; SRR1539211     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; SRR1539210     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1539209     2  0.2647      0.939 0.000 0.880 0.000 0.120
#&gt; SRR1539212     2  0.0817      0.953 0.000 0.976 0.000 0.024
#&gt; SRR1539214     1  0.4888      0.269 0.588 0.000 0.000 0.412
#&gt; SRR1539213     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1539215     2  0.2647      0.939 0.000 0.880 0.000 0.120
#&gt; SRR1539216     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1539217     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; SRR1539218     2  0.2647      0.939 0.000 0.880 0.000 0.120
#&gt; SRR1539220     4  0.6366      0.651 0.240 0.000 0.120 0.640
#&gt; SRR1539219     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1539221     2  0.2647      0.939 0.000 0.880 0.000 0.120
#&gt; SRR1539223     1  0.2647      0.822 0.880 0.000 0.120 0.000
#&gt; SRR1539224     2  0.2589      0.940 0.000 0.884 0.000 0.116
#&gt; SRR1539222     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1539225     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1539227     2  0.2647      0.939 0.000 0.880 0.000 0.120
#&gt; SRR1539226     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; SRR1539228     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1539229     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; SRR1539232     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1539230     2  0.2647      0.939 0.000 0.880 0.000 0.120
#&gt; SRR1539231     2  0.2647      0.939 0.000 0.880 0.000 0.120
#&gt; SRR1539234     2  0.0000      0.955 0.000 1.000 0.000 0.000
#&gt; SRR1539233     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; SRR1539235     4  0.2647      0.968 0.000 0.000 0.120 0.880
#&gt; SRR1539236     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; SRR1539237     2  0.0000      0.955 0.000 1.000 0.000 0.000
#&gt; SRR1539238     4  0.2647      0.968 0.000 0.000 0.120 0.880
#&gt; SRR1539239     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; SRR1539242     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; SRR1539240     2  0.0000      0.955 0.000 1.000 0.000 0.000
#&gt; SRR1539241     4  0.2647      0.968 0.000 0.000 0.120 0.880
#&gt; SRR1539243     2  0.0000      0.955 0.000 1.000 0.000 0.000
#&gt; SRR1539244     4  0.2647      0.968 0.000 0.000 0.120 0.880
#&gt; SRR1539245     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; SRR1539246     2  0.0000      0.955 0.000 1.000 0.000 0.000
#&gt; SRR1539247     4  0.2647      0.968 0.000 0.000 0.120 0.880
#&gt; SRR1539248     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; SRR1539249     2  0.0000      0.955 0.000 1.000 0.000 0.000
#&gt; SRR1539250     4  0.2647      0.968 0.000 0.000 0.120 0.880
#&gt; SRR1539251     4  0.2647      0.968 0.000 0.000 0.120 0.880
#&gt; SRR1539253     2  0.0000      0.955 0.000 1.000 0.000 0.000
#&gt; SRR1539252     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; SRR1539254     4  0.2647      0.968 0.000 0.000 0.120 0.880
#&gt; SRR1539256     2  0.0000      0.955 0.000 1.000 0.000 0.000
#&gt; SRR1539257     4  0.2647      0.968 0.000 0.000 0.120 0.880
#&gt; SRR1539258     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; SRR1539259     2  0.0000      0.955 0.000 1.000 0.000 0.000
#&gt; SRR1539260     4  0.2647      0.968 0.000 0.000 0.120 0.880
#&gt; SRR1539262     2  0.0000      0.955 0.000 1.000 0.000 0.000
#&gt; SRR1539261     1  0.0000      0.948 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3    p4    p5
#&gt; SRR1539207     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539208     1   0.321      0.710 0.788 0.000  0 0.000 0.212
#&gt; SRR1539211     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539210     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539209     4   0.000      1.000 0.000 0.000  0 1.000 0.000
#&gt; SRR1539212     2   0.112      0.936 0.000 0.956  0 0.044 0.000
#&gt; SRR1539214     1   0.422      0.253 0.584 0.000  0 0.000 0.416
#&gt; SRR1539213     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539215     4   0.000      1.000 0.000 0.000  0 1.000 0.000
#&gt; SRR1539216     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539217     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539218     4   0.000      1.000 0.000 0.000  0 1.000 0.000
#&gt; SRR1539220     5   0.342      0.657 0.240 0.000  0 0.000 0.760
#&gt; SRR1539219     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539221     4   0.000      1.000 0.000 0.000  0 1.000 0.000
#&gt; SRR1539223     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539224     2   0.359      0.652 0.000 0.736  0 0.264 0.000
#&gt; SRR1539222     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539225     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539227     4   0.000      1.000 0.000 0.000  0 1.000 0.000
#&gt; SRR1539226     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539228     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539229     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539232     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539230     4   0.000      1.000 0.000 0.000  0 1.000 0.000
#&gt; SRR1539231     4   0.000      1.000 0.000 0.000  0 1.000 0.000
#&gt; SRR1539234     2   0.000      0.972 0.000 1.000  0 0.000 0.000
#&gt; SRR1539233     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539235     5   0.000      0.970 0.000 0.000  0 0.000 1.000
#&gt; SRR1539236     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539237     2   0.000      0.972 0.000 1.000  0 0.000 0.000
#&gt; SRR1539238     5   0.000      0.970 0.000 0.000  0 0.000 1.000
#&gt; SRR1539239     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539242     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539240     2   0.000      0.972 0.000 1.000  0 0.000 0.000
#&gt; SRR1539241     5   0.000      0.970 0.000 0.000  0 0.000 1.000
#&gt; SRR1539243     2   0.000      0.972 0.000 1.000  0 0.000 0.000
#&gt; SRR1539244     5   0.000      0.970 0.000 0.000  0 0.000 1.000
#&gt; SRR1539245     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539246     2   0.000      0.972 0.000 1.000  0 0.000 0.000
#&gt; SRR1539247     5   0.000      0.970 0.000 0.000  0 0.000 1.000
#&gt; SRR1539248     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539249     2   0.000      0.972 0.000 1.000  0 0.000 0.000
#&gt; SRR1539250     5   0.000      0.970 0.000 0.000  0 0.000 1.000
#&gt; SRR1539251     5   0.000      0.970 0.000 0.000  0 0.000 1.000
#&gt; SRR1539253     2   0.000      0.972 0.000 1.000  0 0.000 0.000
#&gt; SRR1539252     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539255     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539254     5   0.000      0.970 0.000 0.000  0 0.000 1.000
#&gt; SRR1539256     2   0.000      0.972 0.000 1.000  0 0.000 0.000
#&gt; SRR1539257     5   0.000      0.970 0.000 0.000  0 0.000 1.000
#&gt; SRR1539258     1   0.000      0.957 1.000 0.000  0 0.000 0.000
#&gt; SRR1539259     2   0.000      0.972 0.000 1.000  0 0.000 0.000
#&gt; SRR1539260     5   0.000      0.970 0.000 0.000  0 0.000 1.000
#&gt; SRR1539262     2   0.000      0.972 0.000 1.000  0 0.000 0.000
#&gt; SRR1539261     1   0.000      0.957 1.000 0.000  0 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3    p4    p5    p6
#&gt; SRR1539207     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539208     1  0.3489      0.623 0.708 0.000  0 0.000 0.288 0.004
#&gt; SRR1539211     1  0.0146      0.856 0.996 0.000  0 0.000 0.000 0.004
#&gt; SRR1539210     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539209     6  0.0146      0.832 0.000 0.000  0 0.004 0.000 0.996
#&gt; SRR1539212     6  0.3050      0.615 0.000 0.236  0 0.000 0.000 0.764
#&gt; SRR1539214     1  0.6078      0.219 0.388 0.000  0 0.276 0.336 0.000
#&gt; SRR1539213     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539215     4  0.3288      1.000 0.000 0.000  0 0.724 0.000 0.276
#&gt; SRR1539216     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539217     1  0.0000      0.857 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1539218     6  0.0146      0.832 0.000 0.000  0 0.004 0.000 0.996
#&gt; SRR1539220     5  0.5805      0.203 0.228 0.000  0 0.276 0.496 0.000
#&gt; SRR1539219     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539221     4  0.3288      1.000 0.000 0.000  0 0.724 0.000 0.276
#&gt; SRR1539223     1  0.0146      0.856 0.996 0.000  0 0.000 0.000 0.004
#&gt; SRR1539224     6  0.0458      0.833 0.000 0.016  0 0.000 0.000 0.984
#&gt; SRR1539222     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539225     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539227     4  0.3288      1.000 0.000 0.000  0 0.724 0.000 0.276
#&gt; SRR1539226     1  0.3288      0.769 0.724 0.000  0 0.276 0.000 0.000
#&gt; SRR1539228     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539229     1  0.3288      0.769 0.724 0.000  0 0.276 0.000 0.000
#&gt; SRR1539232     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539230     4  0.3288      1.000 0.000 0.000  0 0.724 0.000 0.276
#&gt; SRR1539231     4  0.3288      1.000 0.000 0.000  0 0.724 0.000 0.276
#&gt; SRR1539234     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1539233     1  0.3288      0.769 0.724 0.000  0 0.276 0.000 0.000
#&gt; SRR1539235     5  0.0000      0.942 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1539236     1  0.3288      0.769 0.724 0.000  0 0.276 0.000 0.000
#&gt; SRR1539237     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1539238     5  0.0000      0.942 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1539239     1  0.0146      0.856 0.996 0.000  0 0.000 0.000 0.004
#&gt; SRR1539242     1  0.0000      0.857 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1539241     5  0.0000      0.942 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1539243     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1539244     5  0.0000      0.942 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1539245     1  0.3288      0.769 0.724 0.000  0 0.276 0.000 0.000
#&gt; SRR1539246     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1539247     5  0.0000      0.942 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1539248     1  0.0000      0.857 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1539250     5  0.0000      0.942 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1539251     5  0.0000      0.942 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1539253     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1539252     1  0.0000      0.857 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0000      0.857 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1539254     5  0.0000      0.942 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1539256     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1539257     5  0.0000      0.942 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1539258     1  0.0000      0.857 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1539260     5  0.0000      0.942 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1539262     2  0.0000      1.000 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1539261     1  0.0146      0.856 0.996 0.000  0 0.000 0.000 0.004
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:mclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.975           0.952       0.975         0.3587 0.836   0.699
#> 4 4 0.749           0.829       0.837         0.1076 0.943   0.850
#> 5 5 0.795           0.865       0.886         0.1328 0.875   0.614
#> 6 6 0.946           0.870       0.956         0.0506 0.970   0.853
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3
#&gt; SRR1539207     3  0.0237      0.940 0.004  0 0.996
#&gt; SRR1539208     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539211     1  0.0237      0.957 0.996  0 0.004
#&gt; SRR1539210     3  0.0000      0.942 0.000  0 1.000
#&gt; SRR1539209     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539212     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539214     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539213     3  0.0000      0.942 0.000  0 1.000
#&gt; SRR1539215     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539216     3  0.0000      0.942 0.000  0 1.000
#&gt; SRR1539217     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539218     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539220     1  0.1163      0.951 0.972  0 0.028
#&gt; SRR1539219     3  0.0000      0.942 0.000  0 1.000
#&gt; SRR1539221     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539223     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539224     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539222     3  0.0000      0.942 0.000  0 1.000
#&gt; SRR1539225     3  0.0000      0.942 0.000  0 1.000
#&gt; SRR1539227     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539226     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539228     3  0.0237      0.940 0.004  0 0.996
#&gt; SRR1539229     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539232     3  0.6095      0.281 0.392  0 0.608
#&gt; SRR1539230     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539231     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539233     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539235     1  0.2878      0.926 0.904  0 0.096
#&gt; SRR1539236     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539238     1  0.2878      0.926 0.904  0 0.096
#&gt; SRR1539239     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539242     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539241     1  0.2878      0.926 0.904  0 0.096
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539244     1  0.2959      0.924 0.900  0 0.100
#&gt; SRR1539245     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539247     1  0.2878      0.926 0.904  0 0.096
#&gt; SRR1539248     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539250     1  0.2878      0.926 0.904  0 0.096
#&gt; SRR1539251     1  0.2878      0.926 0.904  0 0.096
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539252     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539255     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539254     1  0.2878      0.926 0.904  0 0.096
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539257     1  0.2878      0.926 0.904  0 0.096
#&gt; SRR1539258     1  0.0000      0.959 1.000  0 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539260     1  0.2878      0.926 0.904  0 0.096
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539261     1  0.0237      0.957 0.996  0 0.004
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3  0.0000      0.985 0.000 0.000 1.000 0.000
#&gt; SRR1539208     1  0.3498      0.880 0.832 0.160 0.008 0.000
#&gt; SRR1539211     1  0.7197      0.818 0.660 0.164 0.080 0.096
#&gt; SRR1539210     3  0.1004      0.970 0.000 0.004 0.972 0.024
#&gt; SRR1539209     2  0.3219      0.875 0.000 0.836 0.000 0.164
#&gt; SRR1539212     2  0.3172      0.871 0.000 0.840 0.000 0.160
#&gt; SRR1539214     1  0.0188      0.866 0.996 0.000 0.004 0.000
#&gt; SRR1539213     3  0.0000      0.985 0.000 0.000 1.000 0.000
#&gt; SRR1539215     4  0.2408      0.761 0.000 0.104 0.000 0.896
#&gt; SRR1539216     3  0.0000      0.985 0.000 0.000 1.000 0.000
#&gt; SRR1539217     1  0.3355      0.880 0.836 0.160 0.004 0.000
#&gt; SRR1539218     4  0.4916      0.287 0.000 0.424 0.000 0.576
#&gt; SRR1539220     1  0.0469      0.865 0.988 0.000 0.012 0.000
#&gt; SRR1539219     3  0.0000      0.985 0.000 0.000 1.000 0.000
#&gt; SRR1539221     4  0.2408      0.761 0.000 0.104 0.000 0.896
#&gt; SRR1539223     1  0.1118      0.864 0.964 0.000 0.036 0.000
#&gt; SRR1539224     4  0.4916      0.287 0.000 0.424 0.000 0.576
#&gt; SRR1539222     3  0.0000      0.985 0.000 0.000 1.000 0.000
#&gt; SRR1539225     3  0.0000      0.985 0.000 0.000 1.000 0.000
#&gt; SRR1539227     4  0.2408      0.761 0.000 0.104 0.000 0.896
#&gt; SRR1539226     1  0.3172      0.879 0.840 0.160 0.000 0.000
#&gt; SRR1539228     3  0.0188      0.983 0.000 0.000 0.996 0.004
#&gt; SRR1539229     1  0.3172      0.879 0.840 0.160 0.000 0.000
#&gt; SRR1539232     3  0.2048      0.904 0.064 0.000 0.928 0.008
#&gt; SRR1539230     4  0.2408      0.761 0.000 0.104 0.000 0.896
#&gt; SRR1539231     4  0.2408      0.761 0.000 0.104 0.000 0.896
#&gt; SRR1539234     2  0.4164      0.824 0.000 0.736 0.000 0.264
#&gt; SRR1539233     1  0.3172      0.879 0.840 0.160 0.000 0.000
#&gt; SRR1539235     1  0.2412      0.851 0.908 0.000 0.084 0.008
#&gt; SRR1539236     1  0.3172      0.879 0.840 0.160 0.000 0.000
#&gt; SRR1539237     2  0.3266      0.875 0.000 0.832 0.000 0.168
#&gt; SRR1539238     1  0.2546      0.848 0.900 0.000 0.092 0.008
#&gt; SRR1539239     1  0.3172      0.879 0.840 0.160 0.000 0.000
#&gt; SRR1539242     1  0.3172      0.879 0.840 0.160 0.000 0.000
#&gt; SRR1539240     2  0.3942      0.846 0.000 0.764 0.000 0.236
#&gt; SRR1539241     1  0.2480      0.849 0.904 0.000 0.088 0.008
#&gt; SRR1539243     2  0.3219      0.875 0.000 0.836 0.000 0.164
#&gt; SRR1539244     1  0.3404      0.828 0.864 0.000 0.104 0.032
#&gt; SRR1539245     1  0.3172      0.879 0.840 0.160 0.000 0.000
#&gt; SRR1539246     2  0.4277      0.802 0.000 0.720 0.000 0.280
#&gt; SRR1539247     1  0.2611      0.846 0.896 0.000 0.096 0.008
#&gt; SRR1539248     1  0.3625      0.874 0.828 0.160 0.000 0.012
#&gt; SRR1539249     4  0.4916      0.287 0.000 0.424 0.000 0.576
#&gt; SRR1539250     1  0.2675      0.843 0.892 0.000 0.100 0.008
#&gt; SRR1539251     1  0.2675      0.843 0.892 0.000 0.100 0.008
#&gt; SRR1539253     2  0.4454      0.754 0.000 0.692 0.000 0.308
#&gt; SRR1539252     1  0.3355      0.880 0.836 0.160 0.004 0.000
#&gt; SRR1539255     1  0.3172      0.879 0.840 0.160 0.000 0.000
#&gt; SRR1539254     1  0.2611      0.846 0.896 0.000 0.096 0.008
#&gt; SRR1539256     2  0.3219      0.875 0.000 0.836 0.000 0.164
#&gt; SRR1539257     1  0.2480      0.849 0.904 0.000 0.088 0.008
#&gt; SRR1539258     1  0.3172      0.879 0.840 0.160 0.000 0.000
#&gt; SRR1539259     2  0.4898      0.434 0.000 0.584 0.000 0.416
#&gt; SRR1539260     1  0.2611      0.846 0.896 0.000 0.096 0.008
#&gt; SRR1539262     2  0.3172      0.871 0.000 0.840 0.000 0.160
#&gt; SRR1539261     1  0.7095      0.819 0.668 0.160 0.076 0.096
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.1671      0.938 0.000 0.000 0.924 0.000 0.076
#&gt; SRR1539208     1  0.2929      0.927 0.820 0.000 0.000 0.000 0.180
#&gt; SRR1539211     1  0.2616      0.665 0.888 0.000 0.076 0.036 0.000
#&gt; SRR1539210     3  0.2074      0.893 0.036 0.000 0.920 0.000 0.044
#&gt; SRR1539209     2  0.4171      0.253 0.000 0.604 0.000 0.396 0.000
#&gt; SRR1539212     2  0.0324      0.907 0.000 0.992 0.004 0.004 0.000
#&gt; SRR1539214     5  0.3983      0.306 0.340 0.000 0.000 0.000 0.660
#&gt; SRR1539213     3  0.1908      0.940 0.000 0.000 0.908 0.000 0.092
#&gt; SRR1539215     4  0.0963      0.946 0.000 0.036 0.000 0.964 0.000
#&gt; SRR1539216     3  0.1671      0.938 0.000 0.000 0.924 0.000 0.076
#&gt; SRR1539217     1  0.2929      0.927 0.820 0.000 0.000 0.000 0.180
#&gt; SRR1539218     4  0.2516      0.904 0.000 0.140 0.000 0.860 0.000
#&gt; SRR1539220     5  0.3210      0.638 0.212 0.000 0.000 0.000 0.788
#&gt; SRR1539219     3  0.1908      0.940 0.000 0.000 0.908 0.000 0.092
#&gt; SRR1539221     4  0.0963      0.946 0.000 0.036 0.000 0.964 0.000
#&gt; SRR1539223     1  0.4300      0.346 0.524 0.000 0.000 0.000 0.476
#&gt; SRR1539224     4  0.2516      0.904 0.000 0.140 0.000 0.860 0.000
#&gt; SRR1539222     3  0.1671      0.938 0.000 0.000 0.924 0.000 0.076
#&gt; SRR1539225     3  0.1908      0.940 0.000 0.000 0.908 0.000 0.092
#&gt; SRR1539227     4  0.0963      0.946 0.000 0.036 0.000 0.964 0.000
#&gt; SRR1539226     1  0.2929      0.927 0.820 0.000 0.000 0.000 0.180
#&gt; SRR1539228     3  0.1908      0.940 0.000 0.000 0.908 0.000 0.092
#&gt; SRR1539229     1  0.2929      0.927 0.820 0.000 0.000 0.000 0.180
#&gt; SRR1539232     3  0.4210      0.484 0.000 0.000 0.588 0.000 0.412
#&gt; SRR1539230     4  0.1043      0.946 0.000 0.040 0.000 0.960 0.000
#&gt; SRR1539231     4  0.1043      0.946 0.000 0.040 0.000 0.960 0.000
#&gt; SRR1539234     2  0.2179      0.867 0.000 0.888 0.000 0.112 0.000
#&gt; SRR1539233     1  0.2929      0.927 0.820 0.000 0.000 0.000 0.180
#&gt; SRR1539235     5  0.0000      0.926 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539236     1  0.2929      0.927 0.820 0.000 0.000 0.000 0.180
#&gt; SRR1539237     2  0.0290      0.908 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1539238     5  0.0000      0.926 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539239     1  0.2929      0.927 0.820 0.000 0.000 0.000 0.180
#&gt; SRR1539242     1  0.2929      0.927 0.820 0.000 0.000 0.000 0.180
#&gt; SRR1539240     2  0.0609      0.907 0.000 0.980 0.000 0.020 0.000
#&gt; SRR1539241     5  0.0000      0.926 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539243     2  0.0000      0.908 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539244     5  0.0693      0.907 0.008 0.000 0.012 0.000 0.980
#&gt; SRR1539245     1  0.2929      0.927 0.820 0.000 0.000 0.000 0.180
#&gt; SRR1539246     2  0.1121      0.900 0.000 0.956 0.000 0.044 0.000
#&gt; SRR1539247     5  0.0000      0.926 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539248     1  0.2891      0.923 0.824 0.000 0.000 0.000 0.176
#&gt; SRR1539249     4  0.2471      0.904 0.000 0.136 0.000 0.864 0.000
#&gt; SRR1539250     5  0.0510      0.919 0.000 0.000 0.016 0.000 0.984
#&gt; SRR1539251     5  0.0510      0.919 0.000 0.000 0.016 0.000 0.984
#&gt; SRR1539253     2  0.2230      0.863 0.000 0.884 0.000 0.116 0.000
#&gt; SRR1539252     1  0.3003      0.919 0.812 0.000 0.000 0.000 0.188
#&gt; SRR1539255     1  0.2929      0.927 0.820 0.000 0.000 0.000 0.180
#&gt; SRR1539254     5  0.0000      0.926 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539256     2  0.0000      0.908 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.0000      0.926 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539258     1  0.2929      0.927 0.820 0.000 0.000 0.000 0.180
#&gt; SRR1539259     2  0.2471      0.833 0.000 0.864 0.000 0.136 0.000
#&gt; SRR1539260     5  0.0162      0.923 0.000 0.000 0.004 0.000 0.996
#&gt; SRR1539262     2  0.0162      0.907 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1539261     1  0.2554      0.669 0.892 0.000 0.072 0.036 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; SRR1539207     3   0.000      0.923 0.000 0.000 1.000 0.000 0.000  0
#&gt; SRR1539208     1   0.000      0.959 1.000 0.000 0.000 0.000 0.000  0
#&gt; SRR1539211     6   0.000      1.000 0.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1539210     3   0.000      0.923 0.000 0.000 1.000 0.000 0.000  0
#&gt; SRR1539209     4   0.380      0.212 0.000 0.424 0.000 0.576 0.000  0
#&gt; SRR1539212     2   0.000      0.944 0.000 1.000 0.000 0.000 0.000  0
#&gt; SRR1539214     5   0.383      0.202 0.444 0.000 0.000 0.000 0.556  0
#&gt; SRR1539213     3   0.000      0.923 0.000 0.000 1.000 0.000 0.000  0
#&gt; SRR1539215     4   0.000      0.930 0.000 0.000 0.000 1.000 0.000  0
#&gt; SRR1539216     3   0.000      0.923 0.000 0.000 1.000 0.000 0.000  0
#&gt; SRR1539217     1   0.000      0.959 1.000 0.000 0.000 0.000 0.000  0
#&gt; SRR1539218     4   0.000      0.930 0.000 0.000 0.000 1.000 0.000  0
#&gt; SRR1539220     5   0.346      0.522 0.312 0.000 0.000 0.000 0.688  0
#&gt; SRR1539219     3   0.000      0.923 0.000 0.000 1.000 0.000 0.000  0
#&gt; SRR1539221     4   0.000      0.930 0.000 0.000 0.000 1.000 0.000  0
#&gt; SRR1539223     1   0.378      0.195 0.588 0.000 0.000 0.000 0.412  0
#&gt; SRR1539224     4   0.000      0.930 0.000 0.000 0.000 1.000 0.000  0
#&gt; SRR1539222     3   0.000      0.923 0.000 0.000 1.000 0.000 0.000  0
#&gt; SRR1539225     3   0.000      0.923 0.000 0.000 1.000 0.000 0.000  0
#&gt; SRR1539227     4   0.000      0.930 0.000 0.000 0.000 1.000 0.000  0
#&gt; SRR1539226     1   0.000      0.959 1.000 0.000 0.000 0.000 0.000  0
#&gt; SRR1539228     3   0.000      0.923 0.000 0.000 1.000 0.000 0.000  0
#&gt; SRR1539229     1   0.000      0.959 1.000 0.000 0.000 0.000 0.000  0
#&gt; SRR1539232     3   0.384      0.222 0.000 0.000 0.548 0.000 0.452  0
#&gt; SRR1539230     4   0.000      0.930 0.000 0.000 0.000 1.000 0.000  0
#&gt; SRR1539231     4   0.000      0.930 0.000 0.000 0.000 1.000 0.000  0
#&gt; SRR1539234     2   0.270      0.774 0.000 0.812 0.000 0.188 0.000  0
#&gt; SRR1539233     1   0.000      0.959 1.000 0.000 0.000 0.000 0.000  0
#&gt; SRR1539235     5   0.000      0.903 0.000 0.000 0.000 0.000 1.000  0
#&gt; SRR1539236     1   0.000      0.959 1.000 0.000 0.000 0.000 0.000  0
#&gt; SRR1539237     2   0.000      0.944 0.000 1.000 0.000 0.000 0.000  0
#&gt; SRR1539238     5   0.000      0.903 0.000 0.000 0.000 0.000 1.000  0
#&gt; SRR1539239     1   0.000      0.959 1.000 0.000 0.000 0.000 0.000  0
#&gt; SRR1539242     1   0.000      0.959 1.000 0.000 0.000 0.000 0.000  0
#&gt; SRR1539240     2   0.000      0.944 0.000 1.000 0.000 0.000 0.000  0
#&gt; SRR1539241     5   0.000      0.903 0.000 0.000 0.000 0.000 1.000  0
#&gt; SRR1539243     2   0.000      0.944 0.000 1.000 0.000 0.000 0.000  0
#&gt; SRR1539244     5   0.000      0.903 0.000 0.000 0.000 0.000 1.000  0
#&gt; SRR1539245     1   0.000      0.959 1.000 0.000 0.000 0.000 0.000  0
#&gt; SRR1539246     2   0.026      0.941 0.000 0.992 0.000 0.008 0.000  0
#&gt; SRR1539247     5   0.000      0.903 0.000 0.000 0.000 0.000 1.000  0
#&gt; SRR1539248     1   0.000      0.959 1.000 0.000 0.000 0.000 0.000  0
#&gt; SRR1539249     4   0.000      0.930 0.000 0.000 0.000 1.000 0.000  0
#&gt; SRR1539250     5   0.000      0.903 0.000 0.000 0.000 0.000 1.000  0
#&gt; SRR1539251     5   0.000      0.903 0.000 0.000 0.000 0.000 1.000  0
#&gt; SRR1539253     2   0.282      0.755 0.000 0.796 0.000 0.204 0.000  0
#&gt; SRR1539252     1   0.000      0.959 1.000 0.000 0.000 0.000 0.000  0
#&gt; SRR1539255     1   0.000      0.959 1.000 0.000 0.000 0.000 0.000  0
#&gt; SRR1539254     5   0.000      0.903 0.000 0.000 0.000 0.000 1.000  0
#&gt; SRR1539256     2   0.000      0.944 0.000 1.000 0.000 0.000 0.000  0
#&gt; SRR1539257     5   0.000      0.903 0.000 0.000 0.000 0.000 1.000  0
#&gt; SRR1539258     1   0.000      0.959 1.000 0.000 0.000 0.000 0.000  0
#&gt; SRR1539259     2   0.026      0.941 0.000 0.992 0.000 0.008 0.000  0
#&gt; SRR1539260     5   0.000      0.903 0.000 0.000 0.000 0.000 1.000  0
#&gt; SRR1539262     2   0.000      0.944 0.000 1.000 0.000 0.000 0.000  0
#&gt; SRR1539261     6   0.000      1.000 0.000 0.000 0.000 0.000 0.000  1
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.997       0.998         0.4580 0.544   0.544
#> 3 3 0.935           0.926       0.967         0.4428 0.797   0.627
#> 4 4 0.920           0.878       0.939         0.0876 0.938   0.819
#> 5 5 0.824           0.837       0.863         0.0641 0.926   0.742
#> 6 6 0.962           0.932       0.957         0.0538 0.937   0.737
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1539207     1   0.000      0.997 1.000 0.000
#&gt; SRR1539208     1   0.000      0.997 1.000 0.000
#&gt; SRR1539211     1   0.000      0.997 1.000 0.000
#&gt; SRR1539210     1   0.000      0.997 1.000 0.000
#&gt; SRR1539209     2   0.000      1.000 0.000 1.000
#&gt; SRR1539212     2   0.000      1.000 0.000 1.000
#&gt; SRR1539214     1   0.000      0.997 1.000 0.000
#&gt; SRR1539213     1   0.000      0.997 1.000 0.000
#&gt; SRR1539215     2   0.000      1.000 0.000 1.000
#&gt; SRR1539216     1   0.000      0.997 1.000 0.000
#&gt; SRR1539217     1   0.000      0.997 1.000 0.000
#&gt; SRR1539218     2   0.000      1.000 0.000 1.000
#&gt; SRR1539220     1   0.000      0.997 1.000 0.000
#&gt; SRR1539219     1   0.000      0.997 1.000 0.000
#&gt; SRR1539221     2   0.000      1.000 0.000 1.000
#&gt; SRR1539223     1   0.000      0.997 1.000 0.000
#&gt; SRR1539224     2   0.000      1.000 0.000 1.000
#&gt; SRR1539222     1   0.000      0.997 1.000 0.000
#&gt; SRR1539225     1   0.000      0.997 1.000 0.000
#&gt; SRR1539227     2   0.000      1.000 0.000 1.000
#&gt; SRR1539226     1   0.000      0.997 1.000 0.000
#&gt; SRR1539228     1   0.000      0.997 1.000 0.000
#&gt; SRR1539229     1   0.000      0.997 1.000 0.000
#&gt; SRR1539232     1   0.000      0.997 1.000 0.000
#&gt; SRR1539230     2   0.000      1.000 0.000 1.000
#&gt; SRR1539231     2   0.000      1.000 0.000 1.000
#&gt; SRR1539234     2   0.000      1.000 0.000 1.000
#&gt; SRR1539233     1   0.000      0.997 1.000 0.000
#&gt; SRR1539235     1   0.000      0.997 1.000 0.000
#&gt; SRR1539236     1   0.000      0.997 1.000 0.000
#&gt; SRR1539237     2   0.000      1.000 0.000 1.000
#&gt; SRR1539238     1   0.000      0.997 1.000 0.000
#&gt; SRR1539239     1   0.000      0.997 1.000 0.000
#&gt; SRR1539242     1   0.000      0.997 1.000 0.000
#&gt; SRR1539240     2   0.000      1.000 0.000 1.000
#&gt; SRR1539241     1   0.000      0.997 1.000 0.000
#&gt; SRR1539243     2   0.000      1.000 0.000 1.000
#&gt; SRR1539244     1   0.000      0.997 1.000 0.000
#&gt; SRR1539245     1   0.000      0.997 1.000 0.000
#&gt; SRR1539246     2   0.000      1.000 0.000 1.000
#&gt; SRR1539247     1   0.000      0.997 1.000 0.000
#&gt; SRR1539248     1   0.000      0.997 1.000 0.000
#&gt; SRR1539249     2   0.000      1.000 0.000 1.000
#&gt; SRR1539250     1   0.000      0.997 1.000 0.000
#&gt; SRR1539251     1   0.000      0.997 1.000 0.000
#&gt; SRR1539253     2   0.000      1.000 0.000 1.000
#&gt; SRR1539252     1   0.000      0.997 1.000 0.000
#&gt; SRR1539255     1   0.000      0.997 1.000 0.000
#&gt; SRR1539254     1   0.000      0.997 1.000 0.000
#&gt; SRR1539256     2   0.000      1.000 0.000 1.000
#&gt; SRR1539257     1   0.000      0.997 1.000 0.000
#&gt; SRR1539258     1   0.000      0.997 1.000 0.000
#&gt; SRR1539259     2   0.000      1.000 0.000 1.000
#&gt; SRR1539260     1   0.000      0.997 1.000 0.000
#&gt; SRR1539262     2   0.000      1.000 0.000 1.000
#&gt; SRR1539261     1   0.443      0.899 0.908 0.092
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3
#&gt; SRR1539207     3  0.0000      0.951 0.000  0 1.000
#&gt; SRR1539208     1  0.1163      0.936 0.972  0 0.028
#&gt; SRR1539211     1  0.1529      0.931 0.960  0 0.040
#&gt; SRR1539210     3  0.0000      0.951 0.000  0 1.000
#&gt; SRR1539209     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539212     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539214     1  0.0747      0.940 0.984  0 0.016
#&gt; SRR1539213     3  0.0000      0.951 0.000  0 1.000
#&gt; SRR1539215     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539216     3  0.0000      0.951 0.000  0 1.000
#&gt; SRR1539217     1  0.1031      0.940 0.976  0 0.024
#&gt; SRR1539218     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539220     1  0.1289      0.936 0.968  0 0.032
#&gt; SRR1539219     3  0.0000      0.951 0.000  0 1.000
#&gt; SRR1539221     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539223     1  0.1753      0.930 0.952  0 0.048
#&gt; SRR1539224     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539222     3  0.0000      0.951 0.000  0 1.000
#&gt; SRR1539225     3  0.0000      0.951 0.000  0 1.000
#&gt; SRR1539227     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539226     1  0.0424      0.941 0.992  0 0.008
#&gt; SRR1539228     3  0.0000      0.951 0.000  0 1.000
#&gt; SRR1539229     1  0.0237      0.942 0.996  0 0.004
#&gt; SRR1539232     3  0.0237      0.948 0.004  0 0.996
#&gt; SRR1539230     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539231     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539233     1  0.0000      0.941 1.000  0 0.000
#&gt; SRR1539235     1  0.1289      0.935 0.968  0 0.032
#&gt; SRR1539236     1  0.0000      0.941 1.000  0 0.000
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539238     1  0.3482      0.862 0.872  0 0.128
#&gt; SRR1539239     1  0.0000      0.941 1.000  0 0.000
#&gt; SRR1539242     1  0.0000      0.941 1.000  0 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539241     1  0.2261      0.915 0.932  0 0.068
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539244     1  0.5291      0.646 0.732  0 0.268
#&gt; SRR1539245     1  0.0000      0.941 1.000  0 0.000
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539247     3  0.3116      0.845 0.108  0 0.892
#&gt; SRR1539248     1  0.0000      0.941 1.000  0 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539250     3  0.0000      0.951 0.000  0 1.000
#&gt; SRR1539251     3  0.0000      0.951 0.000  0 1.000
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539252     1  0.0237      0.942 0.996  0 0.004
#&gt; SRR1539255     1  0.0000      0.941 1.000  0 0.000
#&gt; SRR1539254     1  0.4002      0.829 0.840  0 0.160
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539257     1  0.6215      0.283 0.572  0 0.428
#&gt; SRR1539258     1  0.0000      0.941 1.000  0 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539260     3  0.6215      0.161 0.428  0 0.572
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539261     1  0.1163      0.936 0.972  0 0.028
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3  0.0000      0.937 0.000 0.000 1.000 0.000
#&gt; SRR1539208     1  0.0188      0.921 0.996 0.000 0.000 0.004
#&gt; SRR1539211     1  0.0376      0.919 0.992 0.004 0.000 0.004
#&gt; SRR1539210     3  0.0188      0.936 0.000 0.000 0.996 0.004
#&gt; SRR1539209     2  0.1867      0.962 0.000 0.928 0.000 0.072
#&gt; SRR1539212     2  0.1792      0.963 0.000 0.932 0.000 0.068
#&gt; SRR1539214     1  0.3726      0.713 0.788 0.000 0.000 0.212
#&gt; SRR1539213     3  0.0188      0.937 0.000 0.000 0.996 0.004
#&gt; SRR1539215     2  0.2281      0.948 0.000 0.904 0.000 0.096
#&gt; SRR1539216     3  0.0000      0.937 0.000 0.000 1.000 0.000
#&gt; SRR1539217     1  0.0000      0.922 1.000 0.000 0.000 0.000
#&gt; SRR1539218     2  0.1792      0.963 0.000 0.932 0.000 0.068
#&gt; SRR1539220     1  0.2216      0.865 0.908 0.000 0.000 0.092
#&gt; SRR1539219     3  0.0000      0.937 0.000 0.000 1.000 0.000
#&gt; SRR1539221     2  0.1867      0.962 0.000 0.928 0.000 0.072
#&gt; SRR1539223     1  0.0376      0.921 0.992 0.000 0.004 0.004
#&gt; SRR1539224     2  0.1792      0.963 0.000 0.932 0.000 0.068
#&gt; SRR1539222     3  0.0000      0.937 0.000 0.000 1.000 0.000
#&gt; SRR1539225     3  0.0188      0.937 0.000 0.000 0.996 0.004
#&gt; SRR1539227     2  0.1867      0.962 0.000 0.928 0.000 0.072
#&gt; SRR1539226     1  0.0707      0.918 0.980 0.000 0.000 0.020
#&gt; SRR1539228     3  0.0188      0.937 0.000 0.000 0.996 0.004
#&gt; SRR1539229     1  0.1211      0.908 0.960 0.000 0.000 0.040
#&gt; SRR1539232     3  0.2081      0.859 0.000 0.000 0.916 0.084
#&gt; SRR1539230     2  0.1867      0.962 0.000 0.928 0.000 0.072
#&gt; SRR1539231     2  0.1867      0.962 0.000 0.928 0.000 0.072
#&gt; SRR1539234     2  0.0188      0.968 0.000 0.996 0.000 0.004
#&gt; SRR1539233     1  0.1022      0.913 0.968 0.000 0.000 0.032
#&gt; SRR1539235     4  0.3450      0.801 0.156 0.000 0.008 0.836
#&gt; SRR1539236     1  0.0469      0.921 0.988 0.000 0.000 0.012
#&gt; SRR1539237     2  0.0000      0.968 0.000 1.000 0.000 0.000
#&gt; SRR1539238     4  0.5322      0.572 0.312 0.000 0.028 0.660
#&gt; SRR1539239     1  0.0000      0.922 1.000 0.000 0.000 0.000
#&gt; SRR1539242     1  0.0000      0.922 1.000 0.000 0.000 0.000
#&gt; SRR1539240     2  0.0000      0.968 0.000 1.000 0.000 0.000
#&gt; SRR1539241     1  0.5296     -0.142 0.496 0.000 0.008 0.496
#&gt; SRR1539243     2  0.0000      0.968 0.000 1.000 0.000 0.000
#&gt; SRR1539244     4  0.2048      0.807 0.064 0.000 0.008 0.928
#&gt; SRR1539245     1  0.2589      0.841 0.884 0.000 0.000 0.116
#&gt; SRR1539246     2  0.0000      0.968 0.000 1.000 0.000 0.000
#&gt; SRR1539247     4  0.4434      0.651 0.016 0.000 0.228 0.756
#&gt; SRR1539248     1  0.0000      0.922 1.000 0.000 0.000 0.000
#&gt; SRR1539249     2  0.0000      0.968 0.000 1.000 0.000 0.000
#&gt; SRR1539250     3  0.0336      0.935 0.000 0.000 0.992 0.008
#&gt; SRR1539251     3  0.0336      0.935 0.000 0.000 0.992 0.008
#&gt; SRR1539253     2  0.0000      0.968 0.000 1.000 0.000 0.000
#&gt; SRR1539252     1  0.0336      0.922 0.992 0.000 0.000 0.008
#&gt; SRR1539255     1  0.0336      0.922 0.992 0.000 0.000 0.008
#&gt; SRR1539254     1  0.4786      0.714 0.788 0.000 0.108 0.104
#&gt; SRR1539256     2  0.0000      0.968 0.000 1.000 0.000 0.000
#&gt; SRR1539257     4  0.3828      0.806 0.068 0.000 0.084 0.848
#&gt; SRR1539258     1  0.0000      0.922 1.000 0.000 0.000 0.000
#&gt; SRR1539259     2  0.0000      0.968 0.000 1.000 0.000 0.000
#&gt; SRR1539260     3  0.6752      0.198 0.280 0.000 0.588 0.132
#&gt; SRR1539262     2  0.0000      0.968 0.000 1.000 0.000 0.000
#&gt; SRR1539261     1  0.0188      0.921 0.996 0.000 0.000 0.004
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.0000      0.857 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539208     1  0.2561      0.858 0.884 0.000 0.000 0.020 0.096
#&gt; SRR1539211     1  0.2012      0.889 0.920 0.000 0.000 0.020 0.060
#&gt; SRR1539210     3  0.0963      0.840 0.000 0.000 0.964 0.000 0.036
#&gt; SRR1539209     4  0.4196      0.989 0.000 0.356 0.000 0.640 0.004
#&gt; SRR1539212     4  0.4624      0.961 0.000 0.340 0.000 0.636 0.024
#&gt; SRR1539214     1  0.4114      0.723 0.772 0.000 0.004 0.040 0.184
#&gt; SRR1539213     3  0.0000      0.857 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539215     4  0.4045      0.990 0.000 0.356 0.000 0.644 0.000
#&gt; SRR1539216     3  0.0000      0.857 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539217     1  0.0451      0.907 0.988 0.000 0.004 0.008 0.000
#&gt; SRR1539218     4  0.4060      0.991 0.000 0.360 0.000 0.640 0.000
#&gt; SRR1539220     1  0.2026      0.891 0.924 0.000 0.012 0.008 0.056
#&gt; SRR1539219     3  0.0000      0.857 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539221     4  0.4060      0.991 0.000 0.360 0.000 0.640 0.000
#&gt; SRR1539223     1  0.1943      0.892 0.924 0.000 0.000 0.020 0.056
#&gt; SRR1539224     4  0.4074      0.987 0.000 0.364 0.000 0.636 0.000
#&gt; SRR1539222     3  0.0000      0.857 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539225     3  0.0000      0.857 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539227     4  0.4060      0.991 0.000 0.360 0.000 0.640 0.000
#&gt; SRR1539226     1  0.1124      0.893 0.960 0.000 0.000 0.004 0.036
#&gt; SRR1539228     3  0.0000      0.857 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539229     1  0.1725      0.887 0.936 0.000 0.000 0.020 0.044
#&gt; SRR1539232     3  0.2139      0.808 0.000 0.000 0.916 0.052 0.032
#&gt; SRR1539230     4  0.4045      0.990 0.000 0.356 0.000 0.644 0.000
#&gt; SRR1539231     4  0.4045      0.990 0.000 0.356 0.000 0.644 0.000
#&gt; SRR1539234     2  0.1043      0.945 0.000 0.960 0.000 0.040 0.000
#&gt; SRR1539233     1  0.1082      0.907 0.964 0.000 0.000 0.008 0.028
#&gt; SRR1539235     5  0.2361      0.795 0.096 0.000 0.012 0.000 0.892
#&gt; SRR1539236     1  0.0771      0.909 0.976 0.000 0.000 0.004 0.020
#&gt; SRR1539237     2  0.0693      0.954 0.000 0.980 0.000 0.012 0.008
#&gt; SRR1539238     5  0.5572      0.741 0.176 0.016 0.080 0.020 0.708
#&gt; SRR1539239     1  0.0510      0.909 0.984 0.000 0.000 0.016 0.000
#&gt; SRR1539242     1  0.0510      0.909 0.984 0.000 0.000 0.016 0.000
#&gt; SRR1539240     2  0.0703      0.937 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1539241     5  0.5313      0.587 0.300 0.004 0.032 0.020 0.644
#&gt; SRR1539243     2  0.0865      0.934 0.000 0.972 0.000 0.004 0.024
#&gt; SRR1539244     5  0.1949      0.743 0.012 0.000 0.016 0.040 0.932
#&gt; SRR1539245     1  0.3039      0.791 0.836 0.000 0.000 0.012 0.152
#&gt; SRR1539246     2  0.0290      0.954 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1539247     5  0.4248      0.673 0.028 0.000 0.200 0.012 0.760
#&gt; SRR1539248     1  0.0992      0.907 0.968 0.000 0.000 0.008 0.024
#&gt; SRR1539249     2  0.0963      0.949 0.000 0.964 0.000 0.036 0.000
#&gt; SRR1539250     3  0.4880      0.533 0.036 0.000 0.696 0.016 0.252
#&gt; SRR1539251     3  0.4880      0.533 0.036 0.000 0.696 0.016 0.252
#&gt; SRR1539253     2  0.0963      0.949 0.000 0.964 0.000 0.036 0.000
#&gt; SRR1539252     1  0.0404      0.910 0.988 0.000 0.000 0.000 0.012
#&gt; SRR1539255     1  0.0324      0.909 0.992 0.000 0.000 0.004 0.004
#&gt; SRR1539254     1  0.6728     -0.129 0.484 0.000 0.152 0.020 0.344
#&gt; SRR1539256     2  0.1168      0.919 0.000 0.960 0.000 0.008 0.032
#&gt; SRR1539257     5  0.3130      0.775 0.040 0.000 0.072 0.016 0.872
#&gt; SRR1539258     1  0.0404      0.907 0.988 0.000 0.000 0.012 0.000
#&gt; SRR1539259     2  0.0510      0.955 0.000 0.984 0.000 0.016 0.000
#&gt; SRR1539260     3  0.7081     -0.254 0.232 0.000 0.396 0.016 0.356
#&gt; SRR1539262     2  0.1121      0.940 0.000 0.956 0.000 0.044 0.000
#&gt; SRR1539261     1  0.1725      0.895 0.936 0.000 0.000 0.020 0.044
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; SRR1539207     3  0.0146     0.9801 0.000 0.000 0.996 0.000 0.000 NA
#&gt; SRR1539208     5  0.4739     0.0913 0.436 0.000 0.000 0.000 0.516 NA
#&gt; SRR1539211     1  0.3293     0.8140 0.812 0.000 0.000 0.000 0.140 NA
#&gt; SRR1539210     3  0.1528     0.9358 0.000 0.000 0.936 0.000 0.048 NA
#&gt; SRR1539209     4  0.0260     0.9898 0.000 0.008 0.000 0.992 0.000 NA
#&gt; SRR1539212     4  0.0622     0.9849 0.000 0.012 0.000 0.980 0.008 NA
#&gt; SRR1539214     1  0.2605     0.8820 0.864 0.000 0.028 0.000 0.000 NA
#&gt; SRR1539213     3  0.0146     0.9808 0.000 0.000 0.996 0.000 0.000 NA
#&gt; SRR1539215     4  0.0146     0.9877 0.000 0.004 0.000 0.996 0.000 NA
#&gt; SRR1539216     3  0.0000     0.9810 0.000 0.000 1.000 0.000 0.000 NA
#&gt; SRR1539217     1  0.0000     0.9536 1.000 0.000 0.000 0.000 0.000 NA
#&gt; SRR1539218     4  0.0547     0.9929 0.000 0.020 0.000 0.980 0.000 NA
#&gt; SRR1539220     1  0.1865     0.9197 0.920 0.000 0.040 0.000 0.000 NA
#&gt; SRR1539219     3  0.0000     0.9810 0.000 0.000 1.000 0.000 0.000 NA
#&gt; SRR1539221     4  0.0547     0.9929 0.000 0.020 0.000 0.980 0.000 NA
#&gt; SRR1539223     1  0.2250     0.9037 0.896 0.000 0.000 0.000 0.064 NA
#&gt; SRR1539224     4  0.0458     0.9931 0.000 0.016 0.000 0.984 0.000 NA
#&gt; SRR1539222     3  0.0603     0.9724 0.000 0.000 0.980 0.000 0.004 NA
#&gt; SRR1539225     3  0.0146     0.9808 0.000 0.000 0.996 0.000 0.000 NA
#&gt; SRR1539227     4  0.0547     0.9929 0.000 0.020 0.000 0.980 0.000 NA
#&gt; SRR1539226     1  0.0603     0.9484 0.980 0.000 0.016 0.000 0.000 NA
#&gt; SRR1539228     3  0.0146     0.9808 0.000 0.000 0.996 0.000 0.000 NA
#&gt; SRR1539229     1  0.0146     0.9535 0.996 0.000 0.000 0.000 0.000 NA
#&gt; SRR1539232     3  0.1501     0.9415 0.000 0.000 0.924 0.000 0.000 NA
#&gt; SRR1539230     4  0.0458     0.9931 0.000 0.016 0.000 0.984 0.000 NA
#&gt; SRR1539231     4  0.0458     0.9931 0.000 0.016 0.000 0.984 0.000 NA
#&gt; SRR1539234     2  0.0363     0.9924 0.000 0.988 0.000 0.012 0.000 NA
#&gt; SRR1539233     1  0.0146     0.9536 0.996 0.000 0.000 0.000 0.000 NA
#&gt; SRR1539235     5  0.1267     0.8821 0.000 0.000 0.000 0.000 0.940 NA
#&gt; SRR1539236     1  0.0000     0.9536 1.000 0.000 0.000 0.000 0.000 NA
#&gt; SRR1539237     2  0.0146     0.9961 0.000 0.996 0.000 0.004 0.000 NA
#&gt; SRR1539238     5  0.0260     0.8899 0.000 0.000 0.000 0.000 0.992 NA
#&gt; SRR1539239     1  0.0520     0.9512 0.984 0.000 0.000 0.000 0.008 NA
#&gt; SRR1539242     1  0.0717     0.9488 0.976 0.000 0.000 0.000 0.016 NA
#&gt; SRR1539240     2  0.0000     0.9951 0.000 1.000 0.000 0.000 0.000 NA
#&gt; SRR1539241     5  0.0000     0.8899 0.000 0.000 0.000 0.000 1.000 NA
#&gt; SRR1539243     2  0.0000     0.9951 0.000 1.000 0.000 0.000 0.000 NA
#&gt; SRR1539244     5  0.3050     0.7969 0.000 0.000 0.000 0.000 0.764 NA
#&gt; SRR1539245     1  0.1387     0.9291 0.932 0.000 0.000 0.000 0.000 NA
#&gt; SRR1539246     2  0.0146     0.9961 0.000 0.996 0.000 0.004 0.000 NA
#&gt; SRR1539247     5  0.1701     0.8778 0.000 0.000 0.008 0.000 0.920 NA
#&gt; SRR1539248     1  0.1225     0.9380 0.952 0.000 0.000 0.000 0.036 NA
#&gt; SRR1539249     2  0.0260     0.9945 0.000 0.992 0.000 0.008 0.000 NA
#&gt; SRR1539250     5  0.2136     0.8643 0.000 0.000 0.048 0.000 0.904 NA
#&gt; SRR1539251     5  0.2136     0.8643 0.000 0.000 0.048 0.000 0.904 NA
#&gt; SRR1539253     2  0.0146     0.9961 0.000 0.996 0.000 0.004 0.000 NA
#&gt; SRR1539252     1  0.0146     0.9535 0.996 0.000 0.000 0.000 0.000 NA
#&gt; SRR1539255     1  0.0146     0.9535 0.996 0.000 0.000 0.000 0.000 NA
#&gt; SRR1539254     5  0.0858     0.8861 0.000 0.000 0.004 0.000 0.968 NA
#&gt; SRR1539256     2  0.0000     0.9951 0.000 1.000 0.000 0.000 0.000 NA
#&gt; SRR1539257     5  0.1958     0.8723 0.004 0.000 0.000 0.000 0.896 NA
#&gt; SRR1539258     1  0.0000     0.9536 1.000 0.000 0.000 0.000 0.000 NA
#&gt; SRR1539259     2  0.0260     0.9949 0.000 0.992 0.000 0.008 0.000 NA
#&gt; SRR1539260     5  0.0458     0.8890 0.000 0.000 0.000 0.000 0.984 NA
#&gt; SRR1539262     2  0.0260     0.9930 0.000 0.992 0.000 0.008 0.000 NA
#&gt; SRR1539261     1  0.2527     0.8960 0.884 0.004 0.000 0.000 0.064 NA
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 1.000           0.996       0.998         0.3600 0.836   0.699
#> 4 4 0.908           0.954       0.971         0.1461 0.914   0.774
#> 5 5 0.893           0.921       0.960         0.0111 0.995   0.982
#> 6 6 0.936           0.912       0.946         0.0793 0.932   0.764
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3
#&gt; SRR1539207     3  0.0000      1.000 0.000  0 1.000
#&gt; SRR1539208     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539211     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539210     3  0.0000      1.000 0.000  0 1.000
#&gt; SRR1539209     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539212     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539214     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539213     3  0.0000      1.000 0.000  0 1.000
#&gt; SRR1539215     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539216     3  0.0000      1.000 0.000  0 1.000
#&gt; SRR1539217     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539218     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539220     1  0.0747      0.985 0.984  0 0.016
#&gt; SRR1539219     3  0.0000      1.000 0.000  0 1.000
#&gt; SRR1539221     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539223     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539224     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539222     3  0.0000      1.000 0.000  0 1.000
#&gt; SRR1539225     3  0.0000      1.000 0.000  0 1.000
#&gt; SRR1539227     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539226     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539228     3  0.0000      1.000 0.000  0 1.000
#&gt; SRR1539229     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539232     3  0.0000      1.000 0.000  0 1.000
#&gt; SRR1539230     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539231     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539233     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539235     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539236     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539238     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539239     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539242     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539241     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539244     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539245     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539247     1  0.0747      0.985 0.984  0 0.016
#&gt; SRR1539248     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539250     1  0.1289      0.971 0.968  0 0.032
#&gt; SRR1539251     1  0.1289      0.971 0.968  0 0.032
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539252     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539255     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539254     1  0.0237      0.993 0.996  0 0.004
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539257     1  0.0747      0.985 0.984  0 0.016
#&gt; SRR1539258     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539260     1  0.0000      0.996 1.000  0 0.000
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539261     1  0.0000      0.996 1.000  0 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3    p4
#&gt; SRR1539207     3  0.0000      1.000 0.000  0 1.000 0.000
#&gt; SRR1539208     1  0.2081      0.881 0.916  0 0.000 0.084
#&gt; SRR1539211     4  0.0000      0.936 0.000  0 0.000 1.000
#&gt; SRR1539210     3  0.0000      1.000 0.000  0 1.000 0.000
#&gt; SRR1539209     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539212     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539214     1  0.0000      0.926 1.000  0 0.000 0.000
#&gt; SRR1539213     3  0.0000      1.000 0.000  0 1.000 0.000
#&gt; SRR1539215     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539216     3  0.0000      1.000 0.000  0 1.000 0.000
#&gt; SRR1539217     1  0.2081      0.881 0.916  0 0.000 0.084
#&gt; SRR1539218     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539220     1  0.0592      0.922 0.984  0 0.016 0.000
#&gt; SRR1539219     3  0.0000      1.000 0.000  0 1.000 0.000
#&gt; SRR1539221     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539223     1  0.2081      0.881 0.916  0 0.000 0.084
#&gt; SRR1539224     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539222     3  0.0000      1.000 0.000  0 1.000 0.000
#&gt; SRR1539225     3  0.0000      1.000 0.000  0 1.000 0.000
#&gt; SRR1539227     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539226     1  0.3219      0.838 0.836  0 0.000 0.164
#&gt; SRR1539228     3  0.0000      1.000 0.000  0 1.000 0.000
#&gt; SRR1539229     1  0.3219      0.838 0.836  0 0.000 0.164
#&gt; SRR1539232     3  0.0000      1.000 0.000  0 1.000 0.000
#&gt; SRR1539230     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539231     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539233     1  0.3266      0.835 0.832  0 0.000 0.168
#&gt; SRR1539235     1  0.0000      0.926 1.000  0 0.000 0.000
#&gt; SRR1539236     1  0.3219      0.838 0.836  0 0.000 0.164
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539238     1  0.0000      0.926 1.000  0 0.000 0.000
#&gt; SRR1539239     4  0.1637      0.968 0.060  0 0.000 0.940
#&gt; SRR1539242     4  0.1637      0.968 0.060  0 0.000 0.940
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539241     1  0.0000      0.926 1.000  0 0.000 0.000
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539244     1  0.0000      0.926 1.000  0 0.000 0.000
#&gt; SRR1539245     1  0.3219      0.838 0.836  0 0.000 0.164
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539247     1  0.0592      0.922 0.984  0 0.016 0.000
#&gt; SRR1539248     4  0.1637      0.968 0.060  0 0.000 0.940
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539250     1  0.1022      0.913 0.968  0 0.032 0.000
#&gt; SRR1539251     1  0.1022      0.913 0.968  0 0.032 0.000
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539252     1  0.0000      0.926 1.000  0 0.000 0.000
#&gt; SRR1539255     1  0.3219      0.838 0.836  0 0.000 0.164
#&gt; SRR1539254     1  0.0188      0.925 0.996  0 0.004 0.000
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539257     1  0.0592      0.922 0.984  0 0.016 0.000
#&gt; SRR1539258     4  0.1716      0.964 0.064  0 0.000 0.936
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539260     1  0.0000      0.926 1.000  0 0.000 0.000
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539261     4  0.0000      0.936 0.000  0 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3    p4    p5
#&gt; SRR1539207     3  0.0000      1.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1539208     5  0.1851      0.877 0.088  0 0.000 0.000 0.912
#&gt; SRR1539211     1  0.2891      0.732 0.824  0 0.000 0.176 0.000
#&gt; SRR1539210     4  0.2891      0.000 0.000  0 0.176 0.824 0.000
#&gt; SRR1539209     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539212     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539214     5  0.0000      0.923 0.000  0 0.000 0.000 1.000
#&gt; SRR1539213     3  0.0000      1.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1539215     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539216     3  0.0000      1.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1539217     5  0.1851      0.877 0.088  0 0.000 0.000 0.912
#&gt; SRR1539218     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539220     5  0.0566      0.919 0.000  0 0.012 0.004 0.984
#&gt; SRR1539219     3  0.0000      1.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1539221     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539223     5  0.1851      0.877 0.088  0 0.000 0.000 0.912
#&gt; SRR1539224     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539222     3  0.0000      1.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1539225     3  0.0000      1.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1539227     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539226     5  0.2891      0.836 0.176  0 0.000 0.000 0.824
#&gt; SRR1539228     3  0.0000      1.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1539229     5  0.2891      0.836 0.176  0 0.000 0.000 0.824
#&gt; SRR1539232     3  0.0000      1.000 0.000  0 1.000 0.000 0.000
#&gt; SRR1539230     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539231     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539233     5  0.2929      0.833 0.180  0 0.000 0.000 0.820
#&gt; SRR1539235     5  0.0000      0.923 0.000  0 0.000 0.000 1.000
#&gt; SRR1539236     5  0.2891      0.836 0.176  0 0.000 0.000 0.824
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539238     5  0.0000      0.923 0.000  0 0.000 0.000 1.000
#&gt; SRR1539239     1  0.1197      0.877 0.952  0 0.000 0.000 0.048
#&gt; SRR1539242     1  0.1197      0.877 0.952  0 0.000 0.000 0.048
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539241     5  0.0000      0.923 0.000  0 0.000 0.000 1.000
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539244     5  0.0162      0.922 0.004  0 0.000 0.000 0.996
#&gt; SRR1539245     5  0.2891      0.836 0.176  0 0.000 0.000 0.824
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539247     5  0.0566      0.919 0.000  0 0.012 0.004 0.984
#&gt; SRR1539248     1  0.1197      0.877 0.952  0 0.000 0.000 0.048
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539250     5  0.0992      0.910 0.000  0 0.024 0.008 0.968
#&gt; SRR1539251     5  0.0992      0.910 0.000  0 0.024 0.008 0.968
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539252     5  0.0000      0.923 0.000  0 0.000 0.000 1.000
#&gt; SRR1539255     5  0.2891      0.836 0.176  0 0.000 0.000 0.824
#&gt; SRR1539254     5  0.0162      0.922 0.000  0 0.000 0.004 0.996
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539257     5  0.0566      0.919 0.000  0 0.012 0.004 0.984
#&gt; SRR1539258     1  0.1270      0.872 0.948  0 0.000 0.000 0.052
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539260     5  0.0000      0.923 0.000  0 0.000 0.000 1.000
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1539261     1  0.2891      0.732 0.824  0 0.000 0.176 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539208     5  0.2309      0.889 0.084 0.000 0.000 0.028 0.888 0.000
#&gt; SRR1539211     1  0.0000      0.733 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539210     6  0.0000      0.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1539209     2  0.1327      0.964 0.000 0.936 0.000 0.064 0.000 0.000
#&gt; SRR1539212     2  0.1327      0.964 0.000 0.936 0.000 0.064 0.000 0.000
#&gt; SRR1539214     5  0.2823      0.764 0.000 0.000 0.000 0.204 0.796 0.000
#&gt; SRR1539213     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539215     2  0.1327      0.964 0.000 0.936 0.000 0.064 0.000 0.000
#&gt; SRR1539216     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539217     5  0.2309      0.889 0.084 0.000 0.000 0.028 0.888 0.000
#&gt; SRR1539218     2  0.1327      0.964 0.000 0.936 0.000 0.064 0.000 0.000
#&gt; SRR1539220     5  0.0000      0.934 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539219     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539221     2  0.1327      0.964 0.000 0.936 0.000 0.064 0.000 0.000
#&gt; SRR1539223     5  0.2309      0.889 0.084 0.000 0.000 0.028 0.888 0.000
#&gt; SRR1539224     2  0.1327      0.964 0.000 0.936 0.000 0.064 0.000 0.000
#&gt; SRR1539222     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539225     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539227     2  0.1327      0.964 0.000 0.936 0.000 0.064 0.000 0.000
#&gt; SRR1539226     4  0.1327      0.936 0.000 0.000 0.000 0.936 0.064 0.000
#&gt; SRR1539228     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539229     4  0.1327      0.936 0.000 0.000 0.000 0.936 0.064 0.000
#&gt; SRR1539232     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539230     2  0.1327      0.964 0.000 0.936 0.000 0.064 0.000 0.000
#&gt; SRR1539231     2  0.1327      0.964 0.000 0.936 0.000 0.064 0.000 0.000
#&gt; SRR1539234     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539233     4  0.1471      0.931 0.004 0.000 0.000 0.932 0.064 0.000
#&gt; SRR1539235     5  0.0458      0.937 0.000 0.000 0.000 0.016 0.984 0.000
#&gt; SRR1539236     4  0.1327      0.936 0.000 0.000 0.000 0.936 0.064 0.000
#&gt; SRR1539237     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539238     5  0.0458      0.937 0.000 0.000 0.000 0.016 0.984 0.000
#&gt; SRR1539239     1  0.2969      0.877 0.776 0.000 0.000 0.224 0.000 0.000
#&gt; SRR1539242     1  0.2969      0.877 0.776 0.000 0.000 0.224 0.000 0.000
#&gt; SRR1539240     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539241     5  0.0458      0.937 0.000 0.000 0.000 0.016 0.984 0.000
#&gt; SRR1539243     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539244     4  0.3288      0.600 0.000 0.000 0.000 0.724 0.276 0.000
#&gt; SRR1539245     4  0.1327      0.936 0.000 0.000 0.000 0.936 0.064 0.000
#&gt; SRR1539246     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539247     5  0.0000      0.934 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539248     1  0.2969      0.877 0.776 0.000 0.000 0.224 0.000 0.000
#&gt; SRR1539249     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539250     5  0.0508      0.927 0.000 0.000 0.012 0.000 0.984 0.004
#&gt; SRR1539251     5  0.0508      0.927 0.000 0.000 0.012 0.000 0.984 0.004
#&gt; SRR1539253     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539252     5  0.2823      0.764 0.000 0.000 0.000 0.204 0.796 0.000
#&gt; SRR1539255     4  0.1327      0.936 0.000 0.000 0.000 0.936 0.064 0.000
#&gt; SRR1539254     5  0.0363      0.936 0.000 0.000 0.000 0.012 0.988 0.000
#&gt; SRR1539256     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.0000      0.934 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539258     1  0.2996      0.873 0.772 0.000 0.000 0.228 0.000 0.000
#&gt; SRR1539259     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539260     5  0.0458      0.937 0.000 0.000 0.000 0.016 0.984 0.000
#&gt; SRR1539262     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539261     1  0.0000      0.733 1.000 0.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.749           0.957       0.906         0.3313 0.814   0.658
#> 4 4 0.755           0.721       0.765         0.1597 0.877   0.668
#> 5 5 0.716           0.586       0.657         0.0841 0.832   0.479
#> 6 6 0.714           0.752       0.763         0.0400 0.874   0.535
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1539207     3  0.5178      0.980 0.256 0.000 0.744
#&gt; SRR1539208     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539211     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539210     3  0.5178      0.980 0.256 0.000 0.744
#&gt; SRR1539209     2  0.4750      0.892 0.000 0.784 0.216
#&gt; SRR1539212     2  0.4750      0.892 0.000 0.784 0.216
#&gt; SRR1539214     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539213     3  0.5178      0.980 0.256 0.000 0.744
#&gt; SRR1539215     2  0.4750      0.892 0.000 0.784 0.216
#&gt; SRR1539216     3  0.5178      0.980 0.256 0.000 0.744
#&gt; SRR1539217     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539218     2  0.5178      0.891 0.000 0.744 0.256
#&gt; SRR1539220     1  0.0237      0.996 0.996 0.000 0.004
#&gt; SRR1539219     3  0.5178      0.980 0.256 0.000 0.744
#&gt; SRR1539221     2  0.5138      0.892 0.000 0.748 0.252
#&gt; SRR1539223     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539224     2  0.5178      0.891 0.000 0.744 0.256
#&gt; SRR1539222     3  0.5178      0.980 0.256 0.000 0.744
#&gt; SRR1539225     3  0.5178      0.980 0.256 0.000 0.744
#&gt; SRR1539227     2  0.5178      0.891 0.000 0.744 0.256
#&gt; SRR1539226     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539228     3  0.5178      0.980 0.256 0.000 0.744
#&gt; SRR1539229     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539232     3  0.5178      0.980 0.256 0.000 0.744
#&gt; SRR1539230     2  0.5138      0.892 0.000 0.748 0.252
#&gt; SRR1539231     2  0.5138      0.892 0.000 0.748 0.252
#&gt; SRR1539234     2  0.0000      0.900 0.000 1.000 0.000
#&gt; SRR1539233     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539235     1  0.0237      0.996 0.996 0.000 0.004
#&gt; SRR1539236     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539237     2  0.0000      0.900 0.000 1.000 0.000
#&gt; SRR1539238     1  0.0237      0.996 0.996 0.000 0.004
#&gt; SRR1539239     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539242     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539240     2  0.0000      0.900 0.000 1.000 0.000
#&gt; SRR1539241     1  0.0237      0.996 0.996 0.000 0.004
#&gt; SRR1539243     2  0.0000      0.900 0.000 1.000 0.000
#&gt; SRR1539244     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539245     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539246     2  0.1529      0.900 0.000 0.960 0.040
#&gt; SRR1539247     1  0.0237      0.996 0.996 0.000 0.004
#&gt; SRR1539248     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539249     2  0.1031      0.900 0.000 0.976 0.024
#&gt; SRR1539250     3  0.5760      0.903 0.328 0.000 0.672
#&gt; SRR1539251     3  0.5760      0.903 0.328 0.000 0.672
#&gt; SRR1539253     2  0.0237      0.899 0.000 0.996 0.004
#&gt; SRR1539252     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539255     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539254     1  0.0237      0.996 0.996 0.000 0.004
#&gt; SRR1539256     2  0.0000      0.900 0.000 1.000 0.000
#&gt; SRR1539257     1  0.0237      0.996 0.996 0.000 0.004
#&gt; SRR1539258     1  0.0000      0.998 1.000 0.000 0.000
#&gt; SRR1539259     2  0.1860      0.902 0.000 0.948 0.052
#&gt; SRR1539260     1  0.0237      0.996 0.996 0.000 0.004
#&gt; SRR1539262     2  0.1860      0.902 0.000 0.948 0.052
#&gt; SRR1539261     1  0.0000      0.998 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3  0.2053     0.9870 0.072 0.000 0.924 0.004
#&gt; SRR1539208     1  0.0469     0.7276 0.988 0.000 0.000 0.012
#&gt; SRR1539211     1  0.0592     0.7233 0.984 0.000 0.000 0.016
#&gt; SRR1539210     3  0.4093     0.9312 0.072 0.000 0.832 0.096
#&gt; SRR1539209     2  0.5167     0.8206 0.000 0.644 0.016 0.340
#&gt; SRR1539212     2  0.5233     0.8211 0.000 0.648 0.020 0.332
#&gt; SRR1539214     1  0.4981    -0.5209 0.536 0.000 0.000 0.464
#&gt; SRR1539213     3  0.1867     0.9877 0.072 0.000 0.928 0.000
#&gt; SRR1539215     2  0.4855     0.8207 0.000 0.644 0.004 0.352
#&gt; SRR1539216     3  0.2053     0.9870 0.072 0.000 0.924 0.004
#&gt; SRR1539217     1  0.2281     0.7295 0.904 0.000 0.000 0.096
#&gt; SRR1539218     2  0.4776     0.8215 0.000 0.624 0.000 0.376
#&gt; SRR1539220     4  0.5281     0.7225 0.464 0.000 0.008 0.528
#&gt; SRR1539219     3  0.1867     0.9877 0.072 0.000 0.928 0.000
#&gt; SRR1539221     2  0.4776     0.8215 0.000 0.624 0.000 0.376
#&gt; SRR1539223     1  0.4855    -0.2979 0.600 0.000 0.000 0.400
#&gt; SRR1539224     2  0.5055     0.8218 0.000 0.624 0.008 0.368
#&gt; SRR1539222     3  0.2871     0.9740 0.072 0.000 0.896 0.032
#&gt; SRR1539225     3  0.1867     0.9877 0.072 0.000 0.928 0.000
#&gt; SRR1539227     2  0.4776     0.8215 0.000 0.624 0.000 0.376
#&gt; SRR1539226     1  0.3219     0.7113 0.836 0.000 0.000 0.164
#&gt; SRR1539228     3  0.1867     0.9877 0.072 0.000 0.928 0.000
#&gt; SRR1539229     1  0.3219     0.7113 0.836 0.000 0.000 0.164
#&gt; SRR1539232     3  0.1867     0.9877 0.072 0.000 0.928 0.000
#&gt; SRR1539230     2  0.4776     0.8215 0.000 0.624 0.000 0.376
#&gt; SRR1539231     2  0.4776     0.8215 0.000 0.624 0.000 0.376
#&gt; SRR1539234     2  0.0188     0.8289 0.000 0.996 0.004 0.000
#&gt; SRR1539233     1  0.3219     0.7113 0.836 0.000 0.000 0.164
#&gt; SRR1539235     4  0.5281     0.7225 0.464 0.000 0.008 0.528
#&gt; SRR1539236     1  0.3219     0.7113 0.836 0.000 0.000 0.164
#&gt; SRR1539237     2  0.0188     0.8289 0.000 0.996 0.004 0.000
#&gt; SRR1539238     4  0.5281     0.7225 0.464 0.000 0.008 0.528
#&gt; SRR1539239     1  0.0000     0.7343 1.000 0.000 0.000 0.000
#&gt; SRR1539242     1  0.0000     0.7343 1.000 0.000 0.000 0.000
#&gt; SRR1539240     2  0.0000     0.8288 0.000 1.000 0.000 0.000
#&gt; SRR1539241     4  0.5281     0.7225 0.464 0.000 0.008 0.528
#&gt; SRR1539243     2  0.0000     0.8288 0.000 1.000 0.000 0.000
#&gt; SRR1539244     4  0.4989     0.6988 0.472 0.000 0.000 0.528
#&gt; SRR1539245     1  0.3219     0.7113 0.836 0.000 0.000 0.164
#&gt; SRR1539246     2  0.2174     0.8275 0.000 0.928 0.052 0.020
#&gt; SRR1539247     4  0.5281     0.7225 0.464 0.000 0.008 0.528
#&gt; SRR1539248     1  0.0000     0.7343 1.000 0.000 0.000 0.000
#&gt; SRR1539249     2  0.2174     0.8275 0.000 0.928 0.052 0.020
#&gt; SRR1539250     4  0.6980    -0.0243 0.116 0.000 0.400 0.484
#&gt; SRR1539251     4  0.6980    -0.0243 0.116 0.000 0.400 0.484
#&gt; SRR1539253     2  0.0000     0.8288 0.000 1.000 0.000 0.000
#&gt; SRR1539252     1  0.4605     0.1643 0.664 0.000 0.000 0.336
#&gt; SRR1539255     1  0.3219     0.7113 0.836 0.000 0.000 0.164
#&gt; SRR1539254     4  0.5281     0.7225 0.464 0.000 0.008 0.528
#&gt; SRR1539256     2  0.0188     0.8289 0.000 0.996 0.004 0.000
#&gt; SRR1539257     4  0.5281     0.7225 0.464 0.000 0.008 0.528
#&gt; SRR1539258     1  0.0000     0.7343 1.000 0.000 0.000 0.000
#&gt; SRR1539259     2  0.2670     0.8307 0.000 0.908 0.052 0.040
#&gt; SRR1539260     4  0.5281     0.7225 0.464 0.000 0.008 0.528
#&gt; SRR1539262     2  0.2670     0.8307 0.000 0.908 0.052 0.040
#&gt; SRR1539261     1  0.0469     0.7276 0.988 0.000 0.000 0.012
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.1405     0.9675 0.008 0.016 0.956 0.000 0.020
#&gt; SRR1539208     1  0.4678     0.8298 0.712 0.064 0.000 0.000 0.224
#&gt; SRR1539211     1  0.4850     0.8195 0.700 0.076 0.000 0.000 0.224
#&gt; SRR1539210     3  0.4440     0.8429 0.028 0.200 0.752 0.000 0.020
#&gt; SRR1539209     4  0.3558     0.6817 0.108 0.036 0.016 0.840 0.000
#&gt; SRR1539212     4  0.3558     0.6817 0.108 0.036 0.016 0.840 0.000
#&gt; SRR1539214     5  0.4036     0.4676 0.068 0.144 0.000 0.000 0.788
#&gt; SRR1539213     3  0.0609     0.9697 0.000 0.000 0.980 0.000 0.020
#&gt; SRR1539215     4  0.1996     0.7275 0.032 0.036 0.004 0.928 0.000
#&gt; SRR1539216     3  0.1405     0.9675 0.008 0.016 0.956 0.000 0.020
#&gt; SRR1539217     1  0.6132     0.4817 0.508 0.140 0.000 0.000 0.352
#&gt; SRR1539218     4  0.0162     0.7638 0.004 0.000 0.000 0.996 0.000
#&gt; SRR1539220     5  0.0451     0.5972 0.000 0.004 0.008 0.000 0.988
#&gt; SRR1539219     3  0.1059     0.9690 0.008 0.004 0.968 0.000 0.020
#&gt; SRR1539221     4  0.0000     0.7649 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539223     5  0.4787     0.3055 0.208 0.080 0.000 0.000 0.712
#&gt; SRR1539224     4  0.0963     0.7550 0.036 0.000 0.000 0.964 0.000
#&gt; SRR1539222     3  0.2541     0.9442 0.012 0.068 0.900 0.000 0.020
#&gt; SRR1539225     3  0.0609     0.9697 0.000 0.000 0.980 0.000 0.020
#&gt; SRR1539227     4  0.0162     0.7638 0.004 0.000 0.000 0.996 0.000
#&gt; SRR1539226     5  0.6362    -0.1411 0.368 0.168 0.000 0.000 0.464
#&gt; SRR1539228     3  0.0609     0.9697 0.000 0.000 0.980 0.000 0.020
#&gt; SRR1539229     5  0.6362    -0.1411 0.368 0.168 0.000 0.000 0.464
#&gt; SRR1539232     3  0.0609     0.9697 0.000 0.000 0.980 0.000 0.020
#&gt; SRR1539230     4  0.0000     0.7649 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539231     4  0.0000     0.7649 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539234     2  0.4470     0.9330 0.004 0.596 0.004 0.396 0.000
#&gt; SRR1539233     5  0.6362    -0.1411 0.368 0.168 0.000 0.000 0.464
#&gt; SRR1539235     5  0.0404     0.5993 0.000 0.000 0.012 0.000 0.988
#&gt; SRR1539236     5  0.6362    -0.1411 0.368 0.168 0.000 0.000 0.464
#&gt; SRR1539237     2  0.4321     0.9339 0.004 0.600 0.000 0.396 0.000
#&gt; SRR1539238     5  0.0404     0.5993 0.000 0.000 0.012 0.000 0.988
#&gt; SRR1539239     1  0.3424     0.8547 0.760 0.000 0.000 0.000 0.240
#&gt; SRR1539242     1  0.3424     0.8547 0.760 0.000 0.000 0.000 0.240
#&gt; SRR1539240     2  0.4171     0.9352 0.000 0.604 0.000 0.396 0.000
#&gt; SRR1539241     5  0.0404     0.5993 0.000 0.000 0.012 0.000 0.988
#&gt; SRR1539243     2  0.4171     0.9352 0.000 0.604 0.000 0.396 0.000
#&gt; SRR1539244     5  0.1544     0.5676 0.000 0.068 0.000 0.000 0.932
#&gt; SRR1539245     5  0.6362    -0.1411 0.368 0.168 0.000 0.000 0.464
#&gt; SRR1539246     2  0.5854     0.7889 0.096 0.468 0.000 0.436 0.000
#&gt; SRR1539247     5  0.0404     0.5993 0.000 0.000 0.012 0.000 0.988
#&gt; SRR1539248     1  0.3424     0.8547 0.760 0.000 0.000 0.000 0.240
#&gt; SRR1539249     2  0.5810     0.8071 0.092 0.480 0.000 0.428 0.000
#&gt; SRR1539250     5  0.5654     0.0803 0.012 0.064 0.340 0.000 0.584
#&gt; SRR1539251     5  0.5654     0.0803 0.012 0.064 0.340 0.000 0.584
#&gt; SRR1539253     2  0.4171     0.9352 0.000 0.604 0.000 0.396 0.000
#&gt; SRR1539252     5  0.5856     0.1936 0.224 0.172 0.000 0.000 0.604
#&gt; SRR1539255     5  0.6362    -0.1411 0.368 0.168 0.000 0.000 0.464
#&gt; SRR1539254     5  0.0404     0.5993 0.000 0.000 0.012 0.000 0.988
#&gt; SRR1539256     2  0.4470     0.9330 0.004 0.596 0.004 0.396 0.000
#&gt; SRR1539257     5  0.0404     0.5993 0.000 0.000 0.012 0.000 0.988
#&gt; SRR1539258     1  0.5821     0.7032 0.604 0.156 0.000 0.000 0.240
#&gt; SRR1539259     4  0.5844    -0.7353 0.096 0.420 0.000 0.484 0.000
#&gt; SRR1539260     5  0.0404     0.5993 0.000 0.000 0.012 0.000 0.988
#&gt; SRR1539262     4  0.5844    -0.7353 0.096 0.420 0.000 0.484 0.000
#&gt; SRR1539261     1  0.4617     0.8332 0.716 0.060 0.000 0.000 0.224
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; SRR1539207     3  0.1837      0.935 0.020 0.000 0.928 0.004 0.004 NA
#&gt; SRR1539208     1  0.4469      0.517 0.504 0.000 0.000 0.028 0.000 NA
#&gt; SRR1539211     1  0.4591      0.510 0.500 0.000 0.000 0.036 0.000 NA
#&gt; SRR1539210     3  0.6186      0.683 0.020 0.000 0.568 0.236 0.020 NA
#&gt; SRR1539209     4  0.7034      0.700 0.000 0.348 0.000 0.396 0.124 NA
#&gt; SRR1539212     4  0.7058      0.695 0.000 0.348 0.000 0.392 0.124 NA
#&gt; SRR1539214     1  0.4929      0.028 0.556 0.000 0.000 0.028 0.392 NA
#&gt; SRR1539213     3  0.0692      0.938 0.020 0.000 0.976 0.000 0.004 NA
#&gt; SRR1539215     4  0.5219      0.819 0.000 0.348 0.004 0.580 0.036 NA
#&gt; SRR1539216     3  0.1837      0.935 0.020 0.000 0.928 0.004 0.004 NA
#&gt; SRR1539217     1  0.2941      0.690 0.868 0.000 0.000 0.024 0.060 NA
#&gt; SRR1539218     4  0.3885      0.872 0.000 0.300 0.004 0.684 0.000 NA
#&gt; SRR1539220     5  0.3465      0.842 0.156 0.000 0.000 0.016 0.804 NA
#&gt; SRR1539219     3  0.1624      0.936 0.020 0.000 0.936 0.000 0.004 NA
#&gt; SRR1539221     4  0.3409      0.876 0.000 0.300 0.000 0.700 0.000 NA
#&gt; SRR1539223     5  0.6575      0.465 0.248 0.000 0.000 0.060 0.500 NA
#&gt; SRR1539224     4  0.5572      0.839 0.000 0.300 0.004 0.592 0.040 NA
#&gt; SRR1539222     3  0.3997      0.877 0.020 0.000 0.800 0.096 0.008 NA
#&gt; SRR1539225     3  0.0692      0.938 0.020 0.000 0.976 0.000 0.004 NA
#&gt; SRR1539227     4  0.3885      0.872 0.000 0.300 0.004 0.684 0.000 NA
#&gt; SRR1539226     1  0.2257      0.684 0.876 0.000 0.000 0.008 0.116 NA
#&gt; SRR1539228     3  0.0692      0.938 0.020 0.000 0.976 0.000 0.004 NA
#&gt; SRR1539229     1  0.2257      0.687 0.876 0.000 0.000 0.008 0.116 NA
#&gt; SRR1539232     3  0.0692      0.938 0.020 0.000 0.976 0.000 0.004 NA
#&gt; SRR1539230     4  0.3547      0.876 0.000 0.300 0.004 0.696 0.000 NA
#&gt; SRR1539231     4  0.3547      0.876 0.000 0.300 0.004 0.696 0.000 NA
#&gt; SRR1539234     2  0.0508      0.837 0.000 0.984 0.012 0.000 0.000 NA
#&gt; SRR1539233     1  0.2257      0.687 0.876 0.000 0.000 0.008 0.116 NA
#&gt; SRR1539235     5  0.2416      0.862 0.156 0.000 0.000 0.000 0.844 NA
#&gt; SRR1539236     1  0.2146      0.686 0.880 0.000 0.000 0.004 0.116 NA
#&gt; SRR1539237     2  0.0146      0.839 0.000 0.996 0.000 0.000 0.000 NA
#&gt; SRR1539238     5  0.2416      0.862 0.156 0.000 0.000 0.000 0.844 NA
#&gt; SRR1539239     1  0.3482      0.624 0.684 0.000 0.000 0.000 0.000 NA
#&gt; SRR1539242     1  0.3482      0.624 0.684 0.000 0.000 0.000 0.000 NA
#&gt; SRR1539240     2  0.0000      0.840 0.000 1.000 0.000 0.000 0.000 NA
#&gt; SRR1539241     5  0.2416      0.862 0.156 0.000 0.000 0.000 0.844 NA
#&gt; SRR1539243     2  0.0000      0.840 0.000 1.000 0.000 0.000 0.000 NA
#&gt; SRR1539244     5  0.3819      0.687 0.280 0.000 0.000 0.020 0.700 NA
#&gt; SRR1539245     1  0.2450      0.686 0.868 0.000 0.000 0.016 0.116 NA
#&gt; SRR1539246     2  0.4149      0.736 0.000 0.728 0.004 0.056 0.000 NA
#&gt; SRR1539247     5  0.2416      0.862 0.156 0.000 0.000 0.000 0.844 NA
#&gt; SRR1539248     1  0.3482      0.624 0.684 0.000 0.000 0.000 0.000 NA
#&gt; SRR1539249     2  0.3810      0.751 0.000 0.752 0.004 0.036 0.000 NA
#&gt; SRR1539250     5  0.5665      0.595 0.032 0.000 0.172 0.056 0.676 NA
#&gt; SRR1539251     5  0.5665      0.595 0.032 0.000 0.172 0.056 0.676 NA
#&gt; SRR1539253     2  0.0000      0.840 0.000 1.000 0.000 0.000 0.000 NA
#&gt; SRR1539252     1  0.4207      0.485 0.720 0.000 0.000 0.028 0.232 NA
#&gt; SRR1539255     1  0.2146      0.686 0.880 0.000 0.000 0.004 0.116 NA
#&gt; SRR1539254     5  0.2416      0.862 0.156 0.000 0.000 0.000 0.844 NA
#&gt; SRR1539256     2  0.0508      0.837 0.000 0.984 0.012 0.000 0.000 NA
#&gt; SRR1539257     5  0.2558      0.861 0.156 0.000 0.000 0.000 0.840 NA
#&gt; SRR1539258     1  0.0937      0.687 0.960 0.000 0.000 0.000 0.000 NA
#&gt; SRR1539259     2  0.4469      0.709 0.000 0.700 0.004 0.076 0.000 NA
#&gt; SRR1539260     5  0.2416      0.862 0.156 0.000 0.000 0.000 0.844 NA
#&gt; SRR1539262     2  0.4469      0.709 0.000 0.700 0.004 0.076 0.000 NA
#&gt; SRR1539261     1  0.4331      0.524 0.516 0.000 0.000 0.020 0.000 NA
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.976       0.991         0.4708 0.532   0.532
#> 3 3 0.840           0.846       0.928         0.3168 0.802   0.633
#> 4 4 1.000           0.940       0.974         0.1422 0.921   0.781
#> 5 5 0.875           0.872       0.890         0.0674 0.921   0.739
#> 6 6 0.809           0.807       0.875         0.0420 0.986   0.938
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1539207     1   0.000      0.988 1.000 0.000
#&gt; SRR1539208     1   0.000      0.988 1.000 0.000
#&gt; SRR1539211     1   0.975      0.301 0.592 0.408
#&gt; SRR1539210     1   0.000      0.988 1.000 0.000
#&gt; SRR1539209     2   0.000      0.994 0.000 1.000
#&gt; SRR1539212     2   0.000      0.994 0.000 1.000
#&gt; SRR1539214     1   0.000      0.988 1.000 0.000
#&gt; SRR1539213     1   0.000      0.988 1.000 0.000
#&gt; SRR1539215     2   0.000      0.994 0.000 1.000
#&gt; SRR1539216     1   0.000      0.988 1.000 0.000
#&gt; SRR1539217     1   0.000      0.988 1.000 0.000
#&gt; SRR1539218     2   0.000      0.994 0.000 1.000
#&gt; SRR1539220     1   0.000      0.988 1.000 0.000
#&gt; SRR1539219     1   0.000      0.988 1.000 0.000
#&gt; SRR1539221     2   0.000      0.994 0.000 1.000
#&gt; SRR1539223     1   0.000      0.988 1.000 0.000
#&gt; SRR1539224     2   0.000      0.994 0.000 1.000
#&gt; SRR1539222     1   0.000      0.988 1.000 0.000
#&gt; SRR1539225     1   0.000      0.988 1.000 0.000
#&gt; SRR1539227     2   0.000      0.994 0.000 1.000
#&gt; SRR1539226     1   0.000      0.988 1.000 0.000
#&gt; SRR1539228     1   0.000      0.988 1.000 0.000
#&gt; SRR1539229     1   0.000      0.988 1.000 0.000
#&gt; SRR1539232     1   0.000      0.988 1.000 0.000
#&gt; SRR1539230     2   0.000      0.994 0.000 1.000
#&gt; SRR1539231     2   0.000      0.994 0.000 1.000
#&gt; SRR1539234     2   0.000      0.994 0.000 1.000
#&gt; SRR1539233     1   0.000      0.988 1.000 0.000
#&gt; SRR1539235     1   0.000      0.988 1.000 0.000
#&gt; SRR1539236     1   0.000      0.988 1.000 0.000
#&gt; SRR1539237     2   0.000      0.994 0.000 1.000
#&gt; SRR1539238     1   0.000      0.988 1.000 0.000
#&gt; SRR1539239     1   0.000      0.988 1.000 0.000
#&gt; SRR1539242     1   0.000      0.988 1.000 0.000
#&gt; SRR1539240     2   0.000      0.994 0.000 1.000
#&gt; SRR1539241     1   0.000      0.988 1.000 0.000
#&gt; SRR1539243     2   0.000      0.994 0.000 1.000
#&gt; SRR1539244     1   0.000      0.988 1.000 0.000
#&gt; SRR1539245     1   0.000      0.988 1.000 0.000
#&gt; SRR1539246     2   0.000      0.994 0.000 1.000
#&gt; SRR1539247     1   0.000      0.988 1.000 0.000
#&gt; SRR1539248     1   0.000      0.988 1.000 0.000
#&gt; SRR1539249     2   0.000      0.994 0.000 1.000
#&gt; SRR1539250     1   0.000      0.988 1.000 0.000
#&gt; SRR1539251     1   0.000      0.988 1.000 0.000
#&gt; SRR1539253     2   0.000      0.994 0.000 1.000
#&gt; SRR1539252     1   0.000      0.988 1.000 0.000
#&gt; SRR1539255     1   0.000      0.988 1.000 0.000
#&gt; SRR1539254     1   0.000      0.988 1.000 0.000
#&gt; SRR1539256     2   0.000      0.994 0.000 1.000
#&gt; SRR1539257     1   0.000      0.988 1.000 0.000
#&gt; SRR1539258     1   0.000      0.988 1.000 0.000
#&gt; SRR1539259     2   0.000      0.994 0.000 1.000
#&gt; SRR1539260     1   0.000      0.988 1.000 0.000
#&gt; SRR1539262     2   0.000      0.994 0.000 1.000
#&gt; SRR1539261     2   0.506      0.871 0.112 0.888
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1539207     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539208     3  0.2959     0.8036 0.100 0.000 0.900
#&gt; SRR1539211     1  0.1411     0.6492 0.964 0.036 0.000
#&gt; SRR1539210     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539209     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539212     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539214     3  0.0592     0.9318 0.012 0.000 0.988
#&gt; SRR1539213     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539215     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539216     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539217     3  0.6026    -0.0385 0.376 0.000 0.624
#&gt; SRR1539218     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539220     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539219     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539221     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539223     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539224     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539222     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539225     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539227     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539226     1  0.6299     0.5020 0.524 0.000 0.476
#&gt; SRR1539228     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539229     1  0.6299     0.5020 0.524 0.000 0.476
#&gt; SRR1539232     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539230     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539231     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539234     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539233     1  0.6299     0.5020 0.524 0.000 0.476
#&gt; SRR1539235     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539236     1  0.6299     0.5020 0.524 0.000 0.476
#&gt; SRR1539237     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539238     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539239     1  0.0000     0.6696 1.000 0.000 0.000
#&gt; SRR1539242     1  0.0000     0.6696 1.000 0.000 0.000
#&gt; SRR1539240     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539241     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539243     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539244     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539245     1  0.6299     0.5020 0.524 0.000 0.476
#&gt; SRR1539246     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539247     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539248     1  0.0000     0.6696 1.000 0.000 0.000
#&gt; SRR1539249     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539250     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539251     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539253     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539252     3  0.6126    -0.1418 0.400 0.000 0.600
#&gt; SRR1539255     1  0.6299     0.5020 0.524 0.000 0.476
#&gt; SRR1539254     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539256     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539257     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539258     1  0.4504     0.6448 0.804 0.000 0.196
#&gt; SRR1539259     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539260     3  0.0000     0.9456 0.000 0.000 1.000
#&gt; SRR1539262     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; SRR1539261     1  0.1411     0.6492 0.964 0.036 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1539207     3  0.0188      0.955 0.000 0.000 0.996 0.004
#&gt; SRR1539208     3  0.4898      0.335 0.000 0.000 0.584 0.416
#&gt; SRR1539211     4  0.0000      0.969 0.000 0.000 0.000 1.000
#&gt; SRR1539210     3  0.1118      0.931 0.000 0.000 0.964 0.036
#&gt; SRR1539209     2  0.0188      0.998 0.004 0.996 0.000 0.000
#&gt; SRR1539212     2  0.0188      0.998 0.004 0.996 0.000 0.000
#&gt; SRR1539214     1  0.2081      0.877 0.916 0.000 0.084 0.000
#&gt; SRR1539213     3  0.0000      0.956 0.000 0.000 1.000 0.000
#&gt; SRR1539215     2  0.0188      0.998 0.004 0.996 0.000 0.000
#&gt; SRR1539216     3  0.0188      0.955 0.000 0.000 0.996 0.004
#&gt; SRR1539217     3  0.5742      0.358 0.368 0.000 0.596 0.036
#&gt; SRR1539218     2  0.0188      0.998 0.004 0.996 0.000 0.000
#&gt; SRR1539220     3  0.0000      0.956 0.000 0.000 1.000 0.000
#&gt; SRR1539219     3  0.0000      0.956 0.000 0.000 1.000 0.000
#&gt; SRR1539221     2  0.0188      0.998 0.004 0.996 0.000 0.000
#&gt; SRR1539223     3  0.1118      0.931 0.000 0.000 0.964 0.036
#&gt; SRR1539224     2  0.0188      0.998 0.004 0.996 0.000 0.000
#&gt; SRR1539222     3  0.0188      0.955 0.000 0.000 0.996 0.004
#&gt; SRR1539225     3  0.0000      0.956 0.000 0.000 1.000 0.000
#&gt; SRR1539227     2  0.0188      0.998 0.004 0.996 0.000 0.000
#&gt; SRR1539226     1  0.0188      0.944 0.996 0.000 0.004 0.000
#&gt; SRR1539228     3  0.0000      0.956 0.000 0.000 1.000 0.000
#&gt; SRR1539229     1  0.0188      0.944 0.996 0.000 0.004 0.000
#&gt; SRR1539232     3  0.0000      0.956 0.000 0.000 1.000 0.000
#&gt; SRR1539230     2  0.0188      0.998 0.004 0.996 0.000 0.000
#&gt; SRR1539231     2  0.0188      0.998 0.004 0.996 0.000 0.000
#&gt; SRR1539234     2  0.0000      0.998 0.000 1.000 0.000 0.000
#&gt; SRR1539233     1  0.0188      0.944 0.996 0.000 0.004 0.000
#&gt; SRR1539235     3  0.0000      0.956 0.000 0.000 1.000 0.000
#&gt; SRR1539236     1  0.0188      0.944 0.996 0.000 0.004 0.000
#&gt; SRR1539237     2  0.0000      0.998 0.000 1.000 0.000 0.000
#&gt; SRR1539238     3  0.0000      0.956 0.000 0.000 1.000 0.000
#&gt; SRR1539239     4  0.1118      0.981 0.036 0.000 0.000 0.964
#&gt; SRR1539242     4  0.1118      0.981 0.036 0.000 0.000 0.964
#&gt; SRR1539240     2  0.0000      0.998 0.000 1.000 0.000 0.000
#&gt; SRR1539241     3  0.0000      0.956 0.000 0.000 1.000 0.000
#&gt; SRR1539243     2  0.0000      0.998 0.000 1.000 0.000 0.000
#&gt; SRR1539244     1  0.3942      0.660 0.764 0.000 0.236 0.000
#&gt; SRR1539245     1  0.0188      0.944 0.996 0.000 0.004 0.000
#&gt; SRR1539246     2  0.0000      0.998 0.000 1.000 0.000 0.000
#&gt; SRR1539247     3  0.0000      0.956 0.000 0.000 1.000 0.000
#&gt; SRR1539248     4  0.1118      0.981 0.036 0.000 0.000 0.964
#&gt; SRR1539249     2  0.0000      0.998 0.000 1.000 0.000 0.000
#&gt; SRR1539250     3  0.0188      0.955 0.000 0.000 0.996 0.004
#&gt; SRR1539251     3  0.0188      0.955 0.000 0.000 0.996 0.004
#&gt; SRR1539253     2  0.0000      0.998 0.000 1.000 0.000 0.000
#&gt; SRR1539252     1  0.1302      0.914 0.956 0.000 0.044 0.000
#&gt; SRR1539255     1  0.0188      0.944 0.996 0.000 0.004 0.000
#&gt; SRR1539254     3  0.0000      0.956 0.000 0.000 1.000 0.000
#&gt; SRR1539256     2  0.0000      0.998 0.000 1.000 0.000 0.000
#&gt; SRR1539257     3  0.0000      0.956 0.000 0.000 1.000 0.000
#&gt; SRR1539258     1  0.0188      0.944 0.996 0.000 0.004 0.000
#&gt; SRR1539259     2  0.0000      0.998 0.000 1.000 0.000 0.000
#&gt; SRR1539260     3  0.0000      0.956 0.000 0.000 1.000 0.000
#&gt; SRR1539262     2  0.0000      0.998 0.000 1.000 0.000 0.000
#&gt; SRR1539261     4  0.0188      0.972 0.004 0.000 0.000 0.996
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.0000      0.838 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539208     3  0.6444      0.235 0.000 0.000 0.484 0.316 0.200
#&gt; SRR1539211     4  0.4088      0.766 0.000 0.000 0.000 0.632 0.368
#&gt; SRR1539210     3  0.2966      0.677 0.000 0.000 0.816 0.000 0.184
#&gt; SRR1539209     2  0.0703      0.988 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1539212     2  0.0703      0.988 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1539214     1  0.2723      0.819 0.864 0.000 0.124 0.000 0.012
#&gt; SRR1539213     3  0.0162      0.837 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1539215     2  0.0703      0.988 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1539216     3  0.0000      0.838 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539217     3  0.6411      0.236 0.364 0.000 0.488 0.008 0.140
#&gt; SRR1539218     2  0.0703      0.988 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1539220     3  0.0794      0.817 0.000 0.000 0.972 0.000 0.028
#&gt; SRR1539219     3  0.0000      0.838 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539221     2  0.0703      0.988 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1539223     3  0.3353      0.669 0.000 0.000 0.796 0.008 0.196
#&gt; SRR1539224     2  0.0703      0.988 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1539222     3  0.0000      0.838 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539225     3  0.0162      0.837 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1539227     2  0.0703      0.988 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1539226     1  0.0000      0.930 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539228     3  0.0162      0.837 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1539229     1  0.0794      0.928 0.972 0.000 0.000 0.000 0.028
#&gt; SRR1539232     3  0.0162      0.837 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1539230     2  0.0703      0.988 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1539231     2  0.0703      0.988 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1539234     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539233     1  0.0794      0.928 0.972 0.000 0.000 0.000 0.028
#&gt; SRR1539235     5  0.4227      0.903 0.000 0.000 0.420 0.000 0.580
#&gt; SRR1539236     1  0.0000      0.930 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539237     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539238     5  0.4227      0.903 0.000 0.000 0.420 0.000 0.580
#&gt; SRR1539239     4  0.0510      0.902 0.016 0.000 0.000 0.984 0.000
#&gt; SRR1539242     4  0.0290      0.904 0.008 0.000 0.000 0.992 0.000
#&gt; SRR1539240     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539241     5  0.4227      0.903 0.000 0.000 0.420 0.000 0.580
#&gt; SRR1539243     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539244     5  0.5507      0.319 0.316 0.000 0.088 0.000 0.596
#&gt; SRR1539245     1  0.0794      0.928 0.972 0.000 0.000 0.000 0.028
#&gt; SRR1539246     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539247     5  0.4227      0.903 0.000 0.000 0.420 0.000 0.580
#&gt; SRR1539248     4  0.0404      0.903 0.012 0.000 0.000 0.988 0.000
#&gt; SRR1539249     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539250     3  0.0510      0.828 0.000 0.000 0.984 0.000 0.016
#&gt; SRR1539251     3  0.0510      0.828 0.000 0.000 0.984 0.000 0.016
#&gt; SRR1539253     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539252     1  0.2286      0.844 0.888 0.000 0.108 0.000 0.004
#&gt; SRR1539255     1  0.0000      0.930 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539254     5  0.4227      0.903 0.000 0.000 0.420 0.000 0.580
#&gt; SRR1539256     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.4227      0.903 0.000 0.000 0.420 0.000 0.580
#&gt; SRR1539258     1  0.2280      0.837 0.880 0.000 0.000 0.120 0.000
#&gt; SRR1539259     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539260     5  0.4227      0.903 0.000 0.000 0.420 0.000 0.580
#&gt; SRR1539262     2  0.0000      0.989 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539261     4  0.2966      0.862 0.000 0.000 0.000 0.816 0.184
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.0000     0.8608 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539208     6  0.6054     0.2424 0.000 0.000 0.304 0.080 0.072 0.544
#&gt; SRR1539211     6  0.2048     0.3779 0.000 0.000 0.000 0.120 0.000 0.880
#&gt; SRR1539210     3  0.4482     0.3970 0.000 0.000 0.628 0.000 0.048 0.324
#&gt; SRR1539209     2  0.3748     0.8812 0.000 0.792 0.000 0.004 0.112 0.092
#&gt; SRR1539212     2  0.3748     0.8812 0.000 0.792 0.000 0.004 0.112 0.092
#&gt; SRR1539214     1  0.3003     0.7084 0.812 0.000 0.172 0.000 0.016 0.000
#&gt; SRR1539213     3  0.0000     0.8608 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539215     2  0.3748     0.8812 0.000 0.792 0.000 0.004 0.112 0.092
#&gt; SRR1539216     3  0.0000     0.8608 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539217     3  0.6767    -0.0583 0.368 0.000 0.392 0.000 0.060 0.180
#&gt; SRR1539218     2  0.3748     0.8812 0.000 0.792 0.000 0.004 0.112 0.092
#&gt; SRR1539220     3  0.1007     0.8383 0.000 0.000 0.956 0.000 0.044 0.000
#&gt; SRR1539219     3  0.0000     0.8608 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539221     2  0.3748     0.8812 0.000 0.792 0.000 0.004 0.112 0.092
#&gt; SRR1539223     3  0.4750     0.3501 0.000 0.000 0.596 0.000 0.064 0.340
#&gt; SRR1539224     2  0.3748     0.8812 0.000 0.792 0.000 0.004 0.112 0.092
#&gt; SRR1539222     3  0.0000     0.8608 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539225     3  0.0000     0.8608 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539227     2  0.3748     0.8812 0.000 0.792 0.000 0.004 0.112 0.092
#&gt; SRR1539226     1  0.0508     0.8880 0.984 0.000 0.000 0.000 0.012 0.004
#&gt; SRR1539228     3  0.0000     0.8608 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539229     1  0.1700     0.8822 0.928 0.000 0.000 0.000 0.048 0.024
#&gt; SRR1539232     3  0.0000     0.8608 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539230     2  0.3748     0.8812 0.000 0.792 0.000 0.004 0.112 0.092
#&gt; SRR1539231     2  0.3748     0.8812 0.000 0.792 0.000 0.004 0.112 0.092
#&gt; SRR1539234     2  0.0000     0.8941 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539233     1  0.1890     0.8778 0.916 0.000 0.000 0.000 0.060 0.024
#&gt; SRR1539235     5  0.2969     0.9572 0.000 0.000 0.224 0.000 0.776 0.000
#&gt; SRR1539236     1  0.0000     0.8867 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539237     2  0.0000     0.8941 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539238     5  0.2969     0.9572 0.000 0.000 0.224 0.000 0.776 0.000
#&gt; SRR1539239     4  0.0146     0.9935 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; SRR1539242     4  0.0146     0.9935 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; SRR1539240     2  0.0000     0.8941 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539241     5  0.2969     0.9572 0.000 0.000 0.224 0.000 0.776 0.000
#&gt; SRR1539243     2  0.0000     0.8941 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539244     5  0.3083     0.6700 0.132 0.000 0.040 0.000 0.828 0.000
#&gt; SRR1539245     1  0.1549     0.8835 0.936 0.000 0.000 0.000 0.044 0.020
#&gt; SRR1539246     2  0.0000     0.8941 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539247     5  0.2969     0.9572 0.000 0.000 0.224 0.000 0.776 0.000
#&gt; SRR1539248     4  0.0363     0.9869 0.012 0.000 0.000 0.988 0.000 0.000
#&gt; SRR1539249     2  0.0000     0.8941 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539250     3  0.1075     0.8357 0.000 0.000 0.952 0.000 0.048 0.000
#&gt; SRR1539251     3  0.1075     0.8357 0.000 0.000 0.952 0.000 0.048 0.000
#&gt; SRR1539253     2  0.0000     0.8941 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539252     1  0.1814     0.8159 0.900 0.000 0.100 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0000     0.8867 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539254     5  0.2969     0.9572 0.000 0.000 0.224 0.000 0.776 0.000
#&gt; SRR1539256     2  0.0000     0.8941 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.2969     0.9572 0.000 0.000 0.224 0.000 0.776 0.000
#&gt; SRR1539258     1  0.3126     0.6287 0.752 0.000 0.000 0.248 0.000 0.000
#&gt; SRR1539259     2  0.0000     0.8941 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539260     5  0.2969     0.9572 0.000 0.000 0.224 0.000 0.776 0.000
#&gt; SRR1539262     2  0.0000     0.8941 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539261     6  0.3860    -0.1973 0.000 0.000 0.000 0.472 0.000 0.528
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.836           0.876       0.929         0.4036 0.836   0.699
#> 4 4 1.000           0.953       0.983         0.1632 0.873   0.667
#> 5 5 0.987           0.948       0.980         0.0780 0.942   0.769
#> 6 6 0.987           0.958       0.980         0.0294 0.973   0.865
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 4 5
```

There is also optional best $k$ = 2 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1 p2   p3
#&gt; SRR1539207     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539208     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539211     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539210     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539209     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539212     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539214     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539213     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539215     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539216     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539217     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539218     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539220     1   0.595      0.642 0.64  0 0.36
#&gt; SRR1539219     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539221     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539223     1   0.595      0.642 0.64  0 0.36
#&gt; SRR1539224     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539222     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539225     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539227     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539226     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539228     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539229     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539232     3   0.000      1.000 0.00  0 1.00
#&gt; SRR1539230     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539231     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539234     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539233     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539235     1   0.595      0.642 0.64  0 0.36
#&gt; SRR1539236     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539237     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539238     1   0.595      0.642 0.64  0 0.36
#&gt; SRR1539239     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539242     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539240     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539241     1   0.595      0.642 0.64  0 0.36
#&gt; SRR1539243     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539244     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539245     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539246     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539247     1   0.595      0.642 0.64  0 0.36
#&gt; SRR1539248     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539249     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539250     1   0.604      0.613 0.62  0 0.38
#&gt; SRR1539251     1   0.604      0.613 0.62  0 0.38
#&gt; SRR1539253     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539252     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539255     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539254     1   0.595      0.642 0.64  0 0.36
#&gt; SRR1539256     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539257     1   0.595      0.642 0.64  0 0.36
#&gt; SRR1539258     1   0.000      0.827 1.00  0 0.00
#&gt; SRR1539259     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539260     1   0.595      0.642 0.64  0 0.36
#&gt; SRR1539262     2   0.000      1.000 0.00  1 0.00
#&gt; SRR1539261     1   0.000      0.827 1.00  0 0.00
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2 p3    p4
#&gt; SRR1539207     3   0.000      1.000 0.000  0  1 0.000
#&gt; SRR1539208     1   0.419      0.601 0.732  0  0 0.268
#&gt; SRR1539211     1   0.000      0.979 1.000  0  0 0.000
#&gt; SRR1539210     3   0.000      1.000 0.000  0  1 0.000
#&gt; SRR1539209     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539212     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539214     4   0.419      0.619 0.268  0  0 0.732
#&gt; SRR1539213     3   0.000      1.000 0.000  0  1 0.000
#&gt; SRR1539215     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539216     3   0.000      1.000 0.000  0  1 0.000
#&gt; SRR1539217     1   0.000      0.979 1.000  0  0 0.000
#&gt; SRR1539218     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539220     4   0.000      0.932 0.000  0  0 1.000
#&gt; SRR1539219     3   0.000      1.000 0.000  0  1 0.000
#&gt; SRR1539221     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539223     4   0.494      0.197 0.436  0  0 0.564
#&gt; SRR1539224     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539222     3   0.000      1.000 0.000  0  1 0.000
#&gt; SRR1539225     3   0.000      1.000 0.000  0  1 0.000
#&gt; SRR1539227     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539226     1   0.000      0.979 1.000  0  0 0.000
#&gt; SRR1539228     3   0.000      1.000 0.000  0  1 0.000
#&gt; SRR1539229     1   0.000      0.979 1.000  0  0 0.000
#&gt; SRR1539232     3   0.000      1.000 0.000  0  1 0.000
#&gt; SRR1539230     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539231     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539234     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539233     1   0.000      0.979 1.000  0  0 0.000
#&gt; SRR1539235     4   0.000      0.932 0.000  0  0 1.000
#&gt; SRR1539236     1   0.000      0.979 1.000  0  0 0.000
#&gt; SRR1539237     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539238     4   0.000      0.932 0.000  0  0 1.000
#&gt; SRR1539239     1   0.000      0.979 1.000  0  0 0.000
#&gt; SRR1539242     1   0.000      0.979 1.000  0  0 0.000
#&gt; SRR1539240     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539241     4   0.000      0.932 0.000  0  0 1.000
#&gt; SRR1539243     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539244     4   0.000      0.932 0.000  0  0 1.000
#&gt; SRR1539245     1   0.000      0.979 1.000  0  0 0.000
#&gt; SRR1539246     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539247     4   0.000      0.932 0.000  0  0 1.000
#&gt; SRR1539248     1   0.000      0.979 1.000  0  0 0.000
#&gt; SRR1539249     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539250     4   0.000      0.932 0.000  0  0 1.000
#&gt; SRR1539251     4   0.000      0.932 0.000  0  0 1.000
#&gt; SRR1539253     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539252     1   0.000      0.979 1.000  0  0 0.000
#&gt; SRR1539255     1   0.000      0.979 1.000  0  0 0.000
#&gt; SRR1539254     4   0.000      0.932 0.000  0  0 1.000
#&gt; SRR1539256     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539257     4   0.000      0.932 0.000  0  0 1.000
#&gt; SRR1539258     1   0.000      0.979 1.000  0  0 0.000
#&gt; SRR1539259     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539260     4   0.000      0.932 0.000  0  0 1.000
#&gt; SRR1539262     2   0.000      1.000 0.000  1  0 0.000
#&gt; SRR1539261     1   0.000      0.979 1.000  0  0 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3    p4    p5
#&gt; SRR1539207     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539208     1   0.361      0.601 0.732 0.000  0 0.000 0.268
#&gt; SRR1539211     1   0.000      0.979 1.000 0.000  0 0.000 0.000
#&gt; SRR1539210     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539209     4   0.000      0.983 0.000 0.000  0 1.000 0.000
#&gt; SRR1539212     4   0.238      0.853 0.000 0.128  0 0.872 0.000
#&gt; SRR1539214     5   0.361      0.619 0.268 0.000  0 0.000 0.732
#&gt; SRR1539213     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539215     4   0.000      0.983 0.000 0.000  0 1.000 0.000
#&gt; SRR1539216     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539217     1   0.000      0.979 1.000 0.000  0 0.000 0.000
#&gt; SRR1539218     4   0.000      0.983 0.000 0.000  0 1.000 0.000
#&gt; SRR1539220     5   0.000      0.931 0.000 0.000  0 0.000 1.000
#&gt; SRR1539219     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539221     4   0.000      0.983 0.000 0.000  0 1.000 0.000
#&gt; SRR1539223     5   0.426      0.197 0.436 0.000  0 0.000 0.564
#&gt; SRR1539224     4   0.000      0.983 0.000 0.000  0 1.000 0.000
#&gt; SRR1539222     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539225     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539227     4   0.000      0.983 0.000 0.000  0 1.000 0.000
#&gt; SRR1539226     1   0.000      0.979 1.000 0.000  0 0.000 0.000
#&gt; SRR1539228     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539229     1   0.000      0.979 1.000 0.000  0 0.000 0.000
#&gt; SRR1539232     3   0.000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1539230     4   0.000      0.983 0.000 0.000  0 1.000 0.000
#&gt; SRR1539231     4   0.000      0.983 0.000 0.000  0 1.000 0.000
#&gt; SRR1539234     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539233     1   0.000      0.979 1.000 0.000  0 0.000 0.000
#&gt; SRR1539235     5   0.000      0.931 0.000 0.000  0 0.000 1.000
#&gt; SRR1539236     1   0.000      0.979 1.000 0.000  0 0.000 0.000
#&gt; SRR1539237     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539238     5   0.000      0.931 0.000 0.000  0 0.000 1.000
#&gt; SRR1539239     1   0.000      0.979 1.000 0.000  0 0.000 0.000
#&gt; SRR1539242     1   0.000      0.979 1.000 0.000  0 0.000 0.000
#&gt; SRR1539240     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539241     5   0.000      0.931 0.000 0.000  0 0.000 1.000
#&gt; SRR1539243     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539244     5   0.000      0.931 0.000 0.000  0 0.000 1.000
#&gt; SRR1539245     1   0.000      0.979 1.000 0.000  0 0.000 0.000
#&gt; SRR1539246     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539247     5   0.000      0.931 0.000 0.000  0 0.000 1.000
#&gt; SRR1539248     1   0.000      0.979 1.000 0.000  0 0.000 0.000
#&gt; SRR1539249     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539250     5   0.000      0.931 0.000 0.000  0 0.000 1.000
#&gt; SRR1539251     5   0.000      0.931 0.000 0.000  0 0.000 1.000
#&gt; SRR1539253     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539252     1   0.000      0.979 1.000 0.000  0 0.000 0.000
#&gt; SRR1539255     1   0.000      0.979 1.000 0.000  0 0.000 0.000
#&gt; SRR1539254     5   0.000      0.931 0.000 0.000  0 0.000 1.000
#&gt; SRR1539256     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539257     5   0.000      0.931 0.000 0.000  0 0.000 1.000
#&gt; SRR1539258     1   0.000      0.979 1.000 0.000  0 0.000 0.000
#&gt; SRR1539259     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539260     5   0.000      0.931 0.000 0.000  0 0.000 1.000
#&gt; SRR1539262     2   0.000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1539261     1   0.000      0.979 1.000 0.000  0 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539208     1  0.1152      0.950 0.952 0.000 0.000 0.000 0.044 0.004
#&gt; SRR1539211     1  0.0458      0.988 0.984 0.000 0.000 0.000 0.000 0.016
#&gt; SRR1539210     6  0.0458      0.864 0.000 0.000 0.016 0.000 0.000 0.984
#&gt; SRR1539209     4  0.0000      0.980 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539212     4  0.2135      0.829 0.000 0.128 0.000 0.872 0.000 0.000
#&gt; SRR1539214     5  0.1010      0.907 0.036 0.000 0.000 0.000 0.960 0.004
#&gt; SRR1539213     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539215     4  0.0000      0.980 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539216     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539217     1  0.0146      0.989 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1539218     4  0.0000      0.980 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539220     5  0.2854      0.744 0.000 0.000 0.000 0.000 0.792 0.208
#&gt; SRR1539219     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539221     4  0.0000      0.980 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539223     6  0.3607      0.502 0.000 0.000 0.000 0.000 0.348 0.652
#&gt; SRR1539224     4  0.0000      0.980 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539222     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539225     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539227     4  0.0000      0.980 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539226     1  0.0146      0.990 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1539228     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539229     1  0.0146      0.990 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1539232     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539230     4  0.0000      0.980 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539231     4  0.0000      0.980 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539233     1  0.0146      0.990 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1539235     5  0.0000      0.945 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539236     1  0.0146      0.990 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1539237     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539238     5  0.0000      0.945 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539239     1  0.0363      0.989 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; SRR1539242     1  0.0363      0.989 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; SRR1539240     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539241     5  0.0000      0.945 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539243     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539244     5  0.0000      0.945 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539245     1  0.0146      0.990 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1539246     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539247     5  0.0000      0.945 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539248     1  0.0363      0.989 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; SRR1539249     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539250     6  0.0547      0.875 0.000 0.000 0.000 0.000 0.020 0.980
#&gt; SRR1539251     6  0.0547      0.875 0.000 0.000 0.000 0.000 0.020 0.980
#&gt; SRR1539253     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539252     1  0.0000      0.990 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0000      0.990 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539254     5  0.0000      0.945 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539256     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539257     5  0.2527      0.792 0.000 0.000 0.000 0.000 0.832 0.168
#&gt; SRR1539258     1  0.0363      0.989 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; SRR1539259     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539260     5  0.0000      0.945 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539262     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1539261     1  0.0363      0.989 0.988 0.000 0.000 0.000 0.000 0.012
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:mclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 1.000           0.991       0.995         0.3628 0.836   0.699
#> 4 4 0.843           0.810       0.922         0.1501 0.905   0.749
#> 5 5 0.802           0.777       0.880         0.0292 1.000   1.000
#> 6 6 0.933           0.884       0.930         0.0575 0.902   0.681
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3
#&gt; SRR1539207     3  0.0000      0.998 0.000  0 1.000
#&gt; SRR1539208     1  0.2356      0.929 0.928  0 0.072
#&gt; SRR1539211     1  0.2356      0.929 0.928  0 0.072
#&gt; SRR1539210     3  0.0000      0.998 0.000  0 1.000
#&gt; SRR1539209     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539212     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539214     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539213     3  0.0000      0.998 0.000  0 1.000
#&gt; SRR1539215     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539216     3  0.0000      0.998 0.000  0 1.000
#&gt; SRR1539217     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539218     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539220     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539219     3  0.0000      0.998 0.000  0 1.000
#&gt; SRR1539221     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539223     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539224     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539222     3  0.0000      0.998 0.000  0 1.000
#&gt; SRR1539225     3  0.0000      0.998 0.000  0 1.000
#&gt; SRR1539227     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539226     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539228     3  0.0000      0.998 0.000  0 1.000
#&gt; SRR1539229     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539232     3  0.0747      0.982 0.016  0 0.984
#&gt; SRR1539230     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539231     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539233     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539235     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539236     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539238     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539239     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539242     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539241     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539244     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539245     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539247     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539248     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539250     1  0.0747      0.979 0.984  0 0.016
#&gt; SRR1539251     1  0.0747      0.979 0.984  0 0.016
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539252     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539255     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539254     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539257     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539258     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539260     1  0.0000      0.991 1.000  0 0.000
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000
#&gt; SRR1539261     1  0.2356      0.929 0.928  0 0.072
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3    p4
#&gt; SRR1539207     3  0.0000      0.999 0.000  0 1.000 0.000
#&gt; SRR1539208     4  0.3266      0.653 0.168  0 0.000 0.832
#&gt; SRR1539211     4  0.0188      0.652 0.004  0 0.000 0.996
#&gt; SRR1539210     3  0.0469      0.991 0.000  0 0.988 0.012
#&gt; SRR1539209     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539212     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539214     1  0.0000      0.800 1.000  0 0.000 0.000
#&gt; SRR1539213     3  0.0000      0.999 0.000  0 1.000 0.000
#&gt; SRR1539215     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539216     3  0.0000      0.999 0.000  0 1.000 0.000
#&gt; SRR1539217     1  0.3975      0.608 0.760  0 0.000 0.240
#&gt; SRR1539218     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539220     1  0.0000      0.800 1.000  0 0.000 0.000
#&gt; SRR1539219     3  0.0000      0.999 0.000  0 1.000 0.000
#&gt; SRR1539221     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539223     1  0.4989     -0.148 0.528  0 0.000 0.472
#&gt; SRR1539224     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539222     3  0.0000      0.999 0.000  0 1.000 0.000
#&gt; SRR1539225     3  0.0000      0.999 0.000  0 1.000 0.000
#&gt; SRR1539227     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539226     1  0.3486      0.675 0.812  0 0.000 0.188
#&gt; SRR1539228     3  0.0000      0.999 0.000  0 1.000 0.000
#&gt; SRR1539229     1  0.4008      0.603 0.756  0 0.000 0.244
#&gt; SRR1539232     3  0.0000      0.999 0.000  0 1.000 0.000
#&gt; SRR1539230     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539231     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539234     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539233     1  0.4103      0.584 0.744  0 0.000 0.256
#&gt; SRR1539235     1  0.0000      0.800 1.000  0 0.000 0.000
#&gt; SRR1539236     1  0.3764      0.645 0.784  0 0.000 0.216
#&gt; SRR1539237     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539238     1  0.0000      0.800 1.000  0 0.000 0.000
#&gt; SRR1539239     4  0.4866      0.441 0.404  0 0.000 0.596
#&gt; SRR1539242     4  0.4989      0.292 0.472  0 0.000 0.528
#&gt; SRR1539240     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539241     1  0.0000      0.800 1.000  0 0.000 0.000
#&gt; SRR1539243     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539244     1  0.0000      0.800 1.000  0 0.000 0.000
#&gt; SRR1539245     1  0.4967      0.086 0.548  0 0.000 0.452
#&gt; SRR1539246     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539247     1  0.0000      0.800 1.000  0 0.000 0.000
#&gt; SRR1539248     4  0.2589      0.677 0.116  0 0.000 0.884
#&gt; SRR1539249     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539250     1  0.3123      0.644 0.844  0 0.000 0.156
#&gt; SRR1539251     1  0.3123      0.644 0.844  0 0.000 0.156
#&gt; SRR1539253     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539252     1  0.2408      0.747 0.896  0 0.000 0.104
#&gt; SRR1539255     1  0.3486      0.675 0.812  0 0.000 0.188
#&gt; SRR1539254     1  0.0000      0.800 1.000  0 0.000 0.000
#&gt; SRR1539256     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539257     1  0.0000      0.800 1.000  0 0.000 0.000
#&gt; SRR1539258     4  0.4996      0.265 0.484  0 0.000 0.516
#&gt; SRR1539259     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539260     1  0.0000      0.800 1.000  0 0.000 0.000
#&gt; SRR1539262     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; SRR1539261     4  0.0336      0.655 0.008  0 0.000 0.992
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3 p4    p5
#&gt; SRR1539207     3  0.0000     0.9587 0.000 0.000 1.000 NA 0.000
#&gt; SRR1539208     1  0.3760     0.7636 0.784 0.000 0.000 NA 0.188
#&gt; SRR1539211     1  0.4141     0.6078 0.728 0.000 0.000 NA 0.024
#&gt; SRR1539210     3  0.4467     0.6825 0.016 0.000 0.640 NA 0.000
#&gt; SRR1539209     2  0.3783     0.7556 0.008 0.740 0.000 NA 0.000
#&gt; SRR1539212     2  0.3783     0.7556 0.008 0.740 0.000 NA 0.000
#&gt; SRR1539214     5  0.0000     0.7550 0.000 0.000 0.000 NA 1.000
#&gt; SRR1539213     3  0.0000     0.9587 0.000 0.000 1.000 NA 0.000
#&gt; SRR1539215     2  0.0000     0.9699 0.000 1.000 0.000 NA 0.000
#&gt; SRR1539216     3  0.0000     0.9587 0.000 0.000 1.000 NA 0.000
#&gt; SRR1539217     5  0.4610     0.2430 0.432 0.000 0.000 NA 0.556
#&gt; SRR1539218     2  0.0404     0.9665 0.000 0.988 0.000 NA 0.000
#&gt; SRR1539220     5  0.0000     0.7550 0.000 0.000 0.000 NA 1.000
#&gt; SRR1539219     3  0.0000     0.9587 0.000 0.000 1.000 NA 0.000
#&gt; SRR1539221     2  0.0510     0.9646 0.000 0.984 0.000 NA 0.000
#&gt; SRR1539223     5  0.5006     0.5009 0.180 0.000 0.000 NA 0.704
#&gt; SRR1539224     2  0.0000     0.9699 0.000 1.000 0.000 NA 0.000
#&gt; SRR1539222     3  0.0000     0.9587 0.000 0.000 1.000 NA 0.000
#&gt; SRR1539225     3  0.0000     0.9587 0.000 0.000 1.000 NA 0.000
#&gt; SRR1539227     2  0.0290     0.9681 0.000 0.992 0.000 NA 0.000
#&gt; SRR1539226     5  0.4444     0.4285 0.364 0.000 0.000 NA 0.624
#&gt; SRR1539228     3  0.0000     0.9587 0.000 0.000 1.000 NA 0.000
#&gt; SRR1539229     5  0.4924     0.3237 0.420 0.000 0.000 NA 0.552
#&gt; SRR1539232     3  0.1845     0.9183 0.016 0.000 0.928 NA 0.000
#&gt; SRR1539230     2  0.0162     0.9693 0.000 0.996 0.000 NA 0.000
#&gt; SRR1539231     2  0.0162     0.9693 0.000 0.996 0.000 NA 0.000
#&gt; SRR1539234     2  0.0162     0.9701 0.000 0.996 0.000 NA 0.000
#&gt; SRR1539233     5  0.5229     0.3238 0.404 0.000 0.000 NA 0.548
#&gt; SRR1539235     5  0.0000     0.7550 0.000 0.000 0.000 NA 1.000
#&gt; SRR1539236     5  0.4182     0.4571 0.352 0.000 0.000 NA 0.644
#&gt; SRR1539237     2  0.0290     0.9699 0.000 0.992 0.000 NA 0.000
#&gt; SRR1539238     5  0.0324     0.7544 0.004 0.000 0.000 NA 0.992
#&gt; SRR1539239     1  0.3993     0.7536 0.756 0.000 0.000 NA 0.216
#&gt; SRR1539242     1  0.4083     0.7211 0.744 0.000 0.000 NA 0.228
#&gt; SRR1539240     2  0.0290     0.9699 0.000 0.992 0.000 NA 0.000
#&gt; SRR1539241     5  0.0162     0.7549 0.004 0.000 0.000 NA 0.996
#&gt; SRR1539243     2  0.0162     0.9701 0.000 0.996 0.000 NA 0.000
#&gt; SRR1539244     5  0.4707     0.5584 0.072 0.000 0.000 NA 0.716
#&gt; SRR1539245     5  0.6553     0.0815 0.364 0.000 0.000 NA 0.432
#&gt; SRR1539246     2  0.0290     0.9699 0.000 0.992 0.000 NA 0.000
#&gt; SRR1539247     5  0.0000     0.7550 0.000 0.000 0.000 NA 1.000
#&gt; SRR1539248     1  0.3456     0.7644 0.800 0.000 0.000 NA 0.184
#&gt; SRR1539249     2  0.0290     0.9699 0.000 0.992 0.000 NA 0.000
#&gt; SRR1539250     5  0.2020     0.7170 0.000 0.000 0.000 NA 0.900
#&gt; SRR1539251     5  0.2020     0.7170 0.000 0.000 0.000 NA 0.900
#&gt; SRR1539253     2  0.0290     0.9699 0.000 0.992 0.000 NA 0.000
#&gt; SRR1539252     5  0.2304     0.7146 0.100 0.000 0.000 NA 0.892
#&gt; SRR1539255     5  0.4165     0.4960 0.320 0.000 0.000 NA 0.672
#&gt; SRR1539254     5  0.0000     0.7550 0.000 0.000 0.000 NA 1.000
#&gt; SRR1539256     2  0.0290     0.9699 0.000 0.992 0.000 NA 0.000
#&gt; SRR1539257     5  0.0000     0.7550 0.000 0.000 0.000 NA 1.000
#&gt; SRR1539258     1  0.4562     0.5969 0.676 0.000 0.000 NA 0.292
#&gt; SRR1539259     2  0.0290     0.9699 0.000 0.992 0.000 NA 0.000
#&gt; SRR1539260     5  0.0162     0.7549 0.004 0.000 0.000 NA 0.996
#&gt; SRR1539262     2  0.0290     0.9681 0.000 0.992 0.000 NA 0.000
#&gt; SRR1539261     1  0.4042     0.6306 0.756 0.000 0.000 NA 0.032
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.0000      0.992 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539208     1  0.1765      0.872 0.924 0.000 0.000 0.024 0.052 0.000
#&gt; SRR1539211     4  0.1863      0.974 0.104 0.000 0.000 0.896 0.000 0.000
#&gt; SRR1539210     6  0.4234      0.000 0.000 0.000 0.324 0.032 0.000 0.644
#&gt; SRR1539209     2  0.4889      0.542 0.000 0.604 0.000 0.084 0.000 0.312
#&gt; SRR1539212     2  0.4904      0.536 0.000 0.600 0.000 0.084 0.000 0.316
#&gt; SRR1539214     5  0.0146      0.935 0.004 0.000 0.000 0.000 0.996 0.000
#&gt; SRR1539213     3  0.0000      0.992 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539215     2  0.0632      0.930 0.000 0.976 0.000 0.000 0.000 0.024
#&gt; SRR1539216     3  0.0000      0.992 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539217     1  0.1387      0.893 0.932 0.000 0.000 0.000 0.068 0.000
#&gt; SRR1539218     2  0.1500      0.916 0.000 0.936 0.000 0.012 0.000 0.052
#&gt; SRR1539220     5  0.0000      0.935 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539219     3  0.0000      0.992 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539221     2  0.1625      0.912 0.000 0.928 0.000 0.012 0.000 0.060
#&gt; SRR1539223     5  0.1523      0.911 0.044 0.000 0.000 0.008 0.940 0.008
#&gt; SRR1539224     2  0.0146      0.931 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1539222     3  0.0000      0.992 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539225     3  0.0000      0.992 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539227     2  0.1124      0.925 0.000 0.956 0.000 0.008 0.000 0.036
#&gt; SRR1539226     1  0.1141      0.903 0.948 0.000 0.000 0.000 0.052 0.000
#&gt; SRR1539228     3  0.0000      0.992 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539229     1  0.1167      0.902 0.960 0.000 0.000 0.008 0.020 0.012
#&gt; SRR1539232     3  0.1036      0.942 0.004 0.000 0.964 0.008 0.000 0.024
#&gt; SRR1539230     2  0.1010      0.927 0.000 0.960 0.000 0.004 0.000 0.036
#&gt; SRR1539231     2  0.1010      0.927 0.000 0.960 0.000 0.004 0.000 0.036
#&gt; SRR1539234     2  0.0632      0.930 0.000 0.976 0.000 0.000 0.000 0.024
#&gt; SRR1539233     1  0.1074      0.904 0.960 0.000 0.000 0.000 0.028 0.012
#&gt; SRR1539235     5  0.0146      0.936 0.004 0.000 0.000 0.000 0.996 0.000
#&gt; SRR1539236     1  0.1387      0.895 0.932 0.000 0.000 0.000 0.068 0.000
#&gt; SRR1539237     2  0.0725      0.930 0.000 0.976 0.000 0.012 0.000 0.012
#&gt; SRR1539238     5  0.0458      0.935 0.016 0.000 0.000 0.000 0.984 0.000
#&gt; SRR1539239     1  0.2912      0.778 0.816 0.000 0.000 0.172 0.012 0.000
#&gt; SRR1539242     1  0.3037      0.777 0.808 0.000 0.000 0.176 0.016 0.000
#&gt; SRR1539240     2  0.0725      0.930 0.000 0.976 0.000 0.012 0.000 0.012
#&gt; SRR1539241     5  0.0458      0.935 0.016 0.000 0.000 0.000 0.984 0.000
#&gt; SRR1539243     2  0.0622      0.931 0.000 0.980 0.000 0.012 0.000 0.008
#&gt; SRR1539244     5  0.4013      0.679 0.212 0.000 0.000 0.008 0.740 0.040
#&gt; SRR1539245     1  0.1262      0.897 0.956 0.000 0.000 0.008 0.016 0.020
#&gt; SRR1539246     2  0.0622      0.930 0.000 0.980 0.000 0.012 0.000 0.008
#&gt; SRR1539247     5  0.0260      0.936 0.008 0.000 0.000 0.000 0.992 0.000
#&gt; SRR1539248     1  0.2814      0.779 0.820 0.000 0.000 0.172 0.008 0.000
#&gt; SRR1539249     2  0.0725      0.930 0.000 0.976 0.000 0.012 0.000 0.012
#&gt; SRR1539250     5  0.0858      0.924 0.000 0.000 0.000 0.004 0.968 0.028
#&gt; SRR1539251     5  0.0858      0.924 0.000 0.000 0.000 0.004 0.968 0.028
#&gt; SRR1539253     2  0.0725      0.930 0.000 0.976 0.000 0.012 0.000 0.012
#&gt; SRR1539252     5  0.3468      0.571 0.284 0.000 0.000 0.000 0.712 0.004
#&gt; SRR1539255     1  0.1141      0.903 0.948 0.000 0.000 0.000 0.052 0.000
#&gt; SRR1539254     5  0.0363      0.936 0.012 0.000 0.000 0.000 0.988 0.000
#&gt; SRR1539256     2  0.0725      0.930 0.000 0.976 0.000 0.012 0.000 0.012
#&gt; SRR1539257     5  0.0000      0.935 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539258     1  0.0458      0.899 0.984 0.000 0.000 0.000 0.016 0.000
#&gt; SRR1539259     2  0.0725      0.930 0.000 0.976 0.000 0.012 0.000 0.012
#&gt; SRR1539260     5  0.0458      0.935 0.016 0.000 0.000 0.000 0.984 0.000
#&gt; SRR1539262     2  0.1151      0.926 0.000 0.956 0.000 0.012 0.000 0.032
#&gt; SRR1539261     4  0.2048      0.975 0.120 0.000 0.000 0.880 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14951 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4570 0.544   0.544
#> 3 3 0.803           0.831       0.925         0.3505 0.836   0.699
#> 4 4 0.597           0.600       0.765         0.1131 0.896   0.746
#> 5 5 0.649           0.607       0.817         0.0766 0.755   0.408
#> 6 6 0.711           0.645       0.824         0.0478 0.930   0.750
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1539207     1       0          1  1  0
#&gt; SRR1539208     1       0          1  1  0
#&gt; SRR1539211     1       0          1  1  0
#&gt; SRR1539210     1       0          1  1  0
#&gt; SRR1539209     2       0          1  0  1
#&gt; SRR1539212     2       0          1  0  1
#&gt; SRR1539214     1       0          1  1  0
#&gt; SRR1539213     1       0          1  1  0
#&gt; SRR1539215     2       0          1  0  1
#&gt; SRR1539216     1       0          1  1  0
#&gt; SRR1539217     1       0          1  1  0
#&gt; SRR1539218     2       0          1  0  1
#&gt; SRR1539220     1       0          1  1  0
#&gt; SRR1539219     1       0          1  1  0
#&gt; SRR1539221     2       0          1  0  1
#&gt; SRR1539223     1       0          1  1  0
#&gt; SRR1539224     2       0          1  0  1
#&gt; SRR1539222     1       0          1  1  0
#&gt; SRR1539225     1       0          1  1  0
#&gt; SRR1539227     2       0          1  0  1
#&gt; SRR1539226     1       0          1  1  0
#&gt; SRR1539228     1       0          1  1  0
#&gt; SRR1539229     1       0          1  1  0
#&gt; SRR1539232     1       0          1  1  0
#&gt; SRR1539230     2       0          1  0  1
#&gt; SRR1539231     2       0          1  0  1
#&gt; SRR1539234     2       0          1  0  1
#&gt; SRR1539233     1       0          1  1  0
#&gt; SRR1539235     1       0          1  1  0
#&gt; SRR1539236     1       0          1  1  0
#&gt; SRR1539237     2       0          1  0  1
#&gt; SRR1539238     1       0          1  1  0
#&gt; SRR1539239     1       0          1  1  0
#&gt; SRR1539242     1       0          1  1  0
#&gt; SRR1539240     2       0          1  0  1
#&gt; SRR1539241     1       0          1  1  0
#&gt; SRR1539243     2       0          1  0  1
#&gt; SRR1539244     1       0          1  1  0
#&gt; SRR1539245     1       0          1  1  0
#&gt; SRR1539246     2       0          1  0  1
#&gt; SRR1539247     1       0          1  1  0
#&gt; SRR1539248     1       0          1  1  0
#&gt; SRR1539249     2       0          1  0  1
#&gt; SRR1539250     1       0          1  1  0
#&gt; SRR1539251     1       0          1  1  0
#&gt; SRR1539253     2       0          1  0  1
#&gt; SRR1539252     1       0          1  1  0
#&gt; SRR1539255     1       0          1  1  0
#&gt; SRR1539254     1       0          1  1  0
#&gt; SRR1539256     2       0          1  0  1
#&gt; SRR1539257     1       0          1  1  0
#&gt; SRR1539258     1       0          1  1  0
#&gt; SRR1539259     2       0          1  0  1
#&gt; SRR1539260     1       0          1  1  0
#&gt; SRR1539262     2       0          1  0  1
#&gt; SRR1539261     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1539207     1  0.0000     0.8791 1.000 0.000 0.000
#&gt; SRR1539208     1  0.0000     0.8791 1.000 0.000 0.000
#&gt; SRR1539211     1  0.0000     0.8791 1.000 0.000 0.000
#&gt; SRR1539210     1  0.0000     0.8791 1.000 0.000 0.000
#&gt; SRR1539209     2  0.0592     0.9932 0.000 0.988 0.012
#&gt; SRR1539212     2  0.0000     0.9954 0.000 1.000 0.000
#&gt; SRR1539214     3  0.5178     0.7281 0.256 0.000 0.744
#&gt; SRR1539213     1  0.0747     0.8794 0.984 0.000 0.016
#&gt; SRR1539215     2  0.0747     0.9918 0.000 0.984 0.016
#&gt; SRR1539216     1  0.0000     0.8791 1.000 0.000 0.000
#&gt; SRR1539217     1  0.0892     0.8782 0.980 0.000 0.020
#&gt; SRR1539218     2  0.0592     0.9932 0.000 0.988 0.012
#&gt; SRR1539220     1  0.2356     0.8438 0.928 0.000 0.072
#&gt; SRR1539219     1  0.0000     0.8791 1.000 0.000 0.000
#&gt; SRR1539221     2  0.0747     0.9918 0.000 0.984 0.016
#&gt; SRR1539223     1  0.0000     0.8791 1.000 0.000 0.000
#&gt; SRR1539224     2  0.0424     0.9941 0.000 0.992 0.008
#&gt; SRR1539222     1  0.0000     0.8791 1.000 0.000 0.000
#&gt; SRR1539225     1  0.0747     0.8794 0.984 0.000 0.016
#&gt; SRR1539227     2  0.0592     0.9932 0.000 0.988 0.012
#&gt; SRR1539226     3  0.3752     0.7986 0.144 0.000 0.856
#&gt; SRR1539228     1  0.0747     0.8794 0.984 0.000 0.016
#&gt; SRR1539229     3  0.1289     0.8054 0.032 0.000 0.968
#&gt; SRR1539232     1  0.4291     0.7327 0.820 0.000 0.180
#&gt; SRR1539230     2  0.0747     0.9918 0.000 0.984 0.016
#&gt; SRR1539231     2  0.0747     0.9918 0.000 0.984 0.016
#&gt; SRR1539234     2  0.0000     0.9954 0.000 1.000 0.000
#&gt; SRR1539233     3  0.1411     0.8063 0.036 0.000 0.964
#&gt; SRR1539235     1  0.3267     0.8059 0.884 0.000 0.116
#&gt; SRR1539236     3  0.5327     0.7058 0.272 0.000 0.728
#&gt; SRR1539237     2  0.0000     0.9954 0.000 1.000 0.000
#&gt; SRR1539238     1  0.0892     0.8782 0.980 0.000 0.020
#&gt; SRR1539239     3  0.6307     0.0965 0.488 0.000 0.512
#&gt; SRR1539242     1  0.6204     0.1706 0.576 0.000 0.424
#&gt; SRR1539240     2  0.0000     0.9954 0.000 1.000 0.000
#&gt; SRR1539241     1  0.0892     0.8782 0.980 0.000 0.020
#&gt; SRR1539243     2  0.0000     0.9954 0.000 1.000 0.000
#&gt; SRR1539244     3  0.1289     0.8054 0.032 0.000 0.968
#&gt; SRR1539245     3  0.1289     0.8054 0.032 0.000 0.968
#&gt; SRR1539246     2  0.0000     0.9954 0.000 1.000 0.000
#&gt; SRR1539247     1  0.3267     0.8059 0.884 0.000 0.116
#&gt; SRR1539248     1  0.5785     0.4513 0.668 0.000 0.332
#&gt; SRR1539249     2  0.0000     0.9954 0.000 1.000 0.000
#&gt; SRR1539250     1  0.0000     0.8791 1.000 0.000 0.000
#&gt; SRR1539251     1  0.0000     0.8791 1.000 0.000 0.000
#&gt; SRR1539253     2  0.0000     0.9954 0.000 1.000 0.000
#&gt; SRR1539252     1  0.6215     0.1562 0.572 0.000 0.428
#&gt; SRR1539255     3  0.4750     0.7653 0.216 0.000 0.784
#&gt; SRR1539254     1  0.0747     0.8794 0.984 0.000 0.016
#&gt; SRR1539256     2  0.0000     0.9954 0.000 1.000 0.000
#&gt; SRR1539257     1  0.5591     0.5164 0.696 0.000 0.304
#&gt; SRR1539258     1  0.6225     0.1401 0.568 0.000 0.432
#&gt; SRR1539259     2  0.0000     0.9954 0.000 1.000 0.000
#&gt; SRR1539260     1  0.0592     0.8795 0.988 0.000 0.012
#&gt; SRR1539262     2  0.0000     0.9954 0.000 1.000 0.000
#&gt; SRR1539261     1  0.0000     0.8791 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3 p4
#&gt; SRR1539207     3  0.0188     0.6550 0.000 0.000 0.996 NA
#&gt; SRR1539208     3  0.5666     0.4061 0.348 0.000 0.616 NA
#&gt; SRR1539211     3  0.5746     0.4045 0.348 0.000 0.612 NA
#&gt; SRR1539210     3  0.0921     0.6495 0.000 0.000 0.972 NA
#&gt; SRR1539209     2  0.3528     0.8832 0.000 0.808 0.000 NA
#&gt; SRR1539212     2  0.3219     0.8880 0.000 0.836 0.000 NA
#&gt; SRR1539214     1  0.5935     0.5247 0.664 0.000 0.080 NA
#&gt; SRR1539213     3  0.3464     0.6176 0.108 0.000 0.860 NA
#&gt; SRR1539215     2  0.3945     0.8755 0.004 0.780 0.000 NA
#&gt; SRR1539216     3  0.0188     0.6550 0.000 0.000 0.996 NA
#&gt; SRR1539217     3  0.5775     0.3337 0.408 0.000 0.560 NA
#&gt; SRR1539218     2  0.3569     0.8824 0.000 0.804 0.000 NA
#&gt; SRR1539220     3  0.5809     0.5282 0.216 0.000 0.692 NA
#&gt; SRR1539219     3  0.0779     0.6528 0.004 0.000 0.980 NA
#&gt; SRR1539221     2  0.3764     0.8769 0.000 0.784 0.000 NA
#&gt; SRR1539223     3  0.5432     0.4592 0.316 0.000 0.652 NA
#&gt; SRR1539224     2  0.3172     0.8881 0.000 0.840 0.000 NA
#&gt; SRR1539222     3  0.0000     0.6553 0.000 0.000 1.000 NA
#&gt; SRR1539225     3  0.3342     0.6231 0.100 0.000 0.868 NA
#&gt; SRR1539227     2  0.3726     0.8782 0.000 0.788 0.000 NA
#&gt; SRR1539226     1  0.2125     0.6167 0.920 0.000 0.076 NA
#&gt; SRR1539228     3  0.3015     0.6310 0.092 0.000 0.884 NA
#&gt; SRR1539229     1  0.0707     0.6065 0.980 0.000 0.000 NA
#&gt; SRR1539232     3  0.5649     0.1316 0.392 0.000 0.580 NA
#&gt; SRR1539230     2  0.3945     0.8755 0.004 0.780 0.000 NA
#&gt; SRR1539231     2  0.3945     0.8755 0.004 0.780 0.000 NA
#&gt; SRR1539234     2  0.0188     0.8922 0.000 0.996 0.000 NA
#&gt; SRR1539233     1  0.0469     0.6066 0.988 0.000 0.000 NA
#&gt; SRR1539235     3  0.7621     0.0987 0.296 0.000 0.468 NA
#&gt; SRR1539236     1  0.2888     0.5933 0.872 0.000 0.124 NA
#&gt; SRR1539237     2  0.1389     0.8807 0.000 0.952 0.000 NA
#&gt; SRR1539238     3  0.6757     0.3549 0.308 0.000 0.572 NA
#&gt; SRR1539239     1  0.5172     0.4316 0.704 0.000 0.260 NA
#&gt; SRR1539242     1  0.5989     0.0607 0.556 0.000 0.400 NA
#&gt; SRR1539240     2  0.1211     0.8840 0.000 0.960 0.000 NA
#&gt; SRR1539241     3  0.5387     0.3770 0.400 0.000 0.584 NA
#&gt; SRR1539243     2  0.0921     0.8878 0.000 0.972 0.000 NA
#&gt; SRR1539244     1  0.5400     0.4232 0.608 0.000 0.020 NA
#&gt; SRR1539245     1  0.3610     0.5413 0.800 0.000 0.000 NA
#&gt; SRR1539246     2  0.0921     0.8878 0.000 0.972 0.000 NA
#&gt; SRR1539247     3  0.7227     0.2348 0.256 0.000 0.544 NA
#&gt; SRR1539248     1  0.5750    -0.0620 0.532 0.000 0.440 NA
#&gt; SRR1539249     2  0.0817     0.8887 0.000 0.976 0.000 NA
#&gt; SRR1539250     3  0.0336     0.6576 0.008 0.000 0.992 NA
#&gt; SRR1539251     3  0.0336     0.6576 0.008 0.000 0.992 NA
#&gt; SRR1539253     2  0.0921     0.8878 0.000 0.972 0.000 NA
#&gt; SRR1539252     1  0.4406     0.3844 0.700 0.000 0.300 NA
#&gt; SRR1539255     1  0.1902     0.6182 0.932 0.000 0.064 NA
#&gt; SRR1539254     3  0.4343     0.5651 0.264 0.000 0.732 NA
#&gt; SRR1539256     2  0.4730     0.6134 0.000 0.636 0.000 NA
#&gt; SRR1539257     1  0.7874     0.1847 0.380 0.000 0.336 NA
#&gt; SRR1539258     1  0.5339     0.2390 0.624 0.000 0.356 NA
#&gt; SRR1539259     2  0.0188     0.8922 0.000 0.996 0.000 NA
#&gt; SRR1539260     3  0.4857     0.5373 0.284 0.000 0.700 NA
#&gt; SRR1539262     2  0.0000     0.8924 0.000 1.000 0.000 NA
#&gt; SRR1539261     3  0.5746     0.4045 0.348 0.000 0.612 NA
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1539207     3  0.1410     0.9043 0.060 0.000 0.940 0.000 0.000
#&gt; SRR1539208     1  0.2361     0.7794 0.892 0.000 0.012 0.000 0.096
#&gt; SRR1539211     1  0.4146     0.7153 0.804 0.012 0.088 0.000 0.096
#&gt; SRR1539210     3  0.2609     0.8189 0.028 0.008 0.896 0.000 0.068
#&gt; SRR1539209     4  0.1484     0.6761 0.000 0.008 0.048 0.944 0.000
#&gt; SRR1539212     4  0.2673     0.6268 0.000 0.028 0.072 0.892 0.008
#&gt; SRR1539214     1  0.4294    -0.1687 0.532 0.000 0.000 0.000 0.468
#&gt; SRR1539213     3  0.3274     0.8946 0.076 0.004 0.856 0.000 0.064
#&gt; SRR1539215     4  0.1502     0.6673 0.000 0.004 0.000 0.940 0.056
#&gt; SRR1539216     3  0.1410     0.9043 0.060 0.000 0.940 0.000 0.000
#&gt; SRR1539217     1  0.1942     0.7895 0.920 0.000 0.012 0.000 0.068
#&gt; SRR1539218     4  0.0162     0.7035 0.000 0.004 0.000 0.996 0.000
#&gt; SRR1539220     1  0.6557    -0.2186 0.440 0.000 0.208 0.000 0.352
#&gt; SRR1539219     3  0.1924     0.9044 0.064 0.004 0.924 0.000 0.008
#&gt; SRR1539221     4  0.0000     0.7047 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539223     1  0.4583     0.6468 0.756 0.004 0.144 0.000 0.096
#&gt; SRR1539224     4  0.0404     0.6986 0.000 0.012 0.000 0.988 0.000
#&gt; SRR1539222     3  0.2139     0.8940 0.052 0.000 0.916 0.000 0.032
#&gt; SRR1539225     3  0.3274     0.8946 0.076 0.004 0.856 0.000 0.064
#&gt; SRR1539227     4  0.0000     0.7047 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1539226     1  0.1043     0.7982 0.960 0.000 0.000 0.000 0.040
#&gt; SRR1539228     3  0.3073     0.8991 0.076 0.004 0.868 0.000 0.052
#&gt; SRR1539229     1  0.1768     0.7791 0.924 0.004 0.000 0.000 0.072
#&gt; SRR1539232     3  0.4239     0.8429 0.080 0.004 0.784 0.000 0.132
#&gt; SRR1539230     4  0.0404     0.7019 0.000 0.000 0.000 0.988 0.012
#&gt; SRR1539231     4  0.0162     0.7045 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1539234     4  0.4273    -0.5658 0.000 0.448 0.000 0.552 0.000
#&gt; SRR1539233     1  0.1270     0.7931 0.948 0.000 0.000 0.000 0.052
#&gt; SRR1539235     5  0.5334     0.1604 0.436 0.000 0.052 0.000 0.512
#&gt; SRR1539236     1  0.0703     0.8028 0.976 0.000 0.000 0.000 0.024
#&gt; SRR1539237     2  0.4219     0.6892 0.000 0.584 0.000 0.416 0.000
#&gt; SRR1539238     1  0.5246     0.5664 0.696 0.020 0.068 0.000 0.216
#&gt; SRR1539239     1  0.0290     0.8044 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1539242     1  0.0162     0.8044 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1539240     2  0.4227     0.6944 0.000 0.580 0.000 0.420 0.000
#&gt; SRR1539241     1  0.2628     0.7811 0.884 0.000 0.028 0.000 0.088
#&gt; SRR1539243     2  0.4305     0.6786 0.000 0.512 0.000 0.488 0.000
#&gt; SRR1539244     5  0.2482     0.5918 0.084 0.000 0.024 0.000 0.892
#&gt; SRR1539245     1  0.4383     0.0278 0.572 0.004 0.000 0.000 0.424
#&gt; SRR1539246     2  0.4307     0.6508 0.000 0.500 0.000 0.500 0.000
#&gt; SRR1539247     5  0.5032     0.6184 0.128 0.000 0.168 0.000 0.704
#&gt; SRR1539248     1  0.0290     0.8048 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1539249     4  0.4262    -0.5469 0.000 0.440 0.000 0.560 0.000
#&gt; SRR1539250     3  0.3622     0.8308 0.124 0.000 0.820 0.000 0.056
#&gt; SRR1539251     3  0.3622     0.8308 0.124 0.000 0.820 0.000 0.056
#&gt; SRR1539253     2  0.4306     0.6723 0.000 0.508 0.000 0.492 0.000
#&gt; SRR1539252     1  0.0703     0.8028 0.976 0.000 0.000 0.000 0.024
#&gt; SRR1539255     1  0.1043     0.7990 0.960 0.000 0.000 0.000 0.040
#&gt; SRR1539254     1  0.3644     0.7385 0.824 0.000 0.080 0.000 0.096
#&gt; SRR1539256     2  0.1043     0.3455 0.000 0.960 0.000 0.040 0.000
#&gt; SRR1539257     5  0.4317     0.6998 0.160 0.000 0.076 0.000 0.764
#&gt; SRR1539258     1  0.0290     0.8044 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1539259     4  0.4192    -0.4290 0.000 0.404 0.000 0.596 0.000
#&gt; SRR1539260     1  0.3648     0.7320 0.824 0.000 0.092 0.000 0.084
#&gt; SRR1539262     4  0.4030    -0.2398 0.000 0.352 0.000 0.648 0.000
#&gt; SRR1539261     1  0.2249     0.7814 0.896 0.000 0.008 0.000 0.096
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1539207     3  0.0000     0.8347 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539208     1  0.2182     0.7794 0.900 0.000 0.004 0.000 0.020 0.076
#&gt; SRR1539211     1  0.3930     0.3621 0.576 0.000 0.004 0.000 0.000 0.420
#&gt; SRR1539210     3  0.3300     0.7130 0.016 0.000 0.812 0.000 0.016 0.156
#&gt; SRR1539209     6  0.3828     0.6816 0.000 0.000 0.000 0.440 0.000 0.560
#&gt; SRR1539212     6  0.4277     0.7227 0.000 0.000 0.064 0.236 0.000 0.700
#&gt; SRR1539214     1  0.4809     0.3213 0.628 0.000 0.052 0.000 0.308 0.012
#&gt; SRR1539213     3  0.1007     0.8327 0.000 0.000 0.956 0.000 0.044 0.000
#&gt; SRR1539215     4  0.5166    -0.0265 0.000 0.000 0.020 0.608 0.304 0.068
#&gt; SRR1539216     3  0.0000     0.8347 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1539217     1  0.1168     0.7921 0.956 0.000 0.000 0.000 0.016 0.028
#&gt; SRR1539218     4  0.0363     0.6794 0.000 0.012 0.000 0.988 0.000 0.000
#&gt; SRR1539220     3  0.5883    -0.2654 0.396 0.000 0.452 0.000 0.140 0.012
#&gt; SRR1539219     3  0.0146     0.8354 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1539221     4  0.0508     0.6667 0.000 0.000 0.000 0.984 0.004 0.012
#&gt; SRR1539223     1  0.2831     0.7514 0.872 0.000 0.064 0.000 0.016 0.048
#&gt; SRR1539224     4  0.0603     0.6801 0.000 0.016 0.000 0.980 0.000 0.004
#&gt; SRR1539222     3  0.1364     0.8226 0.012 0.000 0.952 0.000 0.016 0.020
#&gt; SRR1539225     3  0.1075     0.8314 0.000 0.000 0.952 0.000 0.048 0.000
#&gt; SRR1539227     4  0.0508     0.6791 0.000 0.012 0.000 0.984 0.000 0.004
#&gt; SRR1539226     1  0.0937     0.7943 0.960 0.000 0.000 0.000 0.040 0.000
#&gt; SRR1539228     3  0.1075     0.8314 0.000 0.000 0.952 0.000 0.048 0.000
#&gt; SRR1539229     1  0.2146     0.7534 0.880 0.000 0.000 0.000 0.116 0.004
#&gt; SRR1539232     3  0.1615     0.8188 0.004 0.000 0.928 0.000 0.064 0.004
#&gt; SRR1539230     4  0.0622     0.6653 0.000 0.000 0.000 0.980 0.012 0.008
#&gt; SRR1539231     4  0.0520     0.6678 0.000 0.000 0.000 0.984 0.008 0.008
#&gt; SRR1539234     2  0.3445     0.7826 0.000 0.732 0.000 0.260 0.000 0.008
#&gt; SRR1539233     1  0.2715     0.7575 0.860 0.004 0.000 0.000 0.112 0.024
#&gt; SRR1539235     1  0.5748     0.1816 0.568 0.008 0.076 0.000 0.316 0.032
#&gt; SRR1539236     1  0.0713     0.7967 0.972 0.000 0.000 0.000 0.028 0.000
#&gt; SRR1539237     2  0.1910     0.8154 0.000 0.892 0.000 0.108 0.000 0.000
#&gt; SRR1539238     1  0.7156     0.1215 0.468 0.292 0.028 0.000 0.076 0.136
#&gt; SRR1539239     1  0.0632     0.7976 0.976 0.000 0.000 0.000 0.024 0.000
#&gt; SRR1539242     1  0.0891     0.7986 0.968 0.000 0.000 0.000 0.024 0.008
#&gt; SRR1539240     2  0.2053     0.8152 0.000 0.888 0.000 0.108 0.000 0.004
#&gt; SRR1539241     1  0.5483     0.5523 0.680 0.140 0.012 0.000 0.040 0.128
#&gt; SRR1539243     2  0.3309     0.7554 0.000 0.720 0.000 0.280 0.000 0.000
#&gt; SRR1539244     5  0.3259     0.4039 0.072 0.024 0.028 0.004 0.860 0.012
#&gt; SRR1539245     1  0.4515     0.3449 0.608 0.000 0.028 0.000 0.356 0.008
#&gt; SRR1539246     4  0.3852     0.2848 0.000 0.384 0.000 0.612 0.000 0.004
#&gt; SRR1539247     5  0.5852     0.6554 0.192 0.000 0.252 0.000 0.544 0.012
#&gt; SRR1539248     1  0.1092     0.7989 0.960 0.000 0.000 0.000 0.020 0.020
#&gt; SRR1539249     4  0.3872     0.2632 0.000 0.392 0.000 0.604 0.000 0.004
#&gt; SRR1539250     3  0.3411     0.7128 0.088 0.000 0.836 0.000 0.044 0.032
#&gt; SRR1539251     3  0.3360     0.7193 0.084 0.000 0.840 0.000 0.044 0.032
#&gt; SRR1539253     2  0.3151     0.7908 0.000 0.748 0.000 0.252 0.000 0.000
#&gt; SRR1539252     1  0.0000     0.7981 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1539255     1  0.0405     0.7988 0.988 0.000 0.004 0.000 0.008 0.000
#&gt; SRR1539254     1  0.3960     0.6878 0.808 0.004 0.084 0.000 0.056 0.048
#&gt; SRR1539256     2  0.0862     0.6998 0.000 0.972 0.000 0.004 0.016 0.008
#&gt; SRR1539257     5  0.5916     0.6902 0.248 0.000 0.192 0.000 0.544 0.016
#&gt; SRR1539258     1  0.0458     0.7985 0.984 0.000 0.000 0.000 0.016 0.000
#&gt; SRR1539259     4  0.3584     0.4603 0.000 0.308 0.000 0.688 0.000 0.004
#&gt; SRR1539260     1  0.3617     0.7298 0.828 0.004 0.032 0.000 0.048 0.088
#&gt; SRR1539262     4  0.3584     0.4603 0.000 0.308 0.000 0.688 0.000 0.004
#&gt; SRR1539261     1  0.1707     0.7878 0.928 0.000 0.004 0.000 0.012 0.056
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#>  [1] grid      parallel  stats4    stats     graphics  grDevices utils     datasets  methods  
#> [10] base     
#> 
#> other attached packages:
#>  [1] genefilter_1.66.0           ComplexHeatmap_2.1.1        markdown_1.1               
#>  [4] knitr_1.26                  cola_1.3.2                  SummarizedExperiment_1.14.1
#>  [7] DelayedArray_0.10.0         BiocParallel_1.18.1         matrixStats_0.55.0         
#> [10] Biobase_2.44.0              GenomicRanges_1.36.1        GenomeInfoDb_1.20.0        
#> [13] IRanges_2.18.3              S4Vectors_0.22.1            BiocGenerics_0.30.0        
#> [16] GetoptLong_0.1.7           
#> 
#> loaded via a namespace (and not attached):
#>  [1] bitops_1.0-6           bit64_0.9-7            doParallel_1.0.15      RColorBrewer_1.1-2    
#>  [5] httr_1.4.1             backports_1.1.5        tools_3.6.0            R6_2.4.1              
#>  [9] DBI_1.0.0              lazyeval_0.2.2         colorspace_1.4-1       withr_2.1.2           
#> [13] tidyselect_0.2.5       gridExtra_2.3          bit_1.1-14             compiler_3.6.0        
#> [17] xml2_1.2.2             microbenchmark_1.4-7   pkgmaker_0.28          slam_0.1-46           
#> [21] scales_1.1.0           NMF_0.23.6             stringr_1.4.0          digest_0.6.23         
#> [25] XVector_0.24.0         pkgconfig_2.0.3        bibtex_0.4.2           highr_0.8             
#> [29] rlang_0.4.2            GlobalOptions_0.1.1    RSQLite_2.1.2          impute_1.58.0         
#> [33] shape_1.4.4            mclust_5.4.5           dendextend_1.12.0      dplyr_0.8.3           
#> [37] RCurl_1.95-4.12        magrittr_1.5           GenomeInfoDbData_1.2.1 Matrix_1.2-17         
#> [41] Rcpp_1.0.3             munsell_0.5.0          viridis_0.5.1          lifecycle_0.1.0       
#> [45] stringi_1.4.3          zlibbioc_1.30.0        plyr_1.8.4             blob_1.2.0            
#> [49] crayon_1.3.4           lattice_0.20-38        splines_3.6.0          annotate_1.62.0       
#> [53] circlize_0.4.9         zeallot_0.1.0          pillar_1.4.2           rjson_0.2.20          
#> [57] rngtools_1.4           reshape2_1.4.3         codetools_0.2-16       XML_3.98-1.20         
#> [61] glue_1.3.1             evaluate_0.14          vctrs_0.2.0            png_0.1-7             
#> [65] foreach_1.4.7          polyclip_1.10-0        gtable_0.3.0           purrr_0.3.3           
#> [69] clue_0.3-57            assertthat_0.2.1       ggplot2_3.2.1          xfun_0.11             
#> [73] gridBase_0.4-7         eulerr_6.0.0           xtable_1.8-4           skmeans_0.2-11        
#> [77] survival_2.44-1.1      viridisLite_0.3.0      tibble_2.1.3           iterators_1.0.12      
#> [81] memoise_1.1.0          AnnotationDbi_1.46.1   registry_0.5-1         GTF_0.0.1             
#> [85] cluster_2.1.0          brew_1.0-6
```


