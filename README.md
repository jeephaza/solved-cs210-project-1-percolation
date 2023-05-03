Download Link: https://assignmentchef.com/product/solved-cs210-project-1-percolation
<br>
<table width="719">

 <tbody>

  <tr>

   <td width="719">This document only contains the description of the project and the project problems. For the programming exercises on concepts needed for the project, please refer to the <a href="https://www.swamiiyer.net/cs210/project1_checklist.pdf">project checklist</a>  <a href="https://www.swamiiyer.net/cs210/project1_checklist.pdf">.</a></td>

  </tr>

 </tbody>

</table>

Given a composite system comprising of randomly distributed insulating and metallic materials: what fraction of the system needs to be metallic so that the composite system is an electrical conductor? Given a porous landscape with water on the surface (or oil below), under what conditions will the water be able to drain through to the bottom (or the oil to gush through to the surface)? Scientists have defined an abstract process known as <em>percolation </em>to model such situations.

We model a percolation system using an <em>N</em>-by-<em>N </em>grid of sites. Each site is either open or blocked. A full site is an open site that can be connected to an open site in the top row via a chain of neighboring (left, right, up, down) open sites. We say the system percolates if there is a full site in the bottom row. In other words, a system percolates if we fill all open sites connected to the top row and that process fills some open site on the bottom row. For the insulating/metallic materials example, the open sites correspond to metallic materials, so that a system that percolates has a metallic path from top to bottom, with full sites conducting. For the porous substance example, the open sites correspond to empty space through which water might flow, so that a system that percolates lets water fill open sites, flowing from top to bottom.

In a famous scientific problem, researchers are interested in the following question: if sites are independently set to be open with probability <em>p </em>(and therefore blocked with probability 1 − <em>p</em>), what is the probability that the system percolates? When <em>p </em>equals 0, the system does not percolate; when <em>p </em>equals 1, the system percolates. The plots below show the site vacancy probability <em>p </em>versus the percolation probability for 20-by-20 random grid (left) and 100-by-100 random grid (right).

When <em>N </em>is sufficiently large, there is a threshold value <em>p<sup>? </sup></em>such that when <em>p &lt; p<sup>? </sup></em>a random <em>N</em>-by-<em>N </em>grid almost never percolates, and when <em>p &gt; p<sup>?</sup></em>, a random <em>N</em>-by-<em>N </em>grid almost always percolates. No mathematical solution for determining the percolation threshold <em>p<sup>? </sup></em>has yet been derived. Your task is to write a computer program to estimate <em>p<sup>?</sup></em>.

<strong>Problem 1. </strong>(<em>Model a Percolation System</em>) To model a percolation system, create a data type Percolation in Percolation.java with the following API:

<strong>method                                              description</strong>

<table width="720">

 <tbody>

  <tr>

   <td width="182">Percolation(int N)</td>

   <td width="538">creates an <em>N</em>-by-<em>N </em>grid, with all sites blocked</td>

  </tr>

  <tr>

   <td width="182">void open(int i, int j)</td>

   <td width="538">opens site (<em>i,j</em>)</td>

  </tr>

  <tr>

   <td width="182">boolean isOpen(int i, int j)</td>

   <td width="538">checks if site (<em>i,j</em>) is open</td>

  </tr>

  <tr>

   <td width="182">boolean isFull(int i, int j)</td>

   <td width="538">checks if site (<em>i,j</em>) is full</td>

  </tr>

  <tr>

   <td width="182">int numberOfOpenSites()</td>

   <td width="538">returns the number of open sites</td>

  </tr>

  <tr>

   <td width="182">boolean percolates()</td>

   <td width="538">checks if the system percolates</td>

  </tr>

 </tbody>

</table>

<em>Corner cases. </em>By convention, the row and column indices <em>i </em>and <em>j </em>are integers between 0 and <em>N </em>− 1, where (0<em>,</em>0) is the upper-left site. Throw a java.lang.IndexOutOfBoundsException if any argument to open(), isOpen(), or isFull() is outside its prescribed range. The constructor should throw a java.lang.IllegalArgumentException if <em>N </em>≤ 0.

<em>Performance requirements. </em>The constructor should take time proportional to <em>N</em><sup>2</sup>; all methods should take constant time plus a constant number of calls to the union-find methods union(), connected(), and count().

<table width="720">

 <tbody>

  <tr>

   <td width="720">$ java Percolation data/input10.txt56 open sites percolates$ java Percolation data/input10-no.txt55 open sites does not percolate</td>

  </tr>

 </tbody>

</table>

<strong>Problem 2. </strong>(<em>Estimate Percolation Threshold</em>) To estimate the percolation threshold, consider the following computational (Monte Carlo simulation) experiment:

<ul>

 <li>Initialize all sites to be blocked.</li>

 <li>Repeat the following until the system percolates:

  <ul>

   <li>Choose a site (row <em>i</em>, column <em>j</em>) uniformly at random among all blocked sites.</li>

   <li>Open the site (row <em>i</em>, column <em>j</em>).</li>

  </ul></li>

 <li>The fraction of sites that are opened when the system percolates provides an estimate of the percolation threshold.</li>

</ul>

For example, if sites are opened in a 20-by-20 grid according to the snapshots below, then our estimate of the percolation threshold is 204/400 = 0.51 because the system percolates when the 204th site is opened.

By repeating this computational experiment <em>T </em>times and averaging the results, we obtain a more accurate estimate of the percolation threshold. Let <em>x<sub>t </sub></em>be the fraction of open sites in computational experiment <em>t</em>. The sample mean <em>µ </em>provides an estimate of the percolation threshold, and the sample standard deviation <em>σ </em>measures the sharpness of the threshold:

<em>.</em>

Assuming <em>T </em>is sufficiently large (say, at least 30), the following provides a 95% confidence interval for the percolation threshold:

<em>.</em>

To perform a series of computational experiments, create a data type PercolationStats in PercolationStats.java with the following API:

<strong>method                                                  description</strong>

<table width="720">

 <tbody>

  <tr>

   <td width="193">PercolationStats(int N, int T)</td>

   <td width="527">performs <em>T </em>independent experiments on an <em>N</em>-by-<em>N </em>grid</td>

  </tr>

  <tr>

   <td width="193">double mean()</td>

   <td width="527">returns sample mean of percolation threshold</td>

  </tr>

  <tr>

   <td width="193">double stddev()</td>

   <td width="527">returns sample standard deviation of percolation threshold</td>

  </tr>

  <tr>

   <td width="193">double confidenceLow()</td>

   <td width="527">returns low endpoint of 95% confidence interval</td>

  </tr>

  <tr>

   <td width="193">double confidenceHigh()</td>

   <td width="527">returns high endpoint of 95% confidence interval</td>

  </tr>

 </tbody>

</table>

The constructor should take two arguments <em>N </em>and <em>T</em>, and perform <em>T </em>independent computational experiments (discussed above) on an <em>N</em>-by-<em>N </em>grid. Using this experimental data, it should calculate the mean, standard deviation, and the 95% confidence interval for the percolation threshold.

<em>Corner cases. </em>The constructor should throw a java.lang.IllegalArgumentException if either <em>N </em>≤ 0 or <em>T </em>≤ 0.

$ java PercolationStats 100 1000 mean       = 0.592804 stddev       = 0.015764 confidenceLow = 0.591827 confidenceHigh = 0.593781

<strong>Acknowledgements </strong>This project is an adaptation of the Percolation assignment developed at Princeton University by Robert Sedgewick and Kevin Wayne.