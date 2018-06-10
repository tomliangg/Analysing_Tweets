# Analysing_Tweets-CMPT353_e2_p2
<h3>This repo is created for documentation purpose. The repo contains my personal work toward the SFU CMPT353 (Computational Data Science) course. You may use my solution as a reference. The .zip archive contains the original exercise files. For practice purpose, you can download the .zip archive and start working from there.</h3>

<p><a href="https://coursys.sfu.ca/2018su-cmpt-353-d1/pages/AcademicHonesty">Academic Honesty</a>: it's important, as always.</p>
<p>Below is the exercise description </p>
<hr>

<h2 id="h-pup-inflation-analysing-tweets">Pup Inflation: Analysing Tweets</h2>
<p>This question is heavily inspired by <a href="http://dhmontgomery.com/2017/03/dogrates/">David H. Montgomery's Pup Inflation</a> post. His analysis is an excellent data science task, and we will ask the same question here: has there been grade inflation on the  <a href="https://twitter.com/dog_rates">@dog_rates</a> Twitter, which rates the cuteness of users' dog pictures?</p>
<p>I scraped the @dog_rates feed with <a href="https://gist.github.com/yanofsky/5436496">tweet_dumper.py</a>. The result it produced is provided in the <code>dog_rates_tweets.csv</code> file, so we don't all have to scrape the data.</p>
<p>Do this analysis in a <strong>Jupyter notebook <code>dog-rates.ipynb</code></strong>. To look for score inflation, we'll first have to make sense of the data. The steps I think are necessary to do this:</p>
<ul><li>Load the data from the CSV into a DataFrame. (Assume a <code>dog_rates_tweets.csv</code> file is in the same folder as the notebook file.)
</li><li>Find tweets that contain an <span>&ldquo;</span>\(n\)/10<span>&rdquo;</span> rating (because not all do). Extract the numeric rating. Exclude tweets that don't contain a rating.
</li><li>Remove outliers: there are a few obvious ones. Exclude rating values that are too large to make sense. (Maybe larger than 25/10?)
</li><li>Make sure the 'created_at' column is a datetime value, not a string. You can either do this by applying a function that parses the string to a date (likely using <a href="https://docs.python.org/3/library/datetime.html#datetime.datetime.strptime">strptime</a> to create a datetime object), or by asking Pandas' <code>read_csv</code> function to parse dates in that column with a <code>parse_dates</code> argument.
</li><li>Create a scatter plot of date vs rating, so you can see what the data looks like.
</li></ul>
<p>[The question continues, and there are a few hints below. You may want to do this part of the question and make sure things are working before continuing.]</p>
<h3 id="h-linear-fitting">Linear Fitting</h3>
<p>One analysis Montgomery didn't do on the data: a best-fit line.</p>
<p>The <code>scipy.stats.linregress</code> function can do a linear regression for us, but it works on numbers, not datetime objects. Datetime objects have a <code>.timestamp()</code> method that will give us a number (of seconds after some epoch), but we need to get that into our data before using it. If you write a function <code>to_timestamp</code> then you can do one of these (if it's a normal Python function, or if it's a NumPy ufunc, respectively):</p>
<pre class="highlight lang-python">data['timestamp'] = data['created_at'].apply(to_timestamp)
data['timestamp'] = to_timestamp(data['created_at'])</pre>
<p>You can then use <code>linregress</code> to get a slope and intercept for a best fit line.</p>
<p>Produce results like those found in the provided screenshot, <code>dog-rates-result.png</code>. <strong>At the end of your notebook</strong> (so the TA knows where to look), show the data itself, the slope and intercept of the best-fit line, and a scatterplot with fit line.</p>
<h3 id="h-hints">Hints</h3>
<p>This  <a href="https://docs.python.org/3/library/re.html">Python regular expression</a> will look for <span>&ldquo;</span>\(n\)/10<span>&rdquo;</span> strings in the format they seem to occur in the tweets. If this is found by searching in a tweet, then the resulting <a href="https://docs.python.org/3/library/re.html#match-objects">match object</a> can be used to get the numeric rating as a string, which can then be converted to a float.</p>
<pre class="highlight lang-python">r'(\d+(\.\d+)?)/10'</pre>
<p>I think the easiest way to <span>&ldquo;</span>exclude<span>&rdquo;</span> some rows from the DataFrame is to return <code>None</code> for rating values that aren't valid ratings, and then use <a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.notnull.html#pandas.Series.notnull">Series.notnull</a> to create a boolean index. There are certainly other ways to do the job as well.</p>
<p>To plot the best-fit line, the \(x\) values must be datetime objects, not the timestamps. To add the best-fit line, you can plot <code>data['created_at']</code> against <code>data['timestamp']*fit.slope + fit.intercept</code> to get a fit line (assuming you stored the results of <code>linregress</code> in a variable <code>fit</code>).</p>
<p>Here are some hints to style the plot as it appears in my screenshot, which seems to look nice enough:</p>
<pre class="highlight lang-python">plt.xticks(rotation=25)
plt.plot(???, ???, 'b.', alpha=0.5)
plt.plot(???, ???, 'r-', linewidth=3)</pre>
<h2 id="h-questions">Questions</h2>
<p>Answer these questions in a file <code>answers.txt</code>. [Generally, these questions should be answered in a few sentences each.]</p>
<ol><li>In the hint above, describe the values that are the result of <code>data['timestamp']*fit.slope + fit.intercept</code>? How is this calculated?
</li><li>In the same hint, why does this produce a fit line on the graph? Why are the <code>created_at</code> values and <code>timestamp</code> values paired correctly to make points on the plot?
</li></ol>
