<div id="readme" class="blob instapaper_body">
    <article class="markdown-body entry-content" itemprop="mainContentOfPage"><h1>
<a name="user-content-project-for-getting-and-cleaning-data" class="anchor" href="#project-for-getting-and-cleaning-data"><span class="octicon octicon-link"></span></a>Project for Getting and Cleaning Data</h1>

<p>Author: ffamorim (<a href="https://github.com/ffamorim/GettingandCleaningData">https://github.com/ffamorim/GettingandCleaningData</a>)</p>

<h2>
<a name="user-content-parameters-for-the-project" class="anchor" href="#parameters-for-the-project"><span class="octicon octicon-link"></span></a>Parameters for the project</h2>

<blockquote>
<p>The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.  </p>

<p>One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained: </p>

<p><a href="http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones">http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones</a> </p>

<p>Here are the data for the project: </p>

<p><a href="https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip">https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip</a> </p>

<p>You should create one R script called run_analysis.R that does the following. </p>

<ol class="task-list">
<li>Merges the training and the test sets to create one data set.</li>
<li>Extracts only the measurements on the mean and standard deviation for each measurement.</li>
<li>Uses descriptive activity names to name the activities in the data set.</li>
<li>Appropriately labels the data set with descriptive activity names.</li>
<li>Creates a second, independent tidy data set with the average of each variable for each activity and each subject. </li>
</ol>
<p>Good luck!</p>
</blockquote>

<h2>
<a name="user-content-steps-to-reproduce-this-project" class="anchor" href="#steps-to-reproduce-this-project"><span class="octicon octicon-link"></span></a>Steps to reproduce this project</h2>

<ol class="task-list">
<li>Open the R script <code>run_analysis.r</code> using a text editor.</li>
<li>Change the parameter of the <code>setwd</code> function call to the working directory/folder (i.e., the folder where these the R script file is saved).</li>
<li>Run the R script <code>run_analysis.R</code>. It calls the R Markdown file, <code>run_analysis.Rmd</code>, which contains the bulk of the code.</li>
</ol><h2>
<a name="user-content-outputs-produced" class="anchor" href="#outputs-produced"><span class="octicon octicon-link"></span></a>Outputs produced</h2>

<ul class="task-list">
<li>Tidy dataset file <code>DatasetHumanActivityRecognitionUsingSmartphones.txt</code> (tab-delimited text)</li>
<li>Codebook file <code>codebook.md</code> (Markdown)</li>
</ul></article>
  </div>

  </div>
</div>
