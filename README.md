Download Link: https://assignmentchef.com/product/solved-ee746-assignment2-discerning-timing-dependent-signals
<br>
Before the solution key is uploaded in moodle: If your original score is <em>S </em>and you submitted the HW <em>X </em>hours after the deadline, your score will be <em>S </em>exp(−<em>X/</em>24).

After the solution key is uploaded in moodle: 0 credit.

<h1>Discerning timing dependent signals</h1>

In this assignment, we will understand how integrate and fire neurons with distinct synapses can distinguish timing dependent stimulus with very similar statistical characteristics.

<strong>Problem 1: AEF neuron driven by a synapse receiving Poisson stimulus</strong>

<ul>

 <li>Create a Poisson stimulus with <em>T </em>= 500 ms, ∆<em>t </em>= 0<em>.</em>1 ms and <em>λ </em>= 10<em>/s</em>. Determine the time instants when stimulus arrives, <em>t<sub>k</sub></em>.</li>

 <li>Assume that this stimulus arrives at an AEF RS neuron (from HW1) through a synapse.Then, the total current flowing into the neuron through the synapse at any time <em>t </em>will depend on the stimulus arrival times prior to time <em>t </em>(Say <em>t</em><sup>1</sup><em>,t</em><sup>2</sup><em>,t</em><sup>3</sup><em>,…t<sup>n</sup></em>, with <em>t<sup>n </sup>&lt; t</em>). We will model the current at time <em>t </em>according to the expression</li>

</ul>

(1)

This form of modeling the synaptic current is particularly amenable for hardware implementation, as it depends only on the synaptic strength and stimulus times (and not on post-synaptic potentials).

Assume that <em>I</em><sub>0 </sub>= 1 pA and strength of the synapse is given by <em>w<sub>e </sub></em>= 500. Also, <em>τ </em>= 15 ms represents the time constant of the membrane and <em>τ<sub>s </sub></em>= <em>τ/</em>4 is the synaptic time constant.

Plot the response of the neuron along with the input current and stimulus to convince yourself that the neuron emits spikes only if closely spaced stimulus are present. <strong>8 points Problem 2: AEF neuron driven by multiple synapses</strong>

We would like to use this framework to understand the dynamics of a neuron that is receiving inputs from multiple synapses.

Assume we have a total of <em>N<sub>s </sub></em>= 100 synapses driving the neuron, whose connection strengths are gaussian distributed, with a mean strength of <em>w</em><sub>0 </sub>and standard deviation of <em>σ<sub>w</sub></em>. As you may expect, larger magnitudes of <em>w</em><sub>0 </sub>has higher probability of causing the neuron to spike. (You may use the randn function from MATLAB to create the strengths for the synapses).

<ul>

 <li>Assume <em>w</em><sub>0 </sub>= 50 and <em>σ<sub>w </sub></em>= 5 and create a population of <em>N<sub>s </sub></em> For each synapse, create a Poisson stimulus with <em>T </em>= 500 ms, ∆<em>t </em>= 0<em>.</em>1 ms and <em>λ </em>= 1<em>/s</em>. Determine the total current flowing into the neuron based on equation (1) and plot the response of the neuron. How many spikes are issued by the neuron in the interval [0<em>,T</em>]?</li>

 <li>For the same stimulus in (a), plot the total current and the response of the neuron, butwith a new configuration of synaptic strengths defined by a guassian distribution whose mean is <em>w</em><sub>0 </sub>= 250 and <em>σ<sub>w </sub></em>= 25. How many spikes are issued by the neuron in the interval [0<em>,T</em>]?</li>

</ul>

<strong>Problem 3: Adjusting the weights to elicit a spike response</strong>

As you would have learnt from problem (3), a naive way to elicit a spike response from the neuron is to increase the strength of every synapse. But, we could actually do this in a more efficient manner.

Start with a synaptic configuration given by <em>w</em><sub>0 </sub>= 50 and <em>σ<sub>w </sub></em>= 5 and the same stimulus condition as in Problem 3. Look at the response of the neuron and determine the time instant, <em>t<sub>max </sub></em>where the neuron membrane potential was the maximum in the interval [0<em>,T</em>].

Identify the synapse (call it <em>S<sub>p</sub></em>) that received a stimulus just prior to <em>t<sub>max</sub></em>. (i.e., If the <em>k<sup>th </sup></em>synapse received its stimulus at time <em>t<sub>k</sub></em>, find the synapse for which the quantity ∆<em>t<sub>k </sub></em>= (<em>t<sub>max </sub></em>− <em>t<sub>k</sub></em>) is the minimum (and positive).)

You would expect that if you left all other synapses unaltered, but just increased the magnitude of <em>S<sub>p </sub></em>by a sufficient amount, the neuron will have a higher probability to spike. Verify if this is indeed the case.

A more general way to implement this scheme of changing the synaptic strengths in order to elicit a spike is to modify the strength of each synapse depending on its value of ∆<em>t<sub>k</sub></em>. We will use the rule

∆<em>w</em><em>k </em>= +<em>w</em><em>kγ</em>(<em>e</em>−∆<em>t</em><em>k</em><em>/τ </em>− <em>e</em>−∆<em>t</em><em>k</em><em>/τ</em><em>s</em>)<em>h</em>(∆<em>t</em><em>k</em>)                                                   (2)

where <em>γ </em>is a parameter that controls the rate of learning. <em>h</em>(<em>x</em>) is the heaviside step function defined as <em>h</em>(<em>x</em>) = 1 for <em>x </em>≥ 0 and <em>h</em>(<em>x</em>) = 0 otherwise.

With this rule, synapses that have the potential to contribute the most to a neuron spike are selectively increased. It might be necessary to apply this rule to the entire population of synapses multiple times, depending on the value of the learning rate. Also, typical electronic devices that could be used as synapses have an on-off ratio of about 10 − 50; hence, assume that the maximum synaptic strength can not exceed 500.

<ul>

 <li>Write a program to implement this learning rule and determine the number of iterationsthat are required to cause the neuron to create at least one spike for <em>γ </em>= 1. The output of your code should be the set of weights after training. <strong>14 points</strong></li>

 <li>Plot, on the same graph, the value of ∆<em>w<sub>k </sub></em>vs ∆<em>t<sub>k </sub></em>for every synapse and every training iteration. <strong>1 points Problem 4: Adjusting the weights to remove all spike responses</strong></li>

</ul>

You may also realize now that a naive way to remove all spike response from the neuron is to decrease the strength of every synapse. Again, we could do this in a more efficient manner. Start with a synaptic configuration given by <em>w</em><sub>0 </sub>= 250 and <em>σ<sub>w </sub></em>= 25 and the same stimulus condition as before. Look at the response of the neuron and determine the time instants, <em>t</em><sup>1</sup><em><sub>sp</sub>,t</em><sup>2</sup><em><sub>sp</sub>,t</em><sup>3</sup><em><sub>sp </sub>…, </em>where the neuron spiked in the interval [0<em>,T</em>].

For each spike, identify the synapse, (say <em>S<sub>q</sub></em>) that received a stimulus just prior to the spike. (i.e., If the <em>k<sup>th </sup></em>synapse received its stimulus at time <em>t<sub>k</sub></em>, then, find the synapse for which the quantity ∆) is the minimum (and positive).) By reducing the strength of the synapse <em>S<sub>q </sub></em>appropriately, you should be able to lower the probability of that particular spike. One has to do this exercise iteratively for every spike (by calculating ∆<em>t<sub>k </sub></em>for each spike) to remove all the spikes issued by the neuron.

A general way to implement this scheme of changing the synaptic strengths in order to remove all spike responses is to modify the strength of each synapse depending on the value of ∆<em>t<sub>k</sub></em>, (measured as the difference between stimulus time and the time when the neuron issued the next immediate spike). We will use the rule

∆<em>w</em><em>k </em>= −<em>w</em><em>kγ</em>(<em>e</em>−∆<em>t</em><em>k</em><em>/τ </em>− <em>e</em>−∆<em>t</em><em>k</em><em>/τ</em><em>s</em>)<em>h</em>(∆<em>t</em><em>k</em>)                                                   (3)

where <em>γ </em>is the same parameter as in problem 4. With this rule, synapses that has the potential to contribute the most to a neuron spike is selectively decreased. As before, it might be necessary to apply this rule to the entire population of synapses multiple times, depending on the value of the the learning rate. You should also assume that the synaptic strength can not fall below 10.

<ul>

 <li>Write a program to implement this learning rule and determine the average number of iterations that are required to remove all the spikes of the neuron. The output of your code should be a set of weights that results in removing all the spike from the neuron. In this experiment, start with a new population of synaptic strengths with <em>w</em><sub>0 </sub>= 250 and <em>σ<sub>w </sub></em>= 25, but the same pattern of stimulus as before.</li>

 <li>Plot, on the same graph, the value of ∆<em>w<sub>k </sub></em>vs ∆<em>t<sub>k </sub></em>for every sypanse and every training iteration. <strong>1 points Problem 5: Discriminating stimuli with similar statistical characteristics</strong></li>

</ul>

Now, assume that the synaptic population has a <em>w</em><sub>0 </sub>= 200 and <em>σ<sub>w </sub></em>= 20. Create two stimulus patterns <em>S</em><sub>1 </sub>and <em>S</em><sub>2 </sub>with <em>T </em>= 500 ms, ∆<em>t </em>= 0<em>.</em>1 ms and <em>λ </em>= 1<em>/s</em>. We would like to now determine a single set of weights for the synaptic population which will elicit opposite spike responses when these stimuli are presented.

<ul>

 <li>Determine the response of the neuron for stimulus <em>S</em><sub>1 </sub>and <em>S</em><sub>2 </sub>for the same starting synaptic strengths <em>w</em><sub>0</sub>.</li>

 <li>Apply the routine to remove all the spikes with stimulus <em>S</em><sub>2</sub>, starting with <em>w</em><sub>0</sub>. Call the resulting weight population <em>w</em><sub>1</sub>.</li>

 <li>Determine the response of the neuron for stimulus <em>S</em><sub>1</sub>, but with synaptic strength <em>w</em><sub>1 </sub>from part (b) above. If there is a spike response, you have now obtained a weight configuration that allows you to distinguish <em>S</em><sub>1 </sub>and <em>S</em><sub>2</sub>. <strong>6 points</strong></li>

</ul>

If there is no spike response when <em>S</em><sub>1 </sub>is presented, apply the routine to create a spike for stimulus <em>S</em><sub>1</sub>, but with synaptic strengths <em>w</em><sub>1</sub>. Call the new weight configuration <em>w</em><sub>2</sub>. Go back to step (b) and iterate starting with <em>w</em><sub>2</sub>, till you are able to obtain a single set of weights that causes a spike response for presentation of <em>S</em><sub>1 </sub>and no spike for presentation of <em>S</em><sub>2</sub>.

<ul>

 <li>Now, determine the weights necessary to cause a spike response for presentation of <em>S</em><sub>2 </sub>and no spike for presentation of <em>S</em><sub>1</sub>. <strong>8 points</strong></li>

</ul>

This assignment is loosely based on what is known as the ‘Tempotron’. For those interested in more details, refer to: <em>The tempotron: a neuron that learns spike timing based decisions, Robert Gutig &amp; Haim Sompolinsky, Nature Neuroscience 9, 420 – 428 (2006)</em>.