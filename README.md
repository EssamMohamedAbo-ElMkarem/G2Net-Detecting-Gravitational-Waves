# G2Net Detecting Gravitational Waves in Time and Frequency Domains with(LSTM, ViT and CNN)


## Gravitational Waves

Gravitational waves are disturbances or ripples in the curvature of spacetime, generated by accelerated masses, that propagate as waves outward from their source at the speed of light. They were first proposed by Oliver Heaviside in 1893 and then later by Henri Poincaré in 1905 and subsequently predicted in 1916 by Albert Einstein on the basis of his general theory of relativity. Later he refused to accept gravitational waves. Gravitational waves transport energy as gravitational radiation, a form of radiant energy similar to electromagnetic radiation.Newton's law of universal gravitation, part of classical mechanics, does not provide for their existence, since that law is predicated on the assumption that physical interactions propagate instantaneously (at infinite speed) – showing one of the ways the methods of Newtonian physics are unable to explain phenomena associated with relativity.

The first indirect evidence for the existence of gravitational waves came in 1974 from the observed orbital decay of the Hulse–Taylor binary pulsar, which matched the decay predicted by general relativity as energy is lost to gravitational radiation. In 1993, Russell A. Hulse and Joseph Hooton Taylor Jr. received the Nobel Prize in Physics for this discovery. The first direct observation of gravitational waves was not made until 2015, when a signal generated by the merger of two black holes was received by the LIGO gravitational wave detectors in Livingston, Louisiana, and in Hanford, Washington. The 2017 Nobel Prize in Physics was subsequently awarded to Rainer Weiss, Kip Thorne and Barry Barish for their role in the direct detection of gravitational waves. 

<img src = http://skyandtelescope.org/wp-content/uploads/BHsim-600.jpg width=600/>

## Challenge Context

When scientists detected the first class of gravitational waves in 2015, they expected the discoveries to continue. There are four classes, yet at present only signals from merging black holes and neutron stars have been detected. Among those remaining are continuous gravitational-wave signals. These are weak yet long-lasting signals emitted by rapidly-spinning neutron stars. Imagine the mass of our Sun but condensed into a ball the size of a city and spinning over 1,000 times a second. The extreme compactness of these stars, composed of the densest material in the universe, could allow continuous waves to be emitted and then detected on Earth. There are potentially many continuous signals from neutron stars in our own galaxy and the current challenge for scientists is to make the first detection, and hopefully data science can help with this mission.

<img src=https://storage.googleapis.com/kaggle-media/competitions/G2Net-gravitational-waves/O3h0senscurve%20jpeg.jpg width=500/>

This image, taken from a 2021 paper by the LIGO-Virgo-KAGRA collaboration, shows the maximum amplitude of a continuous wave any of these neutron stars could emit without being found by the search analyses. Circled stars show results constraining the physical properties of specific neutron stars. Traditional approaches to detecting these weak and hard-to-find continuous signals are based on matched-filtering variants. Scientists create a bank of possible signal waveform templates and ask how correlated each waveform is with the measured noisy data. High correlation is consistent with the presence of a signal similar to that waveform. Due to the long duration of these signals, banks could easily contain hundreds of quintillions of templates; yet, with so many possible waveforms, scientists don’t have the computational power to use the approach without making approximations that weaken the sensitivity to the signals.

G2Net is a network of Gravitational Wave, Geophysics and Machine Learning. Via an Action from COST (European Cooperation in Science and Technology), a funding agency for research and innovation networks, G2Net aims to create a broad network of scientists. From four different areas of expertise, namely GW physics, Geophysics, Computing Science and Robotics, these scientists have agreed on a common goal of tackling challenges in data analysis and noise characterization for GW detectors.

## HDF files exploration

Hierarchical Data Format (HDF) is a set of file formats (HDF4, HDF5) designed to store and organize large amounts of data. Originally developed at the U.S. National Center for Supercomputing Applications, it is supported by The HDF Group, a non-profit corporation whose mission is to ensure continued development of HDF5 technologies and the continued accessibility of data stored in HDF.
In keeping with this goal, the HDF libraries and associated tools are available under a liberal, BSD-like license for general use. HDF is supported by many commercial and non-commercial software platforms and programming languages. The freely available HDF distribution consists of the library, command-line utilities, test suite source, Java interface, and the Java-based HDF Viewer (HDFView).
The current version, HDF5, differs significantly in design and API from the major legacy version HDF4. 

## HDF5

<img src=https://raw.githubusercontent.com/NEONScience/NEON-Data-Skills/dev-aten/graphics/HDF5-general/hdf5_structure4.jpg width=500/>

The HDF5 format is designed to address some of the limitations of the HDF4 library, and to address current and anticipated requirements of modern systems and applications. In 2002 it won an R&D 100 Award.
HDF5 simplifies the file structure to include only two major types of object:
HDF Structure Example

* Datasets, which are typed multidimensional arrays
* Groups, which are container structures that can hold datasets and other groups

## Our dataset structures

<a href='https://www.kaggle.com/competitions/g2net-detecting-continuous-gravitational-waves'> Dataset Link!</a>

* <b>ID</b> is the top group of the HDF5 file and links the datapoint to it's label in the train_labels csv (group)
* <b>frequency_Hz</b> contains the range frequencies measured by the dectors (dataset)
* <b>H1</b> contains the data for the LIGO Hanford decector (group)
        - SFTs is the Short-time Fourier Transforms amplitudes for each timestamp at each frequency (dataset)
        - timestamps contains the timestamps for the measurement (dataset)

* <b>L1</b> contains the data for the LIGO Livingston decector (group)
        - SFTs is the Short-time Fourier Transforms amplitudes for each timestamp at each frequency (dataset)
        - timestamps contains the timestamps for the measurement (dataset)
        

## Spectogram analysis

A spectrogram is a visual representation of the spectrum of frequencies of a signal as it varies with time. When applied to an audio signal, spectrograms are sometimes called sonographs, voiceprints, or voicegrams. When the data are represented in a 3D plot they may be called waterfall displays.

<img src=https://upload.wikimedia.org/wikipedia/commons/c/c5/Spectrogram-19thC.png width=500/>

Spectrograms are used extensively in the fields of music, linguistics, sonar, radar, speech processing, seismology, and others. Spectrograms of audio can be used to identify spoken words phonetically, and to analyse the various calls of animals.
A spectrogram can be generated by an optical spectrometer, a bank of band-pass filters, by Fourier transform or by a wavelet transform (in which case it is also known as a scaleogram or scalogram).
Scaleograms from the DWT and CWT for an audio sample
A spectrogram is usually depicted as a heat map, i.e., as an image with the intensity shown by varying the colour or brightness. 

<img src=https://github.com/EssamMohamedAbo-ElMkarem/G2Net-Detecting-Gravitational-Waves/blob/main/docs/spect.png width=600/>

## Time Domain Data Preparation

The STFT is invertible, that is, the original signal can be recovered from the transform by the inverse STFT. The most widely accepted way of inverting the STFT is by using the overlap-add (OLA) method, which also allows for modifications to the STFT complex spectrum. This makes for a versatile signal processing method, referred to as the overlap and add with modifications method. 

The inverse Fourier transform of X(τ,ω) for τ fixed:

<img src=https://wikimedia.org/api/rest_v1/media/math/render/svg/3f1a24905c6c4264cd723848f08ce3549b068e54 width=500/>

<img src=https://github.com/EssamMohamedAbo-ElMkarem/G2Net-Detecting-Gravitational-Waves/blob/main/docs/time%20domain.png width=700/>


## Detected Frequencies Distribution

<img src=https://github.com/EssamMohamedAbo-ElMkarem/G2Net-Detecting-Gravitational-Waves/blob/main/docs/frequency.png width=700/>

## Time Stamp Distribution

<img src=https://github.com/EssamMohamedAbo-ElMkarem/G2Net-Detecting-Gravitational-Waves/blob/main/docs/ts.png width=700/>

## Simulated Gravitational Wave "Distance vs. Amplitude"

<img src='https://github.com/EssamMohamedAbo-ElMkarem/G2Net-Detecting-Gravitational-Waves/blob/main/docs/simulated%20gw.png' width=600/>

## Modeling with LSTM in Time Domain

Long short-term memory (LSTM) is an artificial neural network used in the fields of artificial intelligence and deep learning. Unlike standard feedforward neural networks, LSTM has feedback connections. Such a recurrent neural network (RNN) can process not only single data points (such as images), but also entire sequences of data (such as speech or video). For example, LSTM is applicable to tasks such as unsegmented, connected handwriting recognition, speech recognition, machine translation, robot control, video games, and healthcare. LSTM has become the most cited neural network of the 20th century.

<img src=https://www.mdpi.com/sensors/sensors-21-05625/article_deploy/html/images/sensors-21-05625-g001.png width=800/>

The name of LSTM refers to the analogy that a standard RNN has both "long-term memory" and "short-term memory". The connection weights and biases in the network change once per episode of training, analogous to how physiological changes in synaptic strengths store long-term memories; the activation patterns in the network change once per time-step, analogous to how the moment-to-moment change in electric firing patterns in the brain store short-term memories. The LSTM architecture aims to provide a short-term memory for RNN that can last thousands of timesteps, thus "long short-term memory".

A common LSTM unit is composed of a cell, an input gate, an output gate and a forget gate. The cell remembers values over arbitrary time intervals and the three gates regulate the flow of information into and out of the cell.

LSTM networks are well-suited to classifying, processing and making predictions based on time series data, since there can be lags of unknown duration between important events in a time series. LSTMs were developed to deal with the vanishing gradient problem that can be encountered when training traditional RNNs. Relative insensitivity to gap length is an advantage of LSTM over RNNs, hidden Markov models and other sequence learning methods in numerous applications.

The compact forms of the equations for the forward pass of an LSTM cell with a forget gate are:

<img src=https://wikimedia.org/api/rest_v1/media/math/render/svg/dc89390b3136ccbbc223a1ed110c6d2b4856069c width=300/>

## LSTM Model Architecture

<img src='https://github.com/EssamMohamedAbo-ElMkarem/G2Net-Detecting-Gravitational-Waves/blob/main/docs/lstmmodel.png' width=600/>

## LSTM loss curve

<img src='https://github.com/EssamMohamedAbo-ElMkarem/G2Net-Detecting-Gravitational-Waves/blob/main/docs/lstm%20loss.png' width=600/>



## Modeling with Vision Transformers(ViT) vs. Convolutional Neural Network(CNN)

The concept of Vision Transformer (ViT) is an extension of the original concept of Transformer. It is only the application of Transformer in the image domain with slight modification in the implementation in order to handle the different data modality. More specifically, a ViT uses different methods for tokenization and embedding. However, the generic architecture remains the same. An input image is split into a set of image patches, called visual tokens. The visual tokens are embedded into a set of encoded vectors of fixed dimension. The position of a patch in the image is embedded along with the encoded vector and fed into the transformer encoder network which is essentially the same as the one responsible for processing the text input. 

<img src=https://miro.medium.com/max/1400/1*l37va2Mu8Snx6LLb13430A.png width=700/>

There are multiple blocks in the ViT encoder and each block consists of three major processing elements: Layer Norm, Multi-head Attention Network (MSP) and Multi-Layer Perceptrons (MLP). Layer Norm keeps the training process on track and let model adapt to the variations among the training images. MSP is a network responsible for generation of attention maps from the given embedded visual tokens. These attention maps help network focus on most important regions in the image such as object(s). 

## Convolutional Neural Network

In deep learning, a convolutional neural network (CNN, or ConvNet) is a class of artificial neural network (ANN), most commonly applied to analyze visual imagery. CNNs are also known as Shift Invariant or Space Invariant Artificial Neural Networks (SIANN), based on the shared-weight architecture of the convolution kernels or filters that slide along input features and provide translation-equivariant responses known as feature maps. Counter-intuitively, most convolutional neural networks are not invariant to translation, due to the downsampling operation they apply to the input. They have applications in image and video recognition, recommender systems, image classification, image segmentation, medical image analysis, natural language processing, brain–computer interfaces, and financial time series.

<img src= https://production-media.paperswithcode.com/method_collections/cnn.jpeg width=700/>

CNNs are regularized versions of multilayer perceptrons. Multilayer perceptrons usually mean fully connected networks, that is, each neuron in one layer is connected to all neurons in the next layer. The "full connectivity" of these networks make them prone to overfitting data. Typical ways of regularization, or preventing overfitting, include: penalizing parameters during training (such as weight decay) or trimming connectivity (skipped connections, dropout, etc.) CNNs take a different approach towards regularization: they take advantage of the hierarchical pattern in data and assemble patterns of increasing complexity using smaller and simpler patterns embossed in their filters. Therefore, on a scale of connectivity and complexity, CNNs are on the lower extreme. 

## General CNN vs. ViT talk

The differences between CNNs and Vision Transformers are many and lie mainly in their architectural differences.
In fact, CNNs achieve excellent results even with training based on data volumes that are not as large as those required by Vision Transformers.
This different behaviour seems to derive from the presence in the CNNs of some inductive biases that can be somehow exploited by these networks to grasp more quickly the particularities of the analysed images even if, on the other hand, they end up limiting them making it more complex to grasp global relations.

On the other hand, the Vision Transformers are free from these biases which leads them to be able to capture also global and wider range relations but at the cost of a more onerous training in terms of data.
Vision Transformers also proved to be much more robust to input image distortions such as adversarial patches or permutations.
However, choosing one architecture over another is not always the wisest choice, and excellent results have been obtained in several Computer Vision tasks through hybrid architectures combining convolutional layers with Vision Transformers.


## Vision Transformer Patches Visualization

<img src=https://github.com/EssamMohamedAbo-ElMkarem/G2Net-Detecting-Gravitational-Waves/blob/main/docs/patches.png width=400/>

## Vision Transformer Architecture

<img src=https://github.com/EssamMohamedAbo-ElMkarem/G2Net-Detecting-Gravitational-Waves/blob/main/docs/vitmodel.png width=1000/>

## CNN Architecture

<img src=https://github.com/EssamMohamedAbo-ElMkarem/G2Net-Detecting-Gravitational-Waves/blob/main/docs/cnnarch.png width=600/>
<img src=https://github.com/EssamMohamedAbo-ElMkarem/G2Net-Detecting-Gravitational-Waves/blob/main/docs/cnnmodel.png width=1000/>


## Binary Cross Entropy Loss Curve ViT vs. CNN

<img src='https://github.com/EssamMohamedAbo-ElMkarem/G2Net-Detecting-Gravitational-Waves/blob/main/docs/cnn%20vs%20vit.png' width=1000/>


## Notes about Frequency Domain modeling

* As expected the CNN model could converge faster 3x than the ViT model as the available dataset size isn't the ideal for such architecture(we's talking about 600 instances here) which is based on vision transformers which in turn needs more data than CNN to work properly. 

* The model loss curve is descending but it doesn't mean that the model is learning well, that's why further INVESTIGATION and IMPROVEMENTS shall be done on this work to find out if the imbalanced classes is the main issue here or if we need to try feature extraction techniques(noise cancelation filters, etc.) other than using the SFT's available. One of the possible improvements here is to use pre-trained model and only fine tune it, this shall overcome data limitation  and other issues.

