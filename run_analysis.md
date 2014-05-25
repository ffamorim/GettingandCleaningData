  <div id="readme" class="blob instapaper_body">
    <article class="markdown-body entry-content" itemprop="mainContentOfPage"><h1>
<a name="user-content-run_analysis" class="anchor" href="#run_analysis"><span class="octicon octicon-link"></span></a>run_analysis</h1>

<p>Last updated 2014-05-23 09:04:14 using R version 3.0.2 (2013-09-25).</p>

<h2>
<a name="user-content-instructions-for-project" class="anchor" href="#instructions-for-project"><span class="octicon octicon-link"></span></a>Instructions for project</h2>

<blockquote>
<p>The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.  </p>

<p>One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained: </p>

<p><a href="http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones">http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones</a> </p>

<p>Here are the data for the project: </p>

<p><a href="https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip">https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip</a> </p>

<p>You should create one R script called run_analysis.R that does the following. </p>

<ol class="task-list">
<li>
<strong>DONE</strong> Merges the training and the test sets to create one data set.</li>
<li>
<strong>DONE</strong> Extracts only the measurements on the mean and standard deviation for each measurement.</li>
<li>
<strong>DONE</strong> Uses descriptive activity names to name the activities in the data set.</li>
<li>
<strong>DONE</strong> Appropriately labels the data set with descriptive activity names.</li>
<li>
<strong>DONE</strong> Creates a second, independent tidy data set with the average of each variable for each activity and each subject. </li>
</ol>
<p>Good luck!</p>
</blockquote>

<p><strong>The codebook is at the end of this document.</strong></p>

<h2>
<a name="user-content-preliminaries" class="anchor" href="#preliminaries"><span class="octicon octicon-link"></span></a>Preliminaries</h2>

<p>Load packages.</p>

<div class="highlight highlight-r"><pre><span class="c">packages &lt;- c("data.table", "reshape2")</span>
<span class="c">sapply(packages, require, character.only = TRUE, quietly = TRUE)</span>
</pre></div>

<pre><code>## data.table   reshape2 
##       TRUE       TRUE
</code></pre>

<p>Set path.</p>

<div class="highlight highlight-r"><pre><span class="c">path &lt;- getwd()</span>
<span class="c">path</span>
</pre></div>

<pre><code>## [1] "C:/Users/ffamorim/Documents/Repositories/Coursera/GettingandCleaningData"
</code></pre>

<h2>
<a name="user-content-get-the-data" class="anchor" href="#get-the-data"><span class="octicon octicon-link"></span></a>Get the data</h2>

<p>Download the file. Put it in the <code>Data</code> folder.</p>

<div class="highlight highlight-r"><pre><span class="c">url &lt;- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"</span>
<span class="c">f &lt;- "Dataset.zip"</span>
<span class="c">if (!file.exists(path)) {</span>
<span class="c">    dir.create(path)</span>
<span class="c">}</span>
<span class="c">download.file(url, file.path(path, f))</span>
</pre></div>

<p>Unzip the file.</p>

<div class="highlight highlight-r"><pre><span class="c">executable &lt;- file.path("C:", "Program Files (x86)", "7-Zip", "7z.exe")</span>
<span class="c">parameters &lt;- "x"</span>
<span class="c">cmd &lt;- paste(paste0("\"", executable, "\""), parameters, paste0("\"", file.path(path, </span>
<span class="c">    f), "\""))</span>
<span class="c">system(cmd)</span>
</pre></div>

<p>The archive put the files in a folder named <code>UCI HAR Dataset</code>. Set this folder as the input path. List the files here.</p>

<div class="highlight highlight-r"><pre><span class="c">pathIn &lt;- file.path(path, "UCI HAR Dataset")</span>
<span class="c">list.files(pathIn, recursive = TRUE)</span>
</pre></div>

<pre><code>##  [1] "activity_labels.txt"                         
##  [2] "features.txt"                                
##  [3] "features_info.txt"                           
##  [4] "README.txt"                                  
##  [5] "test/Inertial Signals/body_acc_x_test.txt"   
##  [6] "test/Inertial Signals/body_acc_y_test.txt"   
##  [7] "test/Inertial Signals/body_acc_z_test.txt"   
##  [8] "test/Inertial Signals/body_gyro_x_test.txt"  
##  [9] "test/Inertial Signals/body_gyro_y_test.txt"  
## [10] "test/Inertial Signals/body_gyro_z_test.txt"  
## [11] "test/Inertial Signals/total_acc_x_test.txt"  
## [12] "test/Inertial Signals/total_acc_y_test.txt"  
## [13] "test/Inertial Signals/total_acc_z_test.txt"  
## [14] "test/subject_test.txt"                       
## [15] "test/X_test.txt"                             
## [16] "test/y_test.txt"                             
## [17] "train/Inertial Signals/body_acc_x_train.txt" 
## [18] "train/Inertial Signals/body_acc_y_train.txt" 
## [19] "train/Inertial Signals/body_acc_z_train.txt" 
## [20] "train/Inertial Signals/body_gyro_x_train.txt"
## [21] "train/Inertial Signals/body_gyro_y_train.txt"
## [22] "train/Inertial Signals/body_gyro_z_train.txt"
## [23] "train/Inertial Signals/total_acc_x_train.txt"
## [24] "train/Inertial Signals/total_acc_y_train.txt"
## [25] "train/Inertial Signals/total_acc_z_train.txt"
## [26] "train/subject_train.txt"                     
## [27] "train/X_train.txt"                           
## [28] "train/y_train.txt"
</code></pre>


<p>For the purposes of this project, the files in the <code>Inertial Signals</code> folders are not used.</p>

<h2>
<a name="user-content-read-the-files" class="anchor" href="#read-the-files"><span class="octicon octicon-link"></span></a>Read the files</h2>

<p>Read the subject files.</p>

<div class="highlight highlight-r"><pre><span class="c">dtSubjectTrain &lt;- fread(file.path(pathIn, "train", "subject_train.txt"))</span>
<span class="c">dtSubjectTest &lt;- fread(file.path(pathIn, "test", "subject_test.txt"))</span>
</pre></div>

<p>Read the activity files. For some reason, these are called <em>label</em> files in the <code>README.txt</code> documentation.</p>

<div class="highlight highlight-r"><pre><span class="c">dtActivityTrain &lt;- fread(file.path(pathIn, "train", "Y_train.txt"))</span>
<span class="c">dtActivityTest &lt;- fread(file.path(pathIn, "test", "Y_test.txt"))</span>
</pre></div>

<p>Read the data files. <code>fread</code> seems to be giving me some trouble reading files. Using a helper function, read the file with <code>read.table</code> instead, then convert the resulting data frame to a data table. Return the data table.</p>

<div class="highlight highlight-r"><pre><span class="c">fileToDataTable &lt;- function(f) {</span>
<span class="c">    df &lt;- read.table(f)</span>
<span class="c">    dt &lt;- data.table(df)</span>
<span class="c">}</span>
<span class="c">dtTrain &lt;- fileToDataTable(file.path(pathIn, "train", "X_train.txt"))</span>
<span class="c">dtTest &lt;- fileToDataTable(file.path(pathIn, "test", "X_test.txt"))</span>
</pre></div>

<h2>
<a name="user-content-merge-the-training-and-the-test-sets" class="anchor" href="#merge-the-training-and-the-test-sets"><span class="octicon octicon-link"></span></a>Merge the training and the test sets</h2>

<p>Concatenate the data tables.</p>

<div class="highlight highlight-r"><pre><span class="c">dtSubject &lt;- rbind(dtSubjectTrain, dtSubjectTest)</span>
<span class="c">setnames(dtSubject, "V1", "subject")</span>
<span class="c">dtActivity &lt;- rbind(dtActivityTrain, dtActivityTest)</span>
<span class="c">setnames(dtActivity, "V1", "activityNum")</span>
<span class="c">dt &lt;- rbind(dtTrain, dtTest)</span>
</pre></div>

<p>Merge columns.</p>

<div class="highlight highlight-r"><pre><span class="c">dtSubject &lt;- cbind(dtSubject, dtActivity)</span>
<span class="c">dt &lt;- cbind(dtSubject, dt)</span>
</pre></div>

<p>Set key.</p>

<div class="highlight highlight-r"><pre><span class="c">setkey(dt, subject, activityNum)</span>
</pre></div>

<h2>
<a name="user-content-extract-only-the-mean-and-standard-deviation" class="anchor" href="#extract-only-the-mean-and-standard-deviation"><span class="octicon octicon-link"></span></a>Extract only the mean and standard deviation</h2>

<p>Read the <code>features.txt</code> file. This tells which variables in <code>dt</code> are measurements for the mean and standard deviation.</p>

<div class="highlight highlight-r"><pre><span class="c">dtFeatures &lt;- fread(file.path(pathIn, "features.txt"))</span>
<span class="c">setnames(dtFeatures, names(dtFeatures), c("featureNum", "featureName"))</span>
</pre></div>

<p>Subset only measurements for the mean and standard deviation.</p>

<div class="highlight highlight-r"><pre><span class="c">dtFeatures &lt;- dtFeatures[grepl("mean\\(\\)|std\\(\\)", featureName)]</span>
</pre></div>

<p>Convert the column numbers to a vector of variable names matching columns in <code>dt</code>.</p>

<div class="highlight highlight-r"><pre><span class="c">dtFeatures$featureCode &lt;- dtFeatures[, paste0("V", featureNum)]</span>
<span class="c">head(dtFeatures)</span>
</pre></div>

<pre><code>##    featureNum       featureName featureCode
## 1:          1 tBodyAcc-mean()-X          V1
## 2:          2 tBodyAcc-mean()-Y          V2
## 3:          3 tBodyAcc-mean()-Z          V3
## 4:          4  tBodyAcc-std()-X          V4
## 5:          5  tBodyAcc-std()-Y          V5
## 6:          6  tBodyAcc-std()-Z          V6
</code></pre>

<div class="highlight highlight-r"><pre><span class="c">dtFeatures$featureCode</span>
</pre></div>

<pre><code>##  [1] "V1"   "V2"   "V3"   "V4"   "V5"   "V6"   "V41"  "V42"  "V43"  "V44" 
## [11] "V45"  "V46"  "V81"  "V82"  "V83"  "V84"  "V85"  "V86"  "V121" "V122"
## [21] "V123" "V124" "V125" "V126" "V161" "V162" "V163" "V164" "V165" "V166"
## [31] "V201" "V202" "V214" "V215" "V227" "V228" "V240" "V241" "V253" "V254"
## [41] "V266" "V267" "V268" "V269" "V270" "V271" "V345" "V346" "V347" "V348"
## [51] "V349" "V350" "V424" "V425" "V426" "V427" "V428" "V429" "V503" "V504"
## [61] "V516" "V517" "V529" "V530" "V542" "V543"
</code></pre>

<p>Subset these variables using variable names.</p>

<div class="highlight highlight-r"><pre><span class="c">select &lt;- c(key(dt), dtFeatures$featureCode)</span>
<span class="c">dt &lt;- dt[, select, with = FALSE]</span>
</pre></div>

<h2>
<a name="user-content-use-descriptive-activity-names" class="anchor" href="#use-descriptive-activity-names"><span class="octicon octicon-link"></span></a>Use descriptive activity names</h2>

<p>Read <code>activity_labels.txt</code> file. This will be used to add descriptive names to the activities.</p>

<div class="highlight highlight-r"><pre><span class="c">dtActivityNames &lt;- fread(file.path(pathIn, "activity_labels.txt"))</span>
<span class="c">setnames(dtActivityNames, names(dtActivityNames), c("activityNum", "activityName"))</span>
</pre></div>

<h2>
<a name="user-content-label-with-descriptive-activity-names" class="anchor" href="#label-with-descriptive-activity-names"><span class="octicon octicon-link"></span></a>Label with descriptive activity names</h2>

<p>Merge activity labels.</p>

<div class="highlight highlight-r"><pre><span class="c">dt &lt;- merge(dt, dtActivityNames, by = "activityNum", all.x = TRUE)</span>
</pre></div>

<p>Add <code>activityName</code> as a key.</p>

<div class="highlight highlight-r"><pre><span class="c">setkey(dt, subject, activityNum, activityName)</span>
</pre></div>

<p>Melt the data table to reshape it from a short and wide format to a tall and narrow format.</p>

<div class="highlight highlight-r"><pre><span class="c">dt &lt;- data.table(melt(dt, key(dt), variable.name = "featureCode"))</span>
</pre></div>

<p>Merge activity name.</p>

<div class="highlight highlight-r"><pre><span class="c">dt &lt;- merge(dt, dtFeatures[, list(featureNum, featureCode, featureName)], by = "featureCode", </span>
<span class="c">    all.x = TRUE)</span>
</pre></div>

<p>Create a new variable, <code>activity</code> that is equivalent to <code>activityName</code> as a factor class.
Create a new variable, <code>feature</code> that is equivalent to <code>featureName</code> as a factor class.</p>

<div class="highlight highlight-r"><pre><span class="c">dt$activity &lt;- factor(dt$activityName)</span>
<span class="c">dt$feature &lt;- factor(dt$featureName)</span>
</pre></div>

<p>Seperate features from <code>featureName</code> using the helper function <code>grepthis</code>.</p>

<div class="highlight highlight-r"><pre><span class="c">grepthis &lt;- function(regex) {</span>
<span class="c">    grepl(regex, dt$feature)</span>
<span class="c">}</span>
<span class="c">## Features with 2 categories</span>
<span class="c">n &lt;- 2</span>
<span class="c">y &lt;- matrix(seq(1, n), nrow = n)</span>
<span class="c">x &lt;- matrix(c(grepthis("^t"), grepthis("^f")), ncol = nrow(y))</span>
<span class="c">dt$featDomain &lt;- factor(x %*% y, labels = c("Time", "Freq"))</span>
<span class="c">x &lt;- matrix(c(grepthis("Acc"), grepthis("Gyro")), ncol = nrow(y))</span>
<span class="c">dt$featInstrument &lt;- factor(x %*% y, labels = c("Accelerometer", "Gyroscope"))</span>
<span class="c">x &lt;- matrix(c(grepthis("BodyAcc"), grepthis("GravityAcc")), ncol = nrow(y))</span>
<span class="c">dt$featAcceleration &lt;- factor(x %*% y, labels = c(NA, "Body", "Gravity"))</span>
<span class="c">x &lt;- matrix(c(grepthis("mean()"), grepthis("std()")), ncol = nrow(y))</span>
<span class="c">dt$featVariable &lt;- factor(x %*% y, labels = c("Mean", "SD"))</span>
<span class="c">## Features with 1 category</span>
<span class="c">dt$featJerk &lt;- factor(grepthis("Jerk"), labels = c(NA, "Jerk"))</span>
<span class="c">dt$featMagnitude &lt;- factor(grepthis("Mag"), labels = c(NA, "Magnitude"))</span>
<span class="c">## Features with 3 categories</span>
<span class="c">n &lt;- 3</span>
<span class="c">y &lt;- matrix(seq(1, n), nrow = n)</span>
<span class="c">x &lt;- matrix(c(grepthis("-X"), grepthis("-Y"), grepthis("-Z")), ncol = nrow(y))</span>
<span class="c">dt$featAxis &lt;- factor(x %*% y, labels = c(NA, "X", "Y", "Z"))</span>
</pre></div>

<p>Check to make sure all possible combinations of <code>feature</code> are accounted for by all possible combinations of the factor class variables.</p>

<div class="highlight highlight-r"><pre><span class="c">r1 &lt;- nrow(dt[, .N, by = c("feature")])</span>
<span class="c">r2 &lt;- nrow(dt[, .N, by = c("featDomain", "featAcceleration", "featInstrument", </span>
<span class="c">    "featJerk", "featMagnitude", "featVariable", "featAxis")])</span>
<span class="c">r1 == r2</span>
</pre></div>

<pre><code>## [1] TRUE
</code></pre>

<p>Yes, I accounted for all possible combinations. <code>feature</code> is now redundant.</p>

<h2>
<a name="user-content-create-a-tidy-data-set" class="anchor" href="#create-a-tidy-data-set"><span class="octicon octicon-link"></span></a>Create a tidy data set</h2>

<p>Create a data set with the average of each variable for each activity and each subject.</p>

<div class="highlight highlight-r"><pre><span class="c">setkey(dt, subject, activity, featDomain, featAcceleration, featInstrument, </span>
<span class="c">    featJerk, featMagnitude, featVariable, featAxis)</span>
<span class="c">dtTidy &lt;- dt[, list(count = .N, average = mean(value)), by = key(dt)]</span>
</pre></div>

<p>Make codebook.</p>

<div class="highlight highlight-r"><pre><span class="c">knit("makeCodebook.Rmd", output = "codebook.md", encoding = "ISO8859-1", quiet = TRUE)</span>
</pre></div>

<pre><code>## [1] "codebook.md"
</code></pre>

<div class="highlight highlight-r"><pre><span class="c">markdownToHTML("codebook.md", "codebook.html")</span>
</pre></div></article> </div>

 