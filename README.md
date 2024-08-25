## In this notebook:
- **Basic concepts**
    - Cortical neuron
    - Mathematical neuron model
- **Modeling neurons in motor units based on EEG signals**
    - Importing libraries
    - Loading data
    - Data analysis
    - Defining spiking neuron class
- **Simulating neuron firings**
    - Different preprocessing
        - Raw data
        - Rectified data
    - Different parameters
        - Parameter a
        - Parameter b
        - Parameter d
    - Neuron firing spikes as intensity control
        - Frame preparation
        - Comparing RMS and Number-of-Spikes
    - Modelling nerve bundle
    - Replicating neuron firings
- **Conclusion**

        

# Basic concepts
## Cortical neuron
Cortical neurons, found in the brain's cerebral cortex, are essential players in how we think, remember, and perceive the world around us. The cerebral cortex itself is a complex layer of neural tissue, made up of about 10 to 14 billion neurons, and it's involved in nearly everything that makes us who we are. Because of their crucial role in these processes, primary cortical neurons are incredibly valuable in neurobiology research. Scientists rely on them to explore the brain's intricate workings and to better understand how our minds function, making them a key focus in studies aimed at unraveling the mysteries of the human brain.
![Image](https://i.imgur.com/xOQs8jA.png)

The neocortex, which makes up the majority of the human brain, is a highly structured and organized region that supports complex cognitive functions. Neurons in the neocortex are arranged in layers and columns, each containing a specific type and number of neurons.

There are 5 main classes of neurons:

- Non-Accommodating (NAC): These neurons fire repetitively without frequency adaptation, meaning their firing rate doesn't decrease during prolonged stimuli. Their action potentials are brief, with a deep fast afterhyperpolarization (fAHP).

- Accommodating (AC): These neurons exhibit frequency adaptation during prolonged stimuli, meaning their firing rate decreases over time.

- Stuttering (STUT): These neurons fire in clusters of action potentials separated by periods of silence. The action potentials within a cluster show little to no adaptation, and the duration of the silent periods between clusters is unpredictable.

- Irregular Spiking (IS): These neurons discharge single action potentials randomly during sustained stimuli, showing marked accommodation.

- Bursting (BST): These neurons typically fire a burst of action potentials at the onset of a stimulus, followed by a strong afterhyperpolarization (sAHP). The response can vary from repetitive bursting to a single powerful burst followed by silence.


![Image](https://i.imgur.com/t9zH3t8.png)

![Image](https://i.imgur.com/DMnaXQ9.png)

Source: [Markram, H., Toledo-Rodriguez, M., Wang, Y., Gupta, A., Silberberg, G., & Wu, C. (2004). Interneurons of the neocortical inhibitory system. Nature Reviews Neuroscience, 5(10), 793-807. doi:10.1038/nrn1519](https://www.nature.com/articles/nrn1519)

## Mathematical neuron model
This metod shows simple and effective modeling of neuron firing. It allows us to model different neurons based on the amplitude of EEG. First, we acquire signal from one channel. We apply following algorithm:

![Image](https://i.imgur.com/Pk5U2eW.png)

and spiking, resetting:

![Image](https://i.imgur.com/T1BTJJf.png)

where:

- *v* is voltage, neurons membrane potential at given time
- *v'* is small difference changing with smallest possible quantity of time, described as dv/dt
- *u* is recovery variable , which accounts for the activation of K+ ionic currents and inactivation of N+ ionic currents, and it provides negative feedback to v
- *u'* is small difference changing with smallest possible quantity of time, described as du/dt
- *a* describes time scale of the recovery variable u. Smaller a means slower recovery, default a =  0.01
- *b* describes sensitivity of the recovery variable u to the subthreshold fluctuations of membrane potential v. Bigger b couples v and u more, default b = 0.2
- *c* is default starting point and after-spike reset value of membrane potential v, default c = -65 mV
- *d* describes after-spike reset of the recovery variable u caused by slow high-thresholds Na+ and K+ conductances, default d = 2
- *I* is input value at current time. Usually it is defined in amps or miliamps, in our case it is amplitude of EEG signal

Different parameters create different neuron behavior. Such table is show below.

![Image](https://i.imgur.com/ZGUkAwm.png)

 A possible extension of the model is to treat u, a and b as vectors, and use sum(u) instead of u in the voltage. This accounts for slow conductances with multiple time scales. Additionally  if one is interested in the behavior of a single neuron, then other choices of the function are available, and sometimes more preferable. For example, the function 0.04v^2+4.1v+108 with b = 0.1 is a better choice for the RS neuron, since it leads to the saddle-node on invariant circle bifurcation and Class 1 excitability. For more information:

Source: [Simple Model of Spiking Neurons by Eugene M. Izhikevich](https://www.izhikevich.org/publications/spikes.pdf)

# Conclusion

In this notebook I have modeled neurons firing patterns with different parameters based on EEG signals. I've shown theory on how cortal neuron operates, mathematical algorithm for producing firings and explained all variables. The model has been contained in a single class called SpikingNeuron. It's parameters can be set at definitions or left blank (they will be set to default values). It has two methods: _update which is private method used to update recovery variable u and voltage at current time v. Method transform takes a list of datapoints, performs right calculations and returns a list of potential values of neuron with spikes.

![Image](https://i.imgur.com/V3bJv4N.png)

In section "Simulating neuron firings" I've visualised different behaviours based on:
- Data
    - raw,
    - rectified,
- Parameters
    - a,
    - b,
    - d.

For more information about parameters and behavior please refer to the intro section "Mathematical neuron model".

Next, a nerve bundle made out of 5 neurons have been modeled. It is opening doors to many more ideas such as using neurons as a features in machine learning models, using them as threshhold and it brings us closer to fuller, better decomposition of EEG signal for more precise control.

Finally, I've tried to recreate a firing pattern of a recorded signal by using spiking neuron model. I've accomplished partial succes, as the model could be more precisely calibrated.

![Image](https://i.imgur.com/qtnboLx.png)
