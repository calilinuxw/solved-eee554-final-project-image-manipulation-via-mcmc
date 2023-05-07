Download Link: https://assignmentchef.com/product/solved-eee554-final-project-image-manipulation-via-mcmc
<br>



In this project, you will use Markov Chain Monte Carlo (MCMC) to create abstract image mosaics from real photographs. Figure 1 shows an example: on the left is the original photo (of the Opera House in Sydney, Australia), and on the right is a mosaic of the same image constructed with MCMC.

Figure 1: A real photograph on the left, and an abstract mosaic created from it using MCMC on the right.

First, read and understand this document. Then you will write code to implement MCMC, first to analyze a simple distribution, and then to do image manipulation. (As usual, Matlab is recommended but not required.) You should turn in a report which includes your well-documented code, as well as a write-up describing your implementation details, and the answers to specific questions asked below. Specific tasks that <em>must </em>be covered in your project report will be labeled by the bold word Task. To reiterate this: communication skills, combined with technical knowledge, are critical to any career in engineering. Part of the purpose of the projects in this class is for you to practice these communication skills by writing a report that clearly describes what you did and how you made your design decisions.

Your grade for the final project will be 50% based on the correctness of your implementations, and 50% based on the clarity of your project report. You may discuss the project in broad terms with other students, but you must work independently.

<h1>1          MCMC and the Metropolis-Hastings Algorithm</h1>

The basic theory of MCMC was covered in Recitation 12 on November 12. You are encouraged to watch this recitation video if you haven’t already. MCMC is a technique to generate samples from a target distribution <em>p<sub>X</sub></em>(<em>x</em>) or <em>f<sub>X</sub></em>(<em>x</em>) (the distribution can be discrete or continuous). The idea is to set up a homogenous Markov chain <em>X</em>[<em>n</em>] with stationary distribution equal to the target distribution. Thus, the Markov chain can be initialized at any value, then propagated forward long enough for the Markov chain to mix, at which point it will generate samples from the target distribution.

There are several algorithms that implement the MCMC idea. The specific algorithm we will use in this project is called Metropolis-Hastings, which is outlined in the following pseudocode.

Input: Initial value <em>X</em>[0], target distribution <em>p<sub>X</sub></em>(<em>x</em>), and a conditional distribution <em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) where <sup>P</sup><em><sub>x</sub></em>0 <em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) = 1 for all <em>x</em>, and where <em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) = <em>g</em>(<em>x</em>|<em>x</em><sup>0</sup>). Output: Markov chain <em>X</em>[<em>n</em>] with stationary distribution <em>p<sub>X</sub></em>(<em>x</em>)

A few notes about this algorithm:

<ol>

 <li>The pseudocode above is written for a discrete distribution <em>p<sub>X</sub></em>, but the algorithm can also be used for a continuous distribution <em>f<sub>X</sub></em>. The only changes required are to replace the ratio of PMFs with the ratio of PDFs <sup>0 </sup>, and to replace the conditional PMF <em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) with a conditional PDF; i.e., <sup>R </sup><em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>)<em>dx</em><sup>0 </sup>= 1 for all <em>x</em>. (It is still required that <em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) = <em>g</em>(<em>x</em>|<em>x</em><sup>0</sup>).)</li>

 <li>Metropolis-Hastings only requires the calculation of the ratio. Thus, if it is easier to compute a function <em>h</em>(<em>x</em>) where <em>p<sub>X</sub></em>(<em>x</em>) = <em>ch</em>(<em>x</em>) for some constant <em>c</em>, then it is equally good</li>

</ol>

0 to use the ratio.

<ol start="3">

 <li>While it is technically true that Metropolis-Hastings yields a Markov chain <em>X</em>[<em>n</em>] with the correct stationary distribution no matter what <em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) is used, in practice the choice of this conditional distribution has a big impact on the efficiency of the algorithm. First, a conditional distribution <em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) should be chosen that is easy to generate samples from. Second, <em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) can impact how quickly the Markov chain mixes; if it mixes too slowly, then the algorithm must be run for a long time for the samples produced in <em>X</em>[<em>n</em>] to be from the correct distribution. One of the disadvantages of MCMC is that it is usually hard to know for sure whether the Markov chain has mixed. Even so, MCMC is very useful in numerous applications.</li>

</ol>

<h1>2          Implement Metropolis-Hastings on a Continuous Scalar Distribution</h1>

To become familiar with Metropolis-Hastings, you will first implement it for a continuous distribution of a single random variable.

Task 1 <em>Implement Metropolis-Hastings for the same continuous distribution that was assigned to you in the midterm project. To initialize the Markov chain, set </em><em>X</em>[0] = <em>x</em><sub>0 </sub><em>for a constant </em><em>x</em><sub>0 </sub><em>of your choice. Use </em><em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) = N(<em>x,</em>1)<em>. Explain why this conditional distribution satisfies </em><em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) = <em>g</em>(<em>x</em>|<em>x</em><sup>0</sup>)<em>. Run the algorithm to generate at least 10,000 samples. Use these samples to plot an estimated PDF, and compare against the true PDF.</em>

Next, to understand how quickly this Markov chain mixes, you will conduct some experiments to understand the PDF of <em>X</em>[<em>n</em>] for specific values of <em>n</em>.

Task 2 <em>With the same initialization and choice of </em><em>g as in the previous task, estimate and plot the PDF of </em><em>X</em>[<em>n</em>] <em>for </em><em>n </em>= 1<em>,</em>3<em>,</em>10<em>,</em>30<em>,</em>100<em>. Note that this requires generating multiple samples of </em><em>X</em>[<em>n</em>] <em>for each of these </em><em>n values. For example, to generate multiple samples of </em><em>X</em>[1]<em>, initialize the Markov chain to </em><em>X</em>[0] = <em>x</em><sub>0</sub><em>, run the algorithm for one step to generate </em><em>X</em>[1]<em>, then initialize the Markov chain again to generate the next sample, and so on. When you re-initialize the Markov chain, be sure not to reset the seed for the random number generator, or else you will get the sample of </em><em>X</em>[1] <em>over and over again. Does it seem as if the PDF of </em><em>X</em>[<em>n</em>] <em>approaches </em><em>f<sub>X </sub>as </em><em>n </em>→ ∞<em>?</em>

How quickly the Markov chain mixes depends on the choice of <em>g</em>. Next, you will try different conditional distributions <em>g </em>to see if this improves the mixing time.

Task 3 <em>Repeat Task 2 for each of the following conditional distributions </em><em>g:</em>

<ol>

 <li><em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) = N(<em>x,</em>10)</li>

 <li><em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) = N(<em>x,</em>0<em>.</em>1)</li>

 <li><em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) = U(<em>x </em>− 0<em>.</em>5<em>,x </em>+ 0<em>.</em>5)</li>

</ol>

<em>Also, confirm that each of these distributions satisfy </em><em>g</em>(<em>x</em><sup>0</sup>|<em>x</em>) = <em>g</em>(<em>x</em>|<em>x</em><sup>0</sup>)<em>. Which choice seems to mix fastest?</em>

<h1>3          Choose and Load an Image</h1>

You will create an abstract mosaic by starting from a real photograph. If you can find one that you like from your personal collection, great! Otherwise, feel free to choose a photograph from any source. It is best to choose an image that has multiple shapes of somewhat flat color, so that when it is converted into a mosaic that has only a few colors, the basic shape of the image will be preserved. Crop and/or scale the image so that its resolution is 300×300 pixels. (You may experiment with smaller or larger resolutions, but it is recommended to start at this size.)

Once you have chosen an image, you need to load it into your computing environment. This can take some time to set up correctly. In Matlab, use the function imread to load an image file. By default, imread outputs a three-dimensional array of dimension <em>m </em>× <em>n </em>× 3, where <em>m </em>× <em>n </em>is the resolution of the image, and the third dimension holds the red, green, and blue channels. Images can be displayed in Matlab using the command image, which accepts input

Figure 2: An example of a mosaic image created without using a real photograph as a reference.

arrays in the same format that imread produces. Images can be saved to a file using the command imwrite. One important note is that, by default, imread produces an array with values stored in the uint8 format (unsigned 8-bit integers). To convert an array A to standard floating point for easier manipulation, use the command double(A); to convert back use uint8(A).

Task 4 <em>Choose an image. Write code to load it into your computing environment and then display it.</em>

<h1>4          The Distribution for a Mosaic Image</h1>

In order to produce mosaics images as in Figure 1, we first need to design a distribution that models the characteristics of these images. At first, ignore the requirement that the mosaic image approximates a real photograph, then later you will add this functionality. Figure 2 shows an example of a mosaic image that is not based on any existing photograph. The mosaic distribution will not be a simple scalar distribution like the one that you analyzed above, but rather a complicated distribution composed of multiple random variables. The Metropolis-Hastings algorithm can be generalized to work with any random vector <strong>X</strong>, rather than a scalar random variable <em>X</em>.

A mosaic image consists of contiguous regions of colors selected from a small number of color options (3–15 depending on preference). “Contiguous” means that two neighboring pixels usually have exactly the same color. We can model this probabilistically in the following way. The image will be represented by two parts: a color palette, and a color index for each pixel. The color palette consists of <em>k </em>colors, each of which consists of 3 numbers representing the red, green, and blue channels. That is, the palette consists of 3<em>k </em>random variables denoted <em>P<sub>ij </sub></em>where <em>i </em>= 1<em>,…,k </em>and <em>j </em>= 1<em>,</em>2<em>,</em>3. Each of the palette numbers should be between 0 and 255. The color index for each pixel is given by <em>Z<sub>ij </sub></em>for the pixel at position (<em>i,j</em>), where <em>i </em>= 1<em>,…,m </em>and <em>j </em>= 1<em>,…,n</em>, assuming <em>m </em>× <em>n </em>is the resolution of the image. Each of the index numbers <em>Z<sub>ij </sub></em>∈ {1<em>,</em>2<em>,…,k</em>} indicates which color from the color palette this pixel takes on. That is, <em>Z<sub>ij </sub></em>= <em>` </em>means that the color of the pixel at position (<em>i,j</em>) has RGB representation (<em>P<sub>`,</sub></em><sub>1</sub><em>,P<sub>`,</sub></em><sub>2</sub><em>,P<sub>`,</sub></em><sub>3</sub>). Denote <strong>P </strong>to be the vector of all the palette variables, and <strong>Z </strong>the vector of all index variables. Putting this together, the random vector

Figure 3: A representation of neighboring pixels in an image. Each square is a pixel, and the neighbors of the central pixel are those indicated by the arrows.

<strong>X </strong>= [<strong>P</strong><em>,</em><strong>Z</strong>] represents the entire mosaic image. Note that, in order to display or save the image, you will need to convert <strong>X </strong>into an <em>m </em>× <em>n </em>× 3 image array containing the color of each pixel.

We still need to specify the distribution of these variables. Assume the color palette variables <em>P<sub>ij </sub></em>are uniform on the interval (0<em>,</em>255). The index variables <em>Z<sub>ij </sub></em>should have a distribution such that neighboring pixels are more likely to have the same index than not. We can set this up as follows:

                                                               

                      X                                       

<em>p</em><strong><sub>Z</sub></strong>(<strong>z</strong>) = <em>c </em>exp     <em>r                                          δ</em>[<em>z<sub>ij </sub></em>− <em>z<sub>i</sub></em>0<em><sub>j</sub></em>0]     <em>.</em>

 <em>i,j,i</em><sup>0</sup><em>,j</em><sup>0</sup>&#x1f641;<em>i,j</em>) is a neighbor of (<em>i</em><sup>0</sup><em>,j</em><sup>0</sup>)                                                

Recall that <em>δ</em>[0] = 1 and <em>δ</em>[<em>k</em>] = 0 for <em>k </em>6= 0; thus, the summation inside the exponential counts the number of neighboring pixels (<em>i,j</em>) and (<em>i</em><sup>0</sup><em>,j</em><sup>0</sup>) that have the same color index. That is, this distribution says that it is <em>more likely </em>that neighboring pixels have the same color than not. The constant <em>r </em>is a parameter that you must choose: larger <em>r </em>means whether neighboring pixels are the same has a stronger effect on the probability. The constant <em>c </em>is only there to ensure that this is a proper distribution; to implement Metropolis-Hastings, you do not need to know <em>c</em>.

We still haven’t specified exactly what “neighboring” pixels are. For these purposes, we define neighboring pixels as any two pixels that are adjacent horizontally, vertically, <em>or </em>diagonally. See the diagram in Figure 3.

You will implement Metropolis-Hastings to create samples from this distribution on mosaic images. To do this, you need to carefully select the conditional distribution <em>g</em>(<strong>x</strong><sup>0</sup>|<strong>x</strong>). Recall that the vector of random variables <strong>X </strong>consists of the <em>P<sub>ij </sub></em>variables and the <em>Z<sub>ij </sub></em>variables. The conditional distribution <em>g</em>(<strong>x</strong><sup>0</sup>|<strong>x</strong>) amounts to a way to choose a new guess <strong>x</strong><sup>0 </sup>from an existing guess <strong>x</strong>. This needs to be done in a way so that the distribution is symmetric (i.e., <em>g</em>(<strong>x</strong><sup>0</sup>|<strong>x</strong>) = <em>g</em>(<strong>x</strong>|<strong>x</strong><sup>0</sup>)), so that it is easy to sample from, and so that the entire space will be eventually explored. A good way to implement a conditional distribution <em>g</em>(<strong>x</strong><sup>0</sup>|<strong>x</strong>) is as follows. First, randomly choose one among all of these variables, and then randomly adjust only that single variable. Note that <em>Z<sub>ij </sub></em>are discrete variables, since they only take values in {1<em>,…,k</em>}, while <em>P<sub>ij </sub></em>are continuous, since they can take any value in the interval [0<em>,</em>255]. You will need to find a good way to adjust either one of the <em>Z<sub>ij </sub></em>variables or one of the <em>P<sub>ij </sub></em>variables. Be sure to do so in a way that the conditional distribution is symmetric. Note that, even though you will only adjust one of these variables at a time, since the identity of the variable is chosen randomly, you will still fully explore the space of all possibly images.

Task 5 <em>Implement Metropolis-Hastings to produce a sample from the mosaic image distribution for a </em>300 × 300 <em>image with </em><em>k </em>= 4 <em>colors. Initialize your Markov chain by randomly selecting </em><em>P<sub>ij </sub></em>∼ U(0<em>,</em>255) <em>and </em><em>Z<sub>ij </sub>uniformly from </em>{1<em>,</em>2<em>,…,k</em>}<em>. You will need to experiment to find a good value for the constant </em><em>r. Note that for this task you do not need to store the entire chain. Rather, you only want to produce a single sample from the distribution. Thus, you only need to run the algorithm long enough for the Markov chain to mix, and keep the last value; this will probably take millions of iterations. You will never truly know if your Markov chain has mixed, but if you can produce an image roughly like Figure 2, then your algorithm is working correctly. You may find it helpful to regularly display the current image to see if it looks right. Include the resulting mosaic image in your report.</em>

<em>When implementing your algorithm, you will need to be careful to make your code as efficient as possible, or else the algorithm will take too long to mix. In particular, you should consider how to optimally calculate the ratio</em><em>. It may be helpful to profile your code. In Matlab, this can be done by clicking the “Run and Time” button. This will run the code as usual, and then produce an interactive dataset that tells you how much time the code spent in which sections. If your code is taking surprisingly long at certain operations, you should think about how to make those operations more efficient. (If you are not using Matlab, most other computing environments also have a mechanism to profile your code.)</em>

<h1>5          Incorporating Your Image</h1>

We now need to adjust the probability distribution so that the resulting mosaic image looks roughly like your chosen image. Do this by adding a term to the probability distribution which encourages the color of each pixel in the mosaic image to be close to that in the original image. Let <em>A<sub>ij` </sub></em>be the value of your image at pixel (<em>i,j</em>) in channel <em>`</em>, where <em>` </em>= 1<em>,</em>2<em>,</em>3. Also let <em>C<sub>ij`</sub></em>(<strong>p</strong><em>,</em><strong>z</strong>) be the corresponding value for the mosaic image with color palette variables <strong>p </strong>and index variables <strong>z</strong>. The distribution is given by

<em> .</em>

Here, <em>f</em><strong><sub>P </sub></strong>and <em>p</em><strong><sub>Z </sub></strong>are the same distributions on <strong>P </strong>and <strong>Z </strong>as above; <em>s </em>is a parameter that you must choose; and again <em>c </em>is a constant to make this a proper distribution, which you do not need to worry about. Note that this probability will be larger if <em>C<sub>ij`</sub></em>(<strong>p</strong><em>,</em><strong>z</strong>) is closer to <em>A<sub>ij`</sub></em>.

Task 6 <em>Adjust your Metropolis-Hastings algorithm to incorporate the real image, and produce a mosaic image like that in Figure 1 that preserves the basic shape of the image with only a small number of colors. The number of colors </em><em>k is your choice: choose a number that looks good to you! Be sure to properly document all the decisions you make in your implementation.</em>

Figure 4: A different abstract image made starting from the same original image as in Figure 1.

<h1>6          Make Something Different</h1>

MCMC is a powerful tool for this kind of data manipulation. By choosing different distributions on the color palette and the color indexes, you can create all sorts of different images. An example is shown in Figure 4, made from the same original image as in Figure 1; this one was made using MCMC with a slightly different underlying distribution, but I won’t tell you exactly how. For the final task, you will explore these possibilities.

Task 7 <em>Make a different kind of abstract image via MCMC. Be sure to clearly describe what you did. Be creative! Bonus points will be given for especially cool results. Some ideas to consider (but do not feel limited by these):</em>

<ol>

 <li><em>Change the distribution of the color palette.</em></li>

 <li><em>Change the relationship between color indexes in neighboring pixels.</em></li>

 <li><em>Change the definition of “neighboring” pixels.</em></li>

 <li><em>Change what it means for the mosaic color and the real photo color to be “close”.</em></li>

</ol>