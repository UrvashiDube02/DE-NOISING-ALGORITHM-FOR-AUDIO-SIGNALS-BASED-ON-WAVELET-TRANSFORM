# DE-NOISING-ALGORITHM-FOR-AUDIO-SIGNALS-BASED-ON-WAVELET-TRANSFORM

**ABSTRACT**

In this project I have proposed a real time de-noising algorithm for audio signals based on the Wavelet Transform. White noise is omnipresent in the spectrum and is thus especially hard to filter. I have used the locality of the wavelet function to single out the frequency domains of the signal itself and thereby able to de-noise it. Perfect denoising is not easy, the higher the threshold coefficient is set, the more the noise is detected, but the more the original signal is affected as well. I have implemented a flexible framework for denoising that includes hard and soft thresholding and different Wavelet Transforms. The presented de-noising algorithm is a comparative study of four different Wavelets and two Thresholds.

**INTRODUCTION**

Noise reduction in speech signals is a field of study to recover an original signal from its noise corrupted signal. The noise can be of various types like white, impulsive or even other types of noise usually found in speech signals. Over the past decades, the removal of this noise from audio signals has become an area of interest of several researchers around the world, since the presence of noise can significantly degrade the quality and intelligibility of these signals. Many studies have been conducted since the sixties, with the goal of developing algorithms for improving the quality of audio and speech. Michael T. Johnson et al have demonstrated the application of the Bionic Wavelet Transform (BWT), an adaptive wavelet transform derived from a non-linear auditory model of the cochlea, to the task of speech signal enhancement. Results measured objectively by Signal- to Noise ratio and Segmental SNR and subjectively by Mean Opinion Score were given for additive white Gaussian noise.

**DESCRIPTION**

It is considered that the audio signal is polluted by Additive White Gaussian Noise (AWGN) and the polluted signal is subjected to the removal of noise using the proposed denoising technique. The processes performed in the proposed denoising technique are explained in detail as follows

(i) Transformation of the noisy signal to wavelet domain

(ii) Generation of a set of closer blocks

(iii) Generating a multidimensional array

(iv) Reconstructing the denoised audio signal.

In the proposed work, initially, the noisy audio signal is subjected to Wavelet Transformation. Wavelet transformation produces a few significant coefficients for the signals with discontinuities. Audio signals are smooth with a few discontinuities. Hence, wavelet transformation has better capability for representing these signals when compared to other transformations.

![image](https://user-images.githubusercontent.com/87383888/125574933-ca7bfe1b-c6af-4b56-89af-d8bbf9b0a8a2.png)

**METHODOLOGY**

1. Read the Audio Signal.
2. Generating the noisy signal by adding white gaussian noise. The Noisy Signals of the form:

![image](https://user-images.githubusercontent.com/87383888/125575032-324533c2-acb3-4df1-9e42-c46210049c03.png)

3. Choice of the wavelet used. Choices provided are daubechies- 13, daubechies40, symlet-13, symlet-21.
4. Defining the level set and the number of levels and decomposition stages.
5. The computation of wavelet transforms of the given speech signal and extraction of the wavelet and the scaling coefficients with respect to the wavelet chosen.
6. Choice of threshold set method. Comparison between Universal, minimax and rigrsure thresholding.
7. Choice of thresholding methodology between hard and soft thresholding.
8. Computation of SNR for denoised and noisy signal with respect to the original signal.
9. Replay of the audio denoised signal and plot of the denoised and the original noisy signal for comparison.

**LITERATURE SURVEY**

![image](https://user-images.githubusercontent.com/87383888/125575140-171a6ab3-44af-4187-9800-9f4f9c6e6100.png)

![image](https://user-images.githubusercontent.com/87383888/125575160-e02c5d76-8c1f-4e60-bad1-03de56d9c32c.png)


**MATLAB CODE**

clc

close all

% Reading the audio file

[file,path] = uigetfile('./Databases/*.m4a', 'Select the speech files', 'MultiSelect', 'on');

[ipsignal, Fs] = audioread([path,file]); %reads the audio signal

ipsignal = ipsignal(1:length(ipsignal)/2);

amp = 100;

ipsignal = amp*ipsignal;

N = length(ipsignal);

sn = 10;

ipsignalN = awgn(ipsignal,sn);

sound(ipsignalN,Fs); %giving out the noisy

speech through speaker 

level = 3;

fprintf('\tchoose Wavelet:\n\t1: daubechies-13\n\t2:

Daubechies-40\n\t3: Symlet-13\n\t4: Symlet-21\n\t');

wname = input('Enter you choice: ');

if wname == 1 %if daubechies-13

wavelet type chosen

wt = 'db13';

elseif wname == 2 %if daubechies-40

wavelet type chosen

wt = 'db40';

elseif wname == 3 %if symlet-13 wavelet

type chosen

wt = 'sym13';

elseif wname == 4 %if symlet-21 wavelet

type chosen

wt = 'sym21';

end

[LoD,HiD,LoR,HiR] = wfilters(wt);

[C,L] = wavedec(ipsignalN,level,LoD,HiD); %Wave description

cA3 = appcoef(C,L,wt,level); %Approximation coefficient

[cD1,cD2,cD3] = detcoef(C,L,[1,2,3]); %Detail coeffcients

A3 = wrcoef('a',C,L,LoR,HiR,level); %Wavelet reconstruction

coefficients

D1 = wrcoef('d',C,L,LoR,HiR,1);

D2 = wrcoef('D',C,L,LoR,HiR,2);

D3 = wrcoef('D',C,L,LoR,HiR,3);

%step 2 - Thresholding

fprintf('\n\tChoose Threshold Rule:\n\t1: Universal\n\t2:

Minimax\n\t3: Rigrsure\n\t');

tr = input('Enter your choice: ');

%___________________________________________________________________________________________________________________________

if tr == 1 %If Universal

thresholding rule chosen

D = [D1 D2 D3]; %Wave

tptr = 'sqtwolog';

thr_D1 = thselect(D1,tptr); %Threshold

Selection for denoising

thr_D2 = thselect(D2,tptr);

thr_D3 = thselect(D3,tptr);

th = zeros(1,length(D)); 

Dth = zeros(1,length(D));

fprintf('\n\t choose threshold type:\n\t1: Soft\n\t2:Hard\n\t');

sh = input('Enter you choice: ');

if sh == 1 %if user wants to do

soft thresholding while following the Universal thresholding

rule

sorh = 's';

elseif sh == 2 %if user wants to do

hard thresholding while following the Universal thresholding

rule

sorh = 'h';

end

for g =1:length(D)

th(g) = sqrt(2*log(numel(D(g)))); %formula

Dth(g) = wthresh(D(g),sorh,th(g)); %Setting

threshold value

end

denoised = A3;

for i=1:length(Dth)

denoised = denoised+Dth(i);

end

customplot(ipsignal,ipsignalN,denoised);

sound(denoised,Fs);

%__________________________________________________________________________________________________________________________

elseif tr == 2 %if Minmax

thresholding rule chosen

tptr = 'minimaxi';

thr_D1 = thselect(D1,tptr); %Threshold

Selection for denoising

thr_D2 = thselect(D2,tptr);

thr_D3 = thselect(D3,tptr);

fprintf('\n\t choose threshold type:\n\t1: Soft\n\t2:Hard\n\t');

sh = input('Enter you choice: ');

if sh == 1 %if user wants

to do soft thresholding while following the Minmax

thresholding rule

sorh = 's';

elseif sh == 2 %if user wants

to do hard thresholding while following the Minmax

thresholding rule

sorh = 'h';

end

for g =1:length(D)

th(g) = sqrt(2*log(numel(D(g)))/pow2(i));

Dth(g) = wthresh(D(g),sorh,th(g));

end

denoised = A3;

for i=1:length(Dth)

denoised = denoised+Dth(i);

end

customplot(ipsignal,ipsignalN,denoised);

sound(denoised,Fs);

%__________________________________________________________________________________________________________________________


elseif tr == 3 %if Rigrsure

thresholding rule chosen

tptr = 'rigrsure';

thr_D1 = thselect(D1,tptr); %Threshold


Selection for denoising

thr_D2 = thselect(D2,tptr);

thr_D3 = thselect(D3,tptr);

D = [D1 D2 D3];

th = zeros(1,length(D));

Dth = zeros(1,length(D)); fprintf('\n\t choose threshold type:\n\t1: Soft\n\t2:

Hard\n\t');

sh = input('Enter you choice: ');

if sh == 1 %if user wants

to do soft thresholding while following the rigrsure

thresholding rule

sorh = 's';

elseif sh == 2 %if user wants

to do hard thresholding while following the rigrsure

thresholding rule

sorh = 'h'; 

end

for g =1:length(D)

th(g) = sqrt(2*log(numel(D(g)))/pow2(i)); Dth(g) = wthresh(D(g),sorh,th(g));

end

denoised = A3;

for i=1:length(Dth)

denoised = denoised+Dth(i);

end

customplot(ipsignal,ipsignalN,denoised);

sound(denoised,Fs);

end

%__________________________________________________________________________________________________________________________ 


%Threshold coefficient of details

tD1 = wthresh(D1,sorh,thr_D1); %to perform soft or

hard thresholding

tD2 = wthresh(D2,sorh,thr_D2);

tD3 = wthresh(D3,sorh,thr_D3);

%step 3: Compute Inverse DWT

denoised = A3 + tD1 + tD2 + tD3;

sound(denoised,Fs); %giving out the

denoised speech through speaker

%Signal-to-Noise(SNR) Comparison

NoisySNR = snr(ipsignal,ipsignalN);

DenoisedSNR = snr(ipsignal,denoised);

disp('SNR of Noisy signal:')

disp(abs(NoisySNR))

disp('SNR of Denoised signal:')

disp(DenoisedSNR)%__________________________________________________________________________________________________________________________ 

function customplot(ipsignal,ipsignalN,denoised)

figure

subplot(3,1,1);

plot(ipsignal);

title('Original Speech Signal');

xlabel('samples');

ylabel('Amplitude');

subplot(3,1,2);

plot(ipsignalN);

title('Noisy Speech Signal');

xlabel('Samples');

ylabel('Amplitude');

subplot(3,1,3);

plot(denoised);

ylim([-100 100]);

title('Denoised Speech Signal');

xlabel('Samples');

ylabel('Amplitude');

end

![image](https://user-images.githubusercontent.com/87383888/125575839-a2e0c330-4816-450f-be56-99751e55145c.png)

![image](https://user-images.githubusercontent.com/87383888/125575864-36eeb997-8f81-4613-86bd-12d23c51be70.png)

**RESULTS**

I have deduced these results according to my experiment on the input audio signal:

1. Soft thresholding produces better denoised output (better SNR) as compared to hard thresholding of any wavelet method.
2. Symlet – 13 wavelet method combined with universal thresholding rule and soft thresholding produce the best results out of all the techniques. (Highest denoised SNR value)
3. Daubechies – 13 wavelet method combined with rigrsure thresholding rule and hard thresholding produce the worst results out of all the techniques. (lowest denoised SNR value)

**APPLICATIONS**

1. Denoising is an imperative procedure in Speech Recognition engines like those used in Siri, Google Now, Amazon Alexa etc.
2. Denoising can be employed in hearing-aids.
3. Similar procedures are used in active noise-cancelling headphones.
4. Denoising using wavelets is also done for images to get clearer images.

**CONCLUSION**

In my project I have denoised audio signals using an advanced technique of wavelet transform. As a whole, the taken input audio was denoised using soft and hard thresholding technique. Considering perfect threshold levels and taken ideal characteristic input signals, the process of denoising can be further optimized. As I used random noise in this project, it can also be implemented to other types of noise and better denoising can be achieved. Choosing real-time signals as input for this project can be accomplished by choosing the input signal’s characteristics as needed for the coding employed internally. Using complex wavelet basis functions and employing more than one type of threshold techniques this project can be further developed into a better denoising program

